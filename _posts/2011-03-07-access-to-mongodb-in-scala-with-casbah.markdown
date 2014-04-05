---
layout: post
status: publish
published: true
title: ScalaからMongoDBへアクセスする - Casbah編
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
excerpt: "ここ２，３日、ScalaからMongoDBへのアクセスのため、CasbahとSalatをお試し中です。\r\n\r\nもともとはAkkaを弄ってたんですが、Akkaに含まれるPersistent(MongoDBなどNoSQLへの接続用)がなくなるらしいので、別のがないかなー、と寄り道したのがきっかけです。\r\n\r\n<h3>Casbah</h3>\r\nMongoDBを開発している10gen謹製Scalaライブラリです。\r\nScala製のドライバは他にもLiftの<a
  href=\"http://www.assembla.com/wiki/show/liftweb/MongoDB\">MongoDB</a>や、Liftと組み合わせて使う<a
  href=\"http://engineering.foursquare.com/2011/01/21/rogue-a-type-safe-scala-dsl-for-querying-mongodb/\">Rogue</a>、mongo-java-driverのラッパーの<a
  href=\"http://github.com/alaz/mongo-scala-driver\">mongo-scala-driver</a>などがあります。\r\n一覧は以下に掲載されています。\r\n\r\n<a
  href=\"http://github.com/alaz/mongo-scala-driver\">Java Language Center - MongoDB</a>\r\n\r\nRogueについては、Fungoingのbibrostさんのブログが詳しいのでそちらをご覧ください。\r\n\r\n<a
  href=\"http://fungoing.blogspot.com/2011/02/liftscala-dslroguemongodb.html\">Fungoing
  Labs: Lift用Scala DSL\"Rogue\"を使ってMongoDBにアクセス（１）〜概要編〜</a>\r\n\r\nなぜ、LiftのMongoDBなりRogueなりを使わなかったかというと、フレームワークに依存したくなかったからです。単体で動作するものが使いたいのです。なので、Casbahを選択しました。\r\nが、Casbahだけだと、複雑なレコードを作成するのには少々面倒なところがあります。ORマッパーが欲しくなるのです。その解決策となるSalatがあります。今回はCasbahのまとめにとどめ、Salatについては後日改めてまとめようと思います。\r\n\r\n<blockquote>\r\n※追記※<br
  />\r\nSalat編を書きました。\r\n<a href=\"http://blog.restartr.com/2011/03/09/access-to-mongodb-in-scala-with-salat/\">ScalaからMongoDBへアクセスする
  ? Salat編</a></blockquote>\r\n\r\n\r\n\r\nでは、Casbahの使い方について見ていきます。\r\n\r\n"
wordpress_id: 722
wordpress_url: http://blog.restartr.com/?p=722
date: '2011-03-07 08:45:28 +0900'
date_gmt: '2011-03-06 23:45:28 +0900'
categories:
- Scala
tags:
- Scala
- mongodb
- casbah
comments: []
---
ここ２，３日、ScalaからMongoDBへのアクセスのため、CasbahとSalatをお試し中です。

もともとはAkkaを弄ってたんですが、Akkaに含まれるPersistent(MongoDBなどNoSQLへの接続用)がなくなるらしいので、別のがないかなー、と寄り道したのがきっかけです。

<h3>Casbah</h3>
MongoDBを開発している10gen謹製Scalaライブラリです。
Scala製のドライバは他にもLiftの<a href="http://www.assembla.com/wiki/show/liftweb/MongoDB">MongoDB</a>や、Liftと組み合わせて使う<a href="http://engineering.foursquare.com/2011/01/21/rogue-a-type-safe-scala-dsl-for-querying-mongodb/">Rogue</a>、mongo-java-driverのラッパーの<a href="http://github.com/alaz/mongo-scala-driver">mongo-scala-driver</a>などがあります。
一覧は以下に掲載されています。

<a href="http://github.com/alaz/mongo-scala-driver">Java Language Center - MongoDB</a>

Rogueについては、Fungoingのbibrostさんのブログが詳しいのでそちらをご覧ください。

<a href="http://fungoing.blogspot.com/2011/02/liftscala-dslroguemongodb.html">Fungoing Labs: Lift用Scala DSL"Rogue"を使ってMongoDBにアクセス（１）〜概要編〜</a>

なぜ、LiftのMongoDBなりRogueなりを使わなかったかというと、フレームワークに依存したくなかったからです。単体で動作するものが使いたいのです。なので、Casbahを選択しました。
が、Casbahだけだと、複雑なレコードを作成するのには少々面倒なところがあります。ORマッパーが欲しくなるのです。その解決策となるSalatがあります。今回はCasbahのまとめにとどめ、Salatについては後日改めてまとめようと思います。

<blockquote>
※追記※
Salat編を書きました。
<a href="http://blog.restartr.com/2011/03/09/access-to-mongodb-in-scala-with-salat/">ScalaからMongoDBへアクセスする ? Salat編</a>
</blockquote>
では、Casbahの使い方について見ていきます。

<a id="more"></a><a id="more-722"></a>

<h3>インストール</h3>
sbtの設定はこんな感じです。
project/build/SalatTestProject.scala
<script src="https://gist.github.com/857260.js?file=SalatTestProject.scala"></script>

そしたら、sbtプロンプトでreload, updateをします。

これで、project/lib_managed/scala_2.x.x/compile以下にjarファイルが読み込まれるはず。

<h3>CasbahでMongoDBへアクセス</h3>
目次は以下の通りです。

