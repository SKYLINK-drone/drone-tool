# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 概要

ドローンパイロット管理ツール。パイロット情報を入力し、Instagram 投稿文と LINE 返信文を自動生成する静的 Web アプリ。

## 開発・実行

ビルド不要。ブラウザで直接開く:

```
open index.html
```

依存ライブラリ・パッケージマネージャ・サーバー一切なし。

## アーキテクチャ

`index.html` 1 ファイルに HTML / CSS / JavaScript がすべて含まれる。

### テキスト生成ロジック（JavaScript）

外部 AI API は使用しない。入力テキストへのキーワードマッチングで分岐する。

- `getInputs()` — フォームから値を取得。`account` の `@` を除去。
- `validateInputs()` — 必須 4 項目（エリア・機体・資格・経験）のバリデーション。
- `generateInstagramBody()` — `equipment` / `license` / `experience` をそれぞれ `・,、\n` で分割し、資格ランク（一等/二等）・機体複数所持・経験種別（ウェディング/行政/点検/農業/学校/コンテスト）をフラグ化して文章を組み立てる。
- `generateHashtags()` — エリアマップと機体名・経験キーワードから `#タグ` を動的生成。重複を `Set` で除去。
- `generateLineMessage()` — 同様のフラグを使い、アピールポイントを最大 2 つ `highlightText` に組み込んだ返信文を生成。
- `showInstagram()` / `showLine()` — 生成結果を DOM に反映して出力ブロックを表示。
- `copyText()` — `navigator.clipboard` でクリップボードにコピー。ボタンラベルを 2 秒間変更。

### タブ切り替え

`switchTab(tab)` が `.tab.active` クラスとボタン表示 (`#btn-both` / `#btn-instagram` / `#btn-line`) を切り替える。出力エリア (`output-block`) は `display:none` → `block` でトグル。

### スタイル

LINE 緑 `#06C755`、紫 `#5856D6` をアクセントカラーとして使用。変更時はこれらの色を統一すること。
