---
layout: post
status: publish
published: true
title: 'Hadoop conference japan 2011に参加してきた #hcj2011'
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
excerpt: "Hadoop conference japan 2011に参加してきました。\r\n\r\n * <a href=\"http://hadoop-conference-japan-2011.eventbrite.com/\">Hadoop
  Conference Japan 2011 - Eventbrite</a>\r\n\r\n今回の個人的トピック\r\n<ul>\r\n<li>AWSがHBaseサポート予定</li>\r\n<li>DremelはHadoopの補完的位置づけ</li>\r\n<li>DSLとしてのAsakusaの魅力</li>\r\n<li>AmebaのpatriotはRubyDSLで簡単ジョブ記述</li>\r\n<li>MySQLでもMapReduceできるよ</li>\r\n<li>HBaseを使うとシステムがシンプルになるよ</li>\r\n</ul>\r\n\r\n以下、メモ書きです。\r\n"
wordpress_id: 703
wordpress_url: http://blog.restartr.com/?p=703
date: '2011-02-24 09:09:42 +0900'
date_gmt: '2011-02-24 00:09:42 +0900'
categories:
- hadoop
tags:
- hadoop
- 勉強会
comments:
- id: 43
  author: 'Tweets that mention Hadoop conference japan 2011に参加してきた #hcj2011 | ReSTARTR
    -- Topsy.com'
  author_email: ''
  author_url: http://topsy.com/blog.restartr.com/2011/02/24/attended-hadoop-conference-japan-2011-hcj2011/?utm_source=pingback&amp;utm_campaign=L2
  date: '2011-02-24 09:39:10 +0900'
  date_gmt: '2011-02-24 00:39:10 +0900'
  content: '[...]  This post was mentioned on Twitter by kimukou_26 and ジェイムズ・御徒町・ブッカー,
    Masaki YOSHIDA. Masaki YOSHIDA said: ブログ書きました＞ Hadoop conference japan 2011に参加してきた
    #hcj2011 http://bit.ly/eM2RsQ     [...]'
---
Hadoop conference japan 2011に参加してきました。

 * <a href="http://hadoop-conference-japan-2011.eventbrite.com/">Hadoop Conference Japan 2011 - Eventbrite</a>

今回の個人的トピック

<ul>
<li>AWSがHBaseサポート予定</li>
<li>DremelはHadoopの補完的位置づけ</li>
<li>DSLとしてのAsakusaの魅力</li>
<li>AmebaのpatriotはRubyDSLで簡単ジョブ記述</li>
<li>MySQLでもMapReduceできるよ</li>
<li>HBaseを使うとシステムがシンプルになるよ</li>
</ul>
以下、メモ書きです。
<a id="more"></a><a id="more-703"></a>

### Hadoop on クラウド / Amazon Elastic MapReduceの真価

<div style="width:425px" id="__ss_7006529"> <strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/kentamagawa/amazon-elastic-mapreduce" title="Amazon Elastic MapReduceの紹介(英語)">Amazon Elastic MapReduceの紹介(英語)</a></strong> <object id="__sse7006529" width="425" height="355"><param name="movie" value="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=aws2011q1elasticmapreducev2-110221205244-phpapp01&stripped_title=amazon-elastic-mapreduce&userName=kentamagawa" /><param name="allowFullScreen" value="true"/><param name="allowScriptAccess" value="always"/><embed name="__sse7006529" src="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=aws2011q1elasticmapreducev2-110221205244-phpapp01&stripped_title=amazon-elastic-mapreduce&userName=kentamagawa" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="425" height="355"></embed></object>
<div style="padding:5px 0 12px"> View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/kentamagawa">玉川憲 (Ken Tamagawa) - Amazon Web Services</a> </div>

</div>
 * 11:30?12:05
 * Amazon Web Services, Jeff Barr ( @jeffbarr )
 * EMR removes 'MUCK'(=ぬかるんだ感じ) from big data operation
   * hard to manage, tuning, monitoring, debug
 * issues  prevent operation in cloud
 * instance types
   * data or I/O intensive
   * compute or I/O intensive
 * BestBuy
   * 100 node on demand
   * 3.5 billion records, 71 million unique cookies.
   * increased ROAS(returns of advertising spend) by 500%
 * QA
   * HBaseのサポートもやります。

### MapReduceによる大規模データを利用した機械学習

