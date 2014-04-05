---
layout: post
status: publish
published: true
title: NettyでWebSocketサーバーを実装する
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
excerpt: "この記事は <a href=\"http://atnd.org/events/10683\">Scala Advent Calendar JP
  2010</a> 23 日目(12/29)です。\r\n\r\n前日の <a href=\"http://twitter.com/cooldaemon\">@cooldaemon</a>
  さんが<a href=\"http://d.hatena.ne.jp/cooldaemon/20101228\">Scala Actor + NIO</a>という、ものすごい記事を書いていらっしゃったのでこの流れの中で投稿するのが忍びないくらいです。\r\n＃まさか前日にNIOネタがくるとはぁぁぁ…\r\n\r\nさて、今回はNettyを使ってWebSocketサーバーを実装してみました。\r\nJavaではなくScalaで、です。\r\nとはいっても、目的はJavaコードをScalaに直す練習も兼ねて、公式サンプルにあるJavaコードをScalaに書きなおしただけですが。\r\nご指摘などありましたら謹んでお受けいたします…\r\n"
wordpress_id: 465
wordpress_url: http://blog.restartr.com/?p=465
date: '2010-12-29 18:45:53 +0900'
date_gmt: '2010-12-29 09:45:53 +0900'
categories:
- Scala
tags:
- Scala
- websocket
- netty
comments: []
---
<p>この記事は <a href="http://atnd.org/events/10683">Scala Advent Calendar JP 2010</a> 23 日目(12/29)です。</p>
<p>前日の <a href="http://twitter.com/cooldaemon">@cooldaemon</a> さんが<a href="http://d.hatena.ne.jp/cooldaemon/20101228">Scala Actor + NIO</a>という、ものすごい記事を書いていらっしゃったのでこの流れの中で投稿するのが忍びないくらいです。<br />
＃まさか前日にNIOネタがくるとはぁぁぁ…</p>
<p>さて、今回はNettyを使ってWebSocketサーバーを実装してみました。<br />
JavaではなくScalaで、です。<br />
とはいっても、目的はJavaコードをScalaに直す練習も兼ねて、公式サンプルにあるJavaコードをScalaに書きなおしただけですが。<br />
ご指摘などありましたら謹んでお受けいたします…<br />
<a id="more"></a><a id="more-465"></a></p>
<h3>Netty</h3>
<ul>
<li><a href="http://www.jboss.org/netty/">Netty - the Java NIO Client Server Socket Framework - JBoss Community</a></li>
</ul>
<p>Nettyの概要はこのへんで。</p>
<ul>
<li><a href="http://ameblo.jp/principia-ca/entry-10629939611.html">JavaネットワークアプリケーションフレームワークNettyの紹介｜サイバーエージェント 公式エンジニアブログ</a></li>
</ul>
<h3>開発環境</h3>
<ul>
<li>Scala-2.8.0</li>
<li>Netty-3.2.3.Final</li>
</ul>
<h3>Echoサーバー</h3>
<p>サンプル的にEchoサーバーから書いてみます。</p>
<h4>やること</h4>
<ol>
<li>Echoハンドラを実装</li>
<li>NioChannelFactoryにスレッドプールを登録</li>
<li>BootstrapにNioChannelFactoryを登録</li>
<li>PipelineFactoryにEchoハンドラを登録</li>
<li>BootstrapにPipelineFactoryを登録</li>
<li>InetSocketAddressに（ホストと）ポート番号を指定してBootstrapに登録してサービス開始</li>
</ol>
<p><a href="https://github.com/ReSTARTR/nettyws/blob/master/src/main/scala/com/restartr/nettyws/EchoServer.scala">EchoServer.scala</a><br />
まずは起動元から。</p>

```scala
object EchoServer {
  @throws(classOf[Exception])
  def main(args:Array[String]) {
    // サーバーのセットアップ
    val bootstrap = new ServerBootstrap(
      new NioServerSocketChannelFactory(
        Executors.newCachedThreadPool(), // bossExecutor
        Executors.newCachedThreadPool()  // workerExecutor
      ))
    
    bootstrap.setPipelineFactory(
      // リクエストをそのまま返すハンドラを実装して登録
      new ChannelPipelineFactory() {
        def getPipeline() = Channels.pipeline(new EchoServerHandler())
      }
    )
    
    // 8080番で待ち受け開始
    bootstrap.bind(new InetSocketAddress(8080))
  }
}
```

<p>で、ハンドラ側がこんなかんじ。<br />

SimpleChannelUpstreamHandlerの</p>
<ul>
<li>messageReceived(ctx:ChannelHandlerContext, e:MessageEvent)</li>
<li>def exceptionCaught(ctx: ChannelHandlerContext, e: ExceptionEvent) </li>
</ul>
<p>をオーバーライドして処理内容を定義すればOK。<br />
何かを呼び出し元に返すには、"e.getChannel().write( {レスポンス} )"で。</p>

