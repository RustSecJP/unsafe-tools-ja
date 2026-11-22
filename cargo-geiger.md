{% raw %}
# cargo-geiger ☢️ {#cargo-geiger}

[![CI](https://github.com/geiger-rs/cargo-geiger/actions/workflows/ci.yml/badge.svg)](https://github.com/geiger-rs/cargo-geiger/actions/workflows/ci.yml)
[![unsafe forbidden](https://img.shields.io/badge/unsafe-forbidden-success.svg)](https://github.com/rust-secure-code/safety-dance/)
[![crates.io](https://img.shields.io/crates/v/cargo-geiger.svg)](https://crates.io/crates/cargo-geiger)
[![Crates.io](https://img.shields.io/crates/d/cargo-geiger?label=cargo%20installs)](https://crates.io/crates/cargo-geiger)

Rust のクレートと、そのすべての依存クレートについて、unsafe な Rust コードの使用状況に関する統計を一覧にするツールです。

この cargo プラグインは、もともと次の 2 つのプロジェクトのコードをもとにしています。
* <https://github.com/icefoxen/cargo-osha> と
* <https://github.com/sfackler/cargo-tree>

## インストール {#installation}

システム全体にインストールされている OpenSSL ライブラリを探して使う場合は、次のようにします。

```bash
cargo install --locked cargo-geiger
```

または、OpenSSL をビルドして、cargo-geiger の実行ファイルの一部として静的にリンクします。

```bash
cargo install --locked cargo-geiger --features vendored-openssl
```

このほか、ビルド済みのバイナリを [GitHub releases](https://github.com/geiger-rs/cargo-geiger/releases) から入手することもできます。

## 使い方 {#usage}

1. 解析したい `Cargo.toml` があるディレクトリに移動します。
2. `cargo geiger`

## 想定している使い方 {#intended-use}

このツールは、コードが結局のところ本当に安全でないのかどうかを、直接判定するためのものではありません。

cargo-geiger の目的は、監査のための統計的な材料を提供することです。たとえば、次のものと組み合わせて使います。

- [cargo-crev](https://crates.io/crates/cargo-crev)
- [safety-dance](https://github.com/rust-secure-code/safety-dance)

unsafe の使用には微妙な事情があり、必要な場合もあります。unsafe を使う理由がどのようなものかは、cargo-geiger の扱う範囲の外です。

結果を報告するときは、どのような形であれ、慎重に扱うことが大切です。

- [Reddit: The Stigma around Unsafe](https://www.reddit.com/r/rust/comments/y1u068/the_stigma_around_unsafe/)
- [YouTube: Rust NYC: Jon Gjengset - Demystifying unsafe code](https://youtu.be/QAz-maaH0KM)
- [Rust-lang: WG Unsafe Code Guidelines](https://github.com/rust-lang/unsafe-code-guidelines)

## 出力例 {#output-example}

![出力例](https://user-images.githubusercontent.com/3704611/53132247-845f7080-356f-11e9-9c76-a9498d4a744b.png)

## 既知の問題 {#known-issues}

 - [issue tracker](https://github.com/rust-secure-code/cargo-geiger/issues) を見てください。

## ライブラリ {#libraries}

Cargo Geiger は、3 つのライブラリを公開しています。

 - `cargo-geiger` - バージョン番号による互換性の管理がなく、非常に不安定なライブラリです。`cargo-geiger` バイナリの内部を公開しています。そのため、このライブラリに含まれる関数は、どれも変更される可能性があります。
 - `cargo-geiger-serde` - シリアライズできるレポートの型を収めたライブラリです。
 - `geiger` - [cargo-geiger] が使っている、[cargo] から切り離されたいくつかのコンポーネントを収めたライブラリです。

## 変更履歴 {#changelog}

[変更履歴][changelog]を見てください。

[cargo]: https://crates.io/crates/cargo
[cargo-geiger]: https://crates.io/crates/cargo-geiger
[changelog]: https://github.com/rust-secure-code/cargo-geiger/blob/master/CHANGELOG.md

## 名前の由来 {#why-the-name}

<https://en.wikipedia.org/wiki/Geiger_counter>

unsafe なコードは、電離放射線と同じように、状況によっては避けられません。だからこそ、安全に封じ込めておくべきなのです。
{% endraw %}
