---
layout: post
status: publish
published: true
title: 'イベントの参加人数の遷移が見れるサービス『EventStats』を作りました。 #atnd #zusaar #partake'
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 1096
wordpress_url: http://blog.restartr.com/?p=1096
date: '2011-12-10 18:58:51 +0900'
date_gmt: '2011-12-10 09:58:51 +0900'
categories:
- Webサービス
tags: []
comments: []
---
<h3>イベント管理サービスについて</h3>
<p>勉強会などでよく利用されるのが、イベント管理サービス。

ATNDやZusaar,Partakeなど、様々なサービスがあり、現在も新しいものが生まれている状況。</p>
<p>今や勉強会を開催する上で必須のサービスとなっていますよね。</p>
<p>ですが、主催者や参加者にとってそれらのサービスで閲覧できるのは『今どのような状況か』ということだけです。それまでどのように参加者が増えてきたのか、その後も参加者が増えそうか、ということまでは見ることができません。</p>
<p>「今」ではなく「過去」を知りたい。(by @ReSTARTR)</p>
<p>ということで作りました。</p>
<h3>イベントごとに登録数、補欠数、枠数を記録するサービス「EventStats｣</h3>
<ul>
<li><a href="http://eventstats.restartr.com">EventStats - イベントの統計情報が見れます</a></li>
</ul>
<p>簡単にいうと<a href="http://klout.com">Klout</a>の勉強会バージョン（のとっかかり）です。

このサービスを使えば、登録数と枠数の推移を時系列のチャートで確認することができます。

5分おきに各イベント管理サービスの情報を取得して蓄積し、情報を表示しているだけのシンプルなサービスです。

<em>(※ SVGベースのチャートのため、androidでは2.4/3.0以降でないと見れません)</em></p>
<p>あと、2011年11月頭くらいからデータ蓄積開始したのでそれ以前のイベントの履歴は見れない場合があるのと、Partakeの枠数が0人なのも蓄積ミスです。</p>
<h3>使い方</h3>
<ol>
<li><a href="javascript:(function(){s=window.location.href.split('/');type='';if(s[2]=='atnd.org'&&s[3]=='events')type='atnd';else if(s[2]=='www.zusaar.com'&&s[3]=='event')type='zusaar';else if(s[2]=='partake.in'&&s[3]=='events')type='partake';else return false; window.location.href=['http://eventstats.restartr.com/events/'+type,s[4]].join('/');})();">ブックマークレット</a>をブラウザに登録する</li>
<li>履歴を知りたいイベント管理サービスの詳細ページに移動する</li>
<li>ブックマークレットを実行する</li>
</ol>
<h3>技術的なこと</h3>
<p>とりあえずHeroku上でScalaのUnfilteredを使って動かしています。

技術的な話しは追々。</p>
<h3>今後どう料理するか</h3>
<p>実はMA7の締切りぎりぎりに公開してました。（証拠↓）</p>
<p><a href="https://ma7.mashupaward.jp/works/478?locale=ja">https://ma7.mashupaward.jp/works/478?locale=ja</a></p>
<p>が、リソースが足りてないさくらのVPSで動かしていたので、公開したことはとくにアナウンスとかしてませんでした。（MA7には当然のごとく選考漏れでしたが。）で、それからHerokuへの移行を進めつつ機能の修正などをやってたという訳です。</p>
<p>今後は蓄積したデータをもとに、勉強会運営や勉強会への参加の助けとなる数値を加えていければいいなと。

実はテストとか負荷試験とかあんまりできてないのであまりイジメないでください^^;</p>
<ul>
<li><a href="http://eventstats.restartr.com">EventStats - イベントの統計情報が見れます</a></li>
</ul>
