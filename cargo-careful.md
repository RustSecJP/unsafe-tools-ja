{% raw %}
# cargo-careful {#cargo-careful}

`cargo careful` は、Rust のコードをいつもより念入りに実行するためのツールです。nightly でしか使えない追加の検査をまとめて有効にして未定義動作（Undefined Behavior）を見つけやすくし、さらにデバッグアサーションを有効にした標準ライブラリを使います。
標準ライブラリは、プログラムがデバッグアサーションつきでビルドされていれば、一部の未定義動作を検査します。しかし、そうした検査のいくつかは、性能への影響が大きすぎると判断されて無効になっています。
たとえば `cargo careful` は、次のコード片にあるアラインメントの問題を見つけます。

```rust
fn main() {
    let arr = [1u8, 2, 3, 4];
    for n in [0, 1] {
        let val = unsafe { arr.as_ptr().add(n).cast::<u16>().read() };
        println!("The value is {val}!");
    }
}
```

`cargo careful` を使うには、まずインストールします。

```
cargo install cargo-careful
```

次に、プロジェクトの中で次のコマンドを実行します。

```
cargo +nightly careful test
```

`cargo +nightly careful run` で、バイナリクレートを実行することもできます。`cargo test` と `cargo run` のフラグは、すべて使えます。

`cargo careful` を実行するには、最近の nightly ツールチェーンが必要です。直近 3 か月以内の nightly のバージョンに対応しています。

`cargo careful` を初めて実行するときは、いくつかの準備の手順を実行する必要があります。この準備には rustup のコンポーネント `rustc-src` が必要で、入っていなければ、ツールがインストールするかどうかを尋ねてくれます。

## 何をするのか {#what-does-it-do}

### 未定義動作を検出する {#detect-undefined-behavior}

`cargo careful` がすることのうちで最も重要なのは、デバッグアサーションを有効にして標準ライブラリをビルドすることです。
標準ライブラリは、プログラムがデバッグアサーションつきでビルドされていれば、一部の未定義動作を検査します。しかし、そうした検査のいくつかは、性能への影響が大きすぎると判断されて無効になっています。
さらに `cargo careful` は、実行時の追加の検査を挿入するよう rustc に指示するフラグをいくつか設定します。

これによって有効になる検査の一部を挙げます。

- `ptr.read()` と `ptr.write(v)` が、ポインタがアラインされていて、かつ null でないことを検査します。
- コレクション型が、内部の整合性の検査を数多く行います。
- `mem::zeroed` と、非推奨の `mem::uninitialized` が、型がその種類の初期化を許さない場合にパニックします（既定よりも厳しい検査を使います）。（これは `-Zstrict-init-checks` です。）
- 定数評価の間に、未定義動作の追加の検査を行います。（これは `-Zextra-const-ub-checks` です。）

とはいえ、`cargo careful` では検出*できない*未定義動作もたくさんあります。もっと網羅的に調べたいときは、[Miri](https://github.com/rust-lang/miri) を試してください。
Miri と比べた `cargo careful` の利点は、あらゆるコードで動くこと、任意のシステム関数や C の FFI 関数を使えること、そしてずっと速いことです。

### RUSTFLAGS {#rustflags}

`cargo careful` は、環境変数 `CARGO_ENCODED_RUSTFLAGS` と `RUSTFLAGS`、それに cargo の設定 `build.rustflags` に従います（この順に調べて、最初に設定されていたものを使います）。`target.rustflags` の設定には、いまのところ従い*ません*。従うには、cargo にあるターゲットの `cfg` のロジックをすべて実装し直す必要があるためです。フラグは、sysroot のビルドとプログラム自体の*両方*に適用されます。

### サニタイザーを使う {#sanitizing}

`cargo careful` は、追加の機能として、プログラムと標準ライブラリをサニタイザー（sanitizer）つきでビルドして実行できます。この機能は実験的なもので、既定では無効です。

[もとになっている `rustc` の機能](https://doc.rust-lang.org/nightly/unstable-book/compiler-flags/sanitizer.html)は、[手続きマクロ（procedural macros）](https://doc.rust-lang.org/reference/procedural-macros.html)と相性がよくありません。
ビルド中に手続きマクロに関するエラーメッセージが出た場合は、ターゲットを指定すると解決することがあります（ホストと同じターゲットでもかまいません）。たとえば `--target=x86_64-unknown-linux-gnu` のように指定します。

サニタイザーを使うには、コマンドラインフラグ `-Zcareful-sanitizer=<your_sanitizer>` を `cargo careful` に渡します。
対応しているサニタイザーとターゲットの一覧は、[こちら](https://doc.rust-lang.org/nightly/unstable-book/compiler-flags/sanitizer.html)にあります。
サニタイザーを指定せずに `-Zcareful-sanitizer` を渡すと、[`AddressSanitizer`](https://clang.llvm.org/docs/AddressSanitizer.html) が使われます。

`AddressSanitizer` を使うとき、`cargo careful` は既定で、プログラムの環境に `ASAN_OPTIONS=detect_leaks=0` を設定して、メモリリークの検査を無効にします。メモリリークは、ふつう健全性や正しさの問題ではないためです。環境変数 `ASAN_OPTIONS` を自分で設定すると（空文字列を含め、どんな値でも）、この動作は上書きされます。

### Main Thread Checker {#main-thread-checker}

`cargo careful` は、ユーザーが Xcode をインストールしていれば、macOS、iOS、tvOS、watchOS のターゲットで [Apple の Main Thread Checker](https://developer.apple.com/documentation/xcode/diagnosing-memory-thread-and-crash-issues-early#Detect-improper-UI-updates-on-background-threads) を自動的に有効にします。

これは、これらのプラットフォームで、スレッドセーフでない機能をメインスレッド以外で実行してしまう問題の診断に役立ちます。

### `cfg` フラグ {#cfg-flag}

`cargo careful` は、設定フラグ `careful` を設定します。そのため、Rust のコンパイル時の条件分岐の仕組み（`#[cfg(careful)]`、`#[cfg_attr(careful, ...)]`、`cfg!(careful)`）を使って、コードが念入りに実行されているかどうかを確かめられます。
{% endraw %}
