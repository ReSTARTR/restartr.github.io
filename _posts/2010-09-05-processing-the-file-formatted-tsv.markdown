---
layout: post
status: publish
published: true
title: TSVファイルを処理する
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 304
wordpress_url: http://blog.restartr.com/?p=304
date: '2010-09-05 18:22:53 +0900'
date_gmt: '2010-09-05 09:22:53 +0900'
categories:
- Scala
tags:
- Scala
- csv
- fileio
comments: []
---
<p>Scalaプログラミング入門をざっと読んでから少し間が空いてしまいました。<br />
第１回Scala座を見たりして刺激もらったのでちょっとScala弄りなど。<br />
※Scala座については非公式にトゥギャらせてもらっています。：<a href="http://togetter.com/li/47287">第１回Scala座非公式勝手まとめ</a></p>
<p>ちょっと雰囲気を振り返るため、普段PHPでよくやるTSVファイルの読み込みでテスト。</p>
<p>foo.tsvの中身はこんな感じ。</p>
<pre class="brush:c">
u01,Tokyo
u02,Osaka
u03,Nagoya
u04,Tokyo
u05,Fukuoka
u06,Nagoya
u07,Nagoya
u08,Shiga
u09,Hokkaido
u10,Osaka
</pre>
<p>これの転置インデックスを作成したい。</p>
<p>で、<a href="http://rainyday.blog.so-net.ne.jp/2007-12-01">こちらのブログ記事</a>とかを参考にしたりして、何回か試行錯誤してできたのがこちらのソース。</p>

```scala
import scala.collection.mutable.HashMap
import scala.io.Source
import scala.io.BufferedSource
object ReadTsv 
{
  def main( args: Array[String] ) 
  {
    val source = Source.fromFile( "foo.txt" )
    try {
      var m = new HashMap[String,List[String]]
      for( line <- source.getLines ) {
        val x:Array[String] = line.stripLineEnd.split(",")
        m.update(x(1) , m.get(x(1)) match {
          case Some(s) => x(0) :: s
          case None    => List( x(0) )
        })
      }
      println( m )
    } finally {
      source.asInstanceOf[BufferedSource].close
    }
  }
}
```
<p>実行</p>
```bash
$ scalac ReadTsv.scala
$ scala ReadTsv
Map(Shiga -> List(u08), Nagoya -> List(u07, u06, u03), Tokyo -> List(u04, u01), Osaka -> List(u10, u02), Fukuoka -> List(u05), Hokkaido -> List(u09))
```
<p>手続き型な感じが色濃い気がするけど、もっと関数型的になるのでしょうか。。<br />
とりあえずコレクションとパターンマッチの練習にはなりました。</p>
<p>他人のプログラムを拝見するなどして、もっとScalaの理解を深める必要がありそうです。</p>
