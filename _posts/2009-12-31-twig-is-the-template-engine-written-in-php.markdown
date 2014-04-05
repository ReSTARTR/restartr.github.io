---
layout: post
status: publish
published: true
title: phpのテンプレートエンジンtwigとは
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
excerpt: "<div>最近、PHPのテンプレートエンジン<a href=\"http://www.twig-project.org/\" target=\"_blank\">Twig</a>の紹介記事をちらほら見かけるようになってきました。</div>\r\n<div>\r\n<ul>\r\n\t<li><span
  style=\"font-family: Consolas, Monaco, 'Courier New', Courier, monospace; line-height:
  18px; font-size: 12px; white-space: pre;\"><a href=\"http://labs.unoh.net/2009/12/phptwig.html\"
  target=\"_blank\">ウノウラボ Unoh Labs: PHPテンプレートエンジンTwigをいじってみました</a></span></li>\r\n\t<li><span
  style=\"font-family: Consolas, Monaco, 'Courier New', Courier, monospace; line-height:
  18px; font-size: 12px; white-space: pre;\"><a href=\"http://d.hatena.ne.jp/anatoo/20091225/1261749843\"
  target=\"_blank\">テンプレートエンジンを素のPHPからTwigに乗り換えた理由 - id:anatooのブログ</a></span></li>\r\n\t<li><a
  href=\"http://blog.nully.org/archives/154\">[Twig]なにやら話題のテンプレートエンジンTwigを使ってみた |
  Nullyのぶろぐ</a></li>\r\n</ul>\r\n</div>\r\n<div>自分自身も気になってはいたのですが、試してみるきっかけがなく今に至ってしまいました。現在のプロジェクトではviewは関わらないのですが、それまではSmarty2を使っていました。Smarty3や他テンプレートエンジンも気になるところですが、次はTwigがくるだろうと勝手に予測しています。</div>\r\n<div>とりあえず今回はTwigの特徴と他テンプレートエンジンとの比較をまとめてみます。</div>\r\n<div>"
