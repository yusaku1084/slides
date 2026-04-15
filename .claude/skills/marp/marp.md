# Marpスライド生成スキル

## 概要

このリポジトリのスライドはMarpで作成する。
以下のルールとスタイルを必ず守ること。

---

## 基本構造

```markdown
---
marp: true
theme: default
style: |
  /* スタイル定義 */
paginate: true
---

<!-- スライド1 -->

---

<!-- スライド2 -->
```

`---` でスライドを区切る。

---

## 標準スタイル定義

以下をすべてのスライドのfrontmatterに含めること：

```css
style: |
  section {
    background: #ffffff;
    color: #1a1a2e;
    font-family: 'Helvetica Neue', Arial, sans-serif;
  }
  section.title {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
  }
  section.title h1 {
    font-size: 2.2em;
    font-weight: 800;
    color: white;
    border-bottom: none;
  }
  section.title p {
    font-size: 1em;
    opacity: 0.85;
  }
  h1 {
    color: #667eea;
    font-size: 1.6em;
    border-bottom: 3px solid #667eea;
    padding-bottom: 0.2em;
  }
  h2 {
    color: #764ba2;
    font-size: 1.2em;
  }
  section.problem {
    background: #fff8f0;
  }
  section.problem h1 {
    color: #e05c2a;
    border-bottom: 3px solid #e05c2a;
  }
  section.solution {
    background: #f0f7ff;
  }
  section.solution h1 {
    color: #2a7ae0;
    border-bottom: 3px solid #2a7ae0;
  }
  section.ai {
    background: #f0fff4;
  }
  section.ai h1 {
    color: #27ae60;
    border-bottom: 3px solid #27ae60;
  }
  section.closing {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
  }
  section.closing h1 {
    color: white;
    border-bottom: 3px solid rgba(255,255,255,0.5);
  }
  section.closing code {
    background: rgba(255,255,255,0.2);
    color: white;
    padding: 0.2em 0.4em;
    border-radius: 4px;
  }
  section.closing pre {
    background: rgba(0,0,0,0.3);
    border-radius: 8px;
  }
  section.closing pre code {
    background: transparent;
    color: white;
  }
  .columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2em;
    margin-top: 1em;
  }
  .box {
    background: white;
    border-radius: 12px;
    padding: 1em 1.2em;
    box-shadow: 0 2px 8px rgba(0,0,0,0.08);
  }
  .box.bad {
    border-left: 4px solid #e05c2a;
  }
  .box.good {
    border-left: 4px solid #27ae60;
  }
  .box h2 {
    margin-top: 0;
    font-size: 1em;
  }
  .box.bad h2 { color: #e05c2a; }
  .box.good h2 { color: #27ae60; }
  code {
    background: #f4f4f8;
    color: #764ba2;
    padding: 0.15em 0.4em;
    border-radius: 4px;
    font-size: 0.9em;
  }
  ul {
    line-height: 1.9;
  }
  .highlight {
    background: #fff3cd;
    border-left: 4px solid #ffc107;
    padding: 0.5em 1em;
    border-radius: 0 8px 8px 0;
    margin-top: 1em;
  }
```

---

## 使えるセクションクラス

| クラス | 用途 | 背景色 |
|---|---|---|
| `title` | タイトルスライド | 紫グラデーション |
| `problem` | 問題提起 | オレンジ系 |
| `solution` | 解決策 | ブルー系 |
| `ai` | AI関連 | グリーン系 |
| `closing` | 締め | 紫グラデーション |
| なし | 通常スライド | 白 |

使い方：
```markdown
<!-- _class: problem -->
# 問題提起
```

---

## 使えるHTMLコンポーネント

### 2カラムレイアウト
```html
<div class="columns">
<div class="box bad">

## 😩 Before
内容

</div>
<div class="box good">

## 😊 After
内容

</div>
</div>
```

### ハイライトボックス
```html
<div class="highlight">
強調したい内容
</div>
```

---

## スライド設計のルール

- **1スライド1メッセージ**：伝えたいことは1つに絞る
- **テキスト量**：箇条書きは最大5項目まで
- **コード**：長いコードは分割して複数スライドに
- **ビフォーアフター**：2カラムレイアウトを使う
- **強調**：ハイライトボックスを使う
- **余白**：`<br>` の多用は避ける。要素が多いスライドでは収まりきらなくなる

---

## タイトルスライドのテンプレート

```markdown
<!-- _class: title -->

# タイトル

**サブタイトル**
YYYY-MM-DD 発表者名 / イベント名
```

---

## 締めスライドのテンプレート

```markdown
<!-- _class: closing -->

# まとめ / 今日から試せます

- ポイント①
- ポイント②
- ポイント③

ありがとうございました 🙏
```

---

## GitHub Actionsとの連携

pushすると自動でPDFとHTMLが生成されてGitHub Pagesに公開される。
`talks/YYYY-MM-テーマ名/slides.md` を作成してpushすればOK。
