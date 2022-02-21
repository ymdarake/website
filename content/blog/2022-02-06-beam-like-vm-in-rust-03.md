+++
title = "Rustで作るBEAMライクなLanguage VM - 3"
description = "Rustで、Erlang VMのようなVMをレジスターベースで書いていきます。(Part 3)"
[taxonomies]
tags = [ "Rust", "VM" ]
+++


引き続き、BEAM-like VM in Rustをしていく。
<br><br>
[前回のブログはこちら。](/blog/beam-like-vm-in-rust-02/)
<br><br>
今日は[Part 03](https://blog.subnetzero.io/post/building-language-vm-part-03/)。
<br>
<br>
オペコードを追加していって簡単な算数ができるようになった。<br>
こんな感じ。(エッセンスだけ抜粋)<br>
```rust
// instruction.rs
#[derive(Debug, PartialEq)]
pub enum Opcode {
    LOAD,
    ADD,
    SUB,
    MUL,
    DIV,
    HLT,
    IGL,
}
```
```rust
// vm.rs
fn execute_instruction(&mut self) -> bool {
    if self.pc >= self.program.len() {
        return false;
    }
    match self.decode_opcode() {
        Opcode::LOAD => {
            let target_register = self.next_8_bits() as usize;
            let number = self.next_16_bits() as u16;
            self.registers[target_register] = number as i32;
        }
        Opcode::ADD => {
            let register1 = self.registers[self.next_8_bits() as usize];
            let register2 = self.registers[self.next_8_bits() as usize];
            self.registers[self.next_8_bits() as usize] = register1 + register2;
        }
        Opcode::SUB => {
            let register1 = self.registers[self.next_8_bits() as usize];
            let register2 = self.registers[self.next_8_bits() as usize];
	    self.registers[self.next_8_bits() as usize] = register1 - register2;
        }
        Opcode::MUL => {
            let register1 = self.registers[self.next_8_bits() as usize];
            let register2 = self.registers[self.next_8_bits() as usize];
            self.registers[self.next_8_bits() as usize] = register1 * register2;
        }
        Opcode::DIV => {
            let register1 = self.registers[self.next_8_bits() as usize];
            let register2 = self.registers[self.next_8_bits() as usize];
            self.registers[self.next_8_bits() as usize] = register1 / register2;
            self.remainder = (register1 % register2) as u32;
        }
        Opcode::HLT => {
           println!("HLT encoutered");
           return false;
        }
        Opcode::IGL => {
            println!("Illegal instruction encountered");
            return false;
        }
    }
    true
}
```

見た目は言語処理系あるあるな感じがありつつ、レジスターベースで書くのは初めてなので新鮮な気持ち。
<br><br>
パターンマッチがかける言語って気持ちいいですねぇ。<br>
Rust本格的にやるしかない。<br>

<br>
<br>

## おまけ

ブログの筆者がテストを逐一書いていくスタイルなので、<br>
と前回書いたものの、どうやら今回からはコードの本体もテストもハショられていく模様。
<br><br>
記事を写経してもコンパイルすらしない。<br>
そういうことってあるよね。どんまいどんまい。


[公開されている完成品のリポジトリ](https://gitlab.com/subnetzero/iridium/-/tree/master)を参考にしつつ、自分なりにテストコード+コメントを書きながら記事分の内容を完了させた。
<br>
<br>
今回のコードはこちら。<br>
[https://github.com/ymdarake/iridium/pull/3](https://github.com/ymdarake/iridium/pull/3)


[次回に続く](/blog/beam-like-vm-in-rust-04/)。