wordpress_id: 3
wordpress_url: http://msky.sakura.ne.jp/myblog/?p=3
date: '2009-12-31 02:10:55 +0900'
date_gmt: '2009-12-30 17:10:55 +0900'
categories:
- PHP
tags:
- PHP
- TemplateEngine
- twig
comments: []
---
<div>最近、PHPのテンプレートエンジン<a href="http://www.twig-project.org/" target="_blank">Twig</a>の紹介記事をちらほら見かけるようになってきました。</div>
<div>
<ul>
<li><span style="font-family: Consolas, Monaco, 'Courier New', Courier, monospace; line-height: 18px; font-size: 12px; white-space: pre;"><a href="http://labs.unoh.net/2009/12/phptwig.html" target="_blank">ウノウラボ Unoh Labs: PHPテンプレートエンジンTwigをいじってみました</a></span></li>
<li><span style="font-family: Consolas, Monaco, 'Courier New', Courier, monospace; line-height: 18px; font-size: 12px; white-space: pre;"><a href="http://d.hatena.ne.jp/anatoo/20091225/1261749843" target="_blank">テンプレートエンジンを素のPHPからTwigに乗り換えた理由 - id:anatooのブログ</a></span></li>
<li><a href="http://blog.nully.org/archives/154">[Twig]なにやら話題のテンプレートエンジンTwigを使ってみた | Nullyのぶろぐ</a></li>
</ul>
</div>
<div>自分自身も気になってはいたのですが、試してみるきっかけがなく今に至ってしまいました。現在のプロジェクトではviewは関わらないのですが、それまではSmarty2を使っていました。Smarty3や他テンプレートエンジンも気になるところですが、次はTwigがくるだろうと勝手に予測しています。</div>
<div>とりあえず今回はTwigの特徴と他テンプレートエンジンとの比較をまとめてみます。</div>
<div><a id="more"></a><a id="more-3"></a></div>
<h2>Twigの特徴</h2>
<p>公式サイトから英訳（適当に）してみました。</p>
<div>
<ul>
<li>簡潔に書ける</li>
<li>テンプレート指向文法である</li>
<li>自動エスケープなど必要なものをすべてサポートしている</li>
<li>文法を簡単に学習できる（※他テンプレートエンジンはPHP4ベースで作られていたりして、web開発においてベストプラクティスとして採用できない。）</li>
<li>高い拡張性で独自DSLも作れる</li>
<li>ユニットテストされているのでライブラリは堅牢で、大きなプロジェクトにもすぐに使える。</li>
<li>ちゃんとドキュメント化されている：</li>
<li>セキュリティでは自動出力エスケープやsandboxモードによって安全性を確保</li>
<li>詳細なエラーメッセージでデバッグもカンタン</li>
<li>テンプレートを素のPHPコードにコンパイルするので、普通のPHPコードに比べてオーバーヘッドを最小限に抑えることができる。</li>
</ul>
</div>
<h2>開発者について</h2>
<div>Symfony開発者でもある<a href="http://fabien.potencier.org/article/34/templating-engines-in-php" target="_blank">Fabienのブログ記事</a>（と<a href="http://fabien.potencier.org/article/35/templating-engines-in-php-follow-up" target="_blank">そのフォロー記事</a>）を見れば、開発の経緯や他テンプレートエンジンとの比較、それからPHP自体テンプレートエンジンじゃないかという点について言及しています。</div>
<div>素のPHPをViewとして使いたい人はSymfonyTemplateComponentがスタンドアロンで動作するのでそれを使うと良いと。ViewでPHP的な文法を用いることに疑問を持っていたようです。</div>
<div>
<h2>既存テンプレートエンジンとの比較</h2>
</div>
<div>
<div>もともと彼は以下機能を求めてテンプレートエンジンを探していたので、この観点でそれぞれのテンプレートエンジンを評価しています。(なのでこれがそのままTwigの特徴になっています。）</div>
<div>
<div>
<ul>
<li>Concision (記述が簡潔)</li>
<li>Template oriented syntax (テンプレート指向な文法)</li>
<li>Reusability (再利用性)</li>
<li>Security (安全性)</li>
<li>Sandbox mode (砂場モード)</li>
</ul>
</div>
</div>
</div>
<div>
<div><strong><a href="http://www.smarty.net/" target="_blank">Smarty</a></strong><strong>:<span style="font-weight: normal;">Smarty3で機能要求を満たしているものの、パフォーマンスが悪い。<br />
<strong><a href="http://phptal.org/" target="_blank">PHPTAL</a></strong><strong>:<span style="font-weight: normal;">よく設計されていて、機能もあるが、webデザイナにはとっつきにくく、テンプレートの継承的なことは特にそう。</span></strong></span></strong></div>
<div><strong><span style="font-weight: normal;"><strong><a href="http://www.ezcomponents.org/docs/tutorials/Template" target="_blank">ezComponents Template</a></strong><strong>:<span style="font-weight: normal;">よく設計されていて、多くの機能を持つが、継承をサポートしていないのと、パフォーマンスが他よりかなり劣る。<br />
<strong><a href="http://dwoo.org/" target="_blank">Dowoo</a></strong><strong>:<span style="font-weight: normal;">Smartyの代替プロジェクトとしてSmartyを真似てつくられていて、継承などの新機能も追加されている。パフォーマンスもSmartyより良いけど、sandboxがないのと、コアシステムの拡張性が十分でない。<br />
<strong><a href="http://www.beberlei.de/calypso/" target="_blank">Calyps</a></strong><strong><a href="http://www.beberlei.de/calypso/" target="_blank">o</a></strong><strong>:</strong>DjangoのPHPクローンだけど諸問題により開発停止している。</span></strong></span></strong></span></strong></div>
</div>
<h3>ベンチマーク</h3>
<p>既存のテンプレートエンジンに関するベンチマーク結果です。</p>
<h4>ベンチマーク方法</h4>
<div>
<div id="_mcePaste">
<ul>
<li>３つのアイテムをループさせ、シンプルな装飾のレイアウト出力の比較。</li>
<li>10000回表示するテストを１０回実行した平均値。</li>
<li>継承はヘッダとフィッタを代わりに使用。</li>
<li>自動出力エスケープができないものは手動でエスケープ処理。</li>
<li>コマンドラインでPHPAcceleratorなどを使用しない環境で実行</li>
</ul>
</div>
<h4>処理時間とメモリ消費</h4>
</div>
<div>
<table border="1">
<tbody>
<tr>
<th>Library</th>
<th>Time (sec)</th>
<th>Memory (Ko)</th>
<th>Templates rendered per second</th>
</tr>
<tr>
<th>Twig</th>
<td>3</td>
<td>1,190</td>
<td>3,333</td>
</tr>
<tr>
<th>PHPTAL</th>
<td>3.8</td>
<td>2,100</td>
<td>2,632</td>
</tr>
<tr>
<th>Dwoo</th>
<td>6.9</td>
<td>1,870</td>
<td>1,449</td>
</tr>
<tr>
<th>Smarty 2</th>
<td>12.9</td>
<td>2,350</td>
<td>775</td>
</tr>
<tr>
<th>Smarty 3</th>
<td>14.9</td>
<td>3,230</td>
<td>671</td>
</tr>
<tr>
<th>Calypso</th>
<td>34.3</td>
<td>620</td>
<td>292</td>
</tr>
<tr>
<th>eZ Templates</th>
<td>53</td>
<td>5,850</td>
<td>189</td>
</tr>
</tbody>
</table>
</div>
<h4>コンパイル後のメモリ消費</h4>
<p>※フォロー記事の方がSmartyの数値が妥当らしいのでそっちを引用しています。</p>
<table border="1">
<tbody>
<tr>
<th>Library</th>
<td>Time(sec)</td>
<td>Memory without compilation (Ko)</td>
</tr>
<tr>
<th>Plain PHP</th>
<td>2.4</td>
<td>114</td>
</tr>
<tr>
<th>Twig</th>
<td>3</td>
<td>383</td>
</tr>
<tr>
<th>PHPTAL</th>
<td>3.8</td>
<td>598</td>
</tr>
<tr>
<th>Dwoo</th>
<td>6.9</td>
<td>1,645</td>
</tr>
<tr>
<th>Smarty 2</th>
<td>12.9</td>
<td>610</td>
</tr>
<tr>
<th>Smarty 3</th>
<td>14.9</td>
<td>799</td>
</tr>
<tr>
<th>Calypso</th>
<td>34.3</td>
<td>614</td>
</tr>
<tr>
<th>eZ Templates</th>
<td>53</td>
<td>2,783</td>
</tr>
</tbody>
</table>
<h2>まとめ</h2>
<div>機能性だけでなく速さも兼ね備えたテンプレートエンジンTwig。</div>
<div>便利なテンプレートエンジンはSmartyがデファクトでしたが、Twigはそれを追い抜くかもしれませんね。</div>
<div>Symfony2.0でオプションのViewとして用意するかも、とFabienは個人的見解として言っているので、そうなると日本語圏でも利用が加速するかもしれません。ただ、Symfony2.0はPHP5.3以上必須なのでPHP5.3への移行に依存しますかね。Smarty3が正式リリースまでにどこまでパフォーマンス改善してくるかも楽しみなところです。</div>
<div>
<p>Smartyから乗り換える場合はプログラマよりデザイナの移行コストが高いと思われるのですが、それがクリアできるならぜひ移行してみたいですね。素のPHPよりは断然読みやすく理解しやすですし。</p>
</div>
<p>次は機能的なところをまとめたいと思います。</p>
