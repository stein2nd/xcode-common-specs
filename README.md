# Xcode Common Specs

[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0.en.html)
[![macOS](https://img.shields.io/badge/macOS-26.6.1-000000?logo=apple&logoColor=white)](https://www.apple.com/os/macos/)
[![iOS](https://img.shields.io/badge/iOS-26.6-000000?logo=apple&logoColor=white)](https://www.apple.com/os/ios/)
[![iPadOS](https://img.shields.io/badge/iPadOS-26.6-000000?logo=apple&logoColor=white)](https://www.apple.com/os/ipados/)
[![SwiftUI](https://img.shields.io/badge/SwiftUI-000000?logo=swift&logoColor=white)](https://developer.apple.com/jp/swiftui/)
[![Android](https://img.shields.io/badge/Android-16-000000?logo=android&logoColor=white)](https://www.android.com/intl/ja_JP/)
[![Jetpack Compose](https://img.shields.io/badge/JetpackCompose-000000?logo=jetpackcompose&logoColor=white)](https://developer.android.com/compose)

本リポジトリは、Xcode や Cursor などで Swift/SwiftUI 開発するうえでの **共通仕様** を定義するドキュメント・リポジトリです。

## Description

本リポジトリ配下で開発する Swift/Swift-UI プロジェクトにおいて、「AI 伴走開発」を行う際の共通仕様・ルールを定義します。

## 1. 技術スタック

* **言語**: Swift (最新安定版)
* **UI**: SwiftUI (最優先)。Android 展開時は、Jetpack Compose UI
* **プロジェクト管理**: Xcode (`*.xcodeproj` or SwiftPM)
* **最低対応 OS**: iOS v16 / iPadOS v16 / macOS v12
* **依存管理**: macOS / iOS / iPadOS は、[Swift Package Manager (SPM)](https://docs.swift.org/swiftpm/documentation/packagemanagerdocs/)。Kotlin Multiplatform / Jetpack Compose UI は、Gradle。
* **テスト**: Swift Testing + SnapshotTesting

## 2. 主要な開発ルール

* ドメインの判断は、純関数とし、副作用はアダプタに閉じる。
* 画面構成は、MVU を基本とする。ViewModel を置く場合は、I/O の起動と状態保持だけの薄い接着剤とする。
* アダプタでは、Swift Concurrency (async/await、actor) を優先的に使用する。
* UIKit / AppKit の使用は、最小限に抑える。
* モジュール化 (Feature 単位で分割) を推奨する。
* リソースは、Assets.xcassets に統合する。
* 定数・文字列は、Localizable.strings / .stringsdict を利用する。
* 実装修正の着手は、人間が許可してから実行する。

## 3. コーディング規約

* ソースコードを、[SwiftFormat](https://formulae.brew.sh/formula/swiftformat#default) で自動整形する。
* ソースコードを、[SwiftLint](https://formulae.brew.sh/formula/swiftlint) で検査する。
* 1ファイル1型を原則とする。
* クラスより、struct / enum を優先する。
* 依存の差し替えは、I/O 境界のプロトコルに限る。

## 4. 詳細な仕様

詳細は、[`docs/SPECS.md`](./docs/SPECS.md) を参照してください。関連する別紙は、次のとおりです。

* [`docs/KMP_PORTABILITY.md`](./docs/KMP_PORTABILITY.md) — iOS / iPadOS の移植層
* [`docs/UX_VOCABULARY.md`](./docs/UX_VOCABULARY.md) — 画面・操作・状態の語彙
* [`docs/UI_VIEWPORT.md`](./docs/UI_VIEWPORT.md) — Viewport Integrity
* [`docs/TEST_RESULTS.md`](./docs/TEST_RESULTS.md) — テスト結果のひな型
* [`docs/CICD.md`](./docs/CICD.md) — CI/CD

旧正本は、[`docs/archive/fop-mvu-viewport/`](./docs/archive/fop-mvu-viewport/) に freeze しています。

## License

このプロジェクトは、GPL v3以降の下でライセンスされています - 詳細は [LICENSE](LICENSE) ファイルを参照してください。