```scala
class EchoServerHandler extends SimpleChannelUpstreamHandler {
  val logger = java.util.logging.Logger.getLogger("EchoServerHandler")
  val transferredBytes = new AtomicLong()
  
  // そのまま返す
  override def messageReceived(ctx:ChannelHandlerContext, e:MessageEvent) {
    transferredBytes.addAndGet(
      e.getMessage().asInstanceOf[ChannelBuffer].readableBytes())
    
    println("echo_server: message received: " + e.getMessage())
    // レスポンスを返す
    e.getChannel().write(e.getMessage())
  }
  
  // 例外発生時はここにくる
  override def exceptionCaught(ctx: ChannelHandlerContext, e: ExceptionEvent) {
    logger.log(Level.WARNING, 
               "Unexpected exception from downstream.",
               e.getCause())
    
    e.getChannel().close()
  }
}
```

<h3>WebSocketサーバー</h3>
<p>本題のWebSocketサーバーです。<br />
Echoサーバーの応用で、WebSocket用のハンドラを作成して、Bootstrapに登録すれば良い訳です。</p>
<h4>仕様</h4>
<ul>
<li>WebSocketServer: 起動元</li>
<li>WebSocketServerHandler: プロトコルハンドラ。肝の部分。</li>
<li>WebSocketServerPipelineFactory: パイプライン生成。</li>
<li>WebSocketServeIndexPage: WebSocketクライアント用HTMLの生成</li>
</ul>
<h4>実装</h4>
<p>すべて書くと冗長なので要所だけ抜き出しました。<br />
<a href="https://github.com/ReSTARTR/nettyws/tree/master/src/main/scala/com/restartr/nettyws">全ソースはGithubを見てください。</a></p>
<p><b>WebSocketServerIndexPage.scala</b><br />
まずはクライアント側はこんな感じで、インプットフォームに入力した文字列をWebSocketサーバーに投げ、結果をdivに追記するだけです。</p>

```scala
      var socket; 
      if (window.WebSocket) {
        socket = new WebSocket( 'ws://localhost:8080/uppercase' );
        socket.onmessage = function(event) { log(event.data) }
        socket.onopen = function(event) { log('web socket opened') }
        socket.onclose = function(event) { log('web socket closed') }
      } else {
        alert('your browser does not supported web socket')
      }
      function log(message) {
        p = document.createElement('p');
        p.innerHTML = message
        document.getElementById('log').appendChild(p) ;
      }
      function send(message) {
        if (!window.WebSocket) { return; }
        if (socket.readyState == WebSocket.OPEN) {
          socket.send(message)
        } else {
          alert('the socket is not open.')
        }
      }
```

<p><b>WebSocketServer.scala</b><br />
サーバー起動オブジェクトです。<br />
HTML/WebSocketを扱うハンドラを登録してポートで待ち受けます。</p>

```scala
    // WebSocket用ハンドラを含むPipelineFactoryを登録
    bootstrap.setPipelineFactory(new WebSocketServerPipelineFactory())
    
    // 8080番で待ち受け開始
    bootstrap.bind(new InetSocketAddress("localhost", 8080))
```

<p><b>WebSocketServerHandler.scala</b><br />
実装するのは、Echoサーバーと同じく、messageReceived()とexceptionCaught()の２つ。<br />
処理を分割しているのですこしだけメソッド多めです。<br />
messageRecieved()で受信内容によって実際のハンドラをHttp/WebSocketのどちらかに切り替えます。</p>

