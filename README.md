# 淵野辺 ルート＆スポット探索マップ

神奈川県相模原市・淵野辺エリアを対象にした、徒歩ルートと周辺スポットの探索マップです。
淵野辺駅から青山学院大学 相模原キャンパスまでの徒歩ルートを地図上に表示し、ルート周辺 500m 以内にある飲食店・コンビニ・公園をカテゴリ別に探せます。

本リポジトリは **SotM Asia 2026 Osaka ハッカソン** の成果物です。

🗺️ 公開ページ: https://furuhashilab.github.io/Hackathon_Oct_YutoWatanabe/

## 主な機能

- **徒歩ルート表示**：淵野辺駅（35.5688, 139.3768）→ 青山学院大学 相模原キャンパス（35.5826, 139.3848）の徒歩ルートを TomTom Routing API で計算して表示
- **出発地・目的地マーカー**：淵野辺駅と青学相模原キャンパスにマーカーを表示
- **周辺スポット検索**：ルートから 500m 以内の飲食店・コンビニ・公園を TomTom POI Search API で検索して表示
- **カテゴリ絞り込み**：「すべて／飲食店／コンビニ／公園」のボタンで表示を切り替え
- **スポット一覧**：ルートからの距離が近い順に一覧表示。クリックすると地図がその場所へ移動
- **自動ズーム**：初期表示でルート全体が収まるように表示範囲を自動調整
- 日本語 UI、スマートフォン表示にも対応

## 使用技術

- [TomTom Maps SDK for JavaScript](https://docs.tomtom.com/maps-sdk-js)（`@tomtom-org/maps-sdk`）
  - `TomTomMap`：地図表示（MapLibre GL JS ベース）
  - `calculateRoute` + `RoutingModule`：徒歩ルートの計算と表示（Routing API）
  - `discoverPlaces` + `PlacesModule`：POI 検索と表示（Search API）
- [TomTom Orbis Maps API](https://developer.tomtom.com/)：ベースマップ・ルーティング・検索
- MapLibre GL JS、Turf.js（ルートからの距離計算・500m バッファ生成）

### 旧API（v1）へのフォールバック

API キーのプランで Orbis Maps が使えない場合（401/403 など）は、起動時に自動で TomTom の旧APIに切り替えます。

| 機能 | 新SDK（Orbis） | フォールバック（旧API） |
| --- | --- | --- |
| 地図 | `TomTomMap`（Orbis スタイル） | Map Display API v1 ラスタタイル `map/1/tile/basic/main/{z}/{x}/{y}.png` |
| 徒歩ルート | `calculateRoute`（`travelMode: 'pedestrian'`） | Routing API v1 `routing/1/calculateRoute/...?travelMode=pedestrian` |
| POI 検索 | `discoverPlaces`（ルートの 500m バッファ内） | Search API v2 `search/2/poiSearch/{query}.json`（ルート沿いに円を並べて検索） |

どちらの場合も、検索結果はルートからの実距離で 500m 以内に絞り込んで表示します。

## ファイル構成

```
index.html   … アプリ本体（単体で動作、ビルド不要）
README.md    … このファイル
LICENSE      … CC BY 4.0
```

SDK とその依存ライブラリは CDN（jsDelivr）から ES Modules として読み込みます（`<script type="importmap">` を使用）。
ビルドは不要で、GitHub Pages や任意の静的サーバーにそのまま置くだけで動作します。

## ローカルでの確認方法

ES Modules を使うため、`file://` ではなくローカルサーバー経由で開いてください。

```bash
python -m http.server 8000
# → http://localhost:8000/ を開く
```

## API キーについて

`index.html` 内の `API_KEY` に TomTom の API キーを設定しています。
ブラウザで動作するため、キーはページを開いた人から見えます。
[TomTom Developer Portal](https://developer.tomtom.com/) でキーの利用ドメインを制限することを推奨します。

## ライセンス

このリポジトリの成果物は [クリエイティブ・コモンズ 表示 4.0 国際（CC BY 4.0）](https://creativecommons.org/licenses/by/4.0/deed.ja) の下で提供されています。詳細は [LICENSE](./LICENSE) を参照してください。

地図データ・検索結果の著作権は TomTom およびそれぞれの提供元に帰属します。

## 作者

YutoWatanabe
