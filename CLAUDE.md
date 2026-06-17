# CLAUDE.md

ドローン飛行計画書 作成ツール — フォーム入力からクライアント提出用PDFを生成するWebツール。

## アーキテクチャ
- **単一ファイル構成**: すべて `drone_flight_plan.html`（約3,000行）に内包。HTML + インラインCSS + 素のJavaScript。
- ビルド工程なし。`package.json`・フレームワーク・バンドラなし。ファイルを直接編集する。
- データ永続化は **localStorage のみ**（サーバDBなし）。

## 実行
```bash
python3 -m http.server 8080   # ローカルサーバ起動（要インターネット接続）
```
`http://localhost:8080/drone_flight_plan.html` を開く。`起動.command` のダブルクリックでも可。
本番: GitHub Pages（https://ito-engineering-jp.github.io/drone-flight-plan/drone_flight_plan.html）。

## 外部依存（CDN・バージョン固定）
- Leaflet 1.9.4（cdnjs）— 地図表示
- pdf.js 3.11.174（unpkg）— 許可書PDF読み込み
- 国土地理院 住所検索API（msearch.gsi.go.jp）でジオコーディング、GSIタイルで地図描画
- **インターネット接続必須**（オフラインでは地図・PDF・ジオコーディングが動かない）

## リリース手順（重要）
バージョンを上げるときは **3か所** を必ず同期する:
1. ヘッダーの `<div class="header-badge">Ver. X.YZ</div>`
2. `CHANGELOG.md`（先頭に新エントリ追加）
3. コミットメッセージ `Ver. X.YZ - 変更内容`

## コード規約
- フォームは8セクション（DOM id `s1`〜`s8`）。
- PDF解析系のヘルパー関数は `_pdf*` プレフィックス。
- localStorage キー: `drone_cases_v1`（案件）, `drone_presets_v1`（プリセット）。

## 注意点
- PDF出力はブラウザ印刷（`window.print()` + `@media print` / `@page A4`）。Chrome/Edge推奨。
- localStorageはブラウザ・端末間で共有されない。
