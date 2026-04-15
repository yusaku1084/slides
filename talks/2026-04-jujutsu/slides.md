---
marp: true
theme: default
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
paginate: true
---

<!-- _class: title -->

# Gitのしんどさ、<br>まるっと解決するVCSを紹介します

**Jujutsu（jj）LT**
2026 社内LT会

---

<!-- _class: problem -->

# Gitで開発してて、こういう経験ないですか？

<br>

- 😩 `git add` 忘れて中途半端なコミットをしてしまった
- 😰 やらかしたとき、`git reset --hard` が怖かった
- 😞 コンフリクト解消中に作業が止まってしまった

<br>

> 今日はこの **3つ** をまるっと解決するVCSを紹介します

---

# Jujutsu（jj）とは？

<br>

**Gitのしんどいところを解消した、Git互換のVCS**

<br>

- ✅ GitHubもBitbucketもそのまま使える
- ✅ チームへの影響ゼロ
- ✅ 自分だけ使い始めればいい

<br>

<div class="highlight">
既存リポジトリに1コマンド追加するだけで始められます
</div>

---

<!-- _class: solution -->

# 解決① `git add` が要らない

<div class="columns">
<div class="box bad">

## 😩 Git
```
ファイル編集
  ↓
git add（ステージング）
  ↓         ← よく忘れる
git commit
```

</div>
<div class="box good">

## 😊 jj
```
ファイル編集
  ↓
自動で記録される
  ↓
jj new（次の作業へ）
```

</div>
</div>

<div class="highlight">
ステージングエリア自体が存在しない。<br>git add 忘れが構造的に起きなくなる。
</div>

---

<!-- _class: solution -->

# 解決② やらかしても一発で戻せる

**全操作がログとして記録されている**

<br>

| 状況 | Git | jj |
|---|---|---|
| コミットのやり直し | `git reset --hard`（怖い） | `jj undo`（安心） |
| 操作のログ | コミットのみ | **全操作**が記録 |
| AIがやらかした | 把握が大変 | `jj undo` 一発 |

<br>

<div class="highlight">
Gitの <code>reflog</code> と違い、rebaseやmergeなどの<strong>操作そのもの</strong>が記録される。<br>
AIに作業させるときも <code>jj undo</code> があるから気軽に任せられる。
</div>

---

<!-- _class: solution -->

# 解決③ コンフリクトを後回しにできる

<div class="columns">
<div class="box bad">

## 😩 Git
```
rebase / merge
  ↓
コンフリクト発生
  ↓
その場で解決しないと
先に進めない 🛑
```

</div>
<div class="box good">

## 😊 jj
```
rebase / merge
  ↓
コンフリクト発生
  ↓
そのままコミットできる ✅
  ↓
後から落ち着いて解決
```

</div>
</div>

<div class="highlight">
コンフリクトが起きても作業の流れが止まらない。
</div>

---

<!-- _class: ai -->

# AI時代に真価を発揮する

**workspace機能で複数のClaude Codeを並行稼働**

```
~/myapp/          ← 自分が実装中
~/myapp-blue/     ← Claude Code がバグ修正中
~/myapp-green/    ← Claude Code がテスト書き中
```

↑ **全部同じリポジトリを共有**

<br>

| | Git worktree | jj workspace |
|---|---|---|
| ブランチ名 | 必須 | 不要（匿名でOK） |
| 気軽さ | △ | ◎ |

<div class="highlight">
jj undo で安全 ＋ workspace で並行稼働。<br>AIのために作られたわけじゃないけど、結果的に相性抜群。
</div>

---

<!-- _class: closing -->

# 今日から試せます

<br>

```bash
# インストール
brew install jj

# 既存リポジトリに追加（チームへの影響ゼロ）
jj git init --colocate
```

<br>

- Gitをやめる必要はない
- 気に入らなければいつでも戻せる
- **まず自分だけこっそり使い始めてみてください**

<br>

ありがとうございました 🙏
