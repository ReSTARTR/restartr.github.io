---
layout: post
status: publish
published: true
title: Scalaで設定ファイルを使いたい時どうしたらいいの？
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 1049
wordpress_url: http://blog.restartr.com/?p=1049
date: '2011-08-30 00:02:15 +0900'
date_gmt: '2011-08-29 15:02:15 +0900'
categories:
- Scala
tags:
- Scala
- config
- library
comments:
- id: 108
  author: kmizushima
  author_email: mizukota@gmail.com
  author_url: http://d.hatena.ne.jp/kmizushima
  date: '2011-08-30 06:34:24 +0900'
  date_gmt: '2011-08-29 21:34:24 +0900'
  content: "こんにちは。面白い記事を読ませていただき、ありがとうございました。特に、twitterのutil-eval辺り、あまり\r\n追えて無かったのですが、参考になります。\r\n\r\nちょっと記事を読んで、いくつか気になる点があったのでツッコミをば。\r\n\r\n&gt;
    一時的にjarファイルを生成するので環境に制約あるとダメ（たぶん）\r\n\r\nこれ、実際に試してないので、的外れだったら申し訳ないのですが、ソースちょこっと読んだ限り、仮想ディレクトリ構造の中に仮想ファイルを生成して、その中に仮想的に.classファイル(相当)を生成することができるっぽいので(60行目辺り参照)、実は環境の制約はあまり無かったりする気がします。\r\n\r\n&gt;
    akkaのconfig\r\n&gt; 正規表現で定義されてる \r\n\r\nこれは、正規表現で定義されてる、というよりScalaのパーザコンビネータ(RegexParsersを継承)で定義されている、というのが正確なところかと。"
- id: 109
  author: ReSTARTR
  author_email: yoshida.masaki+blog@gmail.com
  author_url: ''
  date: '2011-08-30 08:08:52 +0900'
  date_gmt: '2011-08-29 23:08:52 +0900'
  content: ">kmizushima さん\r\nコメント＆ツッコミありがとうございます。\r\n\r\n> 一時的にjarファイルを生成するので環境に制約あるとダメ（たぶん）\r\n>
    \r\n> これ、実際に試してないので、的外れだったら申し訳ないのですが、ソースちょこっと読んだ限り、仮想ディレクトリ構造の中に仮想ファイルを生成して、その中に仮想的に.classファイル(相当)を生成することができるっぽいので(60行目辺り参照)、実は環境の制約はあまり無かったりする気がします。\r\n\r\nたしかにソースではVirtualDirectoryを使ってメモリ上で操作していますね。\r\nただ実際使ってみたときには、ファイルシステム上にclassファイルが生成されているのが確認できたので、その結果から少し想像で書いていました。\r\n少し曖昧なのでちゃんと調査してみます。\r\n\r\n\r\n>>
    akkaのconfig\r\n>> 正規表現で定義されてる\r\n> \r\n> これは、正規表現で定義されてる、というよりScalaのパーザコンビネータ(RegexParsersを継承)で定義されている、というのが正確なところかと。\r\n\r\nなるほど。たしかに言葉の表現に不足がありますね。修正しておきます。"
---
<em style="color:red">2011.08.31 kmizushimaさんから頂いた<a href="http://blog.restartr.com/2011/08/30/config-libraries-in-scala/?preview=true&preview_id=1049&preview_nonce=ad0bbeeef6#comment-108">コメント</a>を元に、下記の記述を修正＆追記しました。

* Twitterのutil-evalの一時ファイル生成について
* AkkaのConfigファイルのパース手法について
</em>

TwitterのOAuthの鍵やDB接続情報など、アプリを書く上で環境によって切り替える設定が大抵の場合あると思います。普段使っているPHPの場合、設定を外部ファイルに書きだす場合、ini,yaml,xml,phpのいずれかを使うことが多いのですが、Scalaの場合、設定ファイルってどうするのか気になりました。

ということで、適当に思いついたライブラリやフレームワークがどのように対応しているのか調査。

## ライブラリ

### propertiesファイル

 * javaの古くから使われている
 * キーと値のみ設定可能
 * 依存関係がないので手軽。
 * すべてが文字列
 * 例えばこんな感じ

path/to/conf.properties

```scala
hoge = "moge"
```

```scala
val p = new java.util.Properties()
val config = p.load(new java.io.FileInputStream("path/to/conf.properties");
config.get("hoge") // "moge"
```

### twitterのconfiggy

 * <a href="https://github.com/robey/configgy">https://github.com/robey/configgy</a>
 * 独自フォーマット
 * オワコン

