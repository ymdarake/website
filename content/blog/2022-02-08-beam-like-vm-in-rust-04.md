+++
title = "Rustで作るBEAMライクなLanguage VM - 4"
description = "Rustで、Erlang VMのようなVMをレジスターベースで書いていきます。(Part 4)"
[taxonomies]
tags = [ "Rust", "VM" ]
+++


引き続き、BEAM-like VM in Rustをしていく。
<br><br>
[前回のブログはこちら](/blog/beam-like-vm-in-rust-03/)
<br><br>
今日は [Part 04](https://blog.subnetzero.io/post/building-language-vm-part-04/)から[Part 08](https://blog.subnetzero.io/post/building-language-vm-part-08/)。
<br>

## ▼ [Part 04](https://blog.subnetzero.io/post/building-language-vm-part-04/) ~ [Part 05](https://blog.subnetzero.io/post/building-language-vm-part-05/) : 比較演算・ジャンプ
-----
今回はジャンプ、比較のオペコードを追加して実装もしていきます。<br><br>
書いて見せられると単純ですぐ理解できるものの、ジャンプってこういう仕組みなんだなぁとひとつ賢くなった気がします。（こなみ）<br>
現在位置からの相対的なジャンプというのもあるんですねぇ。<br>
<br>
これで`if`とか`loop`ができるようになりました。<br>
<br>
やったね。<br>
<br>
[https://github.com/ymdarake/iridium/pull/4](https://github.com/ymdarake/iridium/pull/4)<br>
[https://github.com/ymdarake/iridium/pull/5](https://github.com/ymdarake/iridium/pull/5)<br>

```rust
// instructions.rs
pub enum Opcode {
    LOAD,
    ADD,
    SUB,
    MUL,
    DIV,
    HLT,
     // ここから 
    JMP,
    JMPF,
    JMPB,
    EQ,
    NEQ,
    GTE,
    LTE,
    LT,
    GT,
    JMPE,
     // ここまで追加した
    IGL,
}
```
<br>

## ▼ [Part 06](https://blog.subnetzero.io/post/building-language-vm-part-06/) ~ [Part 07](https://blog.subnetzero.io/post/building-language-vm-part-07/) : REPL
-----
実装してきたものを色々お手軽に試すためにも（？）REPLを実装していきます。<br>
これまで実装してきたVMに標準入力経由でプログラムを`read_line`してきて実行するだけ。<br>
<br>
[https://github.com/ymdarake/iridium/pull/6](https://github.com/ymdarake/iridium/pull/6)<br>
[https://github.com/ymdarake/iridium/pull/7](https://github.com/ymdarake/iridium/pull/7)<br>
<br>

```rust
// repl/mod.rs
// TODO: 昔のモジュールの書き方らしいから後で変えたい

match buffer {
".quit" => {
    writeln!(&mut writer, "Farewell! Have a great day!")
        .expect("Unable to execute .quit");
    writer.flush().unwrap();
    true
}
// （中略）
".registers" => {
    writeln!(&mut writer, "Listing registers and all contents:")
        .expect("Unable to execute .registers");
    writeln!(&mut writer, "{:#?}", self.vm.registers)
        .expect("Unable to write registers");
    writeln!(&mut writer, "End of Program Listing")
        .expect("Unable to write ending message of .registers");
    writer.flush().unwrap();
    false
}
_ => {
    let results = self.parse_hex(buffer);
    match results {
        Ok(bytes) => {
            for byte in bytes {
                self.vm.add_byte(byte)
            }
        }
        Err(_e) => {
            writeln!(&mut writer, "Unable to decode hex string. Please enter 4 groups of 2 hex charracters.").expect("Unable to write parse_hex error message");
            writer.flush().unwrap();
        }
    };
    self.vm.run_once();
    false
}
```

いい感じ（？）に自作言語感がでてきましたね。<br>
<br>

### 余談1
ブログ記事の写経でプルリクをマージしたは良いものの、せっかく集計してるカバレッジが80%台まで落ちてしまったので、`stdin`, `stdout`たちをリファクタしてテスタブルにしました。<br>
[https://github.com/ymdarake/iridium/pull/8](https://github.com/ymdarake/iridium/pull/8)<br>
ジェネリクスもいい感じに書きやすいですね。<br>
`where`という言葉のチョイスにHaskellみを感じました。<br>
（別にHaskellが書けるわけではないです。ざんねん。えらいひとにおそわりたい。）<br>
<br>

### 余談2
せっかく設定していたGitHub Actionsによるカバレッジ集計が動かなくなってました。<br>
<br>
適当にRustのアクション（って呼ぶのか？）で人気そうなものを拝借していたんですが、<br>
ビルドキャッシュが良くなかったらしく、カバレッジのリポートファイル生成がこけてました。<br>
<br>
なんでかは調べてないですが、まぁキャッシュなので、そういうこともあるんでしょう。<br>
ありがたいことに無料なのでNoキャッシュ戦略で通してますが、<br>
地球に優しい開発者になりたいのでそのうち直そうと思います。自分も電気も省エネだいじ。<br>
<br>
ちなみにGitHub Actionsも初めて触ってます。<br>
いい機会だと思って使ってみている状況。何事もチャレンジ！（？）<br>
<br>

## ▼ [Part 08](https://blog.subnetzero.io/post/building-language-vm-part-08/) : アセンブラ
-----
自前VM用に自前アセンブラを書いていきます。<br>
<br>
```asm
LOAD $1 #10
```
を
```
00 01 00 0A
```
に変換してあげる感じです。<br>
<br>
さすがに自前パーサーまで書いていると切りがないので、素直にライブラリを使います。<br>
これで<br>
* オペコード: `LOAD`
* レジスター: `$1`
* 数字: `#10`

を `assembler::Token` に変換できるようになりました。<br>
<br>
[https://github.com/ymdarake/iridium/pull/11](https://github.com/ymdarake/iridium/pull/11)<br>
<br>
自前でお勉強コードを書いていると、あらためてライブラリの偉大さを実感しますね。（ただし偉大なものに限る）<br>
<br>
<br>
<br>

### おまけ
Rustの入門も兼ねて写経で進めているんですが、今後あらためて調べる点がありそうです。<br>

1. 執筆当初からRustがバージョンアップしたことで、モジュールの宣言の仕方が変わっている。<br>
`mod.rs`はいまはdeprecatedとのこと。<br>
useする方法もバラバラなので統一したい。

2. マクロシステム。
 [Part 08](https://blog.subnetzero.io/post/building-language-vm-part-08/)で使った [nom](https://github.com/Geal/nom)というパーサーコンビネーターのライブラリがマクロマクロしていた。<br>
マクロを多用するのは主にメタいことを達成したいときだろうし、それほど多用するタイミングもない気はしつつ、使えるようにはなっておきたい。<br>
エディタ上で通常のコードと同様に補完が効く書き方（rust-analyzerも頑張ってると思う）で、目に優しい系マクロシステムだなと思った。<br>

<br>
<br>
ここまで、写経だけだとエラーになったりもしたけど、まずまず順調で内容も楽しめてます。<br>
<br>
次回に続く。
<br>