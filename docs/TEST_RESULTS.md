# Xcode Common Specs - テスト結果 (テンプレート)

## 目的

* 下流アプリの `./docs_mod/test-results.md` のひな型である。本リポジトリ自身の自動テスト結果ではない。
* 自動テストと、Terminal / Simulator / エミュレータ / 実機の確認を、同じ印で一覧する。
* 実機側の合否は、`./test/manual-tests.json` を正本とし、再実行で本ファイルに反映する。

## 結果マーク

| マーク | 意味 |
| --- | --- |
| PASS | 条件を満たす |
| WARN | 条件未達だが、(環境依存などで) 意図的に許容 |
| SKIP | 環境がなく、今回は実行しない |
| PENDING (自動) | 自動テスト未実装 |
| PENDING (実機) | その OS で未実施 |
| FAIL | 条件未達 (要修正) |

## 暫定ターゲット

| OS | 簡便確認の場 | 実機を使う場合 |
| --- | --- | --- |
| macOS | Terminal.app | macOS 26.6.1 "Tahoe" |
| iOS | Simulator | iPhone 16 Pro Max (iOS v26.6) |
| iPadOS | Simulator | iPad Air 5G (iPadOS v26.6) |
| Android | エミュレータ | Galaxy A25 5G (Android v16 / One UI v8.5) |

## 自動テスト

| ID | 条件 | 結果 |
| --- | --- | --- |
| AT-001 | 移植層の純関数テストが通る | PENDING (自動) |
| AT-002 | UI コンポーネントの SnapshotTesting が通る | PENDING (自動) |
| AT-003 | 複数 Viewport (Portrait / Landscape / 狭幅) の SnapshotTesting が通る | PENDING (自動) |
| AT-004 | 横スクロールの始端・終端で、最初と最後の項目が可視矩形に完全包含される | PENDING (自動) |

## 簡便確認 (Terminal / Simulator / エミュレータ)

| ID | OS | 方法 | 条件 | 結果 |
| --- | --- | --- | --- | --- |
| MT-macos-001 | macOS | Terminal.app | 起動とログを Terminal から観測できる | PENDING (実機) |
| MT-ios-001 | iOS | Simulator | 主画面が Simulator で表示される | PENDING (実機) |
| MT-ios-002 | iOS | Simulator | 画面幅を超える横並び項目を終端までスクロールし、最後の項目が完全に見える | PENDING (実機) |
| MT-ios-003 | iOS | Simulator | 起動直後に、左寄せから中央寄せ等の中間レイアウトが見えない | PENDING (実機) |
| MT-ipados-001 | iPadOS | Simulator | 主画面が Simulator で表示される | PENDING (実機) |
| MT-ipados-002 | iPadOS | Simulator | Landscape 起動・回転で、レイアウトの中間状態が見えない | PENDING (実機) |
| MT-ipados-003 | iPadOS | Simulator | Split View の狭幅でも、表示中の操作対象に到達できる | PENDING (実機) |
| MT-android-001 | Android | エミュレータ | 主画面がエミュレータで表示される | PENDING (実機) |
| MT-android-002 | Android | エミュレータ | 画面幅を超える横並び項目を終端までスクロールし、最後の項目が完全に見える | PENDING (実機) |

## `manual-tests.json` の形

```json
{
  "version": 1,
  "targets": {
    "macos": "macOS 26.6.1 Tahoe",
    "ios": "iPhone 16 Pro Max (iOS v26.6)",
    "ipados": "iPad Air 5G (iPadOS v26.6)",
    "android": "Galaxy A25 5G (Android v16 / One UI v8.5)"
  },
  "cases": [
    {
      "id": "MT-ios-001",
      "os": "ios",
      "title": "主画面が表示される",
      "method": "simulator",
      "result": "PENDING",
      "note": ""
    }
  ]
}
```

* `method` は、`terminal` (macOS)、`simulator` (iOS / iPadOS / Android)、`device` (暫定ターゲット実機) のいずれかとする。
* `result` は、`PASS` / `WARN` / `SKIP` / `PENDING` / `FAIL` である。本 Markdown に写すとき、自動未実装は `PENDING (自動)`、確認未実施は `PENDING (実機)` と表記する。
