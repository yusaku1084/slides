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
paginate: true
---

<!-- _class: title -->

# Claude Codeを複数並列で飼う

**git worktreeで作る並列開発環境と、盛大にハマった話**
2026-07 曽我部祐作 / 社内LT会

<!--
トーク原稿（約15秒）:
最近、Claude CodeやCursorを複数同時に走らせる環境を初めて作りました。今日はその作り方と、メインのDocker環境を吹き飛ばした失敗談をセットで話します。
-->

---

<!-- _class: problem -->

# エージェントは速い。でも作業机は1つ

- エージェントに任せられる独立タスクが**同時に複数**ある
- でも作業ディレクトリが1つだと、ブランチ切替もビルドも**取り合い**になる
- 人間がレビューしている間、エージェントを遊ばせておくのはもったいない

<div class="highlight">
エージェントの数だけ「独立した作業机」を貸したい → <strong>git worktree</strong>
</div>

<!--
トーク原稿（約15秒）:
任せたい独立タスクが同時に複数あるのに、作業ディレクトリが1つだと取り合いになって並列にならない。そこで、エージェントの数だけ作業机を作れるgit worktreeを使います。
-->

---

<!-- _class: solution -->

# git worktree とは

**1つのリポジトリから作業ディレクトリを複数生やす機能**

```text
$ git worktree add ../myapp-a -b feat-login

myapp/.git ─── 履歴の実体はここ1つだけ（コピーしない）
   ├── myapp/     (branch: main)
   ├── myapp-a/   (branch: feat-login)  ← 生えた作業ディレクトリ
   └── myapp-b/   (branch: fix-cart)    ← いくつでも生やせる
```

- 作成は**一瞬**。`.git`（履歴）は共有なのでディスクも軽い
- ブランチ・stash・fetch結果も**全worktreeで共有**

<!--
トーク原稿（約30秒）:
1つのリポジトリから作業ディレクトリを複数生やせる、git標準の機能です。このコマンド1行で、隣に新しいディレクトリとブランチができます。履歴の実体は元の.git1つだけなので、作成は一瞬でディスクも食いません。そしてブランチやstash、fetchの結果も全worktreeで共有されます。この「共有」が次のポイントです。
-->

---

<!-- _class: solution -->

# 「cloneを4つ置けばよくない？」

<div class="columns">
<div class="box bad">

## 😩 clone ×4
- 履歴を毎回**丸ごとコピー**
- fetch・設定が**4つ分**
- レビューは**push経由**

</div>
<div class="box good">

## 😊 worktree ×4
- 作成は一瞬（`.git` 共有）
- fetch1回・設定1箇所
- **push不要で即diff・即レビュー**

</div>
</div>

<div class="highlight">
「同じブランチは2箇所でcheckoutできない」制約も、cloneには無い<strong>衝突防止の安全装置</strong>
</div>

<!--
トーク原稿（約30秒）:
「cloneを4個置けば同じでは？」と思うかもしれません。ただcloneは履歴を丸ごとコピーするので重く、fetchや設定も4重になります。一番差が出るのはレビューで、cloneだとpushしてもらわないと見えない作業が、worktreeならブランチがローカル共有なので、メインからその場でdiffできます。
-->

---

<!-- _class: ai -->

# 運用モデル：1エージェント = 1worktree = 1ブランチ

```text
myapp/       ← 人間：レビュー & マージ担当
myapp-a/     ← Claude Code ①（branch: feat-login）
myapp-b/     ← Claude Code ②（branch: fix-cart）
myapp-c/     ← Cursor       （branch: refactor-api）
```

- worktreeはリポジトリの中ではなく**兄弟ディレクトリ**に置く（IDEが他worktreeまでインデックスして重くなるのを防ぐ）
- 机は**常設で使い回す**：タスクごとに作り直さず、中でブランチを切り替え
- 誰が何してるかは `git worktree list`（机⇔ブランチの対応が出る）

<!--
トーク原稿（約30秒）:
原則は1エージェントに1worktree、1ブランチ。人間はメインに居座って、レビューとマージ判断だけを握ります。worktreeは兄弟ディレクトリに置くこと。机は常設で使い回して、タスクごとに中でブランチを切り替えます。どの机で何が動いてるかは、git worktree listで一目瞭然です。
-->

---

<!-- _class: ai -->

# 実際の作業風景

```text
┌ ターミナル（tmuxでもエディタの分割でも）
├─ pane 1: ~/myapp-a      ← Claude Code ①
├─ pane 2: ~/myapp-b      ← Claude Code ②
└─ pane 3: ~/myapp (main) ← 人間：diffを見てレビュー
```

