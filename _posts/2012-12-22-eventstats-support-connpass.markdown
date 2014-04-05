---
layout: post
status: publish
published: true
title: EventStatsをConnpassに対応させました
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 1251
wordpress_url: http://blog.restartr.com/?p=1251
date: '2012-12-22 22:54:54 +0900'
date_gmt: '2012-12-22 13:54:54 +0900'
categories:
- Webサービス
tags:
- 勉強会
- eventstats
comments: []
---
ふと思い立って、イベントの登録者数の推移が見れるEventStatsをConnpassにも対応させました。

* [EventStats - イベントの統計情報が見れます](http://eventstats.restartr.com)

[ConnpassのAPI](http://connpass.com/about/api/)はイベントの参加者リストまでは取得できませんが、イベント検索はほぼatnd準拠。なので追加したコード量はわずかでした。

作った当初は何かいろいろやってたくさんの人に使ってもらえるWebサービスにしたかったれけど、結局は参加者数の推移を見る以外に何の取り柄もないままです。

##### 余談

このWebサービス、クローラーはPython、WebサイトはScalaで書いていて、一年近く前に書いたコードなのでちょっとだけ時間かかりました。仕事で使っているpythonに比べて、シンタックスを見るのも数ヶ月ぶりなScalaは何をやっているか思い出すのにもひと苦労です。もっとScalaにも取り組みたいのですが…

