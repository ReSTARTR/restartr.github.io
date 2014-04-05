---
layout: post
status: publish
published: true
title: pythonのテストにpytestを使ってみた
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 1366
wordpress_url: http://blog.restartr.com/?p=1366
date: '2013-04-05 21:00:24 +0900'
date_gmt: '2013-04-05 12:00:24 +0900'
categories:
- python
tags: []
comments: []
---
pytestを使い始めました。

* <a href="http://pytest.org/latest/">pytest: helps you write better programs</a>

まだまだ機能は把握しきれていませんが、良いと思ったことは3つ。

1. テストがコケた箇所のコードがレポート内容に表示される
2. テスト対象を初期化したものの受け渡し方をスマートにできる
3. pytest.vimがなかなか使える

### 1.テストがコケた箇所のコードがレポート内容に表示される

これが巷でよく聞く一番のメリットかとは思いますが、コケたときの情報が全然違います。
pytestはかなり詳細に表示してくれるので、レポートの内容だけでどこをどう直せば良いか把握できます。

#### unittestの実行結果(-vオプション付き)

<a href="http://www.flickr.com/photos/53244662@N04/8621946368/" title="unittest by ReSTARTR_y, on Flickr"><img src="http://farm9.staticflickr.com/8523/8621946368_d7bc9a450d_z.jpg" width="640" height="220" alt="unittest"></a>

#### pytestの実行結果(-vオプション付き)

<a href="http://www.flickr.com/photos/53244662@N04/8620844529/" title="pytest by ReSTARTR_y, on Flickr"><img src="http://farm9.staticflickr.com/8244/8620844529_ce66ff7f9c_z.jpg" width="640" height="401" alt="pytest"></a>

文字列比較の場合は差分表示してくれたりするので便利です。

### 2. テスト対象を初期化したものの受け渡し方をスマートにできる

これが結構良い感じだと思いました。

かなり単純なクラスを対象にして例を書きます。

{% highlight python %}
# -*- coding: utf-8 -*-

class Hoge(object):

    def __init__(self, v):
        self.val = v

    def update(self, v):
        self.val = v
{% endhighlight %}

#### python同梱のunittestで書く

こんな感じで、self.hogeを使ってテスト対象を受け渡します。

{% highlight python %}
# -*- coding: utf-8 -*-
from hoge import Hoge
import unittest

class TestHoge1(unittest.TestCase):

    def setUp(self):
        self.hoge = Hoge(1)

    def test_type(self):
        self.assertIsInstance(self.hoge, Hoge)

    def test_val(self):
        self.assertEqual(self.hoge.val, 1)

        self.hoge.update('hoge')
        self.assertEqual(self.hoge.val, 'hige')

if __name__ == '__main__':
    unittest.main()
{% endhighlight %}

pytestで書くとこんな感じになります。

{% highlight python %}
# -*- coding: utf-8 -*-
from hoge import Hoge
import pytest

class TestHoge1(object):

    def pytest_funcarg__hoge(request):
        return Hoge(1)

    def test_type(self, hoge):
        assert isinstance(hoge, Hoge)

    def test_val(self, hoge):
        assert hoge.val == 1

        hoge.update('hoge')
        assert hoge.val == 'hige'

if __name__ == '__main__':
    pytest.main()
{% endhighlight %}

何が良いかっていうと、各テストメソッドで必要な初期化済みHogeインスタンスを、pytest_funcarg__hoge()で作って渡してやることができます。pytest_funcarg__NAMEを定義すれば、各テストメソッドでNAMEとして引数にとることができるわけです。(<a href="http://pytest.org/latest-ja/funcargs.html">テスト関数 (funcargs) にオブジェクトを注入</a>)

unittest.TestCaseのsetUpを使う場合、self.hogeに一旦入れてやらないといけないし、各テストメソッドではself.hogeでアクセスする必要も出てきます。

見た目が簡潔になるのはとても良いことです。

### 3. pytest.vimがなかなか使える

vimのプラグインにpytest.vimっていうのがありました。
これもなかなか使い勝手が良いです。

詳しくは下記動画を御覧ください。

* <a href="http://vimeo.com/19774046"> pytest.vim 0.0.5 on Vimeo </a>

pytestの概要は以下スライドにて。

<iframe src="http://www.slideshare.net/slideshow/embed_code/14006990" width="427" height="356" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen webkitallowfullscreen mozallowfullscreen> </iframe>
<div style="margin-bottom:5px"> <strong> <a href="http://www.slideshare.net/pfctdayelise/funcargs-other-fun-with-pytest" title="Funcargs &amp; other fun with pytest" target="_blank">Funcargs &amp; other fun with pytest</a> </strong> from <strong><a href="http://www.slideshare.net/pfctdayelise" target="_blank">Brianna Laugher</a></strong> </div>
他の機能は触りながらおいおい掴んでいければなと。

