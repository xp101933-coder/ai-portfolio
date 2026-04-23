# Portfolio Enhancements Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** ポートフォリオサイトに5つの改善（アニメーション・About・Contact・ダークモード・OGP/SEO）を追加する

**Architecture:** すべての変更は `index.html` 単一ファイルに集約。CSS変数・JS関数ベースの設計を維持し、セクションを追加する形で拡張する。SEO用に `sitemap.xml` を新規作成。

**Tech Stack:** HTML/CSS/JS (vanilla), GitHub Pages, Intersection Observer API

---

### Task 1: アニメーション強化

**Files:**
- Modify: `index.html` — CSS keyframes・JS アニメーション処理

- [ ] Hero入場アニメーション（badge→title→sub→statsの順にfade+slide）
- [ ] ナビバーのスクロール時shadow付与
- [ ] フィルター切替時のカードfade再生
- [ ] カードhoverにscale(1.02)追加

### Task 2: About/スキルセクション

**Files:**
- Modify: `index.html` — hero直後に`#about`セクション追加

- [ ] プロフィール一言テキスト
- [ ] スキルバッジ（Claude Code / Claude API / Python / Whisper / ffmpeg / Node.js / GitHub Actions）

### Task 3: Contactセクション

**Files:**
- Modify: `index.html` — `#works`直後に`#contact`セクション追加

- [ ] メール・X・GitHub・noteをカード3枚で表示
- [ ] navのContactリンクを`#contact`に変更

### Task 4: ダークモード

**Files:**
- Modify: `index.html` — CSS変数にdark定義・JSトグル・ナビボタン

- [ ] `[data-theme="dark"]` セレクタで全変数上書き
- [ ] `prefers-color-scheme: dark` でデフォルト自動検出
- [ ] ナビ右端にトグルボタン（🌙/☀️）

### Task 5: OGP/SEO強化

**Files:**
- Modify: `index.html` — `<head>`にmeta追加・JSON-LD追加
- Create: `sitemap.xml`

- [ ] og:title / og:description / og:image / og:url
- [ ] twitter:card / twitter:site
- [ ] JSON-LD（Person type）
- [ ] `sitemap.xml` 作成

### Task 6: コミット＆デプロイ

- [ ] `git add -A && git commit`
- [ ] `git push origin main`
