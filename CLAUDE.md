# CLAUDE.md — BLENDER ROOM

アニメ背景美術のための Blender 入門講座（全15回）の静的サイト。GitHub Pages で公開している。

- リポジトリ: https://github.com/maso1737/Blender_ROOM_text
- 公開URL: https://maso1737.github.io/Blender_ROOM_text/
- デプロイ: `main` ブランチの `/` (root) を GitHub Pages が自動ビルド。push すれば1〜2分で反映。

## ページ構成

| ファイル | 役割 |
|---|---|
| `index.html` | **トップ＝オープニング動画**。THREE.js による30秒ループのキネティック映像。 |
| `start.html` | シラバスマップ（全15回一覧）。各回カードから `01.html`〜`15.html` へリンク。 |
| `01.html`〜`15.html` | 各講義ページ。 |
| `support.js` | DC ランタイム（React/`x-dc`）。**自動生成物。手で編集しない。** |
| `text-edit.js` / `image-slot.js` | 講義ページ上のインライン編集・画像スロット用ツール。 |
| `_ds/kinetic-stage-design-system-…/` | デザインシステム一式。`index.html` の描画に必須（欠けると動画が出ない）。 |
| `assets/` | 画像。`blender_logo.png` と `assets/op/`（オープニングの静止画）。 |

## オープニング（index.html）の要点

- `support.js` が `<x-dc>` を React コンポーネント化して起動する（DC ランタイム）。ロジックは `<script type="text/x-dc">` 内の `class Component extends DCLogic`。
- 背景の3Dは **THREE.js 0.136 を unpkg の CDN から読み込む**（`three.min.js` / `SVGLoader.js` / `polygon-clipping`）。CDN ロード完了を待って `init3D()`。
- **画像は必ず `assets/op/` を参照する。** 作業フォルダ `OP/` は gitignore 対象で GitHub に上がらないため、`OP/` を直接参照すると本番で画像が欠ける（過去に発生済み）。
  - `assets/op/s1_room_setting.png`（3Dテクスチャ）, `assets/op/1〜3.png`（撮影/光/色のカット）。
- **一度見たら自動スキップ:** `start.html` へ飛ぶ。フラグはパス単位のキー `blroom_seen_opening:<path>` で localStorage に保存（リポジトリ名が変わっても混ざらない）。
  - フラグが立つのは**「スタートへ →」等のボタンを押したときだけ**（ループ再生では立てない）。
  - `?replay=1` を付けると自動スキップを無効化して必ず再生。`start.html` の「BLENDER ROOM」ロゴと、GitHub About 欄の URL にこのパラメータを付けてある。

## start.html の要点

- 講義データは末尾 `<script>` の `CHAPTERS` 配列。カードは JS で描画。
- 公開/準備中の切替は `lectureCard()` の `ready = lec.no <= 15`（現在は全15回公開）。

## 作業フォルダ / gitignore

`.gitignore` で以下を除外（サイト本体ではない作業物）:
- `/uploads/`, `/OP/`, `/_済/`, `/.claude/`, `.thumbnail`, `.image-slots.state.json`
- `Opening.html`, `*.dc.html`, `BLENDER_ROOM_目次.html`

**`_済/` はゴミ箱フォルダ。** 不要になったファイル・フォルダはここへ移動する（削除ではなく退避）。

## ローカルプレビュー

`.claude/launch.json` の `static`（`python -m http.server`）で配信して確認する。

## Git 運用

- コミット末尾に `Co-Authored-By: Claude <noreply@anthropic.com>` を付ける。
- push 先は `origin`（= Blender_ROOM_text リポジトリ）。

## 関連

- `special-tips/` … 元は `Integrations/Blender/TSR_EL_stage_light/`
  （電球グリッドを Geometry Nodes で組む手順書 `bulb-node-steps.html`、`node-diagrams/` の SVG、
  サンプル `bulb-node-steps_06.blend`、AEプロジェクト `bulb-node-steps.aep`、
  連番PNG `bulb-node-steps_renda2.zip`）。このリポジトリに**コピーして同梱**し、GitHub Pages 上でも
  開けるようにしてある。`start.html` の「第1回からはじめる」ボタン下の
  「【期間限定‼】スペシャルTIPS公開中！」から誘導。
  元ファイルを更新したら手動でコピーし直す（自動同期はしていない）。