<ul>
<li>MongoDBObjectの作成</li>
<li>ドキュメントの検索</li>
<li>ドキュメントのJOIN</li>
<li>ListObjectの生成</li>
<li>クエリの構築</li>
<li>クエリの構築(DSLを利用)</li>
</ul>
<h4>MongoDBObjectの作成</h4>
Casbahでは、MongoDB上のドキュメントをMongoDBObjectとして取り扱います。

MongoDBObjectをインスタンス化する、もしくはMongoDBObjectBuilderで逐次フィールドを定義するかのどちらかになります。
{% highlight scala %}
// MongoDBObjectをインスタンス化
val user = MongoDBObject("id"->1, "name"->"me")
println(user) // { "id" : 1 , "name" : "me"}

// MongoDBObjectBuilderで逐次フィールド定義
val builder = MongoDBObject.newBuilder
builder += "id"->2
builder += "name"->"you"
val user2 = builder.result // これでMongoDBObjectができる
{% endhighlight %}

できたら、これをMongoDBに保存します。

まずはコレクションのインスタンス取得から。
{% highlight scala %}
val conn = MongoConnection()
// MongoConnection("localhost", 27017)でホストおよびポート指定が可能
val db = conn("casbah_test")
val collection = db("sample")
{% endhighlight %}
下記のように１行にまとめてもでもOKです。
{% highlight scala %}
val collection = MongoConnection()("casbah_test")("sample")
{% endhighlight %}

で、コレクションに保存します。「+=」を使うだけと、とてもシンプルな操作になっています。
{% highlight scala %}
collection += user // { "_id" : ObjectId("4d7385ebf21423dcecb4c578"), "id" : 1, "name" : "me" }
collection += user2 // { "_id" : ObjectId("4d7385ebf21423dcedb4c578"), "id" : 2, "name" : "you" }
{% endhighlight %}

<h4>ドキュメントの検索</h4>
保存したドキュメントを全件取得します。mongodbのコンソールでいう、db.collection.find()にあたる操作です。
{% highlight scala %}
collection.find().foreach { println(_) }
{% endhighlight %}

<h4>ドキュメントのJOIN</h4>
MongoDBでJoinはできないので、アプリ側で対処する場合に使うんでしょうかね。

やり方は、MongoDBObjectを「++」でつなぐだけ。簡単です。
{% highlight scala %}
val identity = MongoDBObject("name"->"me", "age"->27)
val address = MongoDBObject("country"->"Japan", "prefecture"->"Tokyo")
val user = identity ++ address
println(user) // { "age" : 27 , "country" : "Japan" , "prefecture" : "Tokyo" , "name" : "me"}
{% endhighlight %}

<h4>ListObjectの生成</h4>
List(javascriptでいうArrayのこと)はMongoDBListでつくります。
{% highlight scala %}
val users1 = MongoDBList(
  MongoDBObject("name"->"me"),
  MongoDBObject("name"->"you"))
println(users1) // [ { "name" : "me"} , { "name" : "you"}]
{% endhighlight %}
当然ですが、Listの要素（MongoDBListの引数は）MongoDBObjectだけでなく、文字列や数値もOKです。

でListの場合もBuilder経由で作成が可能です。
{% highlight scala %}
val users = MongoDBList.newBuilder
val user1 = MongoDBObject("name"->"me")
val user2 = MongoDBObject("name"->"you")
users += user1
users += user2
println(users.result) // [ { "name" : "me"} , { "name" : "you"}]
{% endhighlight %}

<h4>クエリの構築</h4>
{% highlight scala %}
// 取得条件を指定
val condition = MongoDBObject("name"->"me")
collection.find(condition).foreach( println )

// 取得フィールドを限定
val fields = MongoDBObject("id"->1)
collection.find(condition, fields).foreach( println )

// 条件の指定をせずに取得フィールドのみ限定する場合
val conditionEmpty = MongoDBObject.empty
collection.find(conditionEmpty, fields).foreach( println )
{% endhighlight %}

<h4>クエリの構築(DSLを利用)</h4>
$exists, $gt, $ltなど、以下にあるものはDSLとして使えるらしいです。

<ul>
<li><a href="http://www.mongodb.org/display/DOCS/Advanced+Queries#AdvancedQueries-ConditionalOperators">Advanced Queries - MongoDB</a></li>
</ul>
findの引数にDSLを使って記述します。複数条件を組み合わせる場合は「++」でつなげます。
{% highlight scala %}
collection.find( "name" $exists true ).foreach{ println }

// 組み合わせる場合は"++"でつなげる
collection.find( ("name" $exists true) ++ ("age" $gte 20 $lt 30 ) ).foreach{ println }
{% endhighlight %}
『("age" $gte 20) ++ ("age" $lt 30)』と書かなくても、『("age" $gte 20 $lt 30 ) 』とかけるので記述量がへって良いですね。

あと、追加ですけど、１ドキュメントのみ取得したい時はfindOneが使えます。
{% highlight scala %}
val obj = collection.findOne().get // Option[DBObject]からDBObjectを取り出すためにgetを呼ぶ
println(obj("name")) //me
{% endhighlight %}

以上、かんたんにSasbahの使い方についてまとめてみました。

<h3>関連リンク</h3>
<ul>
<li><a href="https://github.com/mongodb/casbah">GitHub</a></li>
<li><a href="http://api.mongodb.org/scala/casbah/2.0.2/tutorial.html">Casbahチュートリアル</a></li>
<li><a href="http://api.mongodb.org/scala/casbah/2.0.2/scaladoc/">ScalaDoc (casbah-core v2.0.2)</a></li>
</ul>
ちなみに、細切れで説明した上記サンプルプログラムの全体を以下に掲載しておきます。

※convertObjectTypeメソッドの中身については現在検証中です(；・∀・)
<script src="https://gist.github.com/857260.js?file=CasbahSample.scala"></script>

