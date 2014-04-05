---
layout: post
status: publish
published: true
title: ScalaからMongoDBへアクセスする - Salat編
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
excerpt: "前回はCasbahというライブラリを使ってMongoDBへアクセスしてみました。\r\n\r\n<a href=\"http://blog.restartr.com/2011/03/07/access-to-mongodb-in-scala-with-casbah/\">ScalaからMongoDBへアクセスする
  ? Casbah編</a>\r\n\r\n今回は、Casbahに加えて、<a href=\"https://github.com/novus/salat\">Salat</a>というライブラリをを組み合わせて、より便利にMongoDBとScalaとやりとりをする方法について見ていきます。\r\n\r\n<h3>Salat</h3>\r\n\r\n<a
  href=\"https://github.com/novus/salat\">novus/salat - GitHub</a>\r\n\r\nSalatは、CasbahのMongoDBObjectとscalaのケースクラスと相互変換してくれる、ORマッパーです。wikiから引用するとこうあります。\r\n<blockquote>\r\nSalat
  is a bi-directional Scala case class serialization library that leverages MongoDB's
  DBObject (which uses BSON underneath) as its target format. This project is focused
  on fostering a DWIM and intuitive usage pattern for the end-user's benefit, without
  sacrificing run time performance.\r\n</blockquote>\r\nパフォーマンスの犠牲なしに、より便利にMongoDBとやりとりできるというものらしいです。DWIMって初耳なんですが、\"Do
  What I Mean.\"の略語だそうです。意図したとおりに動いてくれる、くらいの意味でしょうか。\r\nあと、「Salat」って、ロシア語で「サラダ」の意味だそうです。\r\n\r\nでは、Salatの簡単な使い方を見ていきます。\r\n\r\n"
wordpress_id: 749
wordpress_url: http://blog.restartr.com/?p=749
date: '2011-03-09 09:00:38 +0900'
date_gmt: '2011-03-09 00:00:38 +0900'
categories:
- Scala
tags:
- Scala
- mongodb
- casbah
comments:
- id: 130
  author: yamamoto2012
  author_email: do.luck.club@gmail.com
  author_url: ''
  date: '2012-07-19 17:05:02 +0900'
  date_gmt: '2012-07-19 08:05:02 +0900'
  content: "こんにちは。非常に有用な情報をありがとうございます。\r\nさて、クラス名が違うUserAに変換しようとすると変換できてしまう件ですが、Scalaで言うところのStructured
    typingになっている\r\n気がします。UserBの型が違うというならtype mismatchが出るはずですが、 class UserB requires
    value for 'salary'\r\nとなっており、構造をチェックしているように見えますので。\r\nStructured typingという事なら、単純な型チェックより融通がきいてMongoDBを使うには便利だと思います。"
---
前回はCasbahというライブラリを使ってMongoDBへアクセスしてみました。

<a href="http://blog.restartr.com/2011/03/07/access-to-mongodb-in-scala-with-casbah/">ScalaからMongoDBへアクセスする ? Casbah編</a>

今回は、Casbahに加えて、<a href="https://github.com/novus/salat">Salat</a>というライブラリをを組み合わせて、より便利にMongoDBとScalaとやりとりをする方法について見ていきます。

<h3>Salat</h3>
<a href="https://github.com/novus/salat">novus/salat - GitHub</a>

Salatは、CasbahのMongoDBObjectとscalaのケースクラスと相互変換してくれる、ORマッパーです。wikiから引用するとこうあります。

<blockquote>
Salat is a bi-directional Scala case class serialization library that leverages MongoDB's DBObject (which uses BSON underneath) as its target format. This project is focused on fostering a DWIM and intuitive usage pattern for the end-user's benefit, without sacrificing run time performance.

</blockquote>
パフォーマンスの犠牲なしに、より便利にMongoDBとやりとりできるというものらしいです。DWIMって初耳なんですが、"Do What I Mean."の略語だそうです。意図したとおりに動いてくれる、くらいの意味でしょうか。
あと、「Salat」って、ロシア語で「サラダ」の意味だそうです。

では、Salatの簡単な使い方を見ていきます。

<a id="more"></a><a id="more-749"></a>

<h3> インストール </h3>
今回もsbt前提です。
<script src="https://gist.github.com/860266.js?file=SalatTestProject.scala"></script>
CasbahとSalatどちらも必要です。今回はCasbahは2.0.2を、Salatは0.0.5を利用します。あとはいつものように"sbt reload update"を実行するだけです。

<h3>CasbahでMongoDBへアクセス</h3>
まずはCasbaとSalatをインポート。
```scala
import com.novus.salat._
import com.novus.salat.global._
import com.mongodb.casbah.Imports._
```

