+++
title = "Rustで作るBEAMライクなLanguage VM - 1"
description = "Rustで、Erlang VMのようなVMをレジスターベースで書いていきます。(Part 1)"
[taxonomies]
tags = [ "Rust", "VM" ]
+++


面白そうなブログ記事(シリーズ)を見つけた。
RustでBEAM(Erlang, Elixirのアレ)ライクなVMを作ってみよう、というもの。

[So You Want to Build a Language VM - Part 00 - Computer Hardware Crash Course](https://blog.subnetzero.io/post/building-language-vm-part-00/)

少し前に[インタープリタをGoで作ってみて](https://interpreterbook.com/)、かつElixirやRustの入門資料を眺めていた自分には、一石三鳥(?)の内容に思える。

[Part 00](https://blog.subnetzero.io/post/building-language-vm-part-00/) 〜 [Part 33](https://blog.subnetzero.io/post/building-language-vm-part-33/)が存在する模様。

なかなかの長編ポストだ。

いまさっき発見して[Part 00](https://blog.subnetzero.io/post/building-language-vm-part-00/)を読み終えたところで、これから[Part 01](https://blog.subnetzero.io/post/building-language-vm-part-01/)をやっていく。


ネット上にはtree-walkingあるいはstack-basedなVMの例はたくさんあるし、せっかくやるならregister basedなVMで楽しんでいこう、とのこと。

きっと楽しい人なんだろうな。いいね。


<a href="https://rustup.rs" rel="noopener noreferrer" target="_blank">Rustをインストールして</a>、

```sh
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

GitHubに[Part 01](https://blog.subnetzero.io/post/building-language-vm-part-01/)のコードを置く。

[https://github.com/ymdarake/iridium](https://github.com/ymdarake/iridium)

ついでにGitHub Actionsも設定。


次回に続く。
