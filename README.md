# cargo-careful と cargo-geiger（README 日本語訳）

> **これは [RalfJung/cargo-careful](https://github.com/RalfJung/cargo-careful) と [geiger-rs/cargo-geiger](https://github.com/geiger-rs/cargo-geiger) の README の、非公式な日本語訳です。** それぞれの開発者による確認や公認を受けたものではありません。このリポジトリにツール本体のコードはありません。
>
> **読むのはこちら: https://rustsecjp.github.io/unsafe-tools-ja/**
>
> This is an unofficial Japanese translation of the READMEs of cargo-careful and cargo-geiger. It does not contain the tools themselves.

unsafe な Rust コードと向き合うための 2 つのツールの README を訳しています。

- **cargo-careful**: nightly でしか使えない追加の検査を有効にし、デバッグアサーションつきの標準ライブラリを使ってプログラムを実行することで、未定義動作（undefined behavior）を見つけやすくするツールです。訳は [cargo-careful.md](cargo-careful.md) にあります。
- **cargo-geiger**: クレートとそのすべての依存クレートについて、unsafe な Rust コードの使用状況に関する統計を一覧にするツールです。訳は [cargo-geiger.md](cargo-geiger.md) にあります。

## 訳した版

| | 原文の版 | 翻訳日 |
|---|---|---|
| cargo-careful | 2026-10-02 時点（コミット [`133ff58`](https://github.com/RalfJung/cargo-careful/commit/133ff58b13374bde0a70aee408785247bbcb301c)） | 2026-11-22 |
| cargo-geiger | 2026-10-13 時点（コミット [`118a9a2`](https://github.com/geiger-rs/cargo-geiger/commit/118a9a2812098ad757c9174ad00be21deaea7e0d)） | 2026-11-22 |

原文がこの版より後に更新されている場合、訳は古い内容のままです。

## 出典とライセンス

### cargo-careful

- 原文: https://github.com/RalfJung/cargo-careful の `README.md`
- 原著者: Ralf Jung と cargo-careful の貢献者
- ライセンス: MIT または Apache-2.0。全文は [LICENSE-MIT-cargo-careful.txt](LICENSE-MIT-cargo-careful.txt) と [LICENSE-APACHE-cargo-careful.txt](LICENSE-APACHE-cargo-careful.txt)（原文リポジトリの `LICENSE-MIT` と `LICENSE-APACHE` の写し）。
- 訳した版: 2026-10-02 時点（コミット 133ff58b13374bde0a70aee408785247bbcb301c）

### cargo-geiger

- 原文: https://github.com/geiger-rs/cargo-geiger の `README.md`
- 原著者: cargo-geiger の貢献者
- ライセンス: Apache-2.0 または MIT（`cargo-geiger/Cargo.toml` の `license` 欄の記載）。原文リポジトリの直下にライセンスファイルはありません。クレートのディレクトリ `cargo-geiger/` にある `LICENSE-MIT` と `LICENSE-APACHE` の写しを、[LICENSE-MIT-cargo-geiger.txt](LICENSE-MIT-cargo-geiger.txt) と [LICENSE-APACHE-cargo-geiger.txt](LICENSE-APACHE-cargo-geiger.txt) として収録しています。
- 訳した版: 2026-10-13 時点（コミット 118a9a2812098ad757c9174ad00be21deaea7e0d）

変更点は、日本語への翻訳と、表示用の書式の調整です。詳細は [NOTICE.md](NOTICE.md) にあります。

誤訳の指摘や訳語の提案は、Issue か Pull Request でお願いします。

RustSecJP は非公式のコミュニティです。cargo-careful と cargo-geiger の開発者、Rust Project、Rust Foundation とは提携していません。
