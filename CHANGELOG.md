# Xcode Common Specs - CHANGELOG

## unreleased

## 2.0.4 - 2026-08-31

### Changed

* `docs/SPECS.md` の適用範囲表の表記を整える (「1対1」「Windows 11互換」)
* `@s2j/docs-linter` を ^1.0.22 から ^1.0.23 に更新
* `package.json` のバージョンを v2.0.4に更新

## 2.0.3 - 2026-08-17

### Added

* `docs/KMP_PORTABILITY.md` — iOS / iPadOS の KMP 移植層 (許可 / 禁止)
* `docs/UX_VOCABULARY.md` — iOS 基準の UI 語彙
* `docs/UI_VIEWPORT.md` — Viewport Integrity (到達性・安定)
* `docs/TEST_RESULTS.md` — テスト結果のひな型
* `docs_mod/` — 仕様草案用ディレクトリ (`.gitkeep`)
* `README.md` に License / macOS / iOS / iPadOS / SwiftUI / Android / Jetpack Compose のバッジを追加

### Changed

* 共通仕様を FOP / MVU / Viewport Integrity に合わせて書き直し、公開正本を `docs/SPECS.md` と `docs/CICD.md` に改名
* 旧正本 `docs/COMMON_SPEC.md` と `docs/COMMON_SPEC_CICD.md` を `docs/archive/fop-mvu-viewport/` に freeze
* `README.md` の技術スタックと開発ルールを、公開正本に合わせて更新 (MVVM → MVU、プロトコル DI は I/O 境界に限定、単体テストは Swift Testing)
* `package.json` の `files` を `docs/SPECS.md` に変更し、`lint:docs` の対象を公開正本 `docs/*.md` と草案 `docs_mod/**/*.md` に分離 (archive を除外)
* `package.json` のバージョンを v2.0.3に更新

## 2.0.2 - 2026-08-13

### Added

* `package.json` に `allowScripts` を追加し、`@s2j/docs-linter` の install スクリプトを許可
* `.npmignore` を追加し、npm の gitignore-fallback 警告を回避

### Changed

* `@s2j/docs-linter` を v1.0.22に更新

## 2.0.1 - 2026-07-23

### Added

* `.npmrc` を追加
    * npm 12+ の既定値 `allow-git=none` に対応するため `allow-git=all` を設定 (`@s2j/docs-linter` の GitHub 依存取得用)
    * `legacy-peer-deps=true` を設定

### Changed

* `@s2j/docs-linter` を ^1.0.18 から ^1.0.21 に更新

## 2.0.0 - 2026-06-11

### Changed

* ライセンスを GPL-2.0-or-later から GPL-3.0-or-later に更新 (破壊的変更)
    * `LICENSE` を GNU GPL v3の全文に差し替え
    * `README.md` のライセンス表記を GPL v3以降に更新
* `package.json` のバージョンを v2.0.0に更新
