# AI Portfolio Site Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** AI開発成果物を見せるシングルページポートフォリオサイトを単一HTMLファイルとして構築する。

**Architecture:** `index.html` 1ファイルにHTML・CSS・JSをすべてインラインで記述。プロジェクトデータはJS配列で管理し、フィルタリングとモーダル表示をvanilla JSで実現する。外部CDN不使用でオフライン動作可能。

**Tech Stack:** HTML5, CSS3 (CSS Variables / Grid / Flexbox), Vanilla JavaScript (ES6+)

---

## File Structure

```
課題08/
└── index.html          # 全コードを含む単一ファイル
```

---

### Task 1: HTMLスケルトン + CSS基盤

**Files:**
- Create: `index.html`

- [ ] **Step 1: index.html の基本骨格を作成**

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AI Developer Portfolio</title>
  <style>
    /* === CSS Variables === */
    :root {
      --grad: linear-gradient(90deg, #7c3aed, #0ea5e9);
      --grad-bg: linear-gradient(180deg, #faf5ff 0%, #f0f9ff 50%, #f8fafc 100%);
      --bg: #f8fafc;
      --surface: #ffffff;
      --border: #e2e8f0;
      --text: #0f172a;
      --text-muted: #64748b;
      --text-subtle: #94a3b8;
      --purple: #7c3aed;
      --blue: #0ea5e9;
      --radius: 10px;
      --shadow: 0 1px 3px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04);
      --shadow-lg: 0 10px 40px rgba(0,0,0,0.12);
    }

    /* === Reset === */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; background: var(--bg); color: var(--text); line-height: 1.6; }
    button { font-family: inherit; cursor: pointer; border: none; background: none; }
    a { text-decoration: none; color: inherit; }
    img { max-width: 100%; display: block; }
  </style>
</head>
<body>
  <nav id="navbar"></nav>
  <section id="hero"></section>
  <section id="works">
    <div id="filter-bar"></div>
    <div id="card-grid"></div>
  </section>
  <div id="modal-overlay" style="display:none"></div>
  <footer id="footer"></footer>
  <script>
    // JS goes here
  </script>
