---

# Spec-ulation

- https://www.youtube.com/watch?v=oyLBGkS5ICk
- clj-nakano #3 2018/1/16
- 株式会社シグニファイア代表 中村研二 (github: k2n, twitter: @k2nakamura)

---

## 発表者について

- 93-00年 野村総合研究所、00-16年 米国スタートアップ数社で勤務
- 15年、200億円規模の米国証券バックオフィスシステムにClojureを適用
    - compojure-api, core.async, aleph, manifold, gloss, mysql, mongo db, docker, AWS

---

## 発表者について

- 16年、Clojure+Docker+Micro Services+AWSでビスポーク開発を提供する株式会社シグニファイアを設立
- 16年、たばこ卸売業向けEコマースとたばこ税申告システム
    - AWS Lambda, Incanter, compojure-api, clara-rules, reagent, postgresql, docker, rancher, AWS
- 17年、グローバル法律事務所紹介ネットワークシステムでLegalWeek Innovation Awardsを受賞
    - reagent, re-frame, compojure-api, elastic search, postgresql, docker, rancher, AWS

---

## Spec-ulation

- もう一度、このカンファレンスに来ることができて嬉しい。古い友人、新しい人達。みんな嬉しそうに見える。とてもいいことだ。

---

- コミュニティが前向きであることの重要性は繰り返して述べる価値がある。新しいものを試したり、今までとはちがった方法を試そうとする楽観的か、あるいはクレイジーな人がたくさんいる。また同じように楽観的/クレイジーな人の手助けもしてくれている。 

---

- この講演をspec-ulation（考察）と名付けた。実際はこれから怒りをぶちまけるのだが、それを覆い隠すためのタイトルだ。

---

## This is not a talk about spec

- Specについての講演やワークショップが本カンファレンス内でいくつか開催されているが、この講演はSpecそのものについての話ではないし、Specのチュートリアルでも、Specについての技術的な解説でもない。

---

- これは、Specが何のために作られたかについての話である。というのも、みなさんがSpecについて学んだり、Specでこれができる、あれができるという話を聞いて得た内容は、Specを構築するための基礎固めであったり、目立つ特徴についての話であり、これから述べる2つの点を意識してデザインされていることが明白かどうか確信が持てないからである。

---

- Specは、誰かが他の人に渡すことができるものであり、受け取った人が使えるものである。重要な点は、「使える」というのはポジティブな意味であり、従わなければいけないルールではない、ということである。Stuart Hallowayが、ドキュメントがなかったり不足しているコードをみて、どんなmapなのか、どんなキーを持つべきなのか困惑した経験について語っていたが、これはそれについての話である。

---

- もう一点は、最初の点と同様、ユーザーであるあなたが何か間違ったことをしているということではなく、サービスを共有する側の自分が、コードの振る舞いについて約束をするということである。ここでの意味は、将来に渡ってその約束したものを取り上げたりしない、ということである。この点を今日は強調していきたい。

---

## Change

- Specは「変更」というより大きな問題の一部と言える。

---

- Specはこうしなさい、ああしなさいという指示を与えるものと見ることもできるが、実際は、後で変更できるかどうかについて表しているものなのだ。

---

- Specの大部分は物事を後で変更できるかどうかについてなのである。

---

- 今日、「変更」という言葉を何度も繰り返し使った会話をした。「変更」という単語はソフトウェア開発でおこる様々な事象をカバーしている。

---

- 答えを出さなければいけない疑問は、それが「物事」なのか、そしてそれがもし「物事」であるならば、ソフトウェアのライフサイクルの中で、我々が必要としているものであるのか、という点である。

---

## 'Change'

- 今日の話の中でまだ出てきてなかったのがびっくりだが、お約束の辞書の定義だ。

---

- 誰もが私が講演をする前に明らかに当たり前だったり、間違ったことを言わないようにWikipediaを見ておけというが、私は辞書に当たることにしている。

---

- 「変化」の辞書における定義は循環している。定義に使われている2つの言葉の片方に、"change"が含まれているからだ。

---

- 語源は、交換、物々交換である。ドイツ風ボードゲームで遊んだことのある人は何人いますか？いないの？牛を小麦に交換して、小麦を材木に、材木を石に、という具合で、おわかりのように中世ではこの方法によって成功を収めていたわけである。

---

- これは同一の場所で起こる変形、変質ではない。ものを交換しているのである。

---

- 相手の許可や協力なしに交換しようとしたら？ 人からものを盗ったことになる。すくなくとも良いことではない。ただ現実問題として、自分のほうで物事が変わっていくことがある。

---

- 依存関係にまつわる問題を調べたことのある人はいる？それを楽しんだ人はどのくらい？いないよね！

---

- 何ができるだろう？ソフトウェアをイミュータブルにすることはできない。どうやって前に進むことができるだろう？「変化」の代わりになる言葉を探したいのだが、どうやって、利用者が我慢できる範囲でソフトウェアをより良いものに、明日には違うものにしていくことができるのだろう？少なくとも我々が改善したときに、どうやったら正しく変更できるかみんなが理解できるようにすることができるだろう？

---

## Dependencies

- どうやって変更するか知っているよね？我々はmavenやmavenを駆動するものを使ってライブラリなどの成果物を取得している。自分はあるバージョンのライブラリA,B,Cを使いたい。しかしライブラリは、自分が動作するためにはこのバージョンのライブラリXが必要で、ライブラリYも必要である。ライブラリBは違うバージョンのライブラリYが必要で、ライブラリCははライブラリZを必要とする。

---

- ここでライブラリYのバージョン2.1と2.4の間に矛盾が生じる。スライドが見えるかどうかわからないが、矛盾があると想像してほしい。Mavenは通常、新しいバージョンである2.4を自動的に選択して動作するようにするルールがある。この木構造は、直接・間接の依存性を表しており、これがアプリケーションを動作させるのに必要な情報である。正しい？ 誘導された質問にYesと答えるような間抜けではないよね？

--- 

## But...

