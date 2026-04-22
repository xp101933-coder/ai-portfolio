# AI Portfolio Site — Design Spec
Date: 2026-04-22

## Overview
AI開発の成果物スクリーンショットをまとめたポートフォリオサイト。転職・案件獲得とSNS発信の両方に使う。純粋なHTML/CSS/JS（ファイル単体で動作、サーバー不要）。

## Design Direction
- **スタイル**: 白ベース＋パープル/シアングラデーションアクセント（B+C mix）
- **カラー**: 背景 #f8fafc、アクセント linear-gradient(90deg, #7c3aed, #0ea5e9)、テキスト #0f172a
- **フォント**: -apple-system, system-ui（ウェブフォント不使用で高速）

## Page Structure
単一HTMLファイル（`index.html`）。JavaScriptでフィルター・モーダルを制御。

### ① Navbar（sticky）
- 左: ロゴ（グラデーションテキスト）
- 右: Works / About / Contact ナビ、Contactはグラデーションボタン
- スクロール時に背景をblur

### ② Hero Section
- バッジ: 「🤖 AI Developer」
- キャッチコピー: 「AI × 自動化でプロダクトを作る」（グラデーション強調）
- サブテキスト: 使用ツール一覧
- 統計カウンター: Projects数 / AI Tools / 年

### ③ Filter Bar
タグボタンでカードをフィルタリング（data属性ベース、JS）。
カテゴリ: All / Claude Code / Claude API / Automation / Web App

### ④ Card Grid（メインコンテンツ）
- 3列グリッド（モバイルは1列、タブレットは2列）
- 各カード: サムネイル画像orプレースホルダー絵文字、カテゴリバッジ、タイトル、概要（2行）、技術タグ
- クリックでモーダルを開く

### ⑤ Modal
- オーバーレイ背景クリックで閉じる
- コンテンツ: スクリーンショット（複数可、横スクロール）、タイトル、詳細説明、使用技術バッジ、GitHubリンク・Xシェアボタン

### ⑥ Footer
- ロゴ + SNSリンク（X / GitHub / note）
- コピーライト

## Data Model
プロジェクトデータはJS配列で管理（`projects` 配列）。各オブジェクト:
```js
{
  id: 1,
  title: "動画編集自動化 AI",
  category: "claude-code",          // フィルター用
  tags: ["whisper", "ffmpeg"],
  emoji: "🎬",                       // サムネイルプレースホルダー
  image: null,                       // 実画像があれば相対パス
  summary: "無音・フィラーを自動検出",
  description: "詳細テキスト...",
  links: { github: "", x: "" }
}
```
実績が増えたらこの配列に追記するだけ。

## Sample Projects（初期データ10件）
Claude Code系・Claude API系・Automation系・Web App系を2〜3件ずつ配置。スクリーンショット未提供分は絵文字＋グラデーション背景で代替。

## Responsiveness
- Desktop: 3列グリッド
- Tablet (≤900px): 2列
- Mobile (≤600px): 1列

## Constraints
- 外部CDN不使用（オフラインでも動作）
- 画像はoptional（絵文字プレースホルダーで成立）
- 単一ファイル（index.html）に全てをインライン

## Out of Scope
- バックエンド / CMS
- 個別プロジェクトページ（モーダルで完結）
- ダークモード切り替え
