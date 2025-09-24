# COMMON_SPEC.md (Common Template for Swift / SwiftUI Projects)

## 目的
- 本ドキュメントでは、AI 伴走開発を行う際の共通仕様・ルールを定義します。

## 技術スタック
- 言語: Swift (最新安定版)
- UI: SwiftUI (最優先)
- プロジェクト管理: Xcode (.xcodeproj or SwiftPM)
- 最低対応OS: iOS 16 / macOS 12
- 依存管理: [Swift Package Manager (SPM)](https://docs.swift.org/swiftpm/documentation/packagemanagerdocs/)
- テスト: XCTest + SnapshotTesting

## 開発ルール
- Swift Concurrency (async/await, actor) を優先的に使用します。
- UIKit / AppKit の使用は最小限に抑えてください。
- モジュール化 (Feature 単位で分割) を推奨します。
- Assets.xcassets にリソースを統合します。
- 定数・文字列は Localizable.strings / .stringsdict を利用します。
- 設計パターン: MVVM を基本とします。
- 実装修正の着手は、人間が許可してから行なってください。

## ローカライズ
- Localizable.strings, Localizable.stringsdict を必ず使用してください。
- 翻訳キーはコメント付きで管理します。
- Markdown 対応コメントを推奨します。

## コーディング規約
- [SwiftFormat](https://formulae.brew.sh/formula/swiftformat#default) でソースコードを自動整形し、[SwiftLint](https://formulae.brew.sh/formula/swiftlint) でソースコードを検査してください。
- 1ファイル1型を原則とします。
- クラスより struct / enum を優先します。
- 依存注入は、プロトコルベースとします。

## テスト方針
- XCTest によるユニットテストを必須にしてください。
- UI コンポーネントは SnapshotTesting を実施してください。
- カバレッジ 70%以上を目標とします。
- [Swift Testing](https://developer.apple.com/documentation/testing) を参照してください。

## ドキュメント
- SPEC.md を AI伴走開発の基準とする
- 必要に応じて派生 SPEC.md を追加
