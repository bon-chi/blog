+++
title = "自作エミュレータで学ぶx86アーキテクチャコンピュータが動く仕組みを徹底理解！をrustで進める: 3章"
description = "x86-emulator-in-rust"
date = 2018-05-28
template = "page.html"
tags = ["x86-emulator"]
+++

## Chapter3
### 3.2
サンプルコードと同じように修正する。`near_jump`命令だけ[short_jumpのときと同様に](./x86-emulator-in-rust-01.md)符号付き即値を考慮する必要がある。
~~~rust
impl Emulator {
    fn near_jump(&mut self) {
        let diff = self.get_sign_code32(1) + 5;
        match diff > 0 {
            true => {
                self.eip += diff as u32;
            }
            false => {
                self.eip -= diff.abs() as u32;
            }
        }
    }
}
~~~


commitは[こちら](https://github.com/bon-chi/rust_emu/commit/2d73e48f17d4a7ab1a132a95f7f983eda641d80a)

### 3.4
#### モジュール化
今後の拡張に備えて1ファイルに書いていたコードをモジュール化する。サンプルコードでは`.h`ファイルに分けているのをTraitに分けることにする。実装はサンプルコードでは各`.c`ファイルに分かれているが、`src/emulator/mod.rs`にまとまる。
commitは[こちら](https://github.com/bon-chi/rust_emu/commit/2c4922495ae8315a4744ff28a6f2dc2e06132a1c<Paste>)

#### ModR/Mの実装
ModR/Mを実装するのにサンプルコードでは`union`を使っている。RustもC-likeなunionを使える。
~~~rust
#[repr(C)]
pub union Reg {
    pub opecode: u8,
    pub reg_index: u8,
}

#[repr(C)]
pub union Disp {
    pub disp8: i8,
    pub disp32: u32,
}

~~~
ただし、値を取得する際は`unsafe`ブロックで囲う必要がある

commitは[こちら](https://github.com/bon-chi/rust_emu/commit/07bd3ac9c3a574bb9a0f70e170ed476278f53b56)