</body>
</html>
```

- [ ] **Step 2: ブラウザで開いて真っ白な画面が出ることを確認**

```bash
open "/Users/takurou/Documents/08_Claude Code/課題08/index.html"
```

Expected: エラーなく空白ページが表示される。

- [ ] **Step 3: コミット**

```bash
cd "/Users/takurou/Documents/08_Claude Code/課題08"
git init
git add index.html
git commit -m "feat: add HTML skeleton and CSS variables"
```

---

### Task 2: プロジェクトデータ定義

**Files:**
- Modify: `index.html` — `<script>` 内にデータ追加

- [ ] **Step 1: `<script>` タグ内に projects 配列を追記**

`<script>` の中に以下を追加:

```js
const projects = [
  {
    id: 1,
    title: "動画編集自動化 AI",
    category: "claude-code",
    tags: ["Whisper", "ffmpeg", "silero-VAD"],
    emoji: "🎬",
    image: null,
    color: "linear-gradient(135deg, #ede9fe, #dbeafe)",
    summary: "無音・フィラーを自動検出してPremiere Pro用XMLを出力",
    description: "Claude Codeのスキルとして動画のトーク部分を解析。Whisperで文字起こし、silero-VADで無音検出、ffmpegで音量分析を行い、カット編集用のXMLファイルを自動生成する。",
    links: { github: "", x: "" }
  },
  {
    id: 2,
    title: "テロップ自動生成ツール",
    category: "claude-code",
    tags: ["Whisper", "SRT", "Claude API"],
    emoji: "💬",
    image: null,
    color: "linear-gradient(135deg, #dbeafe, #e0f2fe)",
    summary: "編集済み動画からWhisper+Claudeでテロップ用SRTを生成",
    description: "編集済みシーケンスからWhisperで文字起こし後、Claude APIで整文・句読点付与を行い、フレーム同期したSRTファイルを出力する。",
    links: { github: "", x: "" }
  },
  {
    id: 3,
    title: "SEOブログ自動生成",
    category: "automation",
    tags: ["Claude API", "SEO", "Markdown"],
    emoji: "✍️",
    image: null,
    color: "linear-gradient(135deg, #dcfce7, #d1fae5)",
    summary: "キーワードから1万字のSEOブログをAIが自動執筆",
    description: "指定キーワードとターゲット文字数をもとに、Claude APIがリサーチ→構成→執筆を自動実行。char-counterスキルで文字数を検証しながら最適化する。",
    links: { github: "", x: "" }
  },
  {
    id: 4,
    title: "RAGチャットボット",
    category: "claude-api",
    tags: ["Anthropic SDK", "Vector DB", "Python"],
    emoji: "🤖",
    image: null,
    color: "linear-gradient(135deg, #ffe4e6, #fce7f3)",
    summary: "PDF/ドキュメントに基づいて回答するClaudeアプリ",
    description: "社内ドキュメントやPDFをベクトルDBに格納し、Claude APIのRAGパターンで質問応答を実現。Anthropic SDKのprompt cachingで高速・低コストを実現。",
    links: { github: "", x: "" }
  },
  {
    id: 5,
    title: "Claude Code カスタムスキル集",
    category: "claude-code",
    tags: ["Claude Code", "Markdown", "YAML"],
    emoji: "⚡",
    image: null,
    color: "linear-gradient(135deg, #fef3c7, #fde68a)",
    summary: "繰り返し作業を自動化するClaude Codeスキル集",
    description: "analyzer/builder/reviewer パイプライン、summarize、file-summaryなど業務自動化スキルを多数開発。スキルは.mdファイルとして管理しClaude Codeに読み込ませる。",
    links: { github: "", x: "" }
  },
  {
    id: 6,
    title: "ポートフォリオサイト自動生成",
    category: "automation",
    tags: ["HTML/CSS/JS", "Claude Code", "自動化"],
    emoji: "🌐",
    image: null,
    color: "linear-gradient(135deg, #e0e7ff, #ede9fe)",
    summary: "AIとの対話でポートフォリオサイトを自動構築",
    description: "brainstorming → writing-plans → executing-plans のスキルパイプラインを活用し、要件定義から実装まで全工程をClaude Codeで自動化したサイト（このサイト自体）。",
    links: { github: "", x: "" }
  },
  {
    id: 7,
    title: "Xポスト自動生成",
    category: "automation",
    tags: ["Claude API", "X API", "Python"],
    emoji: "𝕏",
    image: null,
    color: "linear-gradient(135deg, #f0fdf4, #dcfce7)",
    summary: "ブログ記事からバズりやすいXポストを自動生成",
    description: "記事URLを入力するとClaude APIが記事を要約し、エンゲージメントを最大化するXポスト文案を複数パターン生成する。",
    links: { github: "", x: "" }
  },
  {
    id: 8,
    title: "Claude API バッチ処理ツール",
    category: "claude-api",
    tags: ["Batch API", "Anthropic SDK", "CSV"],
    emoji: "📊",
    image: null,
    color: "linear-gradient(135deg, #fdf4ff, #fae8ff)",
    summary: "大量データをClaude APIのバッチ処理で低コスト分析",
    description: "Anthropic Message Batch APIを活用し、CSVの大量テキストを50%コスト削減で分類・要約・感情分析する。進捗管理とリトライ機能付き。",
    links: { github: "", x: "" }
  },
  {
    id: 9,
    title: "Webスクレイパー＋AI要約",
    category: "web-app",
    tags: ["Python", "BeautifulSoup", "Claude API"],
    emoji: "🔍",
    image: null,
    color: "linear-gradient(135deg, #fff7ed, #ffedd5)",
    summary: "Webページを取得してAIが自動要約・構造化",
    description: "指定URLのWebページをスクレイプし、Claude APIで要約・カテゴリ分類・キーワード抽出を行う。結果をJSON/Markdownで出力。",
    links: { github: "", x: "" }
  },
  {
    id: 10,
    title: "Gmail × Claude 自動返信",
    category: "automation",
    tags: ["Gmail API", "Claude API", "Google Apps Script"],
    emoji: "📧",
    image: null,
    color: "linear-gradient(135deg, #f0fdf4, #ecfdf5)",
    summary: "受信メールをClaudeが分類・返信案を自動作成",
    description: "Gmail APIで受信メールを取得し、Claude APIで内容を分類・返信文案を生成。Google Apps ScriptからClaude APIを呼び出し、承認後に送信する。",
    links: { github: "", x: "" }
  },
  {
    id: 11,
    title: "音声メモ → タスク変換",
    category: "web-app",
    tags: ["Whisper", "Claude API", "Notion API"],
    emoji: "🎙️",
    image: null,
    color: "linear-gradient(135deg, #eff6ff, #dbeafe)",
    summary: "音声メモをWhisper文字起こし→Claudeでタスク整理",
    description: "録音した音声メモをWhisperで文字起こしし、Claude APIが内容を解析してタスク・期限・優先度に構造化、Notion DBへ自動登録する。",
    links: { github: "", x: "" }
  },
  {
    id: 12,
    title: "コードレビュー自動化",
    category: "claude-api",
    tags: ["GitHub API", "Claude API", "Webhook"],
    emoji: "🔎",
    image: null,
    color: "linear-gradient(135deg, #fdf2f8, #fce7f3)",
    summary: "PRのdiffをClaudeが自動レビュー・コメント投稿",
    description: "GitHub WebhookでPR openイベントを受け取り、diffをClaude APIに送信してコードレビューコメントを自動生成・投稿する。セキュリティ・品質・パフォーマンスの観点で分析。",
    links: { github: "", x: "" }
  }
];

