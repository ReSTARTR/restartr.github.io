---
layout: post
status: publish
published: true
title: ScalaをDotCloudにアップロードするためのsbtサンプル
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 914
wordpress_url: http://blog.restartr.com/?p=914
date: '2011-05-09 23:35:43 +0900'
date_gmt: '2011-05-09 14:35:43 +0900'
categories:
- Scala
tags:
- Scala
- sbt
- dotcloud
- paas
comments: []
---
<a href="http://dotcloud.com/">dotcloud</a>を必要最低限操作するためのactionをsbtに追加してみました。

sbtのアクション自体はじめてなので作法がよくわかってないので、間違っているかも知れませんが。

### 使い方

※scalaファイルは最後に掲載しています。
とりあえず下記を作成するサービス名に置き換えればOKです。

{% highlight bash %}
  val dotApplicationName = "APPNAME"
  val dotServiceName = dotApplicationName + ".SERVNAME"
{% endhighlight %}

あとは、下記を順に実行すればOK(dot_prepareはdot_pushの前に必ず実行されるので省略可能)

 * "sbt dot_create" : サービスの作成
 * "sbt dot_prepare" : リリース用ディレクトリ作成とwarファイルのコピー
 * "sbt dot_push" : dotcloudへのwarファイルpush

### dotcloud用に注意すべきこと

"sbt package"を使用すると、"＜projectName＞-＜scala_ver＞-＜app_ver＞.war"の形式でwarファイルが作成されます。
が、dotcloudのドキュメントにはこう書いてあるので、"root.war"で作成するのが望ましいです。

<blockquote>
The java service will make your application available at http://frontend.myapp.dotcloud.com/ if your archive is named root.war or http://frontend.myapp.dotcloud.com/webapp/ if your archive is named webapp.war (../foobar/ if the archive was named foobar.war and so on). So, you can effectively serve multiple web applications with the same java service.

</blockquote>
<a href="http://docs.dotcloud.com/components/java/">Java &mdash; DotCloud documentation</a>

さらに、"dotcloud push"はwarファイルを含むディレクトリを指定することになり、それ以下がすべて同期されてしまいます。デフォルトだと、"./target/scala_2.8.1/"以下のすべてが。

なので、warのみ格納する"release"ディレクトリを作り、 そこに"root.war"としてひとつだけ存在させておきました。
順番に書くと、

 * sbt packageでtarget/scala_2.8.1/＜projectName＞-＜scala_ver＞-＜app_ver＞.warを生成
 * そのwarファイルをtarget/release/root.warにコピー
 * "dotcloud push ＜dotcloud_appname＞ target/release/" でwarのみアップロード

という方法で対応しました。

以下、サンプルのプロジェクト設定です。

<script src="https://gist.github.com/962534.js?file=RameeProject.scala"></script>

### 参考リンク
 * <a href="http://docs.dotcloud.com/cli/">DotCloud command line &mdash; DotCloud documentation</a>
 * <a href="http://code.google.com/p/simple-build-tool/wiki/Process">Process - simple-build-tool - A build tool for Scala - Google Project Hosting</a>