### twitterのutil-eval

  * <a href="http://twitter.github.com/util/">http://twitter.github.com/util/</a>
  * Evalした値をそのまま利用
  * Scalaのコンパイラに任せられる。つまりScalaコードがそのまま設定ファイルに。
  * 型安全
  * 詳しいことはこちらを参照
    * <a href="http://d.hatena.ne.jp/xuwei/20110805/1312551980">twitter が Scala 大好きすぎて (?) 設定ファイルまで Scala のソースコードな件 - scalaとか・・・</a>
    * <a href="http://blog.youhei.jp/scala-util-eval">Scala アプリケーションのコンフィグレーションに Twitter 製の util-eval を使ってみた - blog.youhei.jp</a>
  * 下記処理にて設定クラスインスタンスをapply経由で取り出せる
    * <a href="https://github.com/twitter/util/blob/master/util-eval/src/main/scala/com/twitter/util/Eval.scala#L247">com.twitter.util.Eval#L247</a>
  * <del>一時的にjarファイルを生成するので環境に制約あるとダメ（たぶん）</del>
    * 一時ファイルを生成するのは、ローカルにcloneした古いままのバージョン（1.2.5）で動作確認していたためでした。
    * 古いコード: <a href="https://github.com/twitter/util/blob/7c81842286f30aee4b2176bceb8c79ded710c88e/src/main/scala/com/twitter/util/Evaluator.scala">com.twitter.util.Evaluator</a>のコメントに<a href="https://github.com/twitter/util/blob/7c81842286f30aee4b2176bceb8c79ded710c88e/src/main/scala/com/twitter/util/Evaluator.scala#L57">All generated .scala and .class files are stored, by default, in System.getProperty("java.io.tmpdir")</a>と書いてあったので、「一時ファイルが生成される」と認識し、実際の動作確認でもその一時ファイルが確認できていました。
    * しかし、新しいコード: <a href="https://github.com/twitter/util/blob/master/util-eval/src/main/scala/com/twitter/util/Eval.scala">com.twitter.util.Eval</a>のコメントには<a href="https://github.com/twitter/util/blob/master/util-eval/src/main/scala/com/twitter/util/Eval.scala#L50">If target is None, the results are compiled to memory (and are therefore ephemeral)</a>とある通り、パス指定がない場合はメモリ上の仮想ディレクトリに対して操作を行う模様です。

  * 使い方

設定のtraitを定義

src/main/scala/com/restartr/utilSample/MyConfig.scala

```scala
package com.restartr.utilSample

trait MyConfig {
  val num: Int
  val str: String
}

```

実際の設定ファイルでは、設定のTraitを継承してインスタンス生成

※クラスインスタンスでなくても文字列やリストでもOK。

path/to/config/MyConfig.scala

```scala
import com.restartr.utilSample.MyConfig
new MyConfig {
  val num = 1
  val str = "san"
}
```

使いたい場所でEval。

```scala
val conf = Eval[MyConfig](new java.io.File("path/to/config/MyConfig.scala"))
conf.num // 1
conf.str // "san"
```

### configrity

  * <a href="https://github.com/paradigmatic/Configrity">https://github.com/paradigmatic/Configrity</a>
  * akkaのフォーマットと同等
    * <a href="https://github.com/paradigmatic/Configrity/wiki/Formats">設定のフォーマット</a>
    * Scala 2.9以上対象。
    * configファイルの<strong>読み書き</strong>ができる
      * immutable, thread safe, allow functional design pattern

## 各種フレームワーク

以下のフレームワークはすべて独自実装でした。Propertiesじゃ役不足だし、かといってデファクトな設定用ライブラリがないからなのでしょうか。

### akkaのconfig

 * akka.confとかがそれ。
 * 独自パーサーを使用
    * 70行程度のシンプルなパーサー
    * <a href="https://github.com/jboner/akka/blob/master/akka-actor/src/main/scala/akka/config/ConfigParser.scala">akka.config.ConfigParser</a>
    * <del>正規表現で定義されてる</del>Scalaのパーザコンビネータ(RegexParsersを継承)で定義されている
     * "{"と"}"で階層構造を表現

```scala
akka {
  cluster{
    name = "test-cluster"
  }
}
```

 * 使える型
  * 数値
  * 文字列
  * 真偽値（on/off , true/false)
  * リスト [1,2,3] / ["hoge","moge"]

### play!frameworkのconfig

 * 独自パーサー
   * <a href="https://github.com/playframework/play/blob/master/framework/src/play/utils/OrderSafeProperties.java">play.utils.OrderSafeProperties</a>
 * java.util.propertiesを継承したもの。
 * 環境ごとにIDを割り当てられる
    * http://playdocja.appspot.com/documentation/1.2.1/production
    * http://playdocja.appspot.com/documentation/1.2.1/guide11
    * IDごとに%{ID}を頭につければ切り替えてくれるみたい

### Lift

 * LiftRulesが設定をもつ
    * <a href="http://simply.liftweb.net/index-3.1.html#toc-Subsection-3.1.2">http://simply.liftweb.net/index-3.1.html#toc-Subsection-3.1.2</a>
    * 実装はここ
      *<a href="https://github.com/lift/framework/blob/master/web/webkit/src/main/scala/net/liftweb/http/LiftRules.scala">net.liftweb.http.LiftRules</a>
    * たぶんこのへん
      * <a href="https://github.com/lift/framework/blob/master/core/util/src/main/scala/net/liftweb/util/Props.scala">net.liftweb.util.Props</a>

ざっと調べて使ってみたところ、手軽にやるならProperties、フレームワークを使うならそれに則り、厳密にやるならTwitterのEvalや、設定ファイルを読み書きできる独特なConfigrityなんかがよさそうです。

XMLは…まぁないでしょうね。

