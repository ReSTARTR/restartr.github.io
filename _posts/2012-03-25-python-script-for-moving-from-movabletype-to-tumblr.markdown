---
layout: post
status: publish
published: true
title: はてなダイアリーからTumblrへデータ移行するpythonスクリプト
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 1191
wordpress_url: http://blog.restartr.com/?p=1191
date: '2012-03-25 02:30:19 +0900'
date_gmt: '2012-03-24 17:30:19 +0900'
categories:
- python
tags:
- python
- tumblr
comments: []
---
どうも。もうすぐ入社３ヶ月が経って試用期間が終わろうとしている状況な僕です。

すっかり停滞気味のブログですが保守も兼ねて投稿を。

### はてダから<del>はてブロ</del>Tumblrへ

とあるブログをはてなダイアリーで書いていたのですが、Tumblrへブログのデータを移行したいという要求が出てきました。ということでpythonで移行スクリプトを書いてみました。

### ソースコード

例によってGitHubにて公開しておきます。

* [https://github.com/ReSTARTR/mt2tumblr](https://github.com/ReSTARTR/mt2tumblr)

python2.6以外はテストしてません( ｰ`дｰ´)ｷﾘｯ

### 使い方

oauth2に依存しているので、実行前にインストールしておいてください。

{% highlight python %}
pip install oauth2

{% endhighlight %}

使い方はREADMEどおりです。いつもどおりのテキトー英文です。

1. はてなダイアリーの管理画面から"管理 > インポート/エクスポート"でMovableType形式のファイルをダウンロード
2. ダウンロードしたファイルをresourcesディレクトリに配置
3. tumblrにアプリケーションを登録( [http://www.tumblr.com/oauth/apps](http://www.tumblr.com/oauth/apps) )
4. consumer_keyとconsumer_secretを入手
5. config.pyを編集する

{% highlight python %}
CONSUMER_KEY = '<consumer_key>'
CONSUMER_SECRET = '<consumer_secret>'
BASE_HOSTNAME = '<your_tumblr_url>' # '<your-id>.tumblr.com'のように"http://"は抜きで。
PARSE_FILE_PATH = 'path/to/movable_type_data.txt'
POST_STATE = 'publish' # 動作テストしたいなら'draft'にすると良いです。
{% endhighlight %}

実行する

{% highlight bash %}
python run.py
{% endhighlight %}
  * まずはファイルの内容を読み込んで、日時、カテゴリ、タイトルが表示されるので問題ないか確認

OAuth認証する。

  * 下記のようにURLが表示されるのでブラウザでアクセス。


{% highlight python %}
open in browser:http://www.tumblr.com/oauth/authorize?oauth_token={OAUTH_TOKEN}

oauth_verifier:

{% endhighlight %}

  * リダイレクト先のURLに含まれる"oauth_verifier"の値をコピーしてターミナルにペースト。
    * リダイレクト先は404になりますが、oauth_verifierが欲しいだけなのでそれで問題なしです。

これで、Tumblerへのインポートが始まります。

(ﾟдﾟ)ｳﾏｰ

### 注意事項

* APIの呼び出し回数の制限に引っかかるかもしれませんがそのへんのエラー制御はできていません。
* MovableType形式のデータをTumblrに移行するスクリプトとしてつくっていますが、実際にははてなダイアリーからMovableType形式でエクスポートしたデータでしかテストしていません。それ以外で動くかは保証できません…
* 投稿時間はJSTからGMTに変換してます。不要なら適当に編集してください。