```scala
class WebSocketServerHandler extends SimpleChannelUpstreamHandler {
  val WEBSOCKET_PATH = "/uppercase"
  
  /**
   * メッセージ受信時の処理
   *   メッセージの内容によってWebSocketかHttpかハンドラを切り替える
   */
  @throws(classOf[Exception])
  override def messageReceived(ctx: ChannelHandlerContext, e: MessageEvent) {
    val msg:Object = e.getMessage()
    msg match {
      case frame: WebSocketFrame => {
          handleWebSocketFrame(ctx, frame)
        }
      case req: HttpRequest => {
          handleHttpRequest(ctx, req)
        }
    }
  }
  /**
   * HTTPリクエスト時の処理
   */
  @throws(classOf[Exception])
  def handleHttpRequest(ctx: ChannelHandlerContext, req: HttpRequest) {
    // DEBUG
    println("handleHttpRequest: " + Thread.currentThread().getName())
    
    // GETリクエスト以外は処理しない
    // "/"にきたらWebSocketクライアント用ページを送信
    // "/uppercase"にきたらリクエスト文字列を大文字に変換して返す
    if (req.getMethod() != GET) {
      sendHttpResponse(
        ctx, 
        req, 
        new DefaultHttpResponse(HTTP_1_1, FORBIDDEN))
    } else if (req.getUri().equalsIgnoreCase("/")) {
    	
      // (中略) デフォルトHTMLの送信
      
    } else if (req.getUri().equalsIgnoreCase(WEBSOCKET_PATH) &&
               Values.UPGRADE.equalsIgnoreCase(req.getHeader(Names.CONNECTION)) &&
               Values.WEBSOCKET.equalsIgnoreCase(req.getHeader(Names.UPGRADE))) {
      
      // (中略) WebSocket接続処理
      
      // ハンドラをHTTPからWebSocketに切り替えて、
      // send the handshake response
      val p = ctx.getChannel().getPipeline()
      p.remove("aggregator")
      p.replace("decoder", "wsdecoder", new WebSocketFrameDecoder())
      
      ctx.getChannel().write(res)
      
      p.replace("encoder", "wsencoder", new WebSocketFrameEncoder())
    } else {
      sendHttpResponse(
        ctx, req, new DefaultHttpResponse(HTTP_1_1, FORBIDDEN))
    }
  }
  /**
   * WebSocketリクエスト時の処理
   */
  @throws(classOf[Exception])
  def handleWebSocketFrame(ctx: ChannelHandlerContext, frame: WebSocketFrame) {
    // 大文字に変換するして、WebSocketFrameにのせてレスポンスを返す。
    ctx.getChannel().write(
      new DefaultWebSocketFrame(
        frame.getTextData().toUpperCase))
  }
  /**
   * HTTPレスポンスの送信
   */
  @throws(classOf[Exception])
  def sendHttpResponse(ctx: ChannelHandlerContext, req: HttpRequest, res: HttpResponse) {
    // ステータスコードが200じゃなければエラーページの表示
    if (res.getStatus().getCode() != 200) {
      res.setContent(
        ChannelBuffers.copiedBuffer(
          res.getStatus().toString(), CharsetUtil.UTF_8))
      HttpHeaders.setContentLength(res, res.getContent().readableBytes())
    }
    
    // keep-aliveでなければ接続を閉じる
    val f = ctx.getChannel().write(res)
    if (!HttpHeaders.isKeepAlive(req) || res.getStatus().getCode() != 200) {
      f.addListener(ChannelFutureListener.CLOSE)
    }
  }
  /**
   * 例外発生時の処理
   */
  @throws(classOf[Exception])
  override def exceptionCaught(ctx: ChannelHandlerContext, e: ExceptionEvent) {
    println("server: exception caught: ")
    e.getCause().printStackTrace()
    e.getChannel().close()
  }
  /**
   * WebSocket接続情報
   */
  def getWebSocketLocation(req: HttpRequest) = 
    "ws://" + req.getHeader(HttpHeaders.Names.HOST) + WEBSOCKET_PATH  
}
```

<p><b>WebSocketServerPipelineFactory.scala</b><br />
パイプラインへの登録を定義します。<br />
リクエストはDecoderを通り、ハンドラで処理され、Encoderを通って返される、という流れです。<br />
ここでは初期状態としてHttp用の設定になっていますが、一旦WebSocket通信開始のリクエストを受け取ると、そのあとはWebSocketのDecoder/Encoderに切り替わります。</p>

```scala
<pre class="brush:scala">
class WebSocketServerPipelineFactory extends ChannelPipelineFactory{
  @throws(classOf[Exception])
  def getPipeline(): ChannelPipeline = {
    val pipeline = Channels.pipeline()
    
    pipeline.addLast("decoder"    , new HttpRequestDecoder())
    pipeline.addLast("aggregator" , new HttpChunkAggregator(65536))
    pipeline.addLast("encoder"    , new HttpResponseEncoder())
    pipeline.addLast("handler"    , new WebSocketServerHandler())
    
    pipeline
  }
}
```
<p>実際、ブラウザでひらいてみると、Webフォームに入力されている「hello, world」が大文字に変換されてフォーム下部に追記されていきます。<br />
まぁWebSocketサーバーを実装するだけなら、jetty7を使ったほうがシンプルに早くかけると思います。<br />
Memcacheプロトコルを話すサービスをつくる場合とかには便利ですね。（messagepack-rpcでもnetty使っているとか。)</p>

<p>参考：</p>

* <a href="http://gihyo.jp/dev/feature/01/websocket">Jettyで始めるWebSocket超入門</a>
* <a href="http://d.hatena.ne.jp/yuroyoro/20100316/1268735022">Jetty7のWebSocketをScalaから使う - ゆろよろ日記</a>

<p>以上、お粗末様でした。。。</p>
<p># あれ？Scalaあんまり関係ない？？</p>
