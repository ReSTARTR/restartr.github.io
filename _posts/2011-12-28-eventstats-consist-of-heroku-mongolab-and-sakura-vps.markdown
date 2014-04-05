---
layout: post
status: publish
published: true
title: EventStatsはherokuとMongoLabとさくらVPSで動いている
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 1174
wordpress_url: http://blog.restartr.com/?p=1174
date: '2011-12-28 21:27:52 +0900'
date_gmt: '2011-12-28 12:27:52 +0900'
categories:
- Scala
- python
- Webサービス
tags:
- Scala
- python
- mongodb
- casbah
- eventstats
- unfiltered
- pymongo
- heroku
comments: []
---
今月頭に[ブログ書きました](/2011/12/10/eventststs)が、EventStatsという勉強会の参加者の推移が見れるサービスを公開しました。

 * [EventStats - イベントの統計情報が見れます](http://eventstats.restartr.com)

まぁ自分が欲しかっただけなんですけど、使ってみて頂ければ幸いです。
今回はそのサービスの構成とかについて書いてみます。

### アジェンダ

 1. 全体像
 1. システム構成
 1. Gitリポジトリ
 1. MongoDBのPaaS
 1. 各イベント管理サービスAPIの違い
 1. 開発メモ

### 1.全体像

開発環境も含めて全体像を図にしてみました。(初Cacooですが超べんりですね！)

赤い線がGit操作で、黒い点線がMongoDBへのアクセスです。

<a href="https://cacoo.com/diagrams/Cp2yo6tQNlxCm5av"><img border="1" alt="全体像" src="https://cacoo.com/diagrams/Cp2yo6tQNlxCm5av-2260A.png"></a>

### 2.システム構成

大きく分けてwebとクローラーの２つです。
webはherokuに、クローラーはさくらのVPSに配置。

まずは優先してデータ蓄積を…ということでクローラーをpythonとmongodbで作成しました。
(サービス的にはやいとこデータためないと意味ないので。)

クローラーは５分おきに起動するのでScalaよりPythonを選択しました。起動コスト重視です。
(Scalaでサクサク開発できる程のスキルではないというのもありますが… )

実行場所はherokuのworkerも考えたましたが、最終的に既に利用していたさくらVPSでcronジョブとして運用することに。

ということでScalaのWebはデータ参照のみで、データの更新はしません。

### 3.Gitリポジトリ

webとクローラーは分けてGitで管理。リモートリポジトリはどちらもさくらのVPS上においています。
ただし、本番リリースは開発PCからherokuに別途pushします。

※webもさくらVPSにリモートリポジトリを持って、本番データを参照するステージング環境として利用しています。

#### eventstats-web

 * host: [heroku](http://www.heroku.com/) (Chedar)
 * scala
     * フレームワーク: [unfiltered](https://github.com/unfiltered/unfiltered) 0.5.1
     * mongodb接続: [casbah](http://api.mongodb.org/scala/casbah/2.1.5.0/) 2.1.5-1
     * テンプレートエンジン: [unfiltered-scalate](https://github.com/unfiltered/unfiltered/tree/master/scalate) (ssp)
     * テスティングライブラリ: [unfiltered-specs](https://github.com/unfiltered/unfiltered/tree/master/spec)
 * チャートのレンダリング: [Google Chart Tools](http://code.google.com/apis/chart/index.html)

#### eventstats-crawler

 * host: さくらのvps
 * python 2.6
   * フレームワーク: なし
   * mongodb接続: [pymongo](http://api.mongodb.org/python/1.11/) 1.11
   * テスティングライブラリ: [nose](http://readthedocs.org/docs/nose/en/latest/)
   * その他: [BeautifulSoup](http://www.crummy.com/software/BeautifulSoup/) (*partake.inのwebスクレイピングに利用*)

### 4.MongoDBのPaas

herokuプラグインとして[MongoLab](https://addons.heroku.com/mongolab)と[MongoHQ](https://addons.heroku.com/mongohq)の２つが提供されています。どちらも無料枠があるのですが、MongoLabの方が無料で利用できる容量が大きいのでこちらを選択。

月額の利用料金は以下です。（括弧内は1MBあたりの金額の目安です）
_これ以上の容量も利用可能ですが個人で払う範囲ではないと思い除外してます。_

#### MongoLab
 * $ 0.00/240MB
 * $10.00/0.5GB  ($0.020/MB)
 * $20.00/2.0GB  ($0.009/MB)

#### MongoHQ

 * $ 0.00/ 16MB
 * $ 5.00/256MB  ($0.019/MB)
 * $15.00/2.0GB  ($0.007/MB)

### 5.各イベント管理サービスAPIの違い

まずは**atnd, zusaar, partake.inの３サービスに対応**。

それぞれ検索APIを提供してくれているのですが、当然ながら規格とかもないのでリクエストもレスポンスも違いがあります。

データ蓄積する際にそのAPIの差異を吸収して、webアプリから参照する際は気にしなくていい戦略をとりました。
APIの違い検索のみに特化して違いをまとめると以下の通りです。

#### atnd

イベント数も多いので、このAPIをスタンダードに設定。
* API仕様
 * [http://api.atnd.org/](http://api.atnd.org/)
* リクエストパス
 * [/events/](http://api.atnd.org/events/)
   * イベントの検索
 * [/events/users/](http://api.atnd.org/events/users/)
   * イベントに参加しているユーザーの検索

#### zusaar

基本的にはatnd準拠っぽい感じだけど細かい違いがあります。
 * API仕様
   * [http://www.zusaar.com/doc/api.html](http://www.zusaar.com/doc/api.html)
 * リクエストパス
   * [/api/event/](http://www.zusaar.com/api/event/)
     * イベントの検索
   * [/api/event/user/](http://www.zusaar.com/api/event/user/)
     * イベントに参加しているユーザーの検索
atndとの違い
 * エントリポイントやデータのキー名が単数形
   * events→event
   * users→user
 * 明確なフィールドとしてのtwitter_idが無い
    * 管理者も参加ユーザーも
 * ハッシュタグがない
 * レスポンスはjson一択

#### partake.in

全然違うAPI。APIリストにあっても未実装がほとんどなので、利用する際はソースを確認したほうが良いです。
今回必要になりそうなAPIは２つくらいでした。

 * API仕様
   * [http://code.google.com/p/partakein/wiki/PublicWebAPI](http://code.google.com/p/partakein/wiki/PublicWebAPI)
 * リクエストパス
   * [/api/event/search](http://partake.in/api/event/search/)
     * イベントの検索
   * [/api/event/get](http://partake.in/api/event/get/)
     * イベントの詳細データ取得
 * APIのソース(抜粋)
   * [in.partake.controller.api.event.SearchAction.java](http://code.google.com/p/partakein/source/browse/trunk/Partake/src/main/java/in/partake/controller/api/event/SearchAction.java)
   * [in.partake.controller.api.event.EventAction.java](http://code.google.com/p/partakein/source/browse/trunk/Partake/src/main/java/in/partake/controller/api/event/EventAction.java)

その他の特徴は以下。

 * 複数イベントを特定して一括取得するAPIはない
 * フィールド名がcamelCase形式
 * レスポンスはjson一択
 * 検索パラメータも特殊かつ少数
 * 検索APIで取得できるのはイベントの固定情報のみ
   * 参加枠数はAPIから取得可能
   * 変動するユーザー数は取得不可能
   * →Webページをスクレイピングするしかないという結論

上記をふまえ、atnd/zusaarはJSON形式でAPIからデータ取得。

partake.inのみイベントのリストをAPIから取得して、ユーザー数はWebページのスクレイピングで対応しました。

### 6.開発メモ

#### web(heroku)からもクローラー(さくらのvps)からも離れた場所にある

開発PC上だと気にならなかったのですが、1件1件findしてinsertやupdateをしていると当然遅いです。なのである程度まとめて一気にinsertする方針に変更しました(ベンチ結果はありません ^^;)。
更新はクローラーの１プロセスからのみ実行されるので、トランザクションとか意識しなくて良いです。なので比較的自由な構成がとれます。

#### ScalaでJSON API

まずはUnfilteredでJSON APIを作成。けど、jsでjson取得〜チャート生成の実行時間が思いの外大きいので、jsonも１枚のHTMLに埋め込む方針に変更。

### さいごに

ざっと書きだすとこんな感じです。まぁこんな構成もあるよ、ってくらいにしか言えませんが。

webとクローラーを分けたことで、開発中のスキーマ変更が柔軟に行えたのは良かったのですが、スキーマ定義を共通で管理していないので、そのあたりうまく管理できると良いなと思ったり。
当初はもう少しwebの機能も多かったのですが、効率化をしているうちにシンプルな形に落ち着きました。Scalaのコードもかなり小規模なものになっています。
イベント管理者の方からのご意見ご要望などいただけると嬉しいです :)

 * [EventStats - イベントの統計情報が見れます](http://eventstats.restartr.com)
