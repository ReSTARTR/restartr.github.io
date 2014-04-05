---
layout: post
status: publish
published: true
title: GoとPythonをZeroMQで繋ぐ
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 1425
wordpress_url: http://blog.restartr.com/?p=1425
date: '2013-04-26 23:34:48 +0900'
date_gmt: '2013-04-26 14:34:48 +0900'
categories:
- python
- golang
tags:
- python
- go
- golang
- zeromq
comments: []
---

最近Rubyでプロジェクトオイラーを解きながらRubyに慣れようとしてるのですが、ちょっと飽きてきたので息抜きにGoを書いたりしています。

ついでにZeroMQも試してみたかったので、GoとPythonをZeroMQで繋いでみました。

構成はこんな感じで、Python(かGo)clientを起動し、Goで動くmonitorq経由でGoのserverにつながります。PUB/SUBでmonitorからモニタリングできるのがミソです。

```ruby
                  +----------------------------------------+
                  |                                        |
  +------+        |--------------------------+     +-----+ |
  |client|--------|9001     monitorq     9002|-----|serv | |
  |(REQ) |        |(ROUTER)   9003   (DEALER)|     |(REP)| |
  +------+        |--------------------------+     +-----+ |
                  |          |(PUB)                        |
                  |          |                             |
                  |          |                             |
                  |          |(SUB)                        |
                  |      +-------+                         |
                  |      |monitor|                         |
                  |      +-------+                         |
                  +----------------------------------------+
```

_※client/server/monitorは複数たちあげるとよしなに振り分けてくれます。_

 * monitorqでfan-in/outする

```bash
$ go run queue.go monitorq
```

 * clientから"PING"を投げる(と、"PONG#<pid>"が帰る)

```bash
$ python run queue.py client
PONG#<28870>
PONG#<28870>
 :
```

 * serverから"PONG"を返す(clientからのPINGを表示)

```bash
$ go run queue.go serv
Recv: PING#<73835>
Recv: PING#<73835>
 :
```

 * monitorでリクエスト総数をモニタリング

```bash
$ go run queue.go monitor
MONITOR: IN: 2082, OUT 2082
MONITOR: IN: 2083, OUT 2083
 :
```

コードはgistにあげてます。

 * [gist](https://gist.github.com/ReSTARTR/5467656)

ZeroMQなら他にもいろんな構成がとれるので、使いどころは結構あるのではないかと。

今回、不慣れなGoで書いてみましたが、これくらいであればもわりと素直にかけるなぁという印象です。

Goで書かれたZeroMQのサンプルは以下githubリポジトリにたくさんあるので、覗いてみると色々勉強になります。

 * [zguide/examples/Go at master ? imatix/zguide](https://github.com/imatix/zguide/tree/master/examples/Go)
