---
layout: post
status: publish
published: true
title: ScalaとJetty7のWebSocketでリアルタイムチェックインマップを作ってみた
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 799
wordpress_url: http://blog.restartr.com/?p=799
date: '2011-04-06 09:00:50 +0900'
date_gmt: '2011-04-06 00:00:50 +0900'
categories:
- Scala
tags:
- Scala
- websocket
- twitter
- jetty
- foursquare
comments: []
---
<a href="http://blog.restartr.com/wp-content/uploads/2011/04/check-in-map-screenshot.png"><img src="http://blog.restartr.com/wp-content/uploads/2011/04/check-in-map-screenshot.png" alt="チェックインマップのスクリーンショット" title="チェックインマップのスクリーンショット" width="619" height="441" class="alignnone size-full wp-image-800" /></a>

最近、会社の組織変更に伴い新グループに移動しました。
その際、グループ内の自己紹介用に作ったものをGithubに上げてみました。
動いているものを公開したかったのですが永続的に公開できる場所がないのでソース公開のみです。

<ul>
<li><a href="https://github.com/ReSTARTR/checkin_map"> ReSTARTR/checkin_map - GitHub </a></li>
</ul>
以前scala advent calender 2010で「<a href="http://blog.restartr.com/2010/12/29/websocket-server-written-in-scala-with-netty/">NettyでWebSocketサーバーを実装する</a>」にてWebSocketサーバーを実装してましたが、今回はJettyを使っての実装にしてみました。

pub/subモデルもどきを採用してみたりしましたが、正直勉強不足でうまい実装方法ができていないのが現状。
少しづつリファクタリングしたいところ。
コメントやフォークでのツッコミいただけると大変嬉しいです。

実行方法
----
こちらに実行方法を記述してる通りにやれば動作するはず。

<ul>
<li><a href="https://github.com/ReSTARTR/checkin_map/blob/master/README.md">https://github.com/ReSTARTR/checkin_map/blob/master/README.md</a></li>
</ul>
仕組み
----
 * TwitterのStreamingAPIを使って、foursquareのチェックインツイートを取得
 * それをWebsocket経由で地図にリアルタイムプロット

Jetty7を使うに当たって
----
今のところ、sbt-0.7.5RC1 + jetty7.3.1.v20110307で動作確認済みです。

jetty7.3のwebsocketを使うには、sbt-0.7.5RC以上を使わないといけないようです。
sbt0.7.4だとjetty7.1までしかコンパイルできず、jetty7.1だとwebsocketの最新仕様にあってなくてソケットオープン時にコケます。
(このあたりの組み合わせは記憶があいまいですが。。。）

scalaのjsonオブジェクトではなくsjsonを使うこと
----
scala.util.parsing.json.JSONをつかってシリアライズすると、javascript側でのevalに失敗します。
（原因はクオート処理しないため。）
Scala内で完結するならまだしも、JavaScriptへ転送する場合はsjsonが良いようです。

