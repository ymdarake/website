+++
title = "Rustで作るBEAMライクなLanguage VM - 2"
description = "Rustで、Erlang VMのようなVMをレジスターベースで書いていきます。(Part 2)"
[taxonomies]
tags = [ "Rust", "VM" ]
+++


引き続き、BEAM-like VM in Rust をしていく。<br>
<br>
今日は<a href="https://blog.subnetzero.io/post/building-language-vm-part-02/" target="_blank" rel="noopener noreferrer">Part 02</a>。<br>
<br>
VMにプログラムとプログラムカウンタを持たせて、ループして処理していく。<br>
<br>
この辺は後でどんどん最適化していくらしい。<br>
プログラムの中身用に別途オペコードを`enum`で定義する。<br>
<br>
ちなみにレジスターが32bit、そのうちオペコードが8bitの設計。<br>
（これは<a href="https://blog.subnetzero.io/post/building-language-vm-part-01/" target="_blank" rel="noopener noreferrer">Part 01</a>で話があった）<br>
<br>
VMが入力のプログラム(いまはただの`Vec<u8>`)をループしてOpcodeを一つずつ処理するように書いて、テストを書いて今日のところはお開き。<br>
<br>
Part02 にしてプログラム(とテスト)の骨格が出来てきていい感じ。<br>
<br>
<br>

### おまけ

ブログの筆者がテストを逐一書いていくスタイルなので、<br>
それならばということで GitHub Actions を追加して、<br>
<a href="https://about.codecov.io/" target="_blank" rel="noopener noreferrer">Codecov</a>でRustのテストカバレッジを測定できるようにした。<br>
<br>
<a href="https://github.com/apps/codecov" target="_blank" rel="noopener noreferrer">CodecovのBotがPRにレポートしてくる設定</a>も追加。<br>
<br>
今回のコードはこちら。<br>
<a href="https://github.com/ymdarake/iridium/pull/2" target="_blank" rel="noopener noreferrer">https://github.com/ymdarake/iridium/pull/2</a><br>

<br>

[次回に続く。](/blog/beam-like-vm-in-rust-03/)
<br>