<div style="width:425px" id="__ss_7022974"> <strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/pfi/mapreduce-7022974" title="MapReduceによる大規模データを利用した機械学習">MapReduceによる大規模データを利用した機械学習</a></strong> <object id="__sse7022974" width="425" height="355"><param name="movie" value="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=hadoopconference2011okanohara-110222203754-phpapp01&stripped_title=mapreduce-7022974&userName=pfi" /><param name="allowFullScreen" value="true"/><param name="allowScriptAccess" value="always"/><embed name="__sse7022974" src="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=hadoopconference2011okanohara-110222203754-phpapp01&stripped_title=mapreduce-7022974&userName=pfi" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="425" height="355"></embed></object>
<div style="padding:5px 0 12px"> View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/pfi">PFI Marketing </a> </div>

</div>
 * 12:05?12:40
 * 株式会社Preferred Infrastructure, 岡野原 大輔 ( @hillbig )
 * mahout
  * 大規模分散並列処理
 * グラフィカルモデル
  * 写真の人物切り出しとか、これでできる
 * グラフィカルモデルの推論は一般に困難
  * [S, Singh LCCC 2010]
 * 数値最適化の並列分散化
  * 最適なのは[Iterative Parameter Mixture]
   1. データ分割してshardに配布
   2. shardごとに最適化
   3. 全部のθの平均をとる
   4. θを再度各shardに配り１からくりかえす
 * Dremel
  * MRにくらべて低レイテンシ
  * 簡単な統計処理のみ
  * クエリ言語はSQL
  * top-k, joinなども可能
  * use 6 years in google
  * クロール、障害分析などに利用
 * 列志向DB
  * 木構造に列志向を導入
  * M/Rの補助にDremel
 * 使い分け
  * データの構築コストは高い

### モバゲーの大規模データマイニング基盤におけるHadoop活用

<div style="width:425px" id="__ss_7009088"> <strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/hamadakoichi/hadoop-hadoop-conference-japan-2011-hcj2011" title="モバゲーの大規模データマイニング基盤におけるHadoop活用?Hadoop Conference Japan 2011? #hcj2011 ">モバゲーの大規模データマイニング基盤におけるHadoop活用?Hadoop Conference Japan 2011? #hcj2011 </a></strong> <object id="__sse7009088" width="425" height="355"><param name="movie" value="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=dataminingmbgahadoopconference2011-110222022814-phpapp02&stripped_title=hadoop-hadoop-conference-japan-2011-hcj2011&userName=hamadakoichi" /><param name="allowFullScreen" value="true"/><param name="allowScriptAccess" value="always"/><embed name="__sse7009088" src="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=dataminingmbgahadoopconference2011-110222022814-phpapp02&stripped_title=hadoop-hadoop-conference-japan-2011-hcj2011&userName=hamadakoichi" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="425" height="355"></embed></object>
<div style="padding:5px 0 12px"> View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/hamadakoichi">Koichi Hamada</a> </div>

</div>
 * 13:40?14:15
 * 株式会社ディー・エヌ・エー,  濱田 晃一( @hamadakoichi )
 * DeNA: 2010/07 23億のアクティビティ
 * データマイニング基盤
    * KPI定常算出・共有
    * 変化の検知基盤
  * Business Planning
    * 意思決定
  * Service
    * Hadoop
    * DFS
  * 全行動ログを統一形式で蓄積
 * 適用範囲
  * Pig
     * 一時的なもの、BI
  * Zebra
     * スキーマ管理（＋Pig）
  * M/R
     * JavaだけでなくPerlでも。
     * ゲームの分散シミュレーション
  * R
     * 二次集計
     * データマイニング
     * Streamingもあわせて活用
  * Mahout
     * データマイニング＆機械学習
  * DeNA Datamining libraries
     * 独自レコメンドエンジン
 * Tuning
  * LZOなど最適化
  * Pig
    * Partitioner実装最適化
    * 多段M/RのTempを圧縮
    * 独自UDF
    * 汎用の日次処理や文字列処理、ソーシャル用の独自Mapも。
    * 共通ログ：Loader
  * Mahout
    * 目的に応じた組み合わせ、ジョブの実装
 * 楽しさ
   * 統計的有意
   * 多くの人々への還元（２３００万ユーザー）
   * 感情を伴った行動情報
   * ユーザーのソーシャル体験への還元
   * パターン
   * 楽しさ：夢中になるきっかけ
 * 健全なプラットフォームへ
   * 不正書きこみの判別、年齢詐称の判別
   * ユーザーの声
   * テキストマイニング
 * 統一の行動記述
 * 重要な要素
   * Hadoop上にすべてある。
   * 記述が統一されている
   * すべてがHadoop上に統一されている
   * 解析に力を入れることができる