CasbahのみでMongoDBに入れる例を復習します。
```scala
val collection = MongoConnection()("salat_test")("sample")
collection += MongoDBObject("id"->1, "name"->"me", "age"->27)
println(collection.findOne( MongoDBObject("id"->1)).get )
// { "_id" : { "$oid" : "4d76475ce10d23dcda26857d"} , "id" : 1 , "name" : "me" , "age" : 27}
```
で、このような値がすでにMongoDBに入っているとして、それぞれのフィールドにアクセスする場合、以下のようになります。
```scala
val me2 = collection.findOne( MongoDBObject("id"->1)).get
println( me2.getClass ) // class com.mongodb.BasicDBObject
println( me2.get("name") ) // me
```
最後の"me2.get("name")"というのが格好悪いですね。もしかしたらDBから取り出したときに"name"というキーが存在しないかもしれません。ということで、そのフォーマットをケースクラスで定義できるSalatの出番です。
Salatでは、grater[<Type>]のインスタンスを用いてMongoDBObjectとケースクラスの変換を行います。シリアライズは
DBに保存する際は「asDBObject」メソッドで取り出してクラスインスタンスとして扱う場合は「asObject」メソッドを使います。

まずはクラスインスタンスをDBに入れる例。

Userというケースクラスを定義して、graterを用いてDBObjectに変換しています。
```scala
case class User(id: Int, name: String, age: Int)

val me = User(id=2, name="me2", age=54)
val g = grater[User]
collection += g.asDBObject(me)
```

つぎに、DBからとりだした値をクラスインスタンスに変換する例です。
```scala
val meInDB = collection.findOne( MongoDBObject("id"->2)).get
println( meInDB.getClass )
// class com.mongodb.BasicDBObject
println( meInDB )
// { "_id" : { "$oid" : "4d764830e10d23dc4758c29a"} , "_typeHint" : "User" , "id" : 2 , "name" : "me2" , "age" : 54}
println( g.asObject(meInDB) )
// User(2,me2,54)
```
このように、ケースクラスへのマッピングが行われることにより、Scalaのコード中で扱うMongoDBのドキュメントの型が明確になり、見通しがよくなります。

「asObject(meInDB)」で変換する前の、DBからとりだしたままの状態(BasicDBObject型)の段階で

<blockquote>
"_typeHint" : "User"

</blockquote>
というキーと値が見えます。この値がこのドキュメントに対応するクラス型を定義することになります。

<h4>問題点：意図しないケースクラスへの変換</h4>
別のケースクラスに変換しようとするとどうなるでしょうか。
```scala
case class UserA(id: Int, name: String, age: Int)
println( grater[UserA].asObject(meInDB) ) // UserA(2,me2,54)
case class UserB(id: Int, name: String, salary: Int)
println( grater[UserB].asObject(meInDB) )
// java.lang.Exception: class UserB requires value for 'salary'
```
クラス名が違うUserAに変換しようとすると変換できてしまいます。キー名が異なるUserBに変換しようとした場合はコンパイルエラーとなります。「_typeHint」の値がうまく機能しているのか、少々疑問が残ります…

<h4>問題点：クラス階層の保持</h4>
また、クラス階層をもつ場合がテストケースにあるのですが、これも微妙な挙動をします。

<a href="https://github.com/novus/salat/blob/master/salat-core/src/test/scala/com/novus/salat/test/model/TestModel.scala">salat-core/src/test/scala/com/novus/salat/test/model/TestModel.scala at master from novus/salat - GitHub</a>

上記はテストケース用のモデル定義なのですが、Desmondのように別クラスの型を保持するケースクラスをDBObjectに変換する際に、Desmond型は保持できるのですが、それに含まれるAlice型が保持できずにリストに変換されてしまいます。

こんな感じのコードで試してみました。
```scala
case class Group(group_id: Int, name: String, leader: User, members: List[User])

val me = User(id=11, name="me", age=27)
val you = User(id=12, name="you", age=30)
val members = MongoDBList.newBuilder
members += me
members += you
val group = Group(group_id=1, name="you and me", leader=me, members=List(me, you))
println(group)
// Group(1,you and me,List(User(11,me,27), User(12,you,30)))

println(grater[Group].asDBObject(group))
// // { "_typeHint" : "Group" , "group_id" : 1 , "name" : "you and me" , "leader" : [ 11 , "me" , 27] , "members" : [ [ 11 , "me" , 27] , [ 12 , "you" , 30]]}
```
DBObjectに変換すると、leaderの値やmembersのList要素がリストに変換されてます。
このままMongoDBに保存して、それを取り出したあとで"grater[Group].asObject(group)"しようとするとエラーになります。

まだ触り始めたばかりでGithubのWikiもちゃんと読み込んでないので、扱い方が違っているのかもしれません。引き続き動作検証をすすめたいと思います。（結局、Rogueを使うというオチになりそうな気がしないくもないですが…）

<h3>関連リンク</h3>
<ul>
<li><a href="https://github.com/novus/salat/wiki/Quick-start">Quick start - GitHub</a></li>
</ul>
今回も一応全コードを掲載しておきます。
<script src="https://gist.github.com/860266.js?file=SalatSample.scala"></script>