const CATEGORIES = [
  { key: "all",        label: "All" },
  { key: "claude-code",label: "Claude Code" },
  { key: "claude-api", label: "Claude API" },
  { key: "automation", label: "Automation" },
  { key: "web-app",    label: "Web App" }
];
```

- [ ] **Step 2: ブラウザでコンソールエラーがないことを確認**

```bash
open "/Users/takurou/Documents/08_Claude Code/課題08/index.html"
```

ブラウザのDevToolsコンソールでエラーがないことを確認。

- [ ] **Step 3: コミット**

```bash
cd "/Users/takurou/Documents/08_Claude Code/課題08"
git add index.html
git commit -m "feat: add project data array (12 projects)"
```

---

### Task 3: Navbar コンポーネント

**Files:**
- Modify: `index.html` — CSS追加、JS で `#navbar` を描画

- [ ] **Step 1: Navbar の CSS を `<style>` に追加**

```css
/* === Navbar === */
#navbar {
  position: sticky; top: 0; z-index: 100;
  background: rgba(255,255,255,0.85);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--border);
  padding: 0 24px;
  height: 56px;
  display: flex; align-items: center; justify-content: space-between;
}
.nav-logo {
  font-size: 16px; font-weight: 800;
  background: var(--grad); -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  background-clip: text;
}
.nav-links { display: flex; gap: 24px; align-items: center; }
.nav-links a { font-size: 13px; color: var(--text-muted); transition: color 0.2s; }
.nav-links a:hover { color: var(--text); }
.nav-cta {
  font-size: 12px; font-weight: 600; color: white !important;
  background: var(--grad); padding: 6px 16px; border-radius: 20px;
  transition: opacity 0.2s;
}
.nav-cta:hover { opacity: 0.85; }
```

- [ ] **Step 2: JS で Navbar を描画（`<script>` に追加）**

```js
function renderNavbar() {
  document.getElementById('navbar').innerHTML = `
    <span class="nav-logo">takurou.ai</span>
    <nav class="nav-links">
      <a href="#works">Works</a>
      <a href="#about">About</a>
      <a href="#footer" class="nav-cta">Contact</a>
    </nav>
  `;
}
renderNavbar();
```

- [ ] **Step 3: ブラウザで確認**

```bash
open "/Users/takurou/Documents/08_Claude Code/課題08/index.html"
```

Expected: 上部に白いNavbarが表示され、スクロール時にstickyになる。

