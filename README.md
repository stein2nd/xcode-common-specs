# Xcode Common Specs

本リポジトリは、Xcode や Cursor などで Swift/SwiftUI 開発するうえでの **共通仕様** を定義するドキュメント・リポジトリです。

## Description

本リポジトリ配下で開発する Swift/Swift-UI プロジェクトにおいて「AI 伴走開発」を行う際の共通仕様・ルールを定義します。

## 1. 技術スタック

* **言語**: Swift (最新安定版)
* **UI**: SwiftUI (最優先)
* **プロジェクト管理**: Xcode (`*.xcodeproj` or SwiftPM)
* **最低対応 OS**: iOS v16 / macOS v12
* **依存管理**: [Swift Package Manager (SPM)](https://docs.swift.org/swiftpm/documentation/packagemanagerdocs/)
* **テスト**: XCTest + SnapshotTesting

## 2. 主要な開発ルール

* Swift Concurrency (async/await, actor) を優先的に使用します。
* UIKit / AppKit の使用は、最小限に抑えてください。
* モジュール化 (Feature 単位で分割) を推奨します。
* リソースは、Assets.xcassets に統合します。
* 定数・文字列は、Localizable.strings / .stringsdict を利用します。
* 設計パターンは、MVVM を基本とします。
* 実装修正の着手は、人間が許可してから実行してください。

## 3. コーディング規約

* ソースコードを、[SwiftFormat](https://formulae.brew.sh/formula/swiftformat#default) で自動整形してください。
* ソースコードを、[SwiftLint](https://formulae.brew.sh/formula/swiftlint) で検査してください。
* 1ファイル1型を原則とします。
* クラスより struct / enum を優先します。
* 依存注入は、プロトコル・ベースとします。

## 4. 詳細な仕様

詳細な仕様、プロジェクト構成、AI 伴走開発のガイドライン、Git 運用ルールなどについては、[`docs/COMMON_SPEC.md`](./docs/COMMON_SPEC.md) を参照してください。

## License

このプロジェクトは GPL v2以降の下でライセンスされています - 詳細は [LICENSE](LICENSE) ファイルを参照してください。
