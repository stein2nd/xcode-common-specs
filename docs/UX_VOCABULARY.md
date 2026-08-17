# Xcode Common Specs - UI 語彙 (iOS / iPadOS / Android / macOS)

## 目的

* iOS / iPadOS と Android (Jetpack Compose UI) で、ユーザー体験を同一に近づけるための語彙表である。
* 語彙の基準は、iOS / iPadOS の簡便さである。ウィジェットの1対1対応ではない。
* 相手側に無い操作は、共有辞書から外す。
* macOS は、Windows 11を考慮しない。macOS 固有の語彙は、末尾の表に分離し、iOS → Android の辞書に混ぜない。

## 使い方

* 個別 `SPEC.md` の画面定義は、左列の日本語語彙を使う。
* 実装は、中列 / 右列の代表 API に寄せる。同じ語彙でも、OS の標準コンポーネントを使う。
* ここに無い語彙を新しく使う場合は、本表に追加してから画面仕様に書く。

## 共有語彙 (iOS / iPadOS ↔ Android)

| 語彙 | SwiftUI (基準) | Jetpack Compose UI |
| --- | --- | --- |
| 画面 | `NavigationStack` 上の1画面 | `NavHost` の1destination |
| 戻る | 標準の戻る (ツールバー) | `TopAppBar` の戻る |
| 進む | `navigationDestination` | `navController.navigate` |
| タブ | `TabView` | `NavigationBar` / `TabRow` |
| リスト | `List` | `LazyColumn` |
| 詳細 | リストから進んだ先の画面 | 同左 |
| 分割 (iPad) | `NavigationSplitView` | list-detail の adaptive レイアウト |
| シート | `.sheet` | `ModalBottomSheet` |
| ダイアログ | `.alert` | `AlertDialog` |
| 確認 | 破壊的操作の前のダイアログ | 同左 |
| 保存 / キャンセル / 送信 | ツールバーまたはダイアログのアクション名 | 同じアクション名 |
| 読み込み中 | プログレス表示 | 同左 |
| 空 | リストが0件のプレースホルダ | 同左 |
| エラー | 再試行できる提示 | 同左 |
| 検索 | `.searchable` | `SearchBar` / Docked search |
| 設定 | 設定画面 (アプリ内) | 同左 |
| プルして更新 | `.refreshable` | `PullToRefreshBox` |
| Viewport | 画面の可視矩形 (Safe Area を含む) | `WindowInsets` 適用後の可視矩形 |
| 横スクロール列 | 幅が Viewport を超える横並び。終端まで到達できる | `horizontalScroll` / `LazyRow` |

## 状態名 (共有)

画面の State は、下記の語彙で揃えます。実装名は、PascalCase でも構いませんが、仕様書では日本語を使います。

| 語彙 | 意味 |
| --- | --- |
| 初期 | まだロードしていない |
| ロード中 | 取得中 |
| 本文 | 表示できるデータがある |
| 空 | 成功したが0件 |
| エラー | 失敗。再試行手段を持つ |

## 共有しない語彙 (iOS / iPadOS のみ)

Android に同じ体験を要求しません。iOS / iPadOS の個別 `SPEC.md` にだけ書きます。

* スワイプバック
* Dynamic Island / ライブアクティビティ
* コントロールセンター連携
* 3D Touch 相当を必須にした操作
* ホームインジケータ前提のジェスチャ

## macOS 固有 (iOS → Android 辞書の外)

macOS は、HIG に寄せます。次の語彙を iOS / Android の共有辞書に入れてはいけません。

| 語彙 | SwiftUI / AppKit |
| --- | --- |
| ウィンドウ | `Window` / `NSWindow` |
| メニューバー | `CommandMenu` |
| 設定ウィンドウ | `Settings` |
| ツールバー | `Toolbar` (ウィンドウ) |
| キーボードショートカット | `.keyboardShortcut` を第一の操作にしてよい |

## 禁止

* SwiftUI の型名を、Android 仕様の見出しにしない (例: 「NavigationStack 画面」ではなく「画面」)。
* Compose の型名を、iOS 仕様の見出しにしない。
* macOS のメニューバー操作を、iOS / Android の必須操作にしない。