- [ ] **Step 4: コミット**

```bash
git add index.html
git commit -m "feat: add sticky navbar"
```

---

### Task 4: Hero セクション

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Hero の CSS を `<style>` に追加**

```css
/* === Hero === */
#hero {
  padding: 72px 24px 52px;
  text-align: center;
  background: var(--grad-bg);
}
.hero-badge {
  display: inline-block;
  background: linear-gradient(90deg, #ede9fe, #dbeafe);
  border: 1px solid #c4b5fd;
  color: var(--purple); font-size: 12px; font-weight: 600;
  padding: 4px 14px; border-radius: 20px; margin-bottom: 20px;
}
.hero-title {
  font-size: clamp(28px, 5vw, 48px);
  font-weight: 900; color: var(--text); line-height: 1.2; margin-bottom: 12px;
}
.hero-title .grad-text {
  background: var(--grad); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
}
.hero-sub { font-size: 14px; color: var(--text-muted); margin-bottom: 36px; }
.hero-stats { display: flex; justify-content: center; gap: 40px; }
.hero-stat { text-align: center; }
.hero-stat .num {
  font-size: 32px; font-weight: 900;
  background: var(--grad); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
}
.hero-stat .label { font-size: 11px; color: var(--text-subtle); margin-top: 2px; }
.stat-divider { width: 1px; background: var(--border); }
```

- [ ] **Step 2: JS で Hero を描画（`renderNavbar()` の後に追加）**

```js
function renderHero() {
  document.getElementById('hero').innerHTML = `
    <div class="hero-badge">🤖 AI Developer</div>
    <h1 class="hero-title">AI × 自動化で<br><span class="grad-text">プロダクトを作る</span></h1>
    <p class="hero-sub">Claude Code · Claude API · 業務自動化 · LLMアプリ開発</p>
    <div class="hero-stats">
      <div class="hero-stat"><div class="num">${projects.length}+</div><div class="label">Projects</div></div>
      <div class="stat-divider"></div>
      <div class="hero-stat"><div class="num">3+</div><div class="label">AI Tools</div></div>
      <div class="stat-divider"></div>
      <div class="hero-stat"><div class="num">2025</div><div class="label">Year</div></div>
    </div>
  `;
}
renderHero();
```

- [ ] **Step 3: ブラウザで確認**

Expected: グラデーション背景のHeroセクションに、バッジ・タイトル・統計が表示される。

- [ ] **Step 4: コミット**

```bash
git add index.html
git commit -m "feat: add hero section with stats"
```

---

### Task 5: Filter Bar + カードグリッド

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Works セクション・Filter Bar の CSS を追加**

```css
/* === Works Section === */
#works { padding: 0 24px 60px; max-width: 1100px; margin: 0 auto; }

/* Filter Bar */
#filter-bar {
  display: flex; gap: 8px; flex-wrap: wrap; align-items: center;
  padding: 28px 0 20px;
}
.filter-label { font-size: 12px; color: var(--text-subtle); margin-right: 4px; }
.filter-btn {
  font-size: 12px; font-weight: 500;
  padding: 5px 14px; border-radius: 20px; border: 1px solid var(--border);
  background: var(--surface); color: var(--text-muted);
  cursor: pointer; transition: all 0.2s;
}
.filter-btn:hover { border-color: var(--purple); color: var(--purple); }
.filter-btn.active {
  background: var(--grad); border-color: transparent; color: white; font-weight: 600;
}

/* Card Grid */
#card-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}
```

- [ ] **Step 2: カードの CSS を追加**

