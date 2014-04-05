---
layout: post
status: publish
published: true
title: CakePHPのDebugKitにプロファイル情報が見れるXHProfPanelを追加
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
excerpt: "先日のPHPMatsuriのハッカソンにて作成したXHProfの結果がみれるCustomPanelですが、そのまま公開するにはお恥ずかしい感じだったので、多少マシに改修したものを公開してみます。\r\n<h3>DebugKit,
  XHProfPanelとは</h3>\r\nCakePHPのDebugKitというプラグインがあり、それを使えばいろんなデバッグ情報がWeb画面上で確認できます。で、これにXHProfというphp拡張を使ってプロファイラ情報を見れるようにしてみました。\r\n\r\nこんな感じです。\r\n<a
  href=\"http://blog.restartr.com/wp-content/uploads/2010/10/256d829a326e5fc745e5659600527bde.png\"><img
  class=\"alignnone size-full wp-image-390\" title=\"XHProfPanel-screenshot\" src=\"http://blog.restartr.com/wp-content/uploads/2010/10/256d829a326e5fc745e5659600527bde.png\"
  alt=\"\" width=\"464\" height=\"42\" /></a>\r\n\r\n"
wordpress_id: 367
wordpress_url: http://blog.restartr.com/?p=367
date: '2010-10-10 00:59:04 +0900'
date_gmt: '2010-10-09 15:59:04 +0900'
categories:
- PHP
tags:
- PHP
- cakephp
comments: []
---
<p>先日のPHPMatsuriのハッカソンにて作成したXHProfの結果がみれるCustomPanelですが、そのまま公開するにはお恥ずかしい感じだったので、多少マシに改修したものを公開してみます。</p>
<h3>DebugKit, XHProfPanelとは</h3>
<p>CakePHPのDebugKitというプラグインがあり、それを使えばいろんなデバッグ情報がWeb画面上で確認できます。で、これにXHProfというphp拡張を使ってプロファイラ情報を見れるようにしてみました。</p>
<p>こんな感じです。<br />
<a href="http://blog.restartr.com/wp-content/uploads/2010/10/256d829a326e5fc745e5659600527bde.png"><img class="alignnone size-full wp-image-390" title="XHProfPanel-screenshot" src="http://blog.restartr.com/wp-content/uploads/2010/10/256d829a326e5fc745e5659600527bde.png" alt="" width="464" height="42" /></a></p>
<p><a id="more"></a><a id="more-367"></a></p>
<p>で、パネルを開くとこんな感じに各関数呼び出しごとの負荷情報などを閲覧できます。<br />
<a href="http://blog.restartr.com/wp-content/uploads/2010/10/f0c2dac263680a94429c57bd5c1e81ff.png"><img class="alignnone size-medium wp-image-399" title="xhprofpanel-opned-screenshot" src="http://blog.restartr.com/wp-content/uploads/2010/10/f0c2dac263680a94429c57bd5c1e81ff-300x170.png" alt="" width="300" height="170" /></a></p>
<p>さらに、<strong>"xhprof Result"</strong>というリンクをクリックすれば、XHProfが用意してくれているビューアを開くこともできます。ビューアではコールグラフも閲覧できます。</p>
<h3>カスタムパネルの追加方法</h3>
<blockquote><p>helloworld_controllerにプロファイル実行する、という例で説明します。</p></blockquote>
<h4>事前準備</h4>
<p>debugkitはインストール済みとします。入れ方は<br />
<a href="http://d.hatena.ne.jp/cakephper/20090604/1244112188">超便利なDebugkitを画面キャプチャ付きで解説 - cakephperの日記(CakePHP, MongoDB, Lithium)</a><br />
を参照してください。</p>
<p>XHProfのインストールも済ませておきます。</p>
<pre class="brush">wget http://pecl.php.net/get/xhprof-0.9.2.tgz
pecl install xhprof-0.9.2.tgz</pre>
<p>php.iniにも設定追加しておきます。</p>
<pre class="brush">extension=xhprof.so</pre>
<p>tgzファイルをtarで解凍した中身のうち、xhprof_html,xhprof_libをビューア用に用意したバーチャルホスト下において、<br />
別ドメインでアクセスできるようにします。<br />
cakeのアプリは<strong>cake.localhost</strong>、xhprofビューアは<strong>xhprof.localhost</strong>のような感じです。<br />
ディレクトリはこんな感じできっています。</p>
<pre class="brush:c">/var/
  vhosts/
    cake/ // cake.localhostのdocument_root
      app/
    xhprof/
      xhprof_html/ // xhprof.localhostのdocument_root
      xhprof_lib/</pre>
<h4>ファイル構成</h4>
<p>下記ファイルを修正もしくは新規作成します。</p>
<pre class="brush:c">app/
  controllers/
    helloworld_controller.php //新規追加
  plugins/
    debug_kit/
      controllers/
        conmponents/
          toolbar.php //修正
      views/
        elements/
          xhprof_panel.ctp //新規追加</pre>
<h4>1.XHProfPanelの追加</h4>
<blockquote><p>app/plugins/debug_kit/controllers/components/toolbar.php</p></blockquote>
<p>に下記クラスをコピペで追加。<br />
<script src="http://gist.github.com/618254.js?file=toolbar.php"></script></p>
<h4>2.デフォルトパネルにXHProfPanelを追加</h4>
<blockquote><p>app/plugins/debug_kit/controllers/components/toolbar.php</p></blockquote>
<p>の既存行（52行目あたり)に下記のように$_defaultPanelsに'xhprof'を追加。<br />
<script src="http://gist.github.com/618254.js?file=toolbar_.php"></script></p>
<h4>3.controllerの設定にもXHProfPanelを追加</h4>
<p>下記のようにXhprofPanelへの設定情報も渡します。<br />
<script src="http://gist.github.com/618254.js?file=helloworld_controller.php"></script></p>
<h4>4.テンプレートの追加</h4>
<blockquote><p>app/plugins/debug_kit/views/elements/xhprof_panel.ctp</p></blockquote>
<p>を下記内容で新規作成します。<br />
<script src="http://gist.github.com/618254.js?file=xhprof_panel.ctp"></script></p>
<h4>ソート順の変更</h4>
<p>デフォルトではwall timeの降順でソートしてあります。<br />
コントローラでの設定で'sortBy'の値を変えれば変更できます。<br />
(指定しなければ'wt'指定となります。)<br />
設定できるのは<strong>ct, wt, cpu, mu, pmu</strong>の５つ。</p>
<h4>プロファイル範囲</h4>
<p>XhprofPanel::__constructからXhprofPanel::beforeRenderまでです。<br />
結果をみるとLogPanelクラス（DebugKitプラグイン内のクラス）のメソッド呼び出しも含まれていたりするので、<br />
もう少し適当な範囲にできないか検討したいと思います。</p>
<h3>余談</h3>
<p>Githubを有料アカウントにすべきか迷い中です。今回とりあえずgistで公開してみましたが、リポジトリは欲しいような、でも今のところ必要ないような…</p>
