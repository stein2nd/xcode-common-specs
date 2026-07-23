# Xcode Common Specs - CHANGELOG

## unreleased

## 2.0.1 - 2026-07-23

### Added

* `.npmrc` を追加
    * npm 12+ の既定値 `allow-git=none` に対応するため `allow-git=all` を設定（`@s2j/docs-linter` の GitHub 依存取得用）
    * `legacy-peer-deps=true` を設定

### Changed

* `@s2j/docs-linter` を ^1.0.18 から ^1.0.21 に更新

## 2.0.0 - 2026-06-11

### Changed

* ライセンスを GPL-2.0-or-later から GPL-3.0-or-later に更新 (破壊的変更)
    * `LICENSE` を GNU GPL v3の全文に差し替え
    * `README.md` のライセンス表記を GPL v3以降に更新
* `package.json` のバージョンを v2.0.0に更新