```css
/* Cards */
.project-card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--radius); overflow: hidden;
  box-shadow: var(--shadow); cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
}
.project-card:hover { transform: translateY(-3px); box-shadow: 0 8px 24px rgba(0,0,0,0.1); }
.card-thumb {
  height: 120px; display: flex; align-items: center; justify-content: center;
  font-size: 40px; position: relative;
}
.card-thumb img { width: 100%; height: 100%; object-fit: cover; }
.card-category-badge {
  position: absolute; top: 8px; right: 8px;
  font-size: 10px; font-weight: 600; color: white;
  padding: 2px 8px; border-radius: 8px; background: var(--purple);
}
.card-body { padding: 14px; }
.card-title { font-size: 14px; font-weight: 700; color: var(--text); margin-bottom: 4px; }
.card-summary { font-size: 12px; color: var(--text-muted); margin-bottom: 10px; line-height: 1.5; display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; }
.card-tags { display: flex; gap: 4px; flex-wrap: wrap; }
.card-tag {
  font-size: 10px; padding: 2px 7px; border-radius: 6px;
  background: #f1f5f9; color: var(--text-muted);
}
```

- [ ] **Step 3: Filter Bar と Card Grid の JS を追加**

```js
const BADGE_COLORS = {
  "claude-code": "#7c3aed",
  "claude-api":  "#2563eb",
  "automation":  "#16a34a",
  "web-app":     "#ea580c"
};

let activeFilter = "all";

function renderFilterBar() {
  document.getElementById('filter-bar').innerHTML =
    `<span class="filter-label">絞り込み：</span>` +
    CATEGORIES.map(c =>
      `<button class="filter-btn ${activeFilter === c.key ? 'active' : ''}"
         onclick="setFilter('${c.key}')">${c.label}</button>`
    ).join('');
}

function setFilter(key) {
  activeFilter = key;
  renderFilterBar();
  renderCards();
}

function renderCards() {
  const filtered = activeFilter === 'all'
    ? projects
    : projects.filter(p => p.category === activeFilter);

  document.getElementById('card-grid').innerHTML = filtered.map(p => `
    <div class="project-card" onclick="openModal(${p.id})">
      <div class="card-thumb" style="background:${p.color}">
        ${p.image
          ? `<img src="${p.image}" alt="${p.title}">`
          : `<span>${p.emoji}</span>`}
        <span class="card-category-badge" style="background:${BADGE_COLORS[p.category] || '#7c3aed'}">
          ${CATEGORIES.find(c => c.key === p.category)?.label || p.category}
        </span>
      </div>
      <div class="card-body">
        <div class="card-title">${p.title}</div>
        <div class="card-summary">${p.summary}</div>
        <div class="card-tags">${p.tags.map(t => `<span class="card-tag">${t}</span>`).join('')}</div>
      </div>
    </div>
  `).join('');
}

renderFilterBar();
renderCards();
```

- [ ] **Step 4: ブラウザで確認**

Expected:
- フィルターボタンが表示され、クリックするとカードが絞り込まれる
- 12枚のカードが3列グリッドで表示される

- [ ] **Step 5: コミット**

```bash
git add index.html
git commit -m "feat: add filter bar and card grid with 12 projects"
```

---

### Task 6: Modal コンポーネント

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Modal の CSS を追加**

