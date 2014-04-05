---
layout: post
status: publish
published: true
title: '第一回 にこにこテキストマイニングに参加してきた #nicoTextMining'
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
excerpt: "第一回 にこにこテキストマイニングに参加してきました。\r\n\r\n  * <a href=\"http://atnd.org/events/12264\">第1回
  にこにこテキストマイニング勉強会</a>\r\n\r\n主催は@NLP_PRMさんと@toilet_luhchさん。会場はオラクル青山センターさん。\r\n懇親会と二次会にも参加させていただき、皆様の豊富な知識に圧倒されて参りました。精進しようと思います。いろいろな話しをありがとうございました。\r\n\r\n当日の内容はTogetterにまとめられています。\r\n\r\n
  \ * <a href=\"http://togetter.com/li/102922\">Togetter - 「第１回 にこにこテキストマイニング勉強会 #nicoTextMining
  #1</a>\r\n\r\n以下、勉強会の資料と私的メモです。\r\n\r\n\r\n"
wordpress_id: 667
wordpress_url: http://blog.restartr.com/?p=667
date: '2011-02-20 09:30:52 +0900'
date_gmt: '2011-02-20 00:30:52 +0900'
categories:
- データマイニング
tags:
- nlp
- nicoTextMining
- 勉強会
comments: []
---
第一回 にこにこテキストマイニングに参加してきました。

  * <a href="http://atnd.org/events/12264">第1回 にこにこテキストマイニング勉強会</a>

主催は@NLP_PRMさんと@toilet_luhchさん。会場はオラクル青山センターさん。
懇親会と二次会にも参加させていただき、皆様の豊富な知識に圧倒されて参りました。精進しようと思います。いろいろな話しをありがとうございました。

当日の内容はTogetterにまとめられています。

  * <a href="http://togetter.com/li/102922">Togetter - 「第１回 にこにこテキストマイニング勉強会 #nicoTextMining #1</a>

以下、勉強会の資料と私的メモです。

<a id="more"></a><a id="more-667"></a>