- **1worktree = 1ターミナル**（1ペインに1エージェント）
- **迷子対策**：プロンプトにブランチ表示 ＋ 迷ったら `git worktree list`
- 最初に机を並べるのはスクリプトで**ワンコマンド化**（だるいと定着しない）

<!--
トーク原稿（約30秒）:
回し方のイメージです。ターミナルをworktreeの数だけ分割して、1ペインに1エージェント。tmuxでもエディタの分割でも構いません。机は使い回すので、プロンプトにブランチ名を出しておいて、迷ったらgit worktree list。机を最初に並べるところはスクリプトでワンコマンド化しました。
-->

---

<!-- _class: problem -->

# ハマった：worktreeで `make up` → 本体崩壊

コンテナ名は1セットだけ → worktree発の**壊れた構成がメインの環境を上書き**

- ① SSL証明書がgit管理外 → worktreeでは空 → **Apache起動不能**
- ② `vendor/` が別ブランチ作業の名残 → **500エラー**（7週間分のopcacheが不整合を隠していた）
- ③ DBのbind mountがリポジトリ相対パス → **Unknown database**

<div class="highlight">
原因は3つとも同じ：<strong>git管理外のものはworktreeに引き継がれない</strong>
</div>

<!--
トーク原稿（約55秒）:
ここから事故です。worktree側でmake upしたら、メインのDocker環境が壊れました。コンテナ名は1セットしかないので、worktree発の壊れた構成がメインを上書きするんです。壊れ方は三段構え。証明書がgit管理外でworktreeでは空、Apacheが起動しない。vendorが別ブランチ作業の名残で500エラー。これは7週間分のopcacheが不整合を隠していて、再起動で初めて露呈。最後はDBのbind mountが相対パスで空、Unknown database。原因は3つとも、git管理外は引き継がれない、です。
-->

---

<!-- _class: problem -->

# 教訓：git管理外のファイルに気をつけろ

worktreeに付いてくるのは**gitが知っているファイルだけ**

- 付いてこない例：**証明書 / `vendor`・`node_modules` / `.env` / DBデータ**
- どれも「無い」か「**古い残骸**」の状態で始まる

<div class="highlight">
対策①：worktreeを作ったら、まず「何が<strong>無い</strong>か」を疑う<br>
対策②：Dockerなど共有インフラはworktreeから触らない。<strong>動作確認はメインに一本化</strong>
</div>

<!--
トーク原稿（約35秒）:
教訓です。worktreeに付いてくるのは、gitが知っているファイルだけ。証明書、vendor、.env、DBデータは、無いか古い残骸の状態で始まります。対策は2つ。worktreeを作ったら「何が無いか」を先に疑う。Dockerのような共有インフラは触らず、動作確認はメインに一本化する。これだけで私の事故は全部防げました。
-->

---

<!-- _class: solution -->

# 細かいつまずきポイント集

- **片付け**：フォルダを `rm -rf` しない。`git worktree remove` で消す（残骸が残ったら `git worktree prune`）
- **作った直後は依存が空**：`node_modules` は付いてこない。まずinstall
- **dev serverの同時起動**：ポートが被る。worktreeごとにずらす
- **任せるタスクの選び方**：同じファイルを触るタスクは並列にしない（コンフリクトの元）

<!--
トーク原稿（約35秒）:
細かいつまずきも4つ。削除はrm -rfではなくgit worktree removeで。rmだと管理情報の残骸が残ります。作りたての机は依存が空なので、まずinstall。dev serverを複数同時に立てるとポートが被るので、ずらす。最後が一番大事で、並列に任せるタスクは同じファイルを触らない独立したものを選ぶこと。外すとマージが地獄になります。
-->

---

<!-- _class: closing -->

# まとめ：明日から使えます

```text
git worktree add ../myapp-a -b feat-login
```

- **1エージェント = 1worktree = 1ブランチ**、置き場所は兄弟ディレクトリ
- cloneより軽くて、**push不要でその場レビュー**できる
- **git管理外のファイルに注意**。Dockerでの動作確認はメインで

ありがとうございました 🙏

<!--
トーク原稿（約25秒）:
まとめです。1エージェント1worktree1ブランチで、兄弟ディレクトリに置く。cloneより軽く、push不要でその場レビューできる。git管理外のファイルには気をつけて、Dockerでの動作確認はメインで。ありがとうございました。
-->