```css
/* === Modal === */
#modal-overlay {
  position: fixed; inset: 0; z-index: 200;
  background: rgba(15,23,42,0.6);
  backdrop-filter: blur(4px);
  display: flex; align-items: center; justify-content: center;
  padding: 20px;
}
.modal {
  background: var(--surface); border-radius: 16px;
  max-width: 640px; width: 100%; max-height: 90vh;
  overflow-y: auto; box-shadow: var(--shadow-lg);
  animation: modal-in 0.2s ease;
}
@keyframes modal-in {
  from { opacity: 0; transform: scale(0.96) translateY(8px); }
  to   { opacity: 1; transform: scale(1) translateY(0); }
}
.modal-thumb {
  height: 200px; display: flex; align-items: center; justify-content: center;
  font-size: 64px; border-radius: 16px 16px 0 0; position: relative;
}
.modal-thumb img { width: 100%; height: 100%; object-fit: cover; border-radius: 16px 16px 0 0; }
.modal-close {
  position: absolute; top: 12px; right: 12px;
  background: rgba(255,255,255,0.9); border-radius: 50%;
  width: 32px; height: 32px; font-size: 16px;
  display: flex; align-items: center; justify-content: center;
  cursor: pointer; border: 1px solid var(--border);
  transition: background 0.2s;
}
.modal-close:hover { background: white; }
.modal-body { padding: 24px; }
.modal-category {
  font-size: 11px; font-weight: 600; color: var(--purple);
  text-transform: uppercase; letter-spacing: 0.05em; margin-bottom: 6px;
}
.modal-title { font-size: 20px; font-weight: 800; color: var(--text); margin-bottom: 10px; }
.modal-desc { font-size: 14px; color: var(--text-muted); line-height: 1.7; margin-bottom: 16px; }
.modal-tags-label { font-size: 11px; font-weight: 600; color: var(--text-subtle); text-transform: uppercase; letter-spacing: 0.05em; margin-bottom: 8px; }
.modal-tags { display: flex; gap: 6px; flex-wrap: wrap; margin-bottom: 20px; }
.modal-tag {
  font-size: 12px; padding: 4px 10px; border-radius: 8px;
  background: linear-gradient(90deg, #ede9fe, #dbeafe);
  color: var(--purple); font-weight: 500;
}
.modal-links { display: flex; gap: 10px; }
.modal-link {
  flex: 1; text-align: center; font-size: 13px; font-weight: 600;
  padding: 10px; border-radius: 8px; border: 1px solid var(--border);
  color: var(--text-muted); transition: all 0.2s;
}
.modal-link:hover { border-color: var(--purple); color: var(--purple); }
.modal-link.primary {
  background: var(--grad); border-color: transparent; color: white;
}
.modal-link.primary:hover { opacity: 0.85; }
```

- [ ] **Step 2: Modal の JS を追加**

```js
function openModal(id) {
  const p = projects.find(x => x.id === id);
  if (!p) return;
  const overlay = document.getElementById('modal-overlay');
  overlay.style.display = 'flex';
  overlay.innerHTML = `
    <div class="modal" onclick="event.stopPropagation()">
      <div class="modal-thumb" style="background:${p.color}">
        ${p.image ? `<img src="${p.image}" alt="${p.title}">` : p.emoji}
        <button class="modal-close" onclick="closeModal()">✕</button>
      </div>
      <div class="modal-body">
        <div class="modal-category">${CATEGORIES.find(c => c.key === p.category)?.label || p.category}</div>
        <div class="modal-title">${p.title}</div>
        <div class="modal-desc">${p.description}</div>
        <div class="modal-tags-label">Tech Stack</div>
        <div class="modal-tags">${p.tags.map(t => `<span class="modal-tag">${t}</span>`).join('')}</div>
        <div class="modal-links">
          ${p.links.github ? `<a href="${p.links.github}" target="_blank" class="modal-link primary">GitHub →</a>` : ''}
          ${p.links.x ? `<a href="${p.links.x}" target="_blank" class="modal-link">𝕏 でシェア</a>` : ''}
          ${!p.links.github && !p.links.x ? '<span class="modal-link" style="opacity:0.5">リンク準備中</span>' : ''}
        </div>
      </div>
    </div>
  `;
  document.body.style.overflow = 'hidden';
}

function closeModal() {
  document.getElementById('modal-overlay').style.display = 'none';
  document.body.style.overflow = '';
}

document.getElementById('modal-overlay').addEventListener('click', closeModal);

document.addEventListener('keydown', e => {
  if (e.key === 'Escape') closeModal();
});
```

- [ ] **Step 3: ブラウザで確認**

Expected:
- カードクリックでモーダルが開き、アニメーションで表示される
- ✕ボタン・オーバーレイクリック・Escキーで閉じる

- [ ] **Step 4: コミット**

```bash
git add index.html
git commit -m "feat: add modal with animation and keyboard support"
```

---

### Task 7: Footer

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Footer の CSS と JS を追加**

CSS:
```css
/* === Footer === */
#footer {
  background: #0f172a; padding: 32px 24px;
  display: flex; justify-content: space-between; align-items: center;
  flex-wrap: wrap; gap: 16px;
}
.footer-logo {
  font-size: 16px; font-weight: 800;
  background: linear-gradient(90deg, #a78bfa, #38bdf8);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
}
.footer-links { display: flex; gap: 20px; }
.footer-links a { font-size: 13px; color: #64748b; transition: color 0.2s; }
.footer-links a:hover { color: #94a3b8; }
.footer-copy { font-size: 11px; color: #475569; width: 100%; text-align: center; margin-top: 8px; }
```

