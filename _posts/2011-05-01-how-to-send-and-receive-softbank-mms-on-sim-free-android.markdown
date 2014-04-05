---
layout: post
status: publish
published: true
title: SIMフリーのandroid端末でSoftBankのMMSを受信する方法
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 904
wordpress_url: http://blog.restartr.com/?p=904
date: '2011-05-01 22:18:15 +0900'
date_gmt: '2011-05-01 13:18:15 +0900'
categories:
- gadget
tags:
- desire s
- android
- mms
comments: []
---
Desire SでS!メールを受信しようと、apnの設定をしてみたのですが、全く送受信が成功しませんでした。
いろいろ調査していたところ、SoftBankメールのアプリのバージョンが原因だと判明しました。
古いバージョンをインストールすれば問題ないようです。

SoftBankメールを送受信するための手順を下記に記しておきます。

### 1.apn設定

| 項目名 | 設定値 |
|:----|:-----|
| 名前 | ＜任意＞ |
| APN | smile.world |
| プロキシ | ＜未設定＞ |
| ポート | ＜未設定＞ |
| ユーザー名 | dna1trop |
| パスワード | so2t3k3m2a |
| MMSC | http://mms/ |
| MMSプロキシ | smilemms.softbank.ne.jp |
| MMSポート | 8080 |
| MCC | 440 |
| MNC | 20 |
| 認証タイプ | ＜未設定> |
| APNタイプ | ＜未設定＞ |

※こちらを参考にしました。

* <a href="http://blog.yukinishijima.net/android/android-dev-phone-1-%E3%81%A7mms%E3%82%92%E4%BD%BF%E3%81%88%E3%82%8B%E3%82%88%E3%81%86%E3%81%AB%E3%81%99%E3%82%8B%E3%81%BE%E3%81%A7%E3%81%AE%E9%81%93%E3%81%AE%E3%82%8A">Android Dev Phone 1 でMMSを使えるようにするまでの道のり&nbsp;|&nbsp;Yuki Nishijima Blog</a>

### 2.SoftBankメールをインストール

最新版(2011.05.01時点でv2.1)の<a href="https://market.android.com/details?id=jp.softbank.mb.mail">SoftBankメール</a>では送受信に失敗します。

なので、下記ブログ記事よりv1.6の古いバージョンをダウンロード＆インストールします。
※既にAndroidマーケットから最新版をダウンロードしている場合は、そちらを先にアンインストールしておく必要があります。

* <a href="http://gadgetordiary.wordpress.com/2011/04/13/softbank%E3%83%A1%E3%83%BC%E3%83%AB%E3%82%A2%E3%83%97%E3%83%AA1-6%E3%82%92%E5%86%8D%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB/">Softbankメールアプリ1.6を再インストール &laquo; デジタルガジェッター日記</a>

端末のブラウザからダウンロードしてインストールするか、android-sdkの入っているPCから
{% highlight bash %}
adb install jp.softbank.mb.mail-1.6.apk
{% endhighlight %}
でインストールできます。

あとは、アプリを起動すればメールの閲覧や送信が自由にできます。

