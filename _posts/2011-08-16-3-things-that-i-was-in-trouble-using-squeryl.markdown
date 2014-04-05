---
layout: post
status: publish
published: true
title: ScalaのORMapperのSquerylを試してみてハマった３つのこと
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 1024
wordpress_url: http://blog.restartr.com/?p=1024
date: '2011-08-16 08:00:15 +0900'
date_gmt: '2011-08-15 23:00:15 +0900'
categories:
- Scala
tags:
- Scala
- ORM
- squeryl
- mysql
comments: []
---
ScalaのORMとしてSquerylってのがあります。

<ul>
<li><a href="http://squeryl.org/index.html">Squeryl - A Scala ORM for SQL Databases</a></li>
</ul>
使い方は上記リンク先を見ればだいたいわかります。

あと、<a href="http://twitter.com/jugyo">@jugyo</a>さんのブログに導入あたりはまとまっていますし、つまづいたらGoogleGroupで検索すれば何か見つかるかもしれません。

<ul>
<li><a href="http://blog.twiwt.org/e/f34763"> Twiwt:Blog / jugyo : squeryl を試す </a></li>
<li><a href="http://blog.twiwt.org/e/7e40ce">Twiwt:Blog / jugyo : Squeryl の使い方 - セットアップ, モデルの定義, テーブル作成</a></li>
<li><a href="https://groups.google.com/group/squeryl/about"> Squeryl | Google Groups </a></li>
</ul>
さて、今回はTwitterっぽいものを想定して機能を試してみたのですが、３つほどハマったところを記しておきます。

環境はScala2.9.0.1、Sbt0.7.7、MySQL5.5、Squeryl0.9.4です。

ソースはgistに登録。それを本文末尾にも掲載しておきました。

<h3>１．プライマリーキーの指定での嵌りどころ</h3>
1つのカラムがPKとなるテーブルスキーマの場合、org.squeryl.KeyedEntity[T]を継承して使います。

{% highlight scala %}

class Users(id: Long, text: String) extends KeyedEntity[Long]

{% endhighlight %}

KeyedEntityにはidというフィールドが用意されていて、継承時に型を指定することで、PKの型に適用させることができます。今回の場合であればLong型のPK「id」ということになります。

ただし、この場合、PKはautoincrementedになってしまいます。

コード的には下記のようにKeyedEntity[T]を使わずSchema継承時に定義するのと同等になるわけです。

{% highlight scala %}

class Users(id: Long, text: String)

class Db extends Schema {

  val users = table[User]("users")

  on(users)(u => declare(

    u.id is (primaryKey, autoincremented))

}

{% endhighlight %}

これは結構困ります。そんな場合はSchemaを継承するときに定義を上書きすればOK。

{% highlight scala %}

class Users(id: Long, text: String) extends KeyedEntity[Long]

class Db extends Schema {

  val users = table[User]("users")

  on(users)(u => declare(

    u.id is (primaryKey))

}

{% endhighlight %}

これはSquerylのGoogleGroupに書いてました。

<ul>
<li><a href="https://groups.google.com/forum/#!topic/squeryl/BTrKBwikMqs">how to cancel "autoincremented" from KeyedEntity[T]</a></li>
</ul>
<h3>２．DDLのカラムの順番の嵌りどころ</h3>
Db.printDdlを実行した時にカラムの順番が予測できません。

PKが最初にくるのかと思えばそうでもないみたい。ここは細かく追ってないですが、これもGoogleGroupに答えがありました。

<ul>
<li><a href="https://groups.google.com/forum/#!topic/squeryl/ZwiDf5Q-IUI">CREATE TABLE columns in order of constructor arguments</a></li>
</ul>
答えとしては、org.squeryl.internals.DatabaseAdapter:: writeCreateTableあたりをorverrideしてなんとかしてくれとのこと。

<a href="https://github.com/max-l/Squeryl/blob/master/src/main/scala/org/squeryl/internals/DatabaseAdapter.scala#L255">https://github.com/max-l/Squeryl/blob/master/src/main/scala/org/squeryl/internals/DatabaseAdapter.scala#L255</a>

なんか良い方法はないの…

とりあえず、printDdlした結果をコピーして、順番だけ書き換えて、手動で直接DBにクエリ発行すればなんとかなるでしょう。

<h3>３．外部キーの利用でのはまりどころ</h3>
２つのテーブルにRelationを設定してからDb.createした際、外部キーが設定されない問題がありました。

MySQLは5.5を使っているので外部キーに対応していない訳でもないです。

問題はAdapterの選定にありました。

org.squeryl.adapter.MySQLAdapter

をつかっていたのですが、

org.squeryl.adapter.MySQLInnoDBAdapter

を使えばOKでした。

根本の原因は、org.squeryl.adapter.MySQLAdapterに

{% highlight scala %}

override def supportsForeignKeyConstraints = false

{% endhighlight %}

と定義してあり、一方のorg.squeryl.adapter.MySQLInnoDBAdapterには

{% highlight scala %}

override def supportsForeignKeyConstraints = true

{% endhighlight %}

と定義してあります。

この値がtrueになっていないとForeignKeyの制約がDDLに含まれなくなってしまうので注意です。

<a href="https://github.com/max-l/Squeryl/blob/master/src/main/scala/org/squeryl/adapters/MySQLAdapter.scala#L75">Docコメントにも書いてある</a>ので注意です。

以上、Squerylを弄ってみて嵌ったことをまとめてみました。

<h3>Squerylの使用感</h3>
ちょっと前にTwitterのStreamAPIのデータをMySQLに格納するためにSquerylを使ったのと今回少し弄ってみただけなのでまだまだ知らないことだらけです。他にも機能的はたくさん用意されていると思いますし、APIも割となじみやすいので複雑すぎないテーブル定義の場合には積極的にSquerylを使っていこうと思います。

<em>※ドキュメントや本体のソースを追いかけたりしながら使い方を学ぶのは非常に楽しいです:)</em>

で、ソースは以下です。

<script src="https://gist.github.com/1146854.js"> </script>

