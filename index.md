# cargo-careful と cargo-geiger（README 日本語訳）

unsafe な Rust コードと向き合うための 2 つのツール、[cargo-careful](https://github.com/RalfJung/cargo-careful) と [cargo-geiger](https://github.com/geiger-rs/cargo-geiger) の README の、非公式な日本語訳です。

- **cargo-careful** は、nightly でしか使えない追加の検査を有効にし、デバッグアサーションつきの標準ライブラリを使ってプログラムを実行することで、未定義動作（undefined behavior）を見つけやすくするツールです。
- **cargo-geiger** は、クレートとそのすべての依存クレートについて、unsafe な Rust コードの使用状況に関する統計を一覧にするツールです。

> **ご注意**: この翻訳は、cargo-careful や cargo-geiger の開発者の確認や公認を受けたものではありません。コマンドやフラグの名前は英語のまま残しています。実際に使うときは、必ず最新の原文を確かめてください。

## 文書

1. [cargo-careful]({{ '/cargo-careful.html' | relative_url }})
2. [cargo-geiger ☢️]({{ '/cargo-geiger.html' | relative_url }})

## 出典

### cargo-careful

- 原文: [RalfJung/cargo-careful](https://github.com/RalfJung/cargo-careful) の `README.md`
- 原著者: Ralf Jung と cargo-careful の貢献者
- ライセンス: MIT または Apache-2.0
- 訳した版: 2026-10-02 時点（コミット `133ff58`）

### cargo-geiger

- 原文: [geiger-rs/cargo-geiger](https://github.com/geiger-rs/cargo-geiger) の `README.md`
- 原著者: cargo-geiger の貢献者
- ライセンス: Apache-2.0 または MIT
- 訳した版: 2026-10-13 時点（コミット `118a9a2`）
