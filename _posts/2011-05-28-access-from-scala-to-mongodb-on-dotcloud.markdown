---
layout: post
status: publish
published: true
title: DotCloudのMongoDBをScalaから使ってみる
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 942
wordpress_url: http://blog.restartr.com/?p=942
date: '2011-05-28 20:00:57 +0900'
date_gmt: '2011-05-28 11:00:57 +0900'
categories:
- Scala
tags:
- Scala
- mongodb
- dotcloud
- paas
comments: []
---
DotCloudにDuoStackが<a href="http://gigaom.com/cloud/exclusive-paas-startups-unite-dotcloud-buys-duostack/">買収され</a>て、DotCloudでもMongoDBが使えるようになりましたし、node.jsも使えるしで、ますますDotCloudが魅力的なものになってきました。

ただし、node.jsはサポートされても、WebSocketは正式サポートされていないと<a href="http://docs.dotcloud.com/components/nodejs/">公式マニュアル</a>にも書いてありますがWebSocketサポート済みのDuoStack買収によってどう流れるか気になるところです。

さて今回は、前回作ったものをベースにScalaからMongoDBへアクセスするサンプルをDotCloudで動かすまでを書いておきます。（まぁ、Scalaのコードはオマケみたいなものですけど…）

前回の記事はこちらです。

 * <a href="http://blog.restartr.com/2011/05/09/sample-sbt-project-for-dotcloud/">ScalaをDotCloudにアップロードするためのsbtサンプル</a>

ちなみにDotCloudは下記バージョンにて動作しているみたいです（2011.05.28現在)

   * nginx 0.7.65
   * jetty 6.1
   * mongodb 1.8.1

### 作成したサンプルアプリ

事情により停止する場合があるかもしれませんがご了承を。

 * <a href="http://samplemongo.ramee.dotcloud.com/">http://samplemongo.ramee.dotcloud.com/</a>

### DotCloudにMongoDBサーバーを準備する

DotCloudのMongoDBマニュアルはこちら

  * <a href="http://docs.dotcloud.com/components/mongodb/">MongoDB ? DotCloud documentation</a>

上記マニュアルにしたがって作成すればMongoDBサーバーを準備できます。
基本的には、追加したいサーバーを登録して、ユーザーを作成するだけです。

簡単。

今回は"example.mongo"という名前で作成する例を記してありますので、
それぞれ自分の作成したいアプリ名に読み替えて下さい。

#### サーバーを作成

事前に"dotcloud create example"は実行してあるものとします。
詳しくはこちらの<a href="http://docs.dotcloud.com/tutorials/firststeps/#id2">マニュアル</a>を見てください。

{% highlight bash %}
$ dotcloud deploy -t mongo example.mongo
{% endhighlight %}

{% highlight bash %}
> Created "example.mongo".
{% endhighlight %}

#### DB情報を確認（ID/PASS）

{% highlight bash %}
$ dotcloud info example.mongo
{% endhighlight %}

deployコマンド実行してから実際に作成されるまで少し時間がかかります。
すぐにinfoコマンドを実行すると下記エラーがでます。
※正確に測ってませんが数十秒くらい？

{% highlight bash %}
> Sat May 28 08:14:43 Error: couldn't connect to server 127.0.0.1 shell/mongo.js:79
> exception: connect failed
> Connection to mongo.example.dotcloud.com closed.
> Abort.
{% endhighlight %}

作成完了していれば、下記情報が表示されますので、"mongodb_password: ******"に記載されたパスワードをメモしておきます。

{% highlight bash %}
cluster: wolverine
config:
    mongodb_password: ******
created_at: 1306570413.6722209
name: example.mongo
namespace: example
ports:
-   name: ssh
    url: ssh://mongodb@mongo.example.dotcloud.com:5906
-   name: mongodb
    url: mongodb://root:******@mongo.example.dotcloud.com:5907
state: running
type: mongodb
{% endhighlight %}

#### MongoDBサーバーへログイン

{% highlight bash %}
$ dotcloud run example.mongo mongo
{% endhighlight %}

infoコマンドで表示されたパスワードを使って、"sampledb"というdbにアプリユーザーを追加します。

{% highlight bash %}
> use sampledb
switched to db sampledb
> db.getSisterDB("admin").auth("root", "<infoコマンドで表示されるパスワード>");
1
> db.addUser("APPUSER_NAME", "APPPUSER_PASS");
{
     "user" : "APPUSER_NAME",
     "readOnly" : false,
     "pwd" : "xxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
}
> exit
bye
Connection to mongo.example.dotcloud.com closed.
{% endhighlight %}

※とりあえず一旦ログアウトしてますが、別に必須じゃないです。

#### 再度ログインしてテスト操作してみる
{% highlight bash %}
$ dotcloud run example.mongo mongo
{% endhighlight %}

新規作成したユーザーでDB操作が可能か確認してみます。

{% highlight bash %}
# mongo
Warning: Permanently added '[mongo.example.dotcloud.com]:5906,[174.129.17.131]:5906' (RSA) > to the list of known hosts.
MongoDB shell version: 1.8.1
connecting to: test
> use sampledb;
switched to db sampledb
> db.auth("sampleuser", "samplepass");
1
> db.sampledb.save({id:1,name:"foo"});
> db.sampledb.find();
{ "_id" : ObjectId("4de0b033a1fd29eb0e1522fd"), "id" : 1, "name" : "foo" }
{% endhighlight %}

問題なさそうなので、あとはアプリを"dotcloud push"して動かすだけです。

### scalaからアクセスするサンプル

サービス名を「example.samplemongo"として作成する例です。

requirementsはイカのとおり。

 * scala 2.9.0
 * sbt 0.7.7
 * jetty 7.3.1.v20110307
 * casbah 2.1.5.0

<code>sbt</code>
で基本ディレクトリを作成したら、下記プロジェクト設定を{root}/project/build/MongoSampleProject.scala"として保存します。

前回の設定クラスをコピーしてきたので、"sbt dot_create"と"sbt dot_push"も一応使えます。

※"example.mongo"の名称は適宜読み替えでお願いします。
##### MongodbSampleProject.scala

<script src="https://gist.github.com/996769.js?file=MongodbSampleProject.scala"></script>

配置できたら、

{% highlight bash %}
sbt reload update
{% endhighlight %}

でライブラリを読み込みます。

あとは下記Servletの実装とweb.xmlを用意したらOK．

##### MongodbServlet.scala

<script src="https://gist.github.com/996769.js?file=MonbodbServlet.scala"></script>

##### web.xml

<script src="https://gist.github.com/996769.js?file=web.xml"></script>

できたら、"sbt dot_push"でdotcloudへアップロードされるはずです。

最後に、"http:／／samplemongo.exapmle.dotcloud.com"にアクセスして、フォームからデータ登録ができれば成功です。

RDBMSにくらべてデータ保存・取得までが圧倒的に簡単で、PaaSでもMongoDBはいい感じですね！

