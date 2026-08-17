# Xcode Common Specs - Kotlin Multiplatform 移植層

## 目的

* iOS / iPadOS アプリケーションのロジックを、後日 Kotlin Multiplatform に移植しやすい範囲に絞るための許可 / 禁止リストである。
* 戦略は、「後で移植しやすい Swift を書く」とする。最初から `commonMain` にロジックを置く戦略は採用しない。
* macOS アプリケーションに、本リストの遵守は必須ではない。FOP 自体は、`SPECS.md` に従う。

## 層の分け方

| 層 | 置くもの | 置かないもの |
| --- | --- | --- |
| 移植層 (Domain) | DTO、ドメインモデル、判断、キャッシュ方針、統計、バリデーション、ユースケース関数 | SwiftUI、I/O、`Task`、`actor`、プロパティラッパ |
| アダプタ | URLSession、SwiftData、Keychain、UserDefaults、OSLog、Observation | ドメイン判断 |
| View | SwiftUI (Android では Jetpack Compose UI) | ドメイン判断、I/O の直接呼び出し |

## 型の対応 (移植時の写像)

| Swift (移植層) | Kotlin (commonMain) |
| --- | --- |
| `struct` (イミュータブル) | `data class` |
| `enum` (連想値なし) | `enum class` |
| `enum` (連想値あり) | `sealed class` / `sealed interface` |
| `Result` / 型付きエラー | `sealed` な結果型。例外に落とすのはアダプタ |
| 純関数 | `fun` |
| `async` 関数 (副作用なしの合成に限る) | `suspend fun` |
| I/O 境界の `protocol` | `interface` |
| `Codable` な DTO | `kotlinx.serialization` |

## 許可 (移植層)

* イミュータブルな `struct` / `enum` / `let`
* 純関数による判断、バリデーション、統計、キャッシュ方針
* `Result` または型付きエラーの返却
* コレクションの変換 (`map` / `filter` / `reduce` 等)
* 日付・数値・文字列の計算 (Foundation のうち値の計算に限る)
* I/O を持たないユースケース関数 (例: `func loadTimeline(from items: [Item]) -> TimelineState`)

## 禁止 (移植層)

* SwiftUI、UIKit、AppKit
* Observation、Combine、プロパティラッパ (`@State`、`@Observable` 等)
* `URLSession`、SwiftData、Core Data、UserDefaults、Keychain、FileManager
* `actor`、`Task`、`TaskGroup`、`MainActor`
* クラス階層、継承、巨大なサービスオブジェクト
* ドメイン内部の protocol 量産、protocol エクステンションによる実装の押し込み
* `throw` だけに依存し、呼び出し側が握りつぶせるエラー経路
* OSLog、`print`、stdout / stderr

上記は、アダプタまたは View に置きます。アダプタでは `async` / `await` と `actor` を使って構いません。

## ユースケース関数

* 機能の入口は、サブコマンドではなくユースケース関数として明示する。
* 移植層のユースケースは、値を受け取り値を返す。I/O の起動は、アダプタ側の薄い関数に置く。

```
Adapters: fetchFeed() -> Data
Domain:   decodeFeed(_ data: Data) -> Result<Feed, FeedError>
Domain:   reduceFeed(state: FeedState, event: FeedEvent) -> FeedState
View:     表示と Event の発火
```

## テスト

* 移植層は、Swift Testing で純関数としてテストする。ネットワークやディスクを使わない。
* アダプタのテストでは、I/O 境界のプロトコルだけを差し替える。
