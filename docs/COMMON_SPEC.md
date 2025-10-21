# COMMON_SPEC.md (Common Template for Swift / SwiftUI Projects)

## 目的
* 本ドキュメントでは、本リポジトリ配下で開発する Swift/Swift-UI プロジェクトに於いて AI 伴走開発を行う際の共通仕様・ルールを定義します。

## 技術スタック
* 言語: Swift (最新安定版)
* UI: SwiftUI (最優先)
* プロジェクト管理: Xcode (.xcodeproj or SwiftPM)
* 最低対応OS: iOS 16 / macOS 12
* 依存管理: [Swift Package Manager (SPM)](https://docs.swift.org/swiftpm/documentation/packagemanagerdocs/)
* テスト: XCTest + SnapshotTesting

## 開発ルール
* Swift Concurrency (async/await, actor) を優先的に使用します。
* UIKit / AppKit の使用は最小限に抑えてください。
* モジュール化 (Feature 単位で分割) を推奨します。
* Assets.xcassets にリソースを統合します。
* 定数・文字列は Localizable.strings / .stringsdict を利用します。
* 設計パターン: MVVM を基本とします。
* 実装修正の着手は、人間が許可してから行なってください。

## プロジェクト構成

```
プロジェクト名/
├── SPEC.md
├── プロジェクト名.swift
├── ContentView.swift
├── Localizable.strings (Base, ja, en, …)
├── Assets.xcassets
├── Tests/
├── UITests/
└── Preview Content/
```

## 国際化・ローカライズ
* `Localizable.strings`, `Localizable.stringsdict` を必ず使用してください。
  * ⚠️ Xcode 26.0 以降では **Strings Catalog (.xcstrings)** が推奨。→ 可能であれば `Localizable.xcstrings` を利用し、翻訳管理を効率化。
* 翻訳キーはコメント付きで管理します。
* Markdown 対応コメントを推奨します。

## コーディング規約
* [SwiftFormat](https://formulae.brew.sh/formula/swiftformat#default) でソースコードを自動整形してください。
* [SwiftLint](https://formulae.brew.sh/formula/swiftlint) でソースコードを検査してください。
* 1ファイル1型を原則とします。
* クラスより struct / enum を優先します。
* 依存注入は、プロトコルベースとします。

## デザイン規約
* macOS ネイティブなデザイン ([macOS Human Interface Guidelines](https://developer.apple.com/jp/design/human-interface-guidelines/)) に準拠します。
* タイトル・テキストは San Francisco フォントを使用します。

## テスト方針
* [Swift Testing](https://developer.apple.com/documentation/testing) を参照してください。
* XCTest によるユニットテストを必須にしてください。
* UI コンポーネントは SnapshotTesting を実施してください。
* カバレッジ目標は 70% 以上とします。

## ドキュメント
* 各プロジェクトの個別仕様は、各リポジトリ内の `SPEC.md` に記載します。
* 各プロジェクトでは、必要に応じて派生 `SPEC.md` を追加することがあります。

---

## Appendix A: Xcode プロジェクト作成ウィザード推奨選択肢リスト

* AI 伴走開発を前提とする各プロジェクトでの、Xcode における新規プロジェクト作成時の推奨選択肢を以下にまとめます。
* 本リストは Xcode 26.0.1 におけるウィザード構成・オプションに準拠します。

### 1. テンプレート選択

* **Platform**: macOS
  * ⚠️ `SPEC.md` に基づいて選択してください。

* **Template**: App
  * ⚠️ Xcode 26.0 以降では「Multiplatform」や「Document App」も選択肢として提示されるが、**macOS 専用** プロジェクトでは「App」を選択してください。

### 2. プロジェクト設定

| 項目 | 推奨値 | 理由 |
|--|--|--|
| Product Name |  | `SPEC.md` のプロダクト名と一致 |
| Team | Apple ID に応じて設定 | コード署名のため |
| Organization Identifier | `com.s2j` | ドメイン逆引き規則、一貫性確保 |
| Interface | SwiftUI | SwiftUI ベースを前提 |
| Language | Swift (Swift 7.0) | Xcode 26.0.1 に同梱される Swift バージョン (Objective-C は不要) |
| Use Core Data |  | `SPEC.md` に基づく |
| Include Tests | On | `SPEC.md` に基づきテストを考慮 |
| Include CloudKit |  | `SPEC.md` に基づく |
| Include Document Group |  | `SPEC.md` に基づく |
| Source Control | On (Git) | `SPEC.md` / GitHub 運用をリンクさせるため |

### 3. プロジェクト作成後の初期調整

* **Navigator に SPEC.md を追加**
  * Finder から `SPEC.md` をプロジェクトルートへ配置
  * Xcode の **Project Navigator** にドラッグ & ドロップ
  * 「Add to targets」は **チェックなし** (ビルド不要)

  * 後からの確認手順

    * [ ] `SPEC.md` を選択し、右ペイン **File Inspector** を開く (`⌥⌘1`)
    * [ ] 「Target Membership」がすべて **未チェック** であることを確認

  * 誤ってチェックが入っていた場合の解除手順

    * [ ] **File Inspector → Target Membership** で対象ターゲットのチェックを外す
    * [ ] メニューから **Product > Clean Build Folder…** (`⇧⌘K`) を実行
    * [ ] 再ビルドし、`SPEC.md` がビルドに含まれないことを確認

### 4. 追加推奨設定

#### `Info.plist`
  * `CFBundleName` → プロジェクト名
  * `CFBundleShortVersionString` / `CFBundleVersion` → `SPEC.md` の Version 章と同期

#### ローカライズ
  * Base, English, Japanese を初期追加

#### エディタ設定
  * Text Editing → Show Line Numbers: On
  * Editor → Syntax Coloring: Swift / Markdown を有効化
  * Indentation → Spaces: 4 (プロジェクト標準に従う)

#### **ビルド設定**
  * Deployment Target: macOS 16.0 以上
  * Swift Concurrency チェック: Strict に設定
  * Dead Code Stripping: 有効化

### 5. 補足 (Xcode 26 特有の推奨事項)

* **Preview on device** を積極活用 (SwiftUI Preview + 実機同期)
* **SwiftData / Observable** 等の新 API 利用時は、Appendix に仕様を追記
* **Package Dependencies** は SwiftPM を利用し、外部ライブラリを導入する場合は `SPEC.md` に明記

---

## Appendix B: Spec Driven Developing Rule (AI-伴走開発ガイドライン)

* この付録は、Xcode 26 における AI 伴走型開発 (例：ChatGPT in Xcode、Claude in Xcode) を **SPEC 準拠** で進めるための運用ルール群を定めるものです。
* 本ガイドラインは必須ではなく、「SPEC を唯一の正典としつつ AI を活用する開発スタイル」を推進するための補助ルールと考えてください。

### 1. 用語定義

| 用語 | 意味 |
|---|---|
| SPEC | `COMMON_SPEC.md` をはじめとする、開発仕様・要件を定めた文書群 |
| `CLAUDE.md` | 設計思想、コーディング規範、レビュー方針などを体系化した補助仕様書 |
| AIモデル | ChatGPT in Xcode や Claude in Xcode などの、コード生成／レビュー支援に使う AI モジュール |
| Prompt | AI に投げる指示文。AI をどう動かすかを制御するためのテキスト |

### 2. 基本原則

1. **SPEC を唯一の最終参照源とする**

  * すべての実装判断、動作仕様、入出力仕様はまず SPEC に記述されていることを前提とします。
  * AI による生成物が SPEC と整合しない場合、SPEC 側を優先し、AI に修正を指示します。

2. **補助仕様 (CLAUDE.md 等) は設計規範／レビュー軸として用いる**

  * プロジェクト固有の設計思想、命名規則、責務分割ルールなどを `CLAUDE.md` に定義しておいてください。  
  * AI に対して“CLAUDE.md に従って実装してください”という旨を常にプロンプトに含めてください。

3. **Prompt 運用ルールを定義・共有する**

  * 各機能あるいはモジュール単位で、標準プロンプト定型テンプレート (雛形) をチームで決めておいてください。
  * Prompt に「参照すべき SPEC のセクション」「設計規範を指定するファイル名 (例：CLAUDE.md)」「禁止事項・注意事項」などを明示してください。

4. **AI 出力の自律チェック／レビューを欠かさない**

  * AI が生成したコード／ドキュメントは必ず人がレビューし、SPEC 準拠かをチェックしてください。
  * レビュー時には以下観点を最低限確認してください：

    * a) 入出力仕様が合っているか
    * b) 例外処理やエッジケース対応が抜けていないか
    * c) 命名規則・責務分割・モジュール境界が設計方針 (CLAUDE.md) に準じているか
    * d) セキュリティ／パフォーマンス上の懸念点がないか

5. **AI 生成と手動修正の境界を設ける**

  * 単純な CRUD や定型的な処理は AI に生成させ、複雑な制御や設計判断のある箇所は手動実装または人による指示付き生成とします。
  * AI 出力後、必ず人が「なぜその設計になったか」をコメントで残す (AI 理由説明コメント含む)。

6. **継続的な Prompt チューニングと振り返り**

  * AI に出力させたコード品質を定期的に振り返り、プロンプト改善案をチームで共有してください。
  * Prompt による逸脱や誤生成が続く場合、SPEC や CLAUDE.md の記述を再精査し、曖昧性を排除してください。

### 3. ChatGPT in Xcode 用運用補足

* プロンプトの明示形式例：

  > 「COMMON_SPEC.md の §4.2 入出力仕様に従って、この関数 `fetchUserProfile` を Swift で実装してください。命名規則・例外処理は CLAUDE.md に従ってください。」

* ChatGPT は補完的な提案をしやすいため、必要な前提をすべてプロンプトに盛り込んでください (例：非同期モデル、エラー伝播方針、戻り値型など)。
* 生成されたコードの **差分レビュー** を重視してください。AI が不要な冗長コードや過剰最適化を出すこともあるため、人による簡潔化判断を入れてください。

### 4. Claude in Xcode 用運用補足

* Claude に対しては「設計・レビューモード」での使用が有効です。プロンプト例：

  > 「COMMON_SPEC.md と CLAUDE.md に準拠して、このモジュール全体の設計レビューを行ってください。命名不整合、責務オーバーラップ、拡張性／保守性観点での改善案を示してください。」

* 長文コンテキストをもたせても破綻しにくいため、モジュール横断的な整合性チェックや設計レビューを Claude に任せる運用とします。
* 必要に応じて、Claude に「SPEC と CLAUDE の不一致箇所を抽出してください」という命令を出して、仕様のズレを早期発見する流れを導入するとよいです。

### 5. 運用フロー (AI + 人の協調ループ)

* このループを回すことで、AI 伴走型開発でも **SPEC に準拠するリズム** を保つことを目指します。

1. 開発タスク発生 → 関連 SPEC セクションを明示
2. AI (ChatGPT) に対してプロンプトを投げ、初期コード生成
3. 人がレビュー (SPEC 準拠性／設計妥当性チェック)
4. Claude で設計観点レビュー or 整合性チェック
5. 必要あれば SPEC／CLAUDE.md を修正
6. ChatGPT に修正プロンプトを送り再出力 → 最終レビュー → マージ

### 6. 注意・制限事項

* AI はあくまで“補助ツール”であり、設計判断や要件解釈の最終責任は開発者にあることを忘れないでください。
* プロンプト過多／過重は逆に AI の出力を不安定にすることもあるため、適度な情報量に絞る工夫が必要です。
* 機密情報や認証キーなどはプロンプトに含めないよう注意してください (プロンプトが外部送信されるケースを想定)。

---

## Appendix C: Git 運用ルール

* Git 管理下に含めるべきファイル・含めないファイルを明確にし、環境依存やビルド成果物を排除することで、再現性の高い開発環境を維持します。

### 1. 運用ルール

* `.gitignore` は **リポジトリルートに設置**し、全員が共通利用します。
* `Package.resolved` のコミット有無は、チーム方針に従ってください。
* 秘密情報 (API キーなど) を含む `.env` 系ファイルは必ず ignore してください。
* Xcode のユーザー個別設定 (`xcuserdata`) はコミットしないでください。
* 新規依存管理ツール導入時は `.gitignore` を更新し、Appendix B に追記してください。

### 2. 管理対象ファイル／除外対象ファイル

| ディレクトリ／ファイル | 管理対象 | 備考 |
|--|--|--|
| `xcshareddata` | ✅ | チーム共有設定 (例: scheme, WorkspaceSettings) |
| `xcuserdata` | ❌ | 個々人の環境依存データ (breakpoints、user-specific settings 等) |
| `*.xcworkspace/contents.xcworkspacedata` | ✅ | ワークスペース構成情報 |
| `*.xcworkspace/xcshareddata/WorkspaceSettings.xcsettings` | ✅ | 共有ワークスペース設定 |
| `build/`、`DerivedData/`、`*.xcarchive` 等 | ❌ | ビルド生成物・成果物 |
| `.DS_Store`, `*.log`, `*.swp` など | ❌ | OS やエディタ由来の一時ファイル |
| `Pods/`, `Carthage/Build/` (使用する場合) | ❌ | 外部依存のビルド成果物 |
| `.build/`, `Packages/` 等 | ❌ | SwiftPM のビルドキャッシュ等 |

#### 1. Git 管理に含めるべきファイル

* プロジェクトファイル: `*.xcodeproj`, `*.xcworkspace`
* 設定ファイル: `Info.plist`, `SPEC.md`
* リソース: `Assets.xcassets`, `Localizable.strings`
* ソースコード: `*.swift` など
* 必要に応じて `Package.resolved` (依存バージョン固定を優先する場合)

#### 2. Git 管理から除外すべきファイル・フォルダ

##### macOS 系

```gitignore
.DS_Store
.AppleDouble
.LSOverride
```

##### Xcode

```gitignore
build/
DerivedData/
*.xcworkspace/xcuserdata/
*.xcuserstate
*.xcscmblueprint
```

##### Swift Package Manager

```gitignore
/.build
/Packages
# 依存固定を許可するなら次を除外しない
# /Package.resolved
```

##### CocoaPods (利用する場合のみ)

```gitignore
Pods/
*.lock
!.gitignore
```

##### Carthage (利用する場合のみ)

```gitignore
Carthage/Build/
```

##### Fastlane

```gitignore
fastlane/report.xml
fastlane/Preview.html
fastlane/screenshots/**/*.png
fastlane/test_output
```

##### アーカイブ / IPA

```gitignore
*.xcarchive
*.ipa
*.dSYM.zip
*.dSYM
```

##### Playground

```gitignore
timeline.xctimeline
playground.xcworkspace
```

##### ログ / 一時ファイル

```gitignore
*.log
*.swp
*.swo
*.tmp
```

##### 環境依存ファイル

```gitignore
*.env
*.local
```

### 3. 既に管理対象になっているファイルを解除する手順

1. ターミナルでリポジトリルートに移動
2. 以下を実行してキャッシュから除外 (Git の管理対象から外す)

   ```bash
   git rm --cached -r path/to/除外したいディレクトリ_or_ファイル
   ```
3. `.gitignore` を更新 (該当パターンを追加)
4. コミットしてプッシュ

   ```bash
   git commit -m "Remove unwanted files from repo and update .gitignore"
   git push
   ```

### 4. ブランチ戦略とコミット規約

* **ブランチ戦略**

  * `main` (または `master`): 常に安定／リリース可能な状態
  * `feature/xxx`: 新機能開発用ブランチ
  * `fix/xxx`: バグ修正用ブランチ
  * Pull Request → レビュー ＋ CI 通過後マージ

* **コミットメッセージ規約**

  * `feat: ～` → 機能追加
  * `fix: ～` → バグ修正
  * `chore: ～` → ドキュメント更新、設定変更など
  * `docs: ～` → ドキュメントのみの変更

* **PR／レビュー運用**

  * 少ないファイル差分で提出
  * レビュー承認前に CI が通ること
  * （オプション）マージ前に rebase／squash を行う

---

### 5. CI／テストとの連携ルール

* `.github/workflows` などの CI 設定ファイルは Git 管理対象
* テストスイートが通ることをマージ条件とする
* ビルドキャッシュや生成物は Git 管理しない

---

# **Appendix D: AI 伴走開発ルール**

## 概要

本 Appendix は、Xcode 共通仕様 (COMMON_SPEC) に準拠したプロジェクトにおいて、AI 支援ツール (例: **Cursor**、**Xcode AI Chat**、**GitHub Copilot**、**ChatGPT**) を併用する際の基本方針および運用ルールを定義します。

目的は、**AI 生成物の品質と仕様準拠を維持しつつ、開発効率を最大化すること** です。

---

## 1. 開発フェーズ別の役割分担

| フェーズ | 推奨 AI ツール | 主な役割 | 備考 |
| --- | --- | --- | --- |
| 設計・雛形生成 | **Cursor** / ChatGPT | SPEC.md・COMMON_SPEC の読解、Swift Package 構造の生成 | 長文仕様理解に適する |
| 実装・検証 | **Xcode (AI Chat)** | SwiftUI コード補完、ビルドエラー修正、UI 微調整 | Swift 実行環境と統合 |
| 品質保証・レビュー | **Docs Linter** / SwiftLint / SwiftFormat | コード整形・表記揺れ・Lint 検査 | CI 統合を推奨 |

---

## 2. AI 支援開発の基本原則

1. **AI は設計の補助者であり、仕様の代替ではない。**
   常に SPEC.md および COMMON_SPEC を一次資料とする。
2. **AI 出力物はすべて Git 管理対象とし、生成物のトレーサビリティを確保する。**
   → AI による改変履歴も Pull Request としてレビューする。
3. **仕様の曖昧さを AI に解釈させない。**
   → 必ず「仕様ではこう書かれているが、どちらが適切か？」の質問形式で指示する。
4. **プラットフォーム差異を AI に自動判断させない。**
   → macOS / iPadOS のいずれかを明示し、`#if canImport(AppKit)` / `#if canImport(UIKit)` を常に確認する。
5. **機密・認証情報 (API キー等) を AI に含めない。**

---

## 3. AI ツール間の連携フロー (ハイブリッド開発)

### 標準ワークフロー

```
Cursor: 設計・雛形生成
↓
Git Commit (ai-dev/cursor ブランチ)
↓
Xcode: 実装・ビルド・テスト
↓
Git Commit (ai-dev/xcode ブランチ)
↓
CI: Docs Linter + SwiftLint + Snapshot Testing
↓
Release
```

| 手順 | 内容 | 留意点 |
| --- | --- | --- |
| ① | Cursor に SPEC.md 全体を読み込ませ、Swift Package 雛形を生成 | 「macOS/iPadOS 両対応」と明示 |
| ② | 生成後に `git commit` し、バージョンを固定 | 構成誤差を防ぐ |
| ③ | Xcode で開き、AI チャットに SPEC 抜粋を渡してチューニング | コンテキスト上限に注意 |
| ④ | Docs Linter と SwiftLint を併用 | 出力差を自動整形 |
| ⑤ | PR 時に AI 生成差分を明示 | レビュー担当者が仕様逸脱を確認可能 |

---

## 4. AI との効果的な対話スタイル

| 状況 | 望ましい指示文 | 理由 |
| --- | --- | --- |
| コード生成依頼時 | 「`AboutView.swift` に MarkdownView を統合して」 | ファイル単位で明示的に指示 |
| 仕様判断が必要な時 | 「SPEC.md ではこうだが、SwiftUI 側ではどう扱うべき？」 | 誤解防止・仕様遵守確認 |
| テスト修正時 | 「`S2JAboutWindowTests` に SnapshotTesting を追加して」 | 構造理解を促す |
| macOS/iPadOS 切替時 | 「macOS では NSWindow を使い、iPad では .sheet を使う前提」 | 平行定義を防ぐ |

---

## 5. 品質保証と CI 連携

### 5.1 自動検査ツール

| ツール | 検査対象 | 実行タイミング |
| --- | --- | --- |
| Docs Linter | ドキュメントの表記揺れ・文体統一 | PR 時 |
| SwiftLint / SwiftFormat | Swift コード規約 | コミット時または CI |
| SnapshotTesting | SwiftUI ビューの UI 再現性 | テスト実行時 |

### 5.2 推奨 GitHub Actions

```yaml
# .github/workflows/ai-ci.yml
name: AI CI
on: [push, pull_request]
jobs:
  test:
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: true
      - uses: actions/setup-node@v4
        with:
          node-version: 22
      - run: cd tools/docs-linter && npm ci && npm run lint
      - run: swift test --enable-code-coverage
```

---

## 6. 注意事項 (セキュリティ・倫理)

* AI が参照する文書には、ライセンス上の制限を確認すること。
* OSS コードを AI 出力に取り込む際は、元のライセンス表記を保持する。
* AI モデルが外部サーバーにデータ送信を行う場合、内部コードを含めない。

---

## 7. 推奨運用ルール (プロジェクト横断)

| 項目 | 内容 |
| --- | --- |
| **命名規則** | AI 生成コードも COMMON_SPEC の命名基準に従う (`S2J` プレフィックス等) |
| **Backlog 更新** | AI による仕様変更提案は SPEC.md の Backlog に反映 |
| **学習資料** | AI に参照させる文書は `/docs/` 配下に統一配置 |
| **レビュー** | AI 生成物は常に人間レビューを通過してマージ |

---

## 8. まとめ

AI 伴走開発は、**「AI に設計を任せる」のではなく、「AI に仕様遵守を監督させる」** ことで真価を発揮します。
Cursor と Xcode AI を組み合わせることで、

* 設計段階での仕様整合
* 実装段階での動作品質

を同時に確保できます。

この Appendix は、AI 支援を標準開発プロセスに組み込むための指針として利用してください。

---

## FAQ: 削除ファイルの扱い

### Q1. ローカルで削除したが、まだコミットしていない場合に「Hunk を戻す」を押すと？

* 削除が取り消され、ファイルは直前の Git 管理下の内容で復活する。

### Q2. ローカルで削除をコミット済みの場合に「Hunk を戻す」を押すと？

* 既に履歴に削除が残っているため、そのままでは復活しない。復元するには `git restore` や「履歴から復元」を実行する必要がある。

### Q3. リモート (GitHub) 側で削除されたが、ローカルにファイルが残っている場合は？

* **まだ pull していない場合**: ローカルの Git クライアントは削除を認識していないため、削除差分自体が表示されない。この場合「Hunk を戻す」対象にならない。
* **pull 済みで削除差分が反映された場合**: ローカルの Git クライアント上で「削除されたファイル」と表示される。ここで「Hunk を戻す」を押すと、削除が取り消されファイルが復活する。
* **pull 済みでローカルに未コミット変更がある場合**: 「リモート削除 vs ローカル変更」の競合になる。この場合「Hunk を戻す」を押すと、削除が取り消され、ローカル編集を残したままファイルが復活する。