JS (renderNavbar() などの後に追加):
```js
function renderFooter() {
  document.getElementById('footer').innerHTML = `
    <span class="footer-logo">takurou.ai</span>
    <div class="footer-links">
      <a href="https://x.com/" target="_blank">𝕏 Twitter</a>
      <a href="https://github.com/" target="_blank">GitHub</a>
      <a href="https://note.com/" target="_blank">note</a>
    </div>
    <div class="footer-copy">© 2025 takurou — Built with Claude Code</div>
  `;
}
renderFooter();
```

- [ ] **Step 2: ブラウザで確認**

Expected: ページ末尾に濃紺のFooterが表示される。

- [ ] **Step 3: コミット**

```bash
git add index.html
git commit -m "feat: add footer with social links"
```

---

### Task 8: レスポンシブ対応

**Files:**
- Modify: `index.html` — `<style>` にメディアクエリ追加

- [ ] **Step 1: メディアクエリを `<style>` の末尾に追加**

```css
/* === Responsive === */
@media (max-width: 900px) {
  #card-grid { grid-template-columns: repeat(2, 1fr); }
}
@media (max-width: 600px) {
  #card-grid { grid-template-columns: 1fr; }
  .hero-stats { gap: 20px; }
  .hero-stat .num { font-size: 24px; }
  #navbar { padding: 0 16px; }
  .nav-links a:not(.nav-cta) { display: none; }
  #works { padding: 0 16px 48px; }
  .modal { border-radius: 12px; }
  #footer { flex-direction: column; text-align: center; }
  .footer-links { justify-content: center; }
}
```

- [ ] **Step 2: ブラウザのDevToolsでモバイル表示を確認**

DevTools → デバイスツールバー → iPhone SE などに設定。
Expected: 1列グリッド、Navbarが簡略化される。

- [ ] **Step 3: コミット**

```bash
git add index.html
git commit -m "feat: add responsive layout (mobile/tablet)"
```

---

### Task 9: 最終仕上げ＋ブラウザで開く

**Files:**
- Modify: `index.html`

- [ ] **Step 1: `<head>` に OGP メタタグを追加**

```html
<meta property="og:title" content="AI Developer Portfolio">
<meta property="og:description" content="Claude Code・Claude API・自動化ツールの開発実績">
<meta name="description" content="Claude Code・Claude API・業務自動化の開発実績ポートフォリオ">
```

- [ ] **Step 2: スクロールアニメーションを JS に追加**

```js
// カードのフェードインアニメーション
const observer = new IntersectionObserver(entries => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.style.opacity = '1';
      e.target.style.transform = 'translateY(0)';
    }
  });
}, { threshold: 0.1 });

function observeCards() {
  document.querySelectorAll('.project-card').forEach(card => {
    card.style.opacity = '0';
    card.style.transform = 'translateY(16px)';
    card.style.transition = 'opacity 0.4s ease, transform 0.4s ease';
    observer.observe(card);
  });
}
```

`renderCards()` 関数の末尾に `observeCards();` を追加。

- [ ] **Step 3: ブラウザで完成版を開く**

```bash
open "/Users/takurou/Documents/08_Claude Code/課題08/index.html"
```

確認項目:
- [ ] Navbarがstickyで動作する
- [ ] Heroセクションが正しく表示される
- [ ] フィルターボタンが機能する（All/Claude Code/Claude API/Automation/Web App）
- [ ] 12枚のカードが3列で表示される
- [ ] カードをクリックするとモーダルが開く
- [ ] ESCキー・オーバーレイクリックでモーダルが閉じる
- [ ] ウィンドウ幅を縮めると2列→1列に変わる

- [ ] **Step 4: 最終コミット**

```bash
git add index.html
git commit -m "feat: add OGP meta tags, scroll animation — portfolio complete"
```