### Enterprise Batch Processing Framework for Hadoop

<div style="width:425px" id="__ss_7021975"> <strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/okachimachi/asakusa-enterprise-batch-processing-framework-for-hadoop" title="Asakusa Enterprise Batch Processing Framework for Hadoop">Asakusa Enterprise Batch Processing Framework for Hadoop</a></strong> <object id="__sse7021975" width="425" height="355"><param name="movie" value="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=asakusa2011-2-22disclosed-110222174121-phpapp01&stripped_title=asakusa-enterprise-batch-processing-framework-for-hadoop&userName=okachimachi" /><param name="allowFullScreen" value="true"/><param name="allowScriptAccess" value="always"/><embed name="__sse7021975" src="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=asakusa2011-2-22disclosed-110222174121-phpapp01&stripped_title=asakusa-enterprise-batch-processing-framework-for-hadoop&userName=okachimachi" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="425" height="355"></embed></object>
<div style="padding:5px 0 12px"> View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/okachimachi">okachimachi</a> </div>

</div>
 * 14:15?14:50
 * ウルシステムズ株式会社, 神林 飛志( @okachimachiorz1 )
 * Asakusa
   * 基幹バッチ
   * 夜間バッチゼロ。
   * Hadoopは開発手法や運用に問題有り。
   * Pig/Hiveでは足りない
   * DAGベースの多層DSL
   * TX/Rollback制御をHadoopの外（Asakusa）でやる
   * MRコンパイラ
     * Ashigel
     * 運用スクリプトまではく。
   * ModelGenerator
   * データ層の自動化
   * テストのインテグレーション

### Hiveを用いたAmebaサービスのログ解析共通基盤

<div style="width:425px" id="__ss_7014518"> <strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/toutouzone/hadoop-conferencejapan2011" title="Hadoop conferencejapan2011">Hadoop conferencejapan2011</a></strong> <object id="__sse7014518" width="425" height="355"><param name="movie" value="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=hadoopconferencejapan2011-110222063021-phpapp01&stripped_title=hadoop-conferencejapan2011&userName=toutouzone" /><param name="allowFullScreen" value="true"/><param name="allowScriptAccess" value="always"/><embed name="__sse7014518" src="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=hadoopconferencejapan2011-110222063021-phpapp01&stripped_title=hadoop-conferencejapan2011&userName=toutouzone" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="425" height="355"></embed></object>
<div style="padding:5px 0 12px"> View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/toutouzone">Ichiro Fukuda</a> </div>

