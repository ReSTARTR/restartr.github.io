---
layout: post
status: publish
published: true
title: ScalaとErlangとPHPと私
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
excerpt: "PHPよりScalaが簡単である、という議論に対するふたつのポストがあったので、自分向けにまとめました。\r\n<ul>\r\n\t<li>元記事：<a
  href=\"http://wadearnold.com/blog/scala/scala-is-easier-than-php\">Wade Arnold ?
  Scala is easier than PHP</a></li>\r\n\t<li>返信：<a href=\"http://videlalvaro.github.com/2010/11/reply-to-scala-is-easier-than-php.html\">Reply
  to \"Scala is Easier than PHP\"</a></li>\r\n</ul>\r\n<h3>概要</h3>\r\nざっとこんな感じにまとめてしまいました。\r\n<ul>\r\n\t<li>議論の中心はWebページ生成言語としての言語の\x1E比較ではない</li>\r\n\t<li>ふたりともWebページ生成ならPHPが優れているという立場にかわりはない</li>\r\n\t<li>議論の中心は主にスケーラビリティの確保とプロセス間通信</li>\r\n\t<li>元記事のWade
  ArnoldさんはScala推進派</li>\r\n\t<li>返信者のvidelalvaroさんはErlang推進派</li>\r\n\t<li>スケーラビリティ確保のためには関数型言語という結論</li>\r\n\t<li>ScalaかErlangどちらが簡単か、という議論はない</li>\r\n</ul>\r\n"
wordpress_id: 410
wordpress_url: http://blog.restartr.com/?p=410
date: '2010-11-23 18:57:35 +0900'
date_gmt: '2010-11-23 09:57:35 +0900'
categories:
- PHP
- Scala
tags:
- PHP
- Scala
- Erlang
- Scalability
comments: []
---
<p>PHPよりScalaが簡単である、という議論に対するふたつのポストがあったので、自分向けにまとめました。</p>
<ul>
<li>元記事：<a href="http://wadearnold.com/blog/scala/scala-is-easier-than-php">Wade Arnold ? Scala is easier than PHP</a></li>
<li>返信：<a href="http://videlalvaro.github.com/2010/11/reply-to-scala-is-easier-than-php.html">Reply to "Scala is Easier than PHP"</a></li>
</ul>
<h3>概要</h3>
<p>ざっとこんな感じにまとめてしまいました。</p>
<ul>
<li>議論の中心はWebページ生成言語としての言語の比較ではない</li>
<li>ふたりともWebページ生成ならPHPが優れているという立場にかわりはない</li>
<li>議論の中心は主にスケーラビリティの確保とプロセス間通信</li>
<li>元記事のWade ArnoldさんはScala推進派</li>
<li>返信者のvidelalvaroさんはErlang推進派</li>
<li>スケーラビリティ確保のためには関数型言語という結論</li>
<li>ScalaかErlangどちらが簡単か、という議論はない</li>
</ul>
<p><a id="more"></a><a id="more-410"></a></p>
<h3>ScalaがPHPより簡単な理由</h3>
<p>Wade Arnoldさんが元記事「Scala is easier than PHP」において、マルチコア時代にはScalaが必要だ、みたいなことを書きました。彼はPHP３のころからPHPの開発に関わっていて、最近はZendFrameworkのコミッタをやっていたそうです。<br />
PHPはいまだに最高の言語であると最初に断っておきつつ、Scalaの技術的な点を整理しておこう、という内容。</p>
<p>スケーラビリティの確保のためには、PHPのようにたくさんのツールを必要とせずとも、言語仕様的に多くをサポートするScalaが簡単な理由である、と。Scalaへの移行は時間を必要とするが、スケーラビリティには必要な選択であるといっています。</p>
<blockquote><p>No need for amqp with actors, no beanstalkd with mutable queues, and it’s fast as hell!</p></blockquote>
<p><em>ActorによってAMQPはいらなくなる。ミュータブルなキューもbeanstalkdなしに。そしてそれはものすごく速い。<br />
</em></p>
<h3>PHPの代わりにErlangを使う理由</h3>
<p>それに対してvidelalvaroさんが「Reply to "Scala is easier than PHP"」でコメントをしています。<br />
おおむねWadeさんの意見に同意で、彼はScalaではなくErlangを推しているようです。</p>
<p>意見としてはこんな感じでしょうか。</p>
<ul>
<li>サーバーのような長時間実行するプログラムにはErlangつかうといい</li>
<li>スレッド間通信にはErlangをつかうといい</li>
<li>Webページを生成するだけならPHPをつかうといい</li>
<li>CPUの全コアを利用したいだけならPHP-FPMをつかうといい</li>
</ul>
<h3>スケーラビリティ確保のために必要なもの</h3>
<p>元記事ではPHPのスケーラビリティ確保のために必要なものをあげ、これらを使うのは大変骨が折れると言っています。</p>
<blockquote>
<ul>
<li>Fantastic at PHP internals</li>
<li>Amazing at Apache HTTPD and compiling appropriate PHP extensions.</li>
<li>Nginx</li>
<li>BigIP ? More than round robin load balancing</li>
<li>Intimately know how sessions work and probably write your own handlers</li>
<li>Memcached</li>
<li>APC</li>
<li>AMQP</li>
<li>BeanStalkd</li>
<li>Code based sharding or at least master/slave logic</li>
<li>C/C++</li>
<li>Lots of security! It’s a problem with all dynamic languages.</li>
<li>Zend Framework.</li>
</ul>
</blockquote>
<p>それに対してScalaはたった５つ。</p>
<blockquote>
<ul>
<li>6 months of scala</li>
<li>Functional programming</li>
<li><a onclick="javascript:_gaq.push(['_trackEvent','outbound-article','akkasource.org']);" href="http://akkasource.org/">Akka Framework</a></li>
<li><a onclick="javascript:_gaq.push(['_trackEvent','outbound-article','liftweb.net']);" href="http://liftweb.net/">Lift Framework</a></li>
<li>Nginx / Jetty</li>
</ul>
</blockquote>
<p>ツールに頼らずとも言語が機能を備えているから気にすべきところが非常にシンプルになるようです。</p>
<h3>個人的感想</h3>
<p>PHPをWebページ生成以外で対決させるのはどうなんだろうと思いつつも、Scalaをどのような場面で使っていけばよいか考える良い機会になりました。<br />
個人的な話をすると、Scalaは言語仕様を学ぶばかりで実際のプログラムを書くまでには至っていませんし、性能評価もまだやってないので「PHPからScalaに移行すべきだ」、と言い切るには至っていないのが現状です。<br />
が、Wadeさんは後ほどもっと突っ込んだ記事を書くと冒頭に言っていますし、videlalvaroさんも最後に</p>
<blockquote><p>if we want to sell functional languages like Erlang or Scala to the PHP programmer then we have to look for more compelling features that may attract them to look into these languages. What I think are those features ?I guess?, should be part of another blog post.</p></blockquote>
<p>と、ScalaやErlangをPHPerに売り込むための魅力的な点をPOSTしてくれるかも。<br />
それも楽しみにしておこうと思います。</p>
<h3>余談</h3>
<p>最近思うのが、Scalaって言語仕様があまりに多くて、それを学ぶことが楽しいです。<br />
が、学ぶべき仕様がどこまでなのか見えてこず、さらに、仕様を学ぶこと自体が目的になってきているような気がします。<br />
ということで(?)そろそろ、実際につくりたいプログラムを書きながら仕様を学ぶ方向にシフトしようと思います。</p>
<p>※あとPHP-FPMもちゃんと調査・検証したいし、Kestrelのソースも読みたい。</p>
