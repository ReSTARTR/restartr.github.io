---
layout: post
status: publish
published: true
title: Gearmanでqueueing
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
excerpt: "<h3>queueing</h3>\r\nwebページを生成する際には、今すぐやらなきゃいけないことと、今すぐでなくても良いものがあります。\r\n今すぐやらなきゃいけないこととは、ユーザーリクエストに対するDB参照結果等のことで、今すぐでなくても良いものとはアクセスログなどの処理をDBに書き込んだりメール送信したりなど。それ以外にもあるかもですが大体そんな感じです。\r\n\r\n通常であればすべて一回のHTTPリクエストの中でやるわけですが、queueingをすれば今やるべきでないことを後回しにできます。他にもメリットはありますが割愛で。\r\n\r\nで、phpでqueuingをやるとしたら、こんなのがあるそうです。\r\n<ul>\r\n\t<li><a
  href=\"http://q4m.31tools.com/\">Q4M\r\n</a>\r\n</li>\r\n<li><a href=\"http://gearman.org\">Gearman</a></li>\r\n<li><a
  href=\"http://activemq.apache.org/\">ActiveMQ</a></li>\r\n<li><a href=\"http://kr.github.com/beanstalkd/\">beanstalkd</a></li>\r\n</ul>\r\n\r\nで、Q4Mを試そうとしたらMySQL5.1以降でないとダメとか。\r\nちょっとした事情で「5.1以降」という制約は避けたいので、Gearmanを試してみる事に。\r\n※最後のbeanstalkdは<a
  href=\"http://rad-dev.org/\">lithium</a>から使えるみたいです(<a href=\"http://rad-dev.org/li3_queue\">li3_queue</a>)。\r\n"
wordpress_id: 190
wordpress_url: http://blog.restartr.com/?p=190
date: '2010-01-31 00:16:51 +0900'
date_gmt: '2010-01-30 15:16:51 +0900'
categories:
- PHP
tags:
- jobqueue
- gearman
comments: []
---
<h3>queueing</h3>
<p>webページを生成する際には、今すぐやらなきゃいけないことと、今すぐでなくても良いものがあります。<br />
今すぐやらなきゃいけないこととは、ユーザーリクエストに対するDB参照結果等のことで、今すぐでなくても良いものとはアクセスログなどの処理をDBに書き込んだりメール送信したりなど。それ以外にもあるかもですが大体そんな感じです。</p>
<p>通常であればすべて一回のHTTPリクエストの中でやるわけですが、queueingをすれば今やるべきでないことを後回しにできます。他にもメリットはありますが割愛で。</p>
<p>で、phpでqueuingをやるとしたら、こんなのがあるそうです。</p>
<ul>
<li><a href="http://q4m.31tools.com/">Q4M<br />
</a>
</li>
<li><a href="http://gearman.org">Gearman</a></li>
<li><a href="http://activemq.apache.org/">ActiveMQ</a></li>
<li><a href="http://kr.github.com/beanstalkd/">beanstalkd</a></li>
</ul>
<p>で、Q4Mを試そうとしたらMySQL5.1以降でないとダメとか。<br />
ちょっとした事情で「5.1以降」という制約は避けたいので、Gearmanを試してみる事に。<br />
※最後のbeanstalkdは<a href="http://rad-dev.org/">lithium</a>から使えるみたいです(<a href="http://rad-dev.org/li3_queue">li3_queue</a>)。<br />
<a id="more"></a><a id="more-190"></a></p>
<h3>仕組み</h3>
<p>図解は<a href="http://gearman.org/images/gearman_stack.png">公式サイトの図</a>を見て下さい。</p>
<p>登場人物は大きく３つ。</p>
<ul>
<li> ・Client：処理を依頼する人</li>
<li> ・Worker：依頼された処理を実行する人</li>
<li> ・JobServer：ClientとWorkerの橋渡しをする人</li>
</ul>
<p>JobServerはdaemonで常駐していて、<br />
Clientからの要求がある度に、別途常駐しているWorkerに対して処理を投げます。<br />
かなりシンプルです。</p>
<h3>使い道</h3>
<p>Client側から処理を依頼するパターンはだいたい４つ。</p>
<ul>
<li>今すぐ依頼して、その結果を受け取る</li>
<li>今すぐ依頼して、結果を待たずに終了する</li>
<li>タスクを追加して、最後にまとめて実行して、その結果を受け取る</li>
<li>タスクを追加して、最後にまとめて実行して、結果を待たずに終了する</li>
</ul>
<h3>インストール</h3>
<p>Gearmanサーバーのインストール</p>
<blockquote><p>wget http://launchpad.net/gearmand/trunk/0.11/+download/gearmand-0.11.tar.gz<br />
tar zxf gearmand-0.11.tar.gz<br />
cd gearmand-0.11<br />
./configure<br />
make; make install</p></blockquote>
<p>起動</p>
<blockquote><p>/usr/local/sbin/gearmand  -u root --daemon</p></blockquote>
<p>php extensionのインストール</p>
<blockquote><p>wget http://pecl.php.net/get/gearman-0.6.0.tgz<br />
tar zxf gearman-0.6.0.tgz<br />
cd gearman-0.6.0<br />
phpize<br />
./configure<br />
make; make install</p></blockquote>
<h3>サンプル</h3>
<h4>今すぐ依頼して、結果を受け取るパターン</h4>
<p>worker.php</p>

