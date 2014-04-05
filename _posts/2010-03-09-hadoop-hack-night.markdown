---
layout: post
status: publish
published: true
title: Hadoop Hack Nightに行ってきた
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 217
wordpress_url: http://blog.restartr.com/?p=217
date: '2010-03-09 23:39:08 +0900'
date_gmt: '2010-03-09 14:39:08 +0900'
categories:
- hadoop
tags:
- hadoop
- pig
- hdfs
comments: []
---
<p>3月8日に開催された<a href="http://gihyo.jp/event/2010/hadoophn">Hadoop Hack Night</a>に行ってきました。<br />
ハッシュタグは<a href="http://twitter.com/#search?q=%23hadoophn">#hadoophn</a>。</p>
<p>最近Hadoop界隈を色々調査していたので、これは！と思い応募開始のアナウンスとともに申し込み。<br />
応募者多数のため、申し込み期限が切り上げられたほどの人気ぶりだったようで。<br />
抽選に当たったのは奇跡。なのでかなり気合入れていってきました。</p>
<p>一番印象にのこったのは、</p>
<ul>
<li><a href="http://hadoop.apache.org/hdfs/">HDFS</a>は信頼性が低い</li>
<li><a href="http://hadoop.apache.org/pig/">Pig</a>でカバーできない処理はほぼない</li>
</ul>
<p>の2点でした。</p>
<p>以下、それについての感想です。</p>
<h3>HDFSは信頼性が低い</h3>
<p><a href="http://hadoop.apache.org/hdfs/">HDFS</a>はあくまで処理データの一時保存場所とすること。別にマスタデータは保持しておくべきだと。<br />
まだ不安定性に遭遇したことがないのですが、この点はかなり重要になりそう。<br />
確かに分散するのでコピーした分だけデータ量は増えるので、ずっとHDFS上のみで保管するのは現実的でないのかも。<br />
<em>※ファイル圧縮の機能はHDFSは備えているけど、どちらかと言えば転送量を減らすのが目的のようですね。</em></p>
<h3>PIGでカバーできない処理はほとんどない</h3>
<p><a href="http://hadoop.apache.org/pig/">PIG</a>はMapReduceを使い易くした、一種のスクリプト言語のようなもの（と認識しています。）<br />
逐次処理をJavaとかよりもと単純に書けます。<br />
で、PIGはかなりドメインを限定して開発されたものと勝手にイメージしていたので、だいたいの処理がまかなえると聞いてちょっと驚き。<br />
Hadoop本にはあまり細かいことが書かれていなかったし、「ほぼSQL」な<a href="http://hadoop.apache.org/hive/">Hive</a>中心に調査していたのですが、PIGもドキュメントとかもう少し見てみようと思います。</p>
<p>主催者の皆様、講演者の皆様、参加者の皆様お疲れ様でした。<br />
非常に有意義な時間が過ごせました。</p>
<p><i>※全体を通してのメモは整理して後日追記します</i></p>
