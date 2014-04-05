---
layout: post
status: publish
published: true
title: Hadoop Hack Nignt vol2に行ってきた
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 254
wordpress_url: http://blog.restartr.com/?p=254
date: '2010-08-05 00:13:16 +0900'
date_gmt: '2010-08-04 15:13:16 +0900'
categories:
- hadoop
tags:
- hadoop
comments: []
---
<p>今更ですが、8月4日に開催された<a href="http://gihyo.jp/event/2010/hadoophn2">Hadoop Hack Night vol.2</a>に行ってきました。<br />
ハッシュタグは<a href="http://twitter.com/#search?q=%23hadoophn">#hadoophn</a>。</p>
<p>詳細なレポートは以下を参照。</p>
<ul>
<li><a href="http://gihyo.jp/news/report/2010/08/0901">レポート：見えてきたHadoopの“使いどころ”─「Hadoop Hack Night Vol.2」開催｜gihyo.jp … 技術評論社</a></li>
<li><a href="http://techblog.yahoo.co.jp/cat206/post_25/">満員御礼！「Hadoop Hack Night2」レポート (Yahoo! JAPAN Tech Blog)</a></li>
</ul>
<p>技評のレポートに、</p>
<blockquote><p>古宮氏も，認証機能によって多くのユーザが使いながらジョブを立ち上げたままにできる点を指摘し，データを投げる入り口で認証をかけるため，共有使用時にだれかがオペレーションミスをしてもその影響で全体のデータが壊れることがないというメリットを挙げた。
</p></blockquote>
<p>とあるように、Hadoop with Securityのようなセキュリティ機能強化によって共用クラスタの可能性がみえてきた、というのが今回の一番の収穫ではないでしょうか。</p>
<p>Hadoop with securityについては以下のOwen氏のスライドが参考になります。</p>
<div style="width:425px" id="__ss_3553751"><strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/hadoopusergroup/hadoop-security-preview" title="Hadoop Security Preview">Hadoop Security Preview</a></strong><object id="__sse3553751" width="425" height="355"><param name="movie" value="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=hadoopsecuritypreview-100325134432-phpapp02&stripped_title=hadoop-security-preview" /><param name="allowFullScreen" value="true"/><param name="allowScriptAccess" value="always"/><embed name="__sse3553751" src="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=hadoopsecuritypreview-100325134432-phpapp02&stripped_title=hadoop-security-preview" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="425" height="355"></embed></object>
<div style="padding:5px 0 12px">View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/hadoopusergroup">Hadoop User Group</a>.</div>
</div>
<p>まだ本格的にHadoopを利用していませんが、この可能性は大規模クラスタを準備するのが困難な企業にとって朗報だと思います。<br />
今後の動向に注目ですね。</p>