</div>
 * 14:50?15:25
 * 株式会社サイバーエージェント, 福田 一郎( @toutou )
 * Blog: [http://ameblo.jp/principia-ca/entry-10635727790.html](<http://ameblo.jp/principia-ca/entry-10635727790.html>)
 * 非エンジニア向け
 * hadoop実績
  * pigg：HDFS
  * pico：ログ解析にEMR/Pig
  * アクセス解析(0.13.1)
 * 解析基盤partiot
  * 独自に解析してるけどだいたい定形
  * サービス全体の統合的な現状把握と未来予測
  * 結果の表示にpatriot
  * アドホック集計にhue
 * patriot
  * CDH3b1
  * puppet/nagios/ganglia
  * ext_js/hue/hinemos
  * gzip/sequenceFileのブロック単位圧縮
  * インポート：scp/hdfs get
 * importDSL
  * log
   * 600job/daily
   * 700job/monthly
   * record 1300万以上
   * 前処理を３〜４時間
 * Replication
   * clusterをmaster/slaveにする
   * slaveを非エンジニアに利用させる

### ライトニングトーク
 * 15:40?16:30

#### 分散ファイルシステムGfarm上でのHadoop MapReduce

<div style="width:425px" id="__ss_7029061"> <strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/shun0102/h-7029061" title="分散ファイルシステムGfarm上でのHadoop MapReduce">分散ファイルシステムGfarm上でのHadoop MapReduce</a></strong> <object id="__sse7029061" width="425" height="355"><param name="movie" value="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=hcj2011-110223050641-phpapp01&stripped_title=h-7029061&userName=shun0102" /><param name="allowFullScreen" value="true"/><param name="allowScriptAccess" value="always"/><embed name="__sse7029061" src="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=hcj2011-110223050641-phpapp01&stripped_title=h-7029061&userName=shun0102" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="425" height="355"></embed></object>
<div style="padding:5px 0 12px"> View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/shun0102">shun0102</a> </div>

</div>
 * Shunsuke Mikami( @shun0102 )
 * GlusterFS
 * Ceph: 高負荷で固まる
 * Lustre,PVFS2
 * GFarm: ≒HDFS
   * 複製の作成は同期的
 * 他FSの利用
   * JNIのlayer　／　マウント
 * GlusterFS
   * マスターなし
   * FUSEベース
 * HDFS>HDFS(3reps)>GlusterFS

#### MySQLにMapReduceジョブトラッカを実装する
 * Sadayuki Furuhashi( @frsyuki )

### Hadoop and HBase for ranking processing at Rakuten
 * Yifeng Jiang( @uprush )
 * Hadoop
  * リアルタイムから年次まで。
  * mutable data?扱いにくい
 * HBase
  * usecase
  * pig: realtime ranking
  * 100 pure java jobs per day
 * HBase
  * soft realtime access
  * 40x faster on ranking contents
 * before
  * DB -> HDFS
  * table split is boring
 * after
  * data go to hBase, the processed by MR
  * 1.5k rows insert /s
  * 0.3M rows scan /s
  * system become simple
 * balance
  * HW
  * OS resources
  * config
  * application design

#### Sneak Preview of "Hapyrus" ~ Hadoopアプリ開発＆共有サービス on the CLOUD

<div style="width:425px" id="__ss_7038404"> <strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/fujibee/hadoop-conference-japan-2011-lt-hapyrus" title="Hadoop Conference Japan 2011 LT Hapyrus">Hadoop Conference Japan 2011 LT Hapyrus</a></strong> <object id="__sse7038404" width="425" height="355"><param name="movie" value="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=hadoopconferencejapan2011ltnomovie-110223203652-phpapp02&stripped_title=hadoop-conference-japan-2011-lt-hapyrus&userName=fujibee" /><param name="allowFullScreen" value="true"/><param name="allowScriptAccess" value="always"/><embed name="__sse7038404" src="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=hadoopconferencejapan2011ltnomovie-110223203652-phpapp02&stripped_title=hadoop-conference-japan-2011-lt-hapyrus&userName=fujibee" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="425" height="355"></embed></object>
<div style="padding:5px 0 12px"> View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/fujibee">Koichi Fujikawa</a> </div>

</div>
 * Fujikawa Koichi( @fujibee )
 * 敷居が高い（セットアップ?M/R?運用の工数）
 * Hapyrus
   * サイト：[http://hapyrus.com/](<http://hapyrus.com/>)
   * デモビデオ: [http://www.youtube.com/watch?v=1cF-1tcapvE](<http://www.youtube.com/watch?v=1cF-1tcapvE>)
   * ホスティング（＝EMR）
   * ディストリビューション＝マーケットプレイス
   * 基本無料

#### Bonding とネットワークスループット
 * Takahiro Kaneko
 * 802.3ad + src-dst-id

#### Yuuna Kurita: Hadoop+MongoDBでRで出力する時にRubyでミドルウェアを使う

※これ以降も有用な発表でしたがバッテリー切れのためメモはなしです…orz※

### マルチユーザーでHadoop環境を利用するためのポイント
 * 16:30?17:05
 * 株式会社NTTデータ, 山下 真一

### Hadoopと分析統計ソフトKNIMEを用いた効率的データ活用
 * 17:05?17:40
 * 株式会社リクルート, 中野 猛

### 関連エントリ
 * [http://togetter.com/li/104169](<http://togetter.com/li/104169>)
 * [http://d.hatena.ne.jp/nokuno/20110222/1298364343](<http://d.hatena.ne.jp/nokuno/20110222/1298364343>)
 * [http://d.hatena.ne.jp/hamadakoichi/20110222/p1](<http://d.hatena.ne.jp/hamadakoichi/20110222/p1>)
 * [http://d.hatena.ne.jp/seikoudoku2000/20110222/1298379932](<http://d.hatena.ne.jp/seikoudoku2000/20110222/1298379932>)
 * [http://diary.overlasting.net/2011-02-22-1.html](<http://diary.overlasting.net/2011-02-22-1.html>)

