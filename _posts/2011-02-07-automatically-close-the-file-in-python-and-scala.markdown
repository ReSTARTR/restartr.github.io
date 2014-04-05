---
layout: post
status: publish
published: true
title: pythonとscalaのファイルの自動クローズ
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
excerpt: "エキスパートPythonプログラミングを少しずつ読み進めています。といっても気になるタイトルを拾い読みですが。\r\n\r\nそのなかの\"2.4
  withとcontextlib\"の章のwith文の例としてファイル読み込みのコードが書いてました。\r\n<script src=\"https://gist.github.com/813385.js?file=read_file_using_with.py\"></script>\r\nたったこれだけで、自動クローズまでやってくれるみたい。\r\n\r\n"
wordpress_id: 576
wordpress_url: http://blog.restartr.com/?p=576
date: '2011-02-07 00:10:22 +0900'
date_gmt: '2011-02-06 15:10:22 +0900'
categories:
- Scala
- python
tags:
- Scala
- python
comments:
- id: 30
  author: Tweets that mention pythonとscalaのファイルの自動クローズ | ReSTARTR -- Topsy.com
  author_email: ''
  author_url: http://topsy.com/blog.restartr.com/2011/02/07/automatically-close-the-file-in-python-and-scala/?utm_source=pingback&amp;utm_campaign=L2
  date: '2011-02-07 02:10:57 +0900'
  date_gmt: '2011-02-06 17:10:57 +0900'
  content: '[...]  This post was mentioned on Twitter by katsyoshi, Masaki YOSHIDA.
    Masaki YOSHIDA said: ブログ書きました。#python #scala ＞ pythonとscalaのファイルの自動クローズ | ReSTARTR
    http://bit.ly/hJFCis     [...]'
---
エキスパートPythonプログラミングを少しずつ読み進めています。といっても気になるタイトルを拾い読みですが。

そのなかの"2.4 withとcontextlib"の章のwith文の例としてファイル読み込みのコードが書いてました。
<script src="https://gist.github.com/813385.js?file=read_file_using_with.py"></script>
たったこれだけで、自動クローズまでやってくれるみたい。

<a id="more"></a><a id="more-576"></a>
さらに、with文を使うためのクラス定義も可能らしい。
<script src="https://gist.github.com/813385.js?file=Reader.py"></script>

実装すべきは下記２メソッドのみ。

<ul>
<li>__enter__(self)</li>
<li>__exit__(self, exception_type, exception_value, exception_traceback)</li>
</ul>
__enter__の戻り値はasで受けとれます。
__exit__で例外処理のfinallyに当たる処理を書きます。何も返さないと例外はその呼び出し元に伝播します。

さらにさらに、contextlibというモジュールを使えば、もっと自然に書けます。
<script src="https://gist.github.com/813385.js?file=read_file_with_closing.py"></script>
※これは、contextmanager内のfinally句でcloseを書くのと同じ。(<a href="http://www.python.jp/doc/nightly/library/contextlib.html#contextlib.closing">参考</a>)

<h4>Scalaでwith（っぽいもの）を実装</h4>
pythonのwith文と同様の記述方法をScalaでやるとしたら…こんな感じでしょうか。
<script src="https://gist.github.com/813385.js?file=FileReader.scala"></script>

ファイルの自動クローズについてはこちらを参考にさせていただきました。

* <a href="http://d.hatena.ne.jp/syttru/20080322/1206212125">Scalaでファイル操作 - syttruの日記</a>