```php
<? php
$worker = new GearmanWorker();
$worker->addServer();
$worker-&gt;addFunction('hoge','hoge_func');
// 常駐
while($worker-&gt;work());

function hoge_func(GearmanJob $job)
{
    return 'hoge'.$job-&gt;workload();
}
```
<? php

<p>client.php</p>

```php
<? php
$worker = new GearmanWorker();
$worker->addServer();
echo  $client-&gt;do('hoge', 'hello');
echo "\n";
```

<p>実行してみる</p>
<blockquote><p>$ php woker.php &amp;<br />
$ php client.php<br />
hello, hoge</p></blockquote>
<p>これだけだとほぼ意味ないので、重い処理を遅延実行させてみます。</p>
<h4>今すぐ依頼して、結果を待たずに終了するパターン</h4>
<p>worker.php</p>

```php
<? php
$worker = new GearmanWorker();
$worker-&gt;addServer();
$worker-&gt;addFunction('hoge','hoge_func');
$worker-&gt;addFunction('heavy','heavy_func');
// 常駐
while($worker-&gt;work());

function hoge_func(GearmanJob $job)
{
    return 'hoge, '.$job-&gt;workload();
}
function heavy_func(GearmanJob $job)
{
        echo "wait...";
        sleep(10);
    return 'hoge, '.$job-&gt;workload();
}
```

<p>client.php</p>

```php
<? php
$client = new GearmanClient();
$client->addServer();
echo  $client-&gt;do('hoge', 'hello');
echo "\n";
echo $client-&gt;do('heavy', 'hello (sync)');
echo "\n";
$client-&gt;doBackground('heavy', 'hello (async)');
echo "\n";
```

<p>実行してみる</p>
<blockquote><p>$ php worker.php &amp;<br />
$ php client.php<br />
hoge, hello<br />
hoge, hello (sync)</p></blockquote>
<p>とまぁ、doBackgroundで依頼したキューの場合、clientでは結果を受け取ってません。<br />
これが基本的な使い方。</p>
<h4>タスクを追加して、最後にまとめて実行して、その結果を受け取る</h4>
<p>task_client.php</p>

```php
<? php
$client = new GearmanClient();
$client->addServer();
$client->setCompleteCallback('task_cb');
$client->addTask('hoge', 'arg1');
$client->addTask('heavy', 'arg2');
$client->runTasks();
function task_cb(GearmanTask $task)
{
    echo '[result]'.$task->data();
    echo "\n";
}
```

<blockquote><p>
$ worker.php &<br />
$ task_client.php<br />
[result]hoge, arg1<br />
[result]hoge, arg2
</p></blockquote>

<p>と、ひとつめの結果表示のあとには重い処理待ちが発生します。</p>
<h4>タスクを追加して、最後にまとめて実行して、結果を待たずに終了する</h4>
<p>重い処理をバックグラウンドのタスクとして登録します。<br />
task_client.php</p>

```php
<? php

$client = new GearmanClient();
$client->addServer();
$client->setCompleteCallback('task_cb');
$client->addTask('hoge', 'arg1');
$client->addTaskBackground('heavy', 'arg2');
$client->runTasks();

function task_cb(GearmanTask $task)
{
        echo '[result]'.$task->data();
        echo "\n";
}
```

<p>実行</p>

<blockquote><p>
[result]hoge, arg1
</p></blockquote>
<p>addTaskBackgroundで追加したタスクの結果のみ待たずに処理が終了します。<br />
この例だと意味ないですが、今すぐ結果を必要としない処理を後回しにすることでユーザーへのレスポンスを高速化することができます。</p>
<h3>注意点</h3>
<h4>引数は数字と文字列のみ</h4>
<p>配列やオブジェクトではやりとりできないので、<br />
そういったものはシリアライズするとかjsonにするとかして渡さないとだめです。</h4>
<h4>workerが見つかるまで待つ</h4>
<p>clientからリクエストするworker名が存在しない（常駐していない場合）は、<br />
JobServerはworkerが見つかるまで待ちます。clientがバックグラウンドで依頼しなければそちらでも待ちが発生します。で、対象workerを起動すると即座にJobServerはキュー処理をworkerに渡します。当然ですが。</p>
<p>もう少し現実的な処理の中でqueueingして試したかったのですが、<br />
まずは入り口ということで。次回はもうちょっと突っ込んだ処理をやってみたいと。</p>
<h3>参考資料</h3>
<ul>
<li><a href="http://gearman.org">Gearman(公式サイト)</a></li>
<li><a href="http://www.php.net/manual/en/book.gearman.php">PHP: Gearman - Manual</a></li>
<li><a href="http://www.ibm.com/developerworks/jp/opensource/library/os-php-gearman/">Gearman を使って PHP アプリケーションのワークロードを分散する</a></li>
</ul>
