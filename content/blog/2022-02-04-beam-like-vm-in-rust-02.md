+++
title = "Rustで作るBEAMライクなLanguage VM - 2"
[taxonomies]
tags = [ "Rust", "VM" ]
+++


引き続き、BEAM-like VM in Rust をしていく。

今日は<a href="https://blog.subnetzero.io/post/building-language-vm-part-02/" target="_blank" rel="noopener noreferrer">Part 02</a>。
VMにプログラムとプログラムカウンタを持たせて、ループして処理していく。

この辺は後でどんどん最適化？していくらしい。

プログラムの中身用に別途オペコードを`enum`で定義する。

ちなみにレジスターが32bit、そのうちオペコードが8bitの設計。（これは<a href="https://blog.subnetzero.io/post/building-language-vm-part-01/" target="_blank" rel="noopener noreferrer">Part 01</a>で話があった）

VMが入力のプログラム(いまはただの`Vec<u8>`)をループしてOpcodeを一つずつ処理するように書いて、テストを書いて今日のところはお開き。

Part02 にしてプログラム(とテスト)の骨格が出来てきていい感じ。

**おまけ**

ブログの筆者がテストを逐一書いていくスタイルなので、
それならばということで GitHub Actions を追加して、
<a href="https://about.codecov.io/" target="_blank" rel="noopener noreferrer">Codecov</a>でRustのテストカバレッジを測定できるようにした。
<a href="https://github.com/apps/codecov" target="_blank" rel="noopener noreferrer">CodecovのBotがPRにレポートしてくる設定</a>も追加。

今回のコードはこちら。
<a href="https://github.com/ymdarake/iridium/pull/2" target="_blank" rel="noopener noreferrer">https://github.com/ymdarake/iridium/pull/2</a>


次回に続く。