### [@AntiBayesian](<http://twitter.com/#!/AntiBayesian>) : 「テキストマイニングの歩き方」
発表資料：[テキストマイニングの歩き方](<http://www24.atpages.jp/antibayesian/up/src/up0006.pdf>)

「技術云々ではなく、どのようにテキストマイニングを活かすか」、という今回の勉強会の趣旨に沿った発表内容。ナレッジの自動蓄積のために、Wikiとかを使うのではなく日報メールを解析するというのは面白いですね。情報共有ツールの導入ってほとんどが失敗に終わる気がします。それよりは慣れたフォーマットを活かす方向に考えてみる、と。

 * アンケート
  * 封書解答で20%の回答率
  * 個人情報を求めると10%の回答率
 * 言語処理における様々な解析
   * 押さえるべきところ
    * 形態素解析
    * 構文解析

### [@toilet_lunch](<http://twitter.com/#!/toilet_lunch>) : 「ゆるふわテキストマイニングをしてみよう」
発表資料：
[ゆるふわテキストマイニングをしてみよう](<http://toilet-lunch.sakura.ne.jp/nicoTextMining01.pdf>)

「ゆるふわ」というよりはけっこうガチな内容。私はNLPを少しかじったことがあったので、なんとかひと通りの用語は理解できました。現在の自然言語処理では何ができて、何が問題になるのか、というのがよくわかりました。

   * 評判分析ツール
      * [http://toilet-lunch.shisobu.in/search.cgi](<http://toilet-lunch.shisobu.in/search.cgi>)
      * 一日でつくった
      * 評価表現（ポジティブ、ネガティブの二種）
      * 形態素解析
         * 辞書とマッチさせる
         * 活用語幹を用いる
         * 単語感情極性対応表
      * 精度問題
         * ジャンルごとにネガポジが反転する場合も（「薄い」とか）
      * クリーニング
      * 言語の困難さ
         * 否定表現
            * 「わからない訳でもなくない？」
         * 未知語
            * ヤバい、素で
         * 助詞の省略
         * 複合表現
         * 表記ゆれ
         * 複数評価の混在
            * 一文に含まれる結論が読みにくい
      * 「テキストマイニング」の定義があいまい
         * 目的によって手法が異なる
            * 目的を決め手手法を選ぶこと。
      * QA
         * 名詞より形容詞のほうが未知語になりにくい
         * 皮肉とか大変。

### [@langstat](<http://twitter.com/#!/langstat>) ： 「コピー＆ペーストのみで始めるテキストマイニング超入門」

<div style="width:425px" id="__ss_6973454"><strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/langstat/nicoteki1" title="Nicoteki_1">Nicoteki_1</a></strong><object id="__sse6973454" width="425" height="355"><param name="movie" value="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=nicoteki1-110218081836-phpapp02&stripped_title=nicoteki1&userName=langstat" /><param name="allowFullScreen" value="true"/><param name="allowScriptAccess" value="always"/><embed name="__sse6973454" src="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=nicoteki1-110218081836-phpapp02&stripped_title=nicoteki1&userName=langstat" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="425" height="355"></embed></object>
<div style="padding:5px 0 12px">View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/langstat">Yuichiro Kobayashi</a>.</div>
</div>
自然言語処理における一連の解析を、無料のWebサービスを使ってコピペでやってみよう、というもの。
これらのツールがひと通り用意されていることに驚き。ただし、すべて統合したものはないようなので、ぜひ欲しいなと思います。（※APIとかあれば良いのですが…)

   * 無料の解析用テキスト
      * 青空文庫
      * 首相のスピーチテキストとかパブリックドメインのもの
   * 用例検索
   * 日本語形態素解析
      * 形態素解析
          * [日本語形態素解析](<http://cgi.geocities.jp/ydevnet/sample/jlp/sample2/ma_sample.php>)
          * [Morphological Analyzer - Language Grid Playground](<http://www.langrid.org/playground/morphological-analyzer.html>)
              * Mecab/ChaSenとかで試せる
      * 構文解析
          * [Dependency Parser - Language Grid Playground](<http://www.langrid.org/playground/dependency-parser.html>)
          * 構文解析器
              * [CaboCha](<http://chasen.org/~taku/software/cabocha/>)
              * [KNP](<http://www-lab25.kuee.kyoto-u.ac.jp/nl-resource/knp.html>)：格文法
                  * どちらがよいかは好みの問題
      * [日本語文章の頻度分析](<http://hp.vector.co.jp/authors/VA035840/vba/vbafreq.htm>)
         * VBAツール
         * Webサービスではない。
      * [多機能 WEB 計算機](<http://aoki2.si.gunma-u.ac.jp/calculator/>)

### [@gepuro](<http://twitter.com/#!/gepuro>) ： 「初めてのnltk」

<div style="width:425px" id="__ss_6979340"><strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/gepuro/nltk-for-biginer-6979340" title="Nltk for biginer">Nltk for biginer</a></strong><object id="__sse6979340" width="425" height="355"><param name="movie" value="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=nltkforbiginer-110218204954-phpapp01&stripped_title=nltk-for-biginer-6979340&userName=gepuro" /><param name="allowFullScreen" value="true"/><param name="allowScriptAccess" value="always"/><embed name="__sse6979340" src="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=nltkforbiginer-110218204954-phpapp01&stripped_title=nltk-for-biginer-6979340&userName=gepuro" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="425" height="355"></embed></object>
<div style="padding:5px 0 12px">View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/gepuro">gepuro</a>.</div>
</div>
学部二年生の発表。
nltkとpython-twitterをつかったテキストマイニング。つまづきにもめげず、テキストマイニングをしてみたようです。Tweetもしたんですけど、挫折も含めて発表するのは、これからはじめてみたい方にとって有益な情報だと思いました。同じつまづきをしないためにも。

### [@bob3bob3](<http://twitter.com/#!/bob3bob3>) ： 「アンケート自由回答のテキストマイニング事例」
発表資料：
  * [「楽しい食事」ってどんな食事？
? Text Mining Studio を用いた自由回答の分析事例 ?](<http://www.ikic.co.jp/service/pdf/marketing_6.pdf>)

テキストマイニングをどう活用して何を達成するのか、というとても具体的な活用事例でした。私の場合、仕事がWebサービスなのでそっちに興味が偏りがちですが、こういう実生活における分析にも興味を頂きました。アンケートという自由回答形式だからこそ得られるものもあるんですね。

   * 商用テキストマイニングツール
    * 一番安いワードマイナーでも３０万する。
    * 野村総研のツールが６割のシェア。
    * 単語出現頻度分析
      * 表記の違いは辞書つくる
         * ゴールデンウィークとGWなど
      * 名詞、形容詞、動詞に絞る
   * 特徴語分析
      * 補完類似度
   * コレスポンデンス分析
   * QA1
      * コレスポンデンス分析のグルーピングが困難な場合はグルーピングしない
      * 今回はうまくいった
   * QA2
      * あとで「状況」をグループ化しにくい
         * アンケートなら質問を工夫する「〜な時はどうですか？」
         * ブログとかあとから集めて分析する場合は難しい。
   * QA3
      * 女性より男性のほうがアンケートに含まれる単語数が多い傾向
         * グラフで見ると女性のほうが全体的に上回る（単語出現頻度の分析において）
   * QA4
      * ツールのよしあし
         * TRUE TELLERは完成されているが、機能は限定されている。

### 関連エントリ
 * [第１回 にこにこテキストマイニング勉強会 ( #nicoTextMining #1) に参加してきた - hamadakoichi blog](<http://d.hatena.ne.jp/hamadakoichi/20110219/p1>)
 * [第1回にこにこテキストマイニング勉強会に参加しました #nicotextmining - nokunoの日記](<http://d.hatena.ne.jp/nokuno/20110220/1298102734>)
 * [第1回 にこにこテキストマイニング勉強会（#nicoTextMining）に参加してきた - yokkunsの日記](<http://d.hatena.ne.jp/yokkuns/20110219/1298102645>)
 * [にこテキ #1 - コーパスいぢり ?langstatの研究日誌?](<http://d.hatena.ne.jp/langstat/20110219/p1>)

みなさん日記を書くの早すぎです…勉強会が終わった、と思ったらもうエントリされていましたｗ
そのスピード感、見習いたいです。

### 余談
今回からTwitter名刺を用意していきました。Twitterアイコンがプリントされているので、初めてお会いする方もアイコンは見たことあると言って頂けたのが良かったです。勉強会には、会社の名詞よりTwitter名刺が重要ですね。