- まず、アーティファクト(訳注：Jarなど）はこのレベルで何も使っていない。ライブラリは他のライブラリをこのレベルでは使っていない。アーティファクトは実際のところ何もしていない。単なるパッケージにすぎない。アーティファクトは他のアーティファクトを使っていない。様々な理由でアーティファクトのリストを持っているだけだ。この点については後ほど触れる。

---

- もう一つの点は、少なくともClojureには、このインフラストラクチャを利用する他の言語のほとんどにも、アーティファクトについてのコードは存在しない、ということである。 

---

## Dependencies Redux

- それでは、アプリケーションが実際に必要としているものはなんだろうか？

---

- この問題をもう一度見てみる。今回は少し拡張してある。今回はアーティファクトの中身もみていく。

---

- 何が見える？私があなたにあげるJarの中には、Clojureであれば名前空間, Javaであればパッケージがある。本質的に両者は同じものである。

---

- 実際、我々のアプリケーションも同様に分解することができる。アプリの中には自分で書いた名前空間がいくつかあって、それが、他の名前空間を参照する。

---

- app.ralph名前空間はA.rickey名前空間をrequireし、app.ralph名前空間はc.fredもrequireし、app.trixieは、B.lucyをrequireする。これらはコードの中で見える。これはいいことである。

---

- A.rickeyはY.barneyを必要とし...誰か他の名前があったんだけど、今朝Paulaが違うテレビ番組から名前を拾ってきたから... Barney, Willmaなど。視力がいい人ならこれが真実だとわかるよね？実際の名前空間が他の名前空間やパッケージをインポートしていて、それが実際にコードの中にある。実際のコネクションだ。いいよね？

---

- 視力がいい人なら、この図を見るだけでわかることがあるよね？我々のアプリケーションはライブラリX、Yは必要としているが、Zは必要ない。EthelだけがZを使っていて、アプリはEthelを使っていない。

---

## But...

- 実際のところ、これは前と同じことだ。名前空間はコードじゃない。何もしていない。requireは影響を及ぼす働きがあるからrequireを呼べば副作用は起こるけど、それは今おいておくとすれば、名前空間の宣言自体は何も達成していない。

---

- コードの中で依存関係が見えるという点は好ましいけれど、ここには別のイライラさせる問題があるよね。どうやってある名前空間がどのアーティファクトに含まれているのかわかるのだろう？どのjarに含まれているのか？どのJarにFredが含まれているか教えてくれるような人はいない。これが問題だ。

---

## Dependency Truth (code)

- 真実はどこか、真実を見つけるにはさらに良い視力を持っていなければならない。

---

- 中をさらに開けて、Ralphの中を見てみよう。Ralphは関数を持っている。関数fooがapp.ralphの中にある。

---

- ここに名前空間の美点がある。新しい名前を考えつくのがめんどくさくなったので、全ての名前空間の中の関数には同じfoo, barと名付けたが、衝突することはない。素晴らしい！

---

- ralph.fooはrickey.fooを呼び、rickey.fooはbarney.fooを呼び、rickey.barはfred.barを呼んでいる。以下同様。

---

- これが実際に呼ばれているコードで、実際の依存関係である。そして目で見ることもできる。あるコードを実行するためには他のコードを必要とする。これが真実だ。これは明確になっている。

---

- ただ、紫色の凡例はみることができない。ralph.fooは何をrickey.fooに渡すのか？時間の経過とともに変わるかもしれない。何がrickey.fooからralph.fooに返されるのか？これも見ることができないし、時間とともに変わる可能性がある。非常に微妙な違いとなる。特にmapが返されるときには。

---

- 他に分かることは？ライブラリXは不要、ということだ。ralph.fooはrickey.fooを呼んでいるが、barは呼んでいない。rickey.barだけがライブラリXを必要としている。だから、これは既にそんなに上手くいっていない。依存関係の木構造は、実際に必要としているものを表していない。

---

- 図の一番下で起こっていることは、あまり詳細には触れないが、内部呼び出しもあるという点である。ライブラリYの内側でbarney.barはbetty.fooを呼び出している。この2つの関数はマッチしていなければならないが、外部からそれを木構造を通して知ることができない。

---

- これが、ライブラリ全体を取り込むアプローチの「利点」である。必要な分より遥かに多くのコードを取り込まなければいけないが、マッチしなければいけないものを取り込むことができる。

---

## Do deps Force Versioning?

- これは素晴らしいとは言えないが、問題にはならないはずである。その理由の一つは、セマンティック・バージョンの慣習があるからである。

---

- このセマンテック・バージョンのスペックでは、（このスペック自体にバージョンがあるが）、どのような変更があったか、時がたつにつれ、大量のdiffを追っていかなければならない。大部分は自分がgitの使い方が分かっていないせいかもしれないが、比較したい2つの物事の間の違いを要約しているものがない。

---

- しかし、それはバージョン化されている。もちろんバージョン化を始めるときには、メジャーバージョンがあり、それが変わっていない場合は今までと同様に動作するはずというルールがある。

---

- しかし、これが実際に起こっていることだろうか？末端のライブラリをアプリから認識させるために依存関係のバージョンを上げたことのある人はいますか？みんないつかどこかでやったことあるでしょ？この質問にはYesと答えていいんだよ！

---

- これが現実におこっていることだ。我々はいつもバージョンを上げ続けている。使っているライブラリのどこかが改善されて、でも自分のアプリのコードには無関係で修正する必要がない場合であっても、新しい依存関係を定義し、我々の名前を変更し、我々のアプリに依存しているものの名前を変え、ということをしている。

---

- だからこの主張は嘘である。バージョンアップの連鎖反応はしょっちゅう起こっている。このヤシの木みたいな依存関係の木構造を通してコミュニケーションしようとしているのである。

---

- 私はこれをレベル違反と呼びたい。これからレベルについて話そう。

---

## Names, Levels, Scopes, Contexts

- では、何が実際に起こっているのか？

---

- ここには層になったいくつかの問題がある。

---

- 一番下の層から始めると、関数呼び出しの真実である。我々は関数が他の関数を名前で呼び出していることを知っている。名前空間宣言は、コードローディングを別にすれば、単に別名を付けているだけである。名前空間は分析するのに十分な情報を与えている。fooという関数を参照したときに、rickey.fooについて指していることが分かる。だからrickey.fooについて知る必要がある、という具合に。

---

- 実際のrequireは、実行に必要なコンテキストを与えていて、その中でコードが動く。rickey.fooのためのコードが利用可能になっている。requireはこういう仕事をしている。もちろん他にもたくさん、あなたが呼び出さないコードも利用可能になるが、我々が必要としているものはカバーされていることが分かっている。だからrequireをコードの中に記述するのだ。これによってコンテクストが作り出される。

---

- 次に、アーティファクトのレベルに上がると、同様のことが起こっている。POM(mavenのproject object model)は「私は他のライブラリが必要です」と言っていて、requireが成功するためのコンテクストを作っている。「これは他のライブラリを必要としているので、この名前空間が利用可能になる」というのは、誰か外部の人が、「もしこのjarを使えば、名前空間fredがそこにあるはず」といっているからである。

---

- しかし、根源的に破綻している問題はこの最後のレベルにある。外部の人がこうしろ、といっている部分である。この部分は、完全にマジックである。コードの中にはこれに関する記述が全くない。この点についてはまたのちほど触れる。

---

- これで皆さんレベルについてわかりましたね？関数は関数を呼び、名前空間はアーティファクトを必要とする。

---

## Basis

- では、なんで我々はこんなことをしているのだろう？これが我々に何をしてくれているのだろう。これが本質的に酷いものだとはおもわないけれど、何が起こっているか紐解いていくことにする。

---

- なぜ我々は物事を依存関係やpomやプロジェクトファイルに入れるのだろう？

---

- ひとつには我々がコーディングに取り組んでいるとき、コードが必要だからだ。我々はライブラリではなく、アプリケーションを書いている。依存関係管理ツールを手に入れる前は、自分でJarをダウンロードし、classpathを記述していたものだ。

---

- 正直なところ、今の依存関係ツールを使っている状況は昔より悪くなっているんじゃないのか？だって、昔は少なくとも自分でclasspathのリストを記述して、中に何が入っているのか分かっていた。実体としての感触があったし、信頼を置くことができた。

---

- 次の点は、「我々はライブラリA,B,Cが必要だ」というとmavenが間接依存関係を通してそれらに必要なライブラリX,Y,Zを引っ張ってきてくれる。これは手間を省いてくれる点である。

---

- 我々が依存関係やpomを使って行っている他のことは、立場を反対にして、我々が依存している関係を外部に伝播することである。それによりmavenが我々のアーティファクトを扱えるようになる。依存関係を渡りあることができるようにしてくれるので、他の人が我々のアーティファクトを使えるようになる。

---

- これは、我々がライブラリを書いているときの話である。アプリケーションを使うのは幾分違いがあり、これについては後ほど話すが、これは特に我々がライブラリを書いているときの話で、A,B,Cを我々が書いているときに自分のPOMにあるXを使っており、我々のライブラリを使う人が我々のライブラリに加えてXも取得してコードが動作できるようにする、これによりmavenが提供する便利な機能が使えるようになる。これが我々のしていることだ。

---

- 人々が想像している別の点は、依存関係をPOMファイルに記述することで、なんらかの整合性の約束を与えているということだ。我々がこれらの依存関係にたいして自分のライブラリをテストしているという約束である。自分はこの点は意味がないと思っている。なぜならテストを実行してライブラリに対する検証ができている可能性は小さいからだ。これについても後述する。自分はこの点は利点だとは思っていない。人々はそう思っているが、事実ではない。 

---

- しかし、問題は、粒度が粗いという点である。POM内に記述されている物事は実際に起こっていることを表していない。それらは単にコンテクストを生成しているだけである。

--- 

## (Ex)changes in Software

- 変化についてどのように語るかについて話したい。というのも、物事の変化についてはすでに述べた。バージョンを変更したり、新しいバージョンを取得したり。しかし、それを解きほぐしていきたい。

---

- 全ての変化は、この2つの言葉に集約できると考えている。

---

- ライブラリを作っているときは、自分のライブラリのユーザー向けに必要条件を定義している。何を必要としているか？

---

- 関数を書いているときは、必要条件は引数である。

---

- 名前空間のときは、何が必要か？名前だけである。名前空間は辞書を引くようなものである。ユーザーが名前を渡したら、その名前空間にある変数や関数が返される。

---

- もう一段階レベルを上げると、アーティファクトは何を要求しているのか？Jarを渡したら、何を期待する？名前空間と同様、内容についての情報である。実際のファイルパスなどを意識することなく、classファイルやcljファイルを与えられた名前にもとづいて返す。

---

- 名前空間とアーティファクトは本質的に、名前を渡され、実体を返す関数である。名前空間は名前を渡され変数や関数を返す関数、アーティファクトは名前を渡され名前空間やJavaパッケージを返す関数である。

---

- ひっくり返すこともできる。このライブラリが提供しているものは何か。関数は値を返す。もし必要とするあるものを渡されたら、ある結果を提供する。

---

- もちろん、この議論をサービスやプロシージャのレベルまで広げたい。もし外部に影響をあたえるものを提供しているのなら、例えばもしあなたがこれを呼び出したら、データがデータベースに格納されるとか、Eメールを送るとかである。

---

- 名前空間が提供しているものは何か？単なる検索である。

---

- アーティファクトは何を提供しているのか？名前空間同様、名前空間やパッケージの検索である。

---

- これが物事を交換しているやり方である。

---

## Growing Your Software

- ここまできてはじめて、様々な変更の種類をを分類して見ることができるようになった。その方法がこちらである。

---

- 最初は、あなたのソフトウェアの成長についてである。ソフトウェアがより多くのことをできるようにすることである。

---

- 最初の点は「積み重ねによる成長」である。「私があなたにより多くのものを提供したい。7を渡してくれたら42を返していたが、今度は42に加えて小麦も返す」といったことである。

---

- ここで「提供する」と「要求する」という言葉は非常に限定的な意味で使っている。「より多くのものを提供する」ということは積み重ねによる成長に直結している。

---

- 次の点は、「緩和」である。以前は小麦を2つと一匹のロバを渡されたら鉄を返していたが、今はロバは不要で、小麦さえ渡してもらえれば鉄をあげる、ということである。要求しているものが減っているのである。これが、自分の側における「緩和」である。禅の精神のようなもので、必要なものが少なくなればなるほどあなたは成長する、ということである。これは、、、まあいいや。あんまり自分の考えをさらけ出すのも何だから。

---

- 次は、「直すこと」である。これも辞書で調べたが、実際に、偏執であるという意味以外に、直す、という意味もあるのだ。最後の点は、物事を直すということだ。「提供すること」「要求すること」に変化はない。正しく動くようにしたり、もっと速く処理するようにするということである。あるいは依存関係を減らすとか。いずれにせよ、要求するもの、提供するものには影響を与えない。


---

## Breaking Your Software

- 「変更」という言葉を大まかな意味で使った場合、時としてこの種類の変更を指す場合もある。ソフトウェアを壊す変更である。どうやったらソフトウェアを壊すことができるのか。

---

- より多くを要求した場合。「小麦2つとロバ？足りないな、ゴールドとルビーも必要。そうしたら鉄をあげる」この壊すことを表すのに、「互換性がない」とか、もっと大げさな言葉を使うけれども、明らかに我々は「より多くを要求する」といった、地に足の着いた言葉を使うべきである。もしより多くのものを要求したり、より少ないものを与えるようになったため、必要なものが得られないのであれば、動作しないということである。壊れている、ということである。

---

- これの裏側は、「提供するものが減る」ということである。かつては鉄を返していたのに、今は錫を返す。あなたが建てるビルが壊れないといいね！「提供するものが減る」ということは、以前約束していたより少ないものしか返さない、ということである。

---

- もう一つの、「なんでこんなことをするのか理解に苦しむ」たぐいの変更は、単に変更することである。かつて「物々交換」と呼んでいたことを、何か他のことを表すために変えてしまうということである。典型的な例は、同音異義語である。drawという単語は、「絵を描く」に使うこともできるし、「銃を抜く」にも使うことができる。ここでの意味は銃撃ということになる。これは完全なやり直しである。

---

## Change is Not a Thing

- ここで言いたいのは、「変更は物事ではない」ということである。我々は「変更したよ」というべきではない。それでは何も伝えたことになっていない。

---

- (Growing Your Softwareのスライドに戻して）ここに書かれていることは素晴らしい。渡さなければいけないものは減るし、新たにゴールドを手に入れることもできる。

---

- (Breaking Your Softwareのスライドに移って）ここでは本当に怒っている。これらは全然良いことではない。だから変更したというだけでは全然役に立っていない。

---

− 成長しているか壊しているかのどちらかで語る必要があるのだ。成長か破壊か。

---

- Specの設計目標の一つは、我々が成長させようと思っているのに何かを間違って破壊したときに我々が理解する、もしかしたらプログラム的に検知するのを手助けすることにある。

---

- 「成長」についてはこのすぐ後に論じるが、ここは重要なポイントである。だからspecはmapに対して集合論を用い、シーケンスに対して正規表現文法を使っているのである。それはこれらが既に成長指向の互換性を決定するためのロジックを持っているからである。これらには既に数学的理論が存在している。

---

- だからこれはクルマのセールスマンがするような約束ではなく、まだspecのために互換性を決定するためのプログラムは存在しないが、そのようなプログラムを走らせて知ることができるようにspecは設計されているのである。

---

- これはSpecにバージョニングの考えを持ち込まない限り関数のような小さなスケールで役に立つ。バージョン2のspecではゴールドを渡さなければいけない、などという変更をしない限りは。そういうことをしてはいけない！

---

## Recognize Collections

- それでは大局的な変更はどうだろうか。鍵となるのはコレクションのときに認識すべきなのは、たった2つのルールしかないということである。もしインデックス付きのコレクションを含む、単なるコレクションの場合には、名前を渡して値を受け取るインデックス付きコレクションもコレクションであるが、その場合には操作は2つしかない。コレクションへの追加か、コレクションからの削除である。

---

- 追加は成長、積み重ねによる成長であり、削除はいつでも破壊である。

---

- ここで重要なのはソフトウェアをみるときにこれらのコレクションを見る必要があるということである。

---

- なぜなら我々にいつも起こる別の問題は、変化を度々異なるレベルで合成しているということである。そしてバージョニングシステムはそれを奨励しているのである。

---

- 名前空間は変数のコレクションに過ぎない。アーティファクトは名前空間やパッケージのコレクションに過ぎない。我々はそれを見る必要がある。

---

- Specは集合論をMapのために用いている。キーの意味について記述する余地を与えていない。これについても同じことである。

---

- Mapはキーのコレクションであって、キーの内側にあるものではない。もし自分が帽子をかぶったとしても、それが自分の家族について変化をもたらすものではない。自分の家族は覚えているし、家族の中には以前にいたのと同じ人が後になっても含まれている。帽子をかぶったときに自分の家族をバージョン化することはないにも関わらず、これと同じことをいつもしているのである。だからコレクションを認識しなければならないのである。

---

- 興味を引くことは全て木構造の末端で起こっており、それ以外は全て、追加はOK、削除は破壊という2つのルールを持つコレクションだ、ということなのだ。

---

## "Semantic" Versioning "Semantics"

- さあ、ここからいよいよ怒りをぶちまけるよ。

---

- セマンティック・バージョニング、辞書を引いてみたが、そんな定義はなかった。

---

- だって、もし辞書にバージョンがあったらどうする？これはセマンティック・バージョニングというアイデアそのものに対する根本的な問題である。

---

- ある時点まである意味を指し、その後はその意味を指さないとしたら？これがどうやって手助けになるんだ？理解不能である。

---

- それではセマンティック・バージョニングが約束していることを見ていくことにしよう。

---

- 消費者としては、パッチバージョンが変わった場合、気にする必要がない。 

---

- マイナーバージョンが変わった場合も気にする必要がない。4は3より大きい、それだけの意味しかない。こんな意見の表明があっても、嬉しくともなんともないでしょ？

---

- メジャーバージョンの部分はどうか？どういう意味か？これはお手上げ、ということだ。これは酷い、完全な破滅である。どうして動かなくなるのか何も伝えていない。

---

## Even Worse...

- 実際のところは、「あなたのアプリケーションはお手上げ状態になるかもしれないよ」と言っているのである。

---

- もし誰かから「あなたはもう八方塞がりだよ」と言われたら、「最悪だな」となるよね？もし、「あなたはもう八方塞がりかもしれないよ？」と言われたら、「何だ？？」となってしまうよね？さらに酷い！

---

- なぜか？今まで述べてきたレベルの違いがセマンティックバージョンを作った人には分かっていないからだ。全てのレベルを一緒くたにしてしまっている。

---

- どこに加えた変更なのか、なんのための変更なのか、我々が注意深く、「提供するものは増やし、要求するものは減らす」というルールにのっとって加えてきた変更を、ひとつの見苦しい変更に集約してしまっている。そこでは何が起こってもおかしくなく、「気をつけろ！」としか言うことができないのである。

---

- これが役に立つとは思えない。Stuart Hallowayが私のアイデアを横取りしてこの前に言ってしまっているが、名前を変えることもできるのである。バージョン2.0にすることは誰の手助けにもならない。

---

## Might just as well change the name

- 単に名前を変えればいいだけだろ？メジャーバージョンを変えることは誰の役にも全くたたない。

---

- それはまるで、「これから違うゲームをするけど、名前は同じね」といっているのと同じである。ルールを知っているつもりでいて席についた人は、そのゲームに負けるに違いないと予言できるよ。

---

## But...

- これは変更ではない、新しいものなのだ！

---

## Which Name?

- では、どの名前を変えれば良いのか、という疑問が湧いてくる。全てのレベルを一緒くたにしてアーティファクトのバージョンにするのは良いアイデアではないだろう。もし私が名前を変えろというのなら、この疑問に対して答えられなければならないだろう。どの名前を変えるべきなのか？

---

- もし関数の一つで要求するものが増えたとしたら全体を変えなければならないのだろうか？アーティファクト名を変えるべきなのだろうか？新しいゲームにするのか？

---

## Requiring More args? Providing Less on return?

- いや、これも同じことである。提供と要求のレベルで見ていかなければならない。さらに引数を必要としたり、既存の引数からさらに多くの情報を必要としたり、返り値の内容が減ったりした場合は、破壊であると認識したよね？

---

- 本質的には、その変更を加えた関数に対するspecに互換性がない、ということになる。もしそうなら、新しい関数がほしい。

---

- Clojureでは2つのやり方がある。同じ名前空間にとどまって、`foo2`という関数名をつける。また、もっと系統だった変更が必要な場合もある。APIの全てにもう一つ新しい引数を渡さなければいけない場合は、新しい`api2`という名前空間を定義し、中にある関数は元の名前のままにしておけばいい。

---

- いい名前を思いつくのは難しい。しかし名前空間の場合、よそからとってきた何か違うものを接頭辞として、もとのいい名前につければいいだけである。

---

- しかし、実際のところ、`foo2`を心配する必要もないと思う。そんなにしょっちゅう起こることではないから。

---

- Clojureの美点は、ローカル変数以外に`foo`と名付けらていることはない、ということだ。全てには長い名前空間の接頭辞がついているので名前衝突の危険がない。Specもこの事実を活用しているし、皆さんもこの類いの変更を加える際、それをあてにすることができる。 

---

- 我々が持っているもう一つの美点はエイリアスである。`game1`という名前空間のコードを使っていて、`game2`に名前空間が変わったとしても、コード内では`g/foo`と記述しておいて、`ns`のデコレーションを`(:require [game2 :as g])`と変えれば良いだけであるからだ。もしns名を変更しただけで立ち去ったらアプリケーションは壊れてしまうが、当然この変更を加える際には新しいものに移行するわけだから、スペックを読んだり、ドキュメントを読んだりする。もちろん名前の中には再利用されているものあるだろう。

---

- だが、自分がその変更の役目を負っている。いつこの作業をする必要があるのか？それは自分が新しいゲームをしたくなったときである。もし自分が得意な古いゲームをプレイし続けたければ、し続けることができる。古いのを削除するのに何週間でも時間をかけることができる。

---

## Providing Fewer fns/vars?

- では、もし関数を削除したい場合にはどうすればいいのか？人々がこの関数を呼び出すことに我慢ならない場合にはどうするのか？

---

- そんな関数はClojureの中にはないよ？

---

- 関数を削除したい場合は、その情報を持っているコレクションを、持っていないコレクションに変更しなければならない。それは名前空間である。だから新しい名前空間名を定義する必要がある。これをするためには、大規模なリファクタリングが必要かもしれない。一連の関数にdeprecated宣言をつけてといったことだ。ライブラリ2に移行する。

---

- しかし、ここでの最大の変更は、既存の関数には何も変更がなく、もともとあった関数の半分がいなくなっている、ということである。これほどたくさんの関数はいらないので、新しい名前をつけ、古い関数を取り除いた、ということである。これがこの場合のやり方である。

---

## Providing Fewer namespaces/packages?

- では、ひとつ上のレベルに移行しよう。

---

- もし名前空間を削除したいとしたらどうすれば良いのか？この名前空間に我慢ならないとしたら？人々はまだこの名前空間を使っていて、自分はより良い名前空間を3年前に提供していて、もう古いのを削除したい。

---

- 再び、もし何かを削除したいのなら、そのコレクションから削除しなければならない。アーティファクトIdを変えることを思いつくかもしれない。そして、それは間違いなく可能である。

---

- すぐに出てくるであろう反論は、「それがメジャーバージョンの目的なのでは？」というものだ。それは違う！セマンティック・バージョニング定義のバージョン3が出てきて、セマンティック・バージョニングの意味を完全に変更するのでなければ、メジャーバージョンを変えるということは、それを使っている人全員にとってそれが動かなくなるということであるが、実際のところ人々はセマンティック・バージョニングを信じていないので、メジャーバージョンを変えることはできないのである。

---

- セマンティック・バージョンを定義しているバージョン自体をメジャーバージョンアップできないのである。この事実が、セマンティック・バージョニングが破綻していることを示している。みんなが今使っているセマンティック・バージョンの使用法を破壊することなしにセマンティック・バージョンの定義をバージョンアップすることができないのである。

---

- 不幸なことに、セマンティック・バージョンはすでにこれについて定義してしまっている。これはセマンティックバージョンのスペックからの引用だが、「全てのレベルに渡る、いかなる後方非互換な変更」とある。

---

- これは名前空間やパッケージの削除だけがこの状況を作り出すのだが、それを売り込むことはもはやできないということである。

---

- つまりセマンティックの定義が広すぎるのである。

---

- ここで我々が抱えている問題は、先程述べたマジックである。もし私が、「これが`game2`ライブラリだ」と言ったとして、その中には`turn1`名前空間があるとする。だが、`game1`ライブラリもまた`turn1`名前空間を持っているとしたら？アーティファクトから名前空間へのマッピングはどこにあるのか？自分にはわからない。外部の人が行っていて、その人は今日ここにはいない。この情報はどこにもない。

---

- だから衝突が起こりうる。2つのJarが同じ名前空間を持っていて衝突が起こるのを経験したことがある人はどれくらいいる？楽しんだ人は？

---

- これは起こりうるし、防ぐ手段はない。解決する手段は、名前空間レベルので変更が暗示的に新しいスコープを与えていたということである。foo,barという関数名はあるが、それは`game2`名前空間の下にある。だから`game1/foo,bar`と衝突しないのである。

---

- しかし同じことをこのレベルで行うと、うまくいかない。なぜなら暗示的な変更がないからである。

---

- 対応する手段の一つは、名前空間名を変えることである。というのも通常アーティファクト名と名前空間名には何らかの関連があることが多いからだ。ライブラリ名が両方に含まれているとか。

---

- 実際のところがこれが正しい答えだと確信しているが、これを直したいと考えている。

---

## Doesn't Doing the Right Thing...

- たくさんやらなきゃならないことがある。

---

- 削除するのがいやになるよね？

---

- なぜ他の人がその関数を呼ぶことについてそんなにいらいらしなければならないのか？そんなに重要なことじゃないよね？

---

## Breaking Changes are Broken

- 破壊的変更は不完全。試みようとしないこと。

---

- 正しい方法を見つけようとしないこと。

---

- インターネットで集って、メジャーバージョンアップをすれば解決する、などと言わないこと。

---

- 最良の方法は名前変更の方法をつかって、破壊を成長的変更に変換することである。この方法で同じ目的を達成できている。

---

- いらいらさせる関数を排除することができる。なぜならそれを含まない新しい名前空間を定義したからである。

---

- この論点については明確にした。新しい機能、新しい仕事が必要な場合は新しい関数を定義する。そして古い関数を共存させる。

---

- これは非常に重要なポイントである。共存させることによって、人々は自分のペースで移行を進めることができるからである。さもなければ、心配し続けなければならない。

---

- メジャーバージョンが変わっていないのに、破壊的変更を経験したことがある人はどれくらいいる？楽しんだ？バージョンも関係ないし、どういうつもりで変更したかも関係ないし、テストでチェックしていたのか、していなかったのか、事前通知をしたのか、どんな言い訳があるのか、そういうことも関係なく、破壊的変更は間違った考えである。この共存の方法が好ましい。破壊を成長に変換するのだ。

--- 

## So Maven is Broken?

- ではMavenは壊れているのか？我々はバージョン管理にMavenを使っている。

---

- そんなことはない。間違っているのは自分たちがしていることで、実際のところ、Mavenは非常に興味深い。

---

- 第一に、MavenはMavenリポジトリに格納されているアーティファクトの変更を許していない。

---

- Mavenが壊れることはない、なぜならバージョン化されていないからだ。Maven Centralにバージョン1,600,017なんて存在する？そんなものはない。

---

- 人はMaven Centralに始終変更を加えているが、Mavenが壊れることはない。バージョン化していないから。バージョン化なんて負け犬のすることだから。

---

- Maven Centralは信頼のおける、有名なサイトだ。Maven Centralに行けば、今まで見つけることのできたものはなんでも、永遠に見つけ続けることができる。

---

- これがMaven Centralの理念だ。

---

- Maven Centralを使うときに、バージョン5062を使おうなんて言わないだろ、というか、バージョン番号が天文学的数字になってしまう。

---

- 僕は別のバージョンを使っている、Maven Centralのバージョンのためのバージョニングシステムとか、そんなものは存在しない。

---

- それにも関わらず、動いている。我々はみんな、この名前がMaven Centralの名前であると仮定し、それを共有し、これが将来に渡って動き続けるという一種平穏な気持ちを共有していることがわかっている。

---

- どうしてこううまくいくのか？

---

- 理由は単純である。これは不変的なものの積み重ねによる成長であるからだ。関数プログラマとしては、「当たり前でしょ！」という気持ちだ。当然これはうまくいく。これが我々が小規模に毎日やっていることだから。エコシステムの一番下の層でもうまくいくし、一番上の層でもうまくいっている。

---

## (insert rotten sandwich image here)

- ここでインターネットで腐ったサンドイッチの画像を検索しないように忠告しておくよ。とても不快だからね。

---

- ここに美味しそうなサンドイッチがあるとして、一番下の層に関数型プログラムがあるとして...

---

- JavaのユーザーにClojureについて話すとき、布教する中で最も難しい問題として残っているのが、どこかの時点で相手が認識していない問題をClojureは解決できるということを言わなければならない点である。その問題は可変性と常に付き合わなければないという強烈な不安感とプレッシャーで、その重荷が取り除かれるまで気づくことが出来ないという点にある。Clojureだけがそれをできる言語であるわけではないが、その重荷がとれるまで、まるで誰かが自分の足の上に毎日立っているような苦痛で、その重荷が取れて初めて足を上げてみて初めて、歩くことが簡単になったことに気づくようなものである。

---

- この感覚を下の層で我々は経験してきたように、実際Maven Centralは一番上の層で同じ感覚を味あわせてくれる。Jar-xyz.1234が以前見たものと違う、あるいはなくなっている、という心配をする必要がないからだ。Maven Centralには先程述べたようなゲームのルールがあるからである。名前が永続的な意味を持ち、変更は積み重ねによる追加によってのみ行われている。
- だが、中間層は、われわれがめちゃめちゃにしてしまっている。アーティファクトの扱い方、名前空間の扱い方、関数定義を削除したり、完全に混乱している。

---

## So SemVer is Broken?

- ここまで来たらもう驚きはないと思うが、セマンティック・バージョニングは壊れているのか？完全に壊れている。

---

- 間違っているアイデアで、できるだけ早くこの考えを捨てなければならない。というのも、4は3より大きいとか小さなレベルではなく、根本的に最大の定義、メジャーバージョンに関する定義が、どうやってソフトウェアを壊すかのレシピになっているからである。

---

- これがユーザーを八方塞がりにし、人生を難しくし、健全なソフトウェア開発を蝕んでいる原因だからである。にも関わらず、これは標準で、そのウェブページもあったりするのだ。

---

- 特定の何かを提唱するつもりはないが、4が3よりも大きいという属性と伝えられるものであればなんでも大差はない。ある種の順序性があればいい。

---

- いろいろな選択肢がある。メジャーバージョンを抜きにして、マイナーとパッチバージョンだけを考えても問題点がある。番号付けの基準が相対的である点だ。ここに6つのライブラリがあったとして、バージョン1.2, 3.7, 4.1...この中の1つは11年前のもので、別のものは昨日作られたとして、区別は全くつかない。これらの番号に関連性はない。

---

- だからといって、この時系列のバージョン方法を採用して決定性のあることをしろと言っているわけではない。というのは人々が何を見たかわからないからだ。しかし、Lamport clockのように、もう他の人が見たことのあるものしか持つことができないというロジックを使えば良いかもしれない。

---

- アーティファクト名がMaven Centralのように安定すれば、より柔軟な選択ができる。バージョン4.3が持っているよりも多くの意味を伝えて、ある種の比較可能性をもたせることができるようになる。

---

## What about Git? 

- Gitはどうだろう？

---

- JarやMavenなどのアプローチはGit以前のものだ。

---

- Gitには素晴らしい特性がある。今まで話してきたことと一致することがたくさんある。不変性を持ち、後から作ったのではない、一次的な情報である。

---

- バージョン4.3ということに何の意味があるのか？何の意味もない。

---

- 真実はいつもコードにある。現在では広く採用されている。

---

- コンテンツベースのアドレス体系は素晴らしい特性である。

---

- 先程述べたように、アーティファクト管理システムからはほとんど無視されているが、これはJarやMavenなどのせいではない。それらはGit以前から存在していたからである。

---

- しかし、Gitには難題があるのも事実である。この講演の初めで、「真実はコードの依存関係の中にある」という話をしたが、それが分かる場所はコードが管理されているところである。しかし、コードについて語る方法として、SHAが使われていて、みんなSHAのことが好きではない。SHAのユニークで偽造できないキーとしての特性は好まれているが、リポへのアクセスなしに順序は分からないし、因果関係も分からない。少なくとも4は3より大きいから、後にできた、ということは分かる。あとは読みやすさの問題だ。

---

- しかしこれを統合するやり方があると思っている。最下層から初めて上位層に向かうことでソリューションを作ることができるだろう。

---

## It's a Social Thing

- さて、これは牧師の説教ではないが、我々一人一人が改善することができると思っている。この分野においては、Clojureが完璧な過去の履歴を持っているとは言えないが。

---

- 最も重要なのは、この状況から技術開発をするだけで改善することはできないということを認識することである。

---

- Mavenについて先程述べたように、Maven自体は壊れていない。壊れているのは我々がMaven Centralの中に入れているものなのである。

---

- 違いを生み出すためには壊れているものを入れないようにしなければならないのだ。

---

- これは社会的なことで、他人のことを思いやらなければならない。

---

## Local dev vs Open dev

- これを難しくしている要因の一つはオープンソースだ。

---

- チームで働いている場合は、分散していようと一箇所にいようと、少人数である。スタンドアップミーティングがあり、非公開で、自分たち以外は使わないプロジェクトに取り組んでいる。ミーティングには全員参加していて、「間違いをしたみたいだ。本当はこの作業をするには小麦とトウモロコシが必要だった。これを呼び出す全てのコードで、トウモロコシも渡すように変更しよう。みんなそれでいい？」「大丈夫」「サリー、君は火曜日までに修正して。自分のは金曜日までに終わらせる。来週の月曜日までにはみんなが小麦とトウモロコシを渡すようにしよう。みんなこれでいい？」「OK」「さよなら」でスタンドアップミーティングは終了。

---

- 今度はインターネットに場を移す。我々にはSlackがある。一見スタンドアップミーティングのように見える。みんなログインしていて、このライブラリに取り組んでいる人が何人もいる。「このライブラリは良くない。小麦が渡されているけど、小麦とトウモロコシが必要だ。みんなはどう思う？」「僕もそう思う」その日Slackにログインしている人はみんな同意した。「小麦とトウモロコシを渡すべきだ」「OK, じゃ僕がその変更を加えるよ」gitにコミットして、GitHubにpushして、Clojarsにアーティファクトがアップロードされる。「Slackにいた人みんなに伝えたよ」非公開のチームで働いているときと同じ感覚がある。我々はオープンソースが、参加者が開かれているチームだと考えたいからだ。

---

- しかし問題は2点ある。一つはそのライブラリを書いた人が全員Slackにその日いるとは限らないこと。スタンドアップミーティングの場合は、ライブラリを書いた人も、ライブラリの変更によって影響を受ける人もみんな参加している。Slackの場合、ライブラリを書いた人はみんなオンラインかもしれない。でも影響を受ける人は？どこにいるかなんて分からない。誰も使っていない場合以外、そのライブラリの利用者が誰でどこにいるかなんて分かるわけがない。

---

- ユーザーベースは開かれていて、把握不可能である。見知らぬ人たちのことにも注意を払わなければならないのである。

---

- 現在の政治的状況でこんなことを言うのは突拍子かもしれなけれど、我々は自分たちが知らない人々のことを思いやらなければならないのだ。ソフトウェアの中でも同様である。

---

- だからオープンソースでの開発はクローズドとは違うのだ。Slackはスタンドアップミーティングではない。

---

## Coding for Growth

- では成長のためにコーディングするにはどうすればよいのか？

---

- Alex MillerとStuart HallowayはSpecについてたくさん話をした。その中で聞かれるSpecに関する最も多い質問は、「なぜmapに対して、自分が指定したキー以外が含まれることを禁止するという定義ができないのか？これができないのは腹立たしい」というものである。「これなくして、正しさをどうやってチェックすればいいのか」と。

---

- 今朝、Paulaが論理について素晴らしい講演をしたのを見たよね。ほとんどの論理システムが持っていないものは何か？実際のところ私はこれを持っている論理システムを知らないのだが、「あるもの以外が真であることは決してない」という属性である。論理システムがこの属性を持っていない理由は、一番最初に知らなかったものを後で知るようになったり、演算できるようになることができないシステムで良い論理なんて組み立てられるわけがないからだ。

---

- オープンなスペック、我々が毎日使っているMapのようなオープンなデータ構造は、原則として、自分が関知しないキーがmapに含まれていても構わないスタイルでコードを書くべきであることと一致している。

---

- これはSpecにとって決定的に重要なことである。Specは「何ができるか」についてのものであって、「できないこと」についてのものではない。明日には、小麦を牛に取り替えるかもしれない。将来変更できる柔軟性を保持していたい。とくにそれで何か気の利いたことができるかどうか試したい場合には。

---

- Specを今現在の問題をチェックするためばかりに使うことはできないことを理解すべきである。Specはそのためにあるのではない。Specに加えて更に追加してできることはあるかもしれないが、それをSpecに入れる必要はない。Specに加えて何かを遮断したり、チェックを加えたりする層を加えるのは構わない。だが、そういった要素をパブリックなSpecに盛り込むべきではない。

---

- 公開されたSpecは将来の成長に向けて記述されるべきである。さもないと袋小路に陥ってしまう。

---

- もし禁止することを認めたとしたら、2日後には彼らが構築した世界は破綻すると自信を持って言える。どこにも進める場所がないからだ。

---

- もしXができない、といったら、Xをできるようにすることは決してできない。もし後日Xをできるようにしたいのなら、新しい名前が必要だ。これは今まで行ってきたことと反対である。前に私が述べたのは、破壊的変更を加えるのなら、名前を変えなさいということである。今言っているのは、成長したいのなら、新しい名前を使え、ということである。これはまずいやり方だ。なぜなら、これによってキーを変えなければならず、キーを変えるとそのSpecを変え、そのSpecを含んでいるSpecを変更し、というように続いていくからだ。

---

- Specはこの状況が起こらないように設計されている。成長する際に連鎖的な変更を起こさなくて済む、セマンティックバージョニングとは違って。でも禁止を認めてしまうと、180度入れ替わって、この連鎖反応を起こすようになってしまう。だからSpecには禁止を含むべきではないのだ。もっと短く説明する方法もあるが、とにかく、これが理由である。

---

- 成長のためのコーディングで留意すべきもう一つの点は、知らないものがよそから渡されてくる可能性があることを常に予測しておくべきである、ということである。多くの人がMapに入っている全要素を取り出してスクリーンに表示するようなコードを書いているが、`keys`関数を使って、明示的に取り出すべきである。後になって社会保障番号がMapに含まれるようになるかもしれない。これは好ましくない。無視すべきかもしれないし、それに対して何かポリシーを持つべきかもしれない。

---

- ここで重要なのは、未知の情報が渡されてきても大丈夫なように備えておく、ということである。ただ、それを禁止すべきではない。チェッカーを走らせたりすることはできるかもしれない。だが、Specにおいては、未知の情報を受けいるれるのはOKである。

---

## What about Iterative Development? 

- では、反復的開発についてはどうか？

---

- 今までの話だと、最初から正解を出さなければいけない気がする。でもそうではない。

---

- ハンモックから降りてきて試行錯誤してみる場所がある。少しハンモックの中で考える時間を取れるといいのだけれど、とにかくコーディングをはじめて、コードをプッシュして、自分で見直したり他の人に試してもらったりして、「試したけれどこれはあんまりよくないね」といったフィードバックをもらう。

---

- これは大丈夫。まだ試行錯誤のモードであることを他人に示す必要があるだけのことだ。周りの人はそれを認識して進めればいい。もしアルファバージョンを使いたいのなら、スタンドアップミーティングに参加しなければならない。

---

- しかし、我々にはアーティファクトのリリースより粒度の細かい公開用のツールが必要である。API全体をアルファと呼ぶのは問題がある。なぜなら、アルファ版のステータスから抜け出るときの障壁が高くなるからである。ここも我々が何かもっと明確なやり方を考えつくことのできる分野である。

---

- だが、バージョンを0.0.967みたいにしろ、といっているわけではない。どこかの時点でユーザーが現れてくるわけだし、そのときに1.0.0にするかどうかはそのプロジェクト次第である。

---

- しかし、どのような約束をするかについてはもっとはっきりさせる必要があると思う。「ルビーを渡してくれたら魔法の剣を渡している」という事実を発見したとしても、「そんなことをすると言ったことはないよ」のような状況を避けるため。

--- 

## The Only Truth is Runtime

- （どの辺りまで来たのかな？）

---

- コードから始まって、アーティファクトができて、そこにはマジックのような飛躍がある。だが、ライブラリを作るとき、もう一つの問題がある。

---

- それは、POMがそのライブラリについてどんな情報を持っているか、気にしないということである。POMが示しているものが得られるとは限らないのだ。

---

- 一番最初のスライドで、あるライブラリはライブラリXのバージョン2.1を要求し、別のライブラリは2.4を要求していた。両方がそれぞれ必要としているものを得ることはできない。にも関わらずアプリケーションはその2つのライブラリを使わなければならない。

---

- 依存関係の木構造の中に真実はない。全てはサジェスチョンに過ぎないのだ。「僕はこれがほしい」「僕はあれがほしい」クリスマスみたいなもので、サンタはそのリストを見て「運が良ければね」といっているようなものだ。全員が列車のおもちゃをもらえるわけではない。

---

- 真実は、ランタイムのクラスパスである。ややこしいクラスパスの順序問題を抜きにすれば。誰かがクラスパスを作らなければならない。入力値として、Mavenから依存関係木構造を手に入れることはあるだろう。だが、そこから解決しなければならない。人間の介在が必要で、「この2つは一緒に動かないことを知っている」といったことになるかもしれない。だが、ライブラリを今まで経験したことのない組み合わせのコンポーネント上で動作させなければ行けない可能性は非常に高い。

---

- だから、「ライブラリを作成したが、これは依存ライブラリのバージョン2.1と動く」などということはできない。バージョン2.4との組み合わせでも動作してもらわなければ困る。実行環境では2.4が与えられているのだから。これがコンテクストである。どのようなコンテクストが与えられるか予測することはできない。それがコンテクストの意味するところである。

---

## Testing is Runtime Dependent

- これはテストに影響を与える。

---

- 再現可能な開発環境と再現可能なビルド環境では、依存関係は構築したものに何の影響もあたえない場合がほとんどである。

---

- テストしているのは今現在のライブラリとその環境の下だけである。だがこれは独立しているものである。

---

- 不特定多数の利用者に対しても、依存しているライブラリの変化に対してもテストすることは不可能なのである。

---

- リリース時にアーティファクトに対しておこなっているテストは、限定的なものであるということを認識しなければならない。もちろん自分が意図したように動いていることを確認することができるが、依存関係は変わっていくものなので、依存関係に対しては大した意味をなさない。

---

- しかし、もっと上のレベルでアーティファクトの組み合わせについて話す必要があると思う。これは依存関係の木構造とは関係がない。

---

- たしかに、アプリケーションとして、間接依存関係を1次元のリストにしたJarを一つ一つ記述したファイルを書きたいとは思わない。

---

- 今までにexclusion句やバージョンの明示的に上書きしたことのある人はどれくらいいる？楽しい？いやだよね？でも何かもう少し実用的なものが必要だ。

---

- コードの読み込みから始まって、ライブラリXとZは全く必要ないと言ってくれるツールが必要なのだ。必要ないものを含めないので、ソフトウェア開発がもっとシンプルになる。

---

- このアイデアを上の層にも適用していかなければならない。今までに述べてきたように、名前がもっと永続的であれば、ツールが改善できる余地がもっと増える。全ての最新版を使う、ということもできるし、依存関係を更新したり、ライブラリを更新する副作用として最新版を自動的に認識することができる。

---

## (Live Coding Demo)

- 会場が静まり返っちゃったね。これはスライドのテンプレートに、ここでジョークを入れろと書かれていたんだよ。

---

## What about Web Services? 

- それでは、Webサービスはどうだろう？同じことだ。「Jar? まいったね、そりゃ時代遅れだよ。今は全部Webサービスでやるんだよ。だからJarなんか気にしないし、Jarのバージョン問題なんかないんだよ。Webサービスでやるんだ。Webサービスに問合せをするだけだよ。」

---

- 何も違いはない。同じ問題があって、同じ間違いをする。何も違いはない。

---

- バージョン化されたWebサービスを持っている人はどれくらいいる？メジャーバージョン？ひとつも改善されていないね。

---

- バージョン化は答えではないし、まだ同じ間違いをしている。関数への引数を変えるときにWebサービスをバージョン化している人はどれくらいいる？いいんだよ、正しいことをしている、それがこの業界の慣習なんだから。

---

- だが、それはレベル違反だ。サービスの中になにか操作があって、それを変更する場合、Webサービスだからといって違いはない。単にサービスに帽子をかぶせただけだ。サービスは一連の操作を提供している。Webサービスは操作のコレクションである。それだけである。Webサービスでできることは2つだけで、操作を追加するか、削除するかである。そして操作をいじくりまわす。Webサービスを関数と同じようにみなすことができる。何を要求し、何を提供するのか。

---

- Webサービスを成長させる方法はあるのか？イエス！特にオープン性、オープンなスペックとオープンなデータ構造のアプローチを取った場合は。Webサービスが返すものを増やす方向で成長させることは全く構わない。利用者に対し、「少なくともこれだけは提供します。ただそれ以上かもしれません」といっている限りは。そうすれば供給者と利用者が一緒に成長することができる。

---

- 同様に壊すこともできる。より多くを要求し、提供するものを減らせば。もしそうしたいと思うなら、考え直したほうが良い。fooを壊すという代わりに、foo2を新たに追加することができるのだから。もしfooを壊すとしたら、バージョン2のAPIを定義しなければならない。今度の火曜日にバージョン2のAPIが登場するから、みんなそちらに切り替えて、などなどの手続きをしなければならない。相手の環境に影響を与えてしまう。これを回避する方法はない。

---

- foo2をfooと並べて提供しても、同じようにアナウンスすることはできる。「今週はバミューダ諸島にいるけど、来週foo2を試してみるよ」これは素晴らしいじゃないか。現時点の自分のWebサービスを動作し続ける、なぜならバケーションの最中にfooがなくされていないから。

---

- 積み重ねによる成長によって、全く同じように解決できるのである。

---

## We Need to Bring FP to the Library Ecosystem

- 我々がすべきことは、関数型プログラミングをライブラリのエコシステムに持ち込むことである。これだけだ。

---

- 最下層も、最上層も、真ん中の層もみんないい、サンドイッチを作る必要があるのだ。

---

- 現時点ではデータの書き換えをしていて、それをバージョン化のせいにしている。これはいいことじゃない。

---

- 依存関係地獄と可変性地獄は同じことである。スケールの違いに過ぎない。

---

- このせいでプログラミングが脆くなる。だが最悪なのは、ライブラリの有用性が減じてしまうことである。依存関係を導入するのを躊躇してしまう人はどれくらいいる？私もそうだ。それは自分のプログラムをかさばらせるからじゃなくて、他の人のことを心配しなければならなくなるからである。だけど他の人のことなど心配したくない。誰も他の人のことを心配する必要なんてないと思う。

---

- 悲しむべきことは、ソフトウェアを書いて、オープンソースにして、Slackで告知して、良いことをしているはずなのに、人々はあなたを信用しないということだ。これはあなたが間違ったことをしてるからではない。こういう問題が起こるのを見てきているからだ。前に言ったように、これは一種の社会的な問題なのだ。

---

## "This is Impossible/Impractical"

- これにノーという人はどれくらいいる？「今までの話はもっともだけど、自分が小さなデータを不変的にして、42を43に変更できないようにするのは簡単だけど、これくらいスケールが大きくなると新しい要求が次々と出てきて、変更なしでこんなに大きいものは作れないんじゃないのか？」と思う人は？それは真実じゃない。

---

- これらを見て。UNIXから何かを呼び出すとき、それは1970年代から存在していて、今日でもまだ存在し、動作する。同様に古いJavaプログラムもまだ動作する。古いHTMLも未だに正しく動作するし、Clojureコアも同じだと考えている。完璧じゃないかもしれないけれど、それが長所だ。だから、人々がこの関数は嫌いだから削除してくれ、と言われても、私は拒否し続ける。だからそういう関数も存在し続ける。他の人に迷惑をかけたくないから。

---

- 成功の条件はわからないけれども、成功のための前提条件は互換性であることは間違いない。これを無視することはできない。人々から価値を見出されているものを保持し続けることである。そして人から価値を認められるものを書きたいのなら、この点を考慮する必要がある。

---

## What If We Never Broke Anything?

- 何も破壊しなかったとしたらどうなるんだろう？

---

- 名前は永続的に意味を持つようになる。Maven Centralが今日してくれることも、将来してくれることもわかっている。

---

- 前に話した互換性チェックは可能だと思う。最新版に移行して、ライブラリの作者とは無関係に動作をテストして、正しい組み合わせ、誰が何を、いつ必要としているのかを理解し、動作する組み合わせを作成することで、破壊を心配する必要がなくなる。

---

- 細かい粒度の依存関係を調べることもできる。特に興味を持っている点があるが、それは別のスライドで説明する。

---

- 最新版を、リスクを取ることなく使うことができる。Maven Centralであればこれは可能である。「なんてこった、3週間前にMaven Centralから取ってこなくちゃならなかった」などとなることはないから。

---

- ソフトウェア開発で極めて重要なのは、リスクを取ることなく合成することができるということだ。2つのものをとってきて、3つめが2つのうちのどちらかを必要とする時に、その3つを一緒にしていいかどうかわからないとしたら、関数型プログラマとして実感している合成の恩恵を受けることができない。

---

## Open Challenges

- 未解決の問題はたくさんあると思う。

---

- その一つは、変化の中には見えないものがあるということだ。前に引数と返り値は見えにくいという話をした。それと関数の存在と関数への依存である。コレクションは簡単だ。だが、Specをもっと使うようになればこの分野の手助けになる。なぜなら成長していく変化をSpecから読み取れるからである。コードは機械で検知できるようには変わっていかないかもしれないが、Specは比較可能な形で変化し同じ名前を保持し続けるので、Specの中で何が起こっているのか見ることができる。

---

- Specの互換性はちょっと難しい。提供しているか要求しているかで依存しているものが変わってくるからだ。Specを大きくすることも小さくすることもできるが、片方は破壊的で、もう片方はそうではない。この方向性をSpecの中に組み込みたい。このコンテクストでのSpecが提供側なのか要求側なのかを決定できるようにしたい。

---

- リポジトリからアーティファクト、アーティファクトから名前空間への関係性を直せれば素晴らしい。グローバルレジストリがあって、与えられた名前空間からリポジトリがどこか、アーティファクトがどこかが分かるようになれば素晴らしい。そうすれば下の層から辿っていくことができる。それは、Fredに話しかけて、FredはこのJarを使えといったが、その中にはincanterがあった、などという状況とは対照的である。

---

- もう一つの大きな問題は、メジャーバージョンを使うことで問題を解決しようとする、この問題を助長するツールが存在することである。

---

## Opportunities for Clojure

- Clojureは助けになる。Specがあり、それにより柔軟な依存性検知が可能になると思う。これは脆弱でもろい既存の仕組みとは対象的である。

---

- コードからアーティファクトやリポジトリへの関係を明示的にすることができるかもしれない。

---

- これも前に述べたが、パブリックにすることはそんなにいいことではない、というのもアルファ版の利用者も何かにアクセスする必要があるので、公開するときや、コミットメントを表明することや、非推奨を通知するAPIがあればいい。非推奨とは、廃止するわけではないが、代替の`foo2`を提供してあり、それは2倍速かったり、牛を返したりするからそちらを参照してくれ、ということである。

---

- 今後発表する予定なのは、細かい粒度の依存関係に基づいたテストである。Alex Millerのアイデアをばらしたくはないのだが、これは生成的テストに必要なので既に取り組み始めている機能である。今はエディタのセーブボタンを押すたびにテストが全部実行される。テストを自分で書いて、実際はほとんど何もテストしていないからほとんど役に立っていない。だが、生成的テストは有用だ。だが時間がかかる。

---

- とはいえ、肝心なのはもし対象の関数に変更が加えられていないのなら、なぜテストを実行する必要があるのか。繰り返し繰り返し？自分がテストして、他の人もテストして？意味がない。コードからSHAを生成して、このSHAに対してはテストを実行した、と言えばいいだけのことである。細粒度の依存関係がわかっているので、別の関数とこのコンテクストでこのSHAをテストすることもできる。これが手に入れられるものである。そうすれば、セーブボタンを押したとき、本当に必要なテストだけが実行される。明らかにコストがより多くかかる生成的テストであっても構わなくなる。

---

## Exchange > Change

- 皆さん、私がしようとしていることがわかっただろうか。私が伝えたいのは変更ではなく交換を重視すべきということである。

---

- 他人のためにライブラリを書くということは交換するということである。もし変更しようとするのなら他人に迷惑をかけないよう気をつける必要がある。

---

- 良いやり方は2つある。一つは、あなたのソフトウェアを成長させるということ、もう一つは、破壊的な変更を、積み重ねによる成長へと変換することである。

---

- もし派生的なものがあるのなら、既存のものを乱すのではなく、新しく派生させること。

---

- 子孫のように考えよう。利用者を子供のように扱えと言っているのではなく、子供とは将来のことである。あなたのソフトウェアの将来を考えなさい。将来に渡って不具合を直し、改善し、人々に使ってもらい、好意を持ってもらえるようにしよう。

---

- そのためには、過去を捨て去ることなく前進しなければならない。

---

- 我々みんなで改善することができる。コントリビューションライブラリから始めて新しいやり方を適用していけると思っている。Clojureがこの分野をリードするようにしたい。ここまで私が述べてきたことはClojure独特の問題ではなくて業界標準の問題で、素晴らしいとは言い難い。だから我々がこれを素晴らしくする最初のコミュニティになろうじゃないか。以上。
