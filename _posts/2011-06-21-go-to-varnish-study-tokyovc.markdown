---
layout: post
status: publish
published: true
title: 'Varnish勉強会 #tokyovcl に行ってきた'
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 996
wordpress_url: http://blog.restartr.com/?p=996
date: '2011-06-21 23:56:40 +0900'
date_gmt: '2011-06-21 14:56:40 +0900'
categories:
- 雑記
tags:
- varnish
- cache
- study
comments: []
---
<a href="http://www.flickr.com/photos/53244662@N04/5856931526/" title="IMAG0053 by ReSTARTR_y, on Flickr"><img src="http://farm6.static.flickr.com/5029/5856931526_1119c53a87.jpg" width="500" height="299" alt="IMAG0053"></a>

6/18(土)に開催された[Varnish勉強会 Tokyo.vcl : ATND](http://atnd.org/events/16681)に行ってきました。
初のクックパッドさんオフィス訪問です。

Varnish3のリリースパーティーも兼ねての開催だったようで、ステッカーもいただいてしまいました。
インフラ屋じゃない上に、Varnish触ったのは前日の晩という超初心者だったので結構不安でしたが、
ビアバッシュを含め色々な話しが聞けた実りある良い勉強会でした。

主催の@hmskさんをはじめ登壇者の皆様、ならびにビアバッシュで交流してくださった皆様ありがとうございました。

### 雑感
 * Varnishは画像配信の事例が多い
 * HTTPサーバーの前だけじゃなくてSolrのような内部ネットワーク間の通信にも適用できる。
 * Varnish落ちたときにApacheの負荷を避けるためにSquidが使える。
 * 拡張次第でHTTPサーバーにもKVSにも化ける（やりすぎ注意？）。
 * クラウドでの運用はフロンティア。
 * 設定ファイルのvclはもはやプログラム
 * ESIの事例なかったけど使いにくいのかな？
 * とりあえず実戦投入して様子見ないとわからないことが多そう。
 * あ、どれだけ高速化したかについての言及がなかったような…

## 各発表メモ
スライドと、その丸写しみたいな感じのメモをペタペタ。

### VarnishCache3.0新機能とVUPの仕方(@xcir)

<div style="width:425px" id="__ss_8344516"> <strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/xcir/tokyovclvarnishcache30vup" title="tokyo.vcl発表資料（VarnishCache3.0新機能とVUPの仕方）">tokyo.vcl発表資料（VarnishCache3.0新機能とVUPの仕方）</a></strong> <iframe src="http://www.slideshare.net/slideshow/embed_code/8344516" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>
<div style="padding:5px 0 12px"> View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/xcir">xcir</a> </div>

</div>
   * ESIでのgzip
      * ブロックが圧縮されているとESIできなかった(~2）
      * defaultパラメータが改善された
   * stream support
      * 別のストリームは待機させられる（？

### イカ娘も終わったしVarnishでも使うか(@phji)

<div style="width:425px" id="__ss_8366215"> <strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/phji/varnishika" title="イカ娘も終わったしVarnishでも使うか">イカ娘も終わったしVarnishでも使うか</a></strong> <iframe src="http://www.slideshare.net/slideshow/embed_code/8366215" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>
<div style="padding:5px 0 12px"> View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/phji">Kazutoshi Fujimoto</a> </div>

</div>
   * [RAM8G Xeon 4core SSD256GB ] x 20servers
   * pixiv
      * nginx -> (consistent hash) -> quid -> nginx -> apache
      * lookupとかかないとキャッシュされないｗ

### twitter検索(yats)でvarnish使ってみた(@PENGUINANA_)
   * API 7000M req/month
   * PV 150Mreq/month
   * varnish(vps) -> nginx[ webAPI -> varnish -> Solr ]
   * service : cache + failover
   * Solr: slaveのキャッシュクリアーが重いので、その前面にVarnishをおいている(ラウンドロビンとFailover）
   * autocomplete
      * キャッシュと相性よさそう

### EC2とvarnishで画像配信(@mirakui)

<div style="width:425px" id="__ss_8352369"> <strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/mirakui/ec2varnish" title="EC2とVarnishで画像配信">EC2とVarnishで画像配信</a></strong> <iframe src="http://www.slideshare.net/slideshow/embed_code/8352369" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>
<div style="padding:5px 0 12px"> View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/mirakui">Issei Naruta</a> </div>

</div>
   * 失敗談
   * TOFU
      * アップロード時ではなくオンデマンドでリサイズ
      * akamai（CDN)に7000RPSのうち、ELBには700RPSくる。60%受けるリクエストをキャッシュできればApache負荷軽減できるのでは？
         * という導入経緯（からの失敗
      * 1st challenge
         * EC2(M2.XLARGE[17.1GB RAM] + EBS[100GB for swap])
         * 300rps
         * 結果：オンメモリだけなら順調だけど、あふれてきてOSにSWAPさせたとたんLOADAVERAGEが爆発して死亡。
      * 2nd challenge
         * EC2(M2.XLARGE[17.1GB RAM] + EBS[100GB for ext3])
         * "file storage"を使っても結局メモリ容量が大量に必要
      * ec2のxlargeインスタンスには、IO性能「標準」と「高」がある
         * 今回は「標準」を利用して失敗
         * 「高」ならうまくいくかも
   * varnish3
      * hash director
         * consistent hashingができるかも？
         * nginxいらずに複数varnish運用ができるかも？

### Puppetでvarnishもsquidも面倒みる(@ar1)
   * ふつうはvarnishは落ちるのでLBつかって並列で運用
   * varnishおちるとWebサーバーが負荷かかってしまう
      * LB -> varnish -> squid -> apache
      * squidどうしでキャッシュを共有できるので、１つSquidおちてもWebサーバーにききにいかずにすむ。
   * puppet
      * 設定をらくにする
      * cacheのExpire
      * サーバーのぬきさし

### 楽天でvarnish(@spchidren)
   * L7 balancingがやりたくてVarnish導入
      * URLによってWebサーバーのグループを切り替えたい
   * akamai -> LB -> varnish(RAM24G[malloc18G/hitrate 75%] -> apache(+mod_thumb)
   * LBからVarnishを切り離すのはどうやる？
   * CDNやVarnishのcache clearをどうやるか
      * 画像登録されたときにMQに登録して、必要に応じて消す
      * CDNは常にExpireされない状態にしておける。
   * ImageMagicおもい
      * varnishにサムネイルをつくるしくみを組み込んでみれば？（FreeImage)
      * cc_commandでインクルードさせて、C言語でサムネイルつくるしくみをくみこむ
   *  varnishのいくさき
      * nginxにも近いしredisにもちかづいてきてる
      * どこにいくんでしょ

### TDDでVCL書いてデプロイ(@hmsk)
   * varnishtest
      * RSpecぽい書き方できる
      * xxx.vcltestファイルにケースを記述
   * vmod
      * vclの設定をC言語っぽく描ける
      * パージや圧縮、エラーなどに使える
      * モバイル端末のリストのチェックにもVCLから外出しできる

## 関連リンク
 * ATND
   * [Varnish勉強会 Tokyo.vcl : ATND](http://atnd.org/events/16681)
 * Ustream
   * [http://www.ustream.tv/recorded/15453305](http://www.ustream.tv/recorded/15453305)
   * [http://www.ustream.tv/recorded/15453834](http://www.ustream.tv/recorded/15453834)
 * Togetter
   * [Togetter - 「第一回 Varnish勉強会 Tokyo.vcl」](http://togetter.com/li/151585)
 * 勉強会レポート
   * [Varnish勉強会 Tokyo.vclを行いました - ククラフト](http://d.hatena.ne.jp/hxmasaki/20110620/1308547067)
   * [私がクックパッドの画像配信野郎です - 床のトルストイ、ゲイとするとのこと](http://d.hatena.ne.jp/mirakui/20110619/1308497959)
   * [Tokyo.vclでVarnishCache3の新機能とVUPの仕方を話してきました &laquo; cat /dev/random &gt; /dev/null &amp;](http://blog.xcir.net/?p=438)
   * [東京varnish勉強会(Tokyo.vcl)まとめ &laquo;  blog.udzura.jp](http://blog.udzura.jp/2011/06/20/summary-of-tokyo-varnish-study-on-20110618/)

