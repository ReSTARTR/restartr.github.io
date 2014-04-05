---
layout: post
status: publish
published: true
title: '第二回 #Playframework 勉強会 in Tokyo #play_ja に行ってきた'
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 1073
wordpress_url: http://blog.restartr.com/?p=1073
date: '2011-10-10 15:59:13 +0900'
date_gmt: '2011-10-10 06:59:13 +0900'
categories:
- Scala
tags: []
comments: []
---
 * <a href="http://atnd.org/events/19107">第二回 #Playframework 勉強会 in Tokyo #play_ja</a>

<a href="http://atnd.org/events/17724">第一回</a>は大阪開催だったのとそもそも開催を知らなくて参加できませんでしたが、第二回は有難いことに休日に東京で開催されたので行ってきました。運営の皆様、参加者の皆様、懇親会でお話させて頂いた皆様、どうも有難うございました。

### 勉強会のまとめ記事

下記ブログにありますのでそちらをどうぞ。

 * [第二回 Playframework 勉強会 in Tokyo やりました #play_ja - 複雑系スパゲティソース(はてな版)](http://d.hatena.ne.jp/ikeike443/20111009/p1)
  * まとめ記事へのリンクが最後にあります。
 * [Playframework勉強会#2まとめ（スライド）](http://ponta027.blogspot.com/2011/10/playframework.html)
  * 発表資料をまとめてあります。

なのでここでは、全体的な話しではなく関心の強いところに関してのみ書こうと思います。

ただの感想文です。

### Play!の今とこれから

Play!がどのような分野で使われ、どのように変化していくのかが今の大きな関心事であり、今回の参加理由でした。

今回の発表を聞いていると、Java界隈の救世主（候補）的な位置づけとして期待されているという段階なのでしょう。

主催者の@<a href="https://twitter.com/ikeike443">ikeike443</a>さんの会社のシャノンさんでは実際業務でPlay!を使われていたり、@<a href="https://twitter.com/genki_">genki_</a>さんは今まさに<a href="http://harp.ruru.ne.jp/sol/play/play2.pdf">SI案件で業務アプリケーションにPlay!を導入しようとしている</a>ところだそうで。

### Play!2.x系による変化

ただ、Play!が今後2.x系でScalaベースでの開発に切り替わるので、それによって今の勢いがどう変わっていくのでしょうか。JavaベースのPlay!1.xにScalaユーザーを引き込むのと、ScalaベースのPlay!2.xにJavaユーザーを引きこむのでは、大きく状況が変わってくると思います。自分はScalaユーザーなのでこの動きは非常に嬉しいですが、もしかしたら勢いが減速してしまうのではとちょっと不安になったり。

（プラグインのサポートがどちらか一方の言語に限定されていて、結局導入を見送るなんてこともあると思います。）

### Play!の外部環境

とはいえ、外部環境としてはPlay!のサポートPassSがちょくちょく出てきているので当分は勢いが衰えることはないと思います。単体でサーバーとして動作させることが可能なだけでなく、war化も可能なのでTomcatやJettyに載っけることができる環境なら動かせてしまいます。とくに@<a href="http://twitter.com/hagikuratakeshi">hagikuratakeshi</a>さんや@<a href="https://twitter.com/mitsuhiro">mitsuhiro</a>さんが取り上げていたようにHerokuのPlay!サポートによって趣味プログラミングとして手を出しやすくなってますし。

### ScalaのフレームワークとしてのPlay!

Scala界隈ではUnfilteredやBlueEyesのような積極的にScalaの機能を利用したFWが注目されています。（少なくとも私のTwitterのTL上では…)ただし、日本語ベースのScalaのフレームワークのリファレンスや関連記事はまだまだ少ないのが現状です。

Play!の場合は翻訳も積極的に行われていますし、勉強会に100人近く参加するような日本のPlay!コミュニティの存在は正直無視できないと思います。今後のPlay!界隈の動向に注目です。

