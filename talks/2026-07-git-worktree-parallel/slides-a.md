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

# worktreeで `make up` したら、<br>メインのDocker環境が吹き飛んだ話

**git worktree × AIエージェント並列開発の落とし穴**
2026-07 社内LT会

<!--
トーク原稿（約35秒）:
今日は失敗談です。AIエージェントを並列で走らせる環境を作っていて、git worktreeの中でmake upした瞬間に、メインのDocker環境が丸ごと壊れました。タイトルの通りの事故なんですが、原因を掘っていくと全部たった1つの性質に行き着いたので、その話をします。5分だけお付き合いください。
-->

---

<!-- _class: problem -->

# ある日のターミナル

```text
$ curl https://myapp.localhost
curl: (7) Failed to connect        # ① 接続拒否

（直した）→ 500 Internal Server Error   # ② アプリが死ぬ

（直した）→ SQLSTATE: Unknown database  # ③ DBが消える
```

<div class="highlight">
やったことは、worktree側で <code>make up</code> しただけ。<br>
それだけで、メインの唯一のDocker環境が<strong>三段活用で</strong>崩壊した。
</div>

<!--
トーク原稿（約35秒）:
まず現象から。ある日ターミナルでこうなりました。接続拒否。直したら500エラー。それも直したら、今度はUnknown database。モグラ叩きのように三段構えで壊れていきます。私がやったのは、worktreeのディレクトリの中でmake up、つまりDockerを起動しただけです。なぜこれだけでここまで壊れるのか。まず前提の構成から説明します。
-->

---

<!-- _class: ai -->

# そもそも何をしていたか：エージェント並列開発

```text
myapp/            ← 人間：レビュー & マージ担当
myapp-task-a/     ← Claude Code ①（branch: task-a）
myapp-task-b/     ← Claude Code ②（branch: task-b）
myapp-task-c/     ← Cursor       （branch: task-c）
```

- git worktreeでリポジトリの**兄弟ディレクトリ**に作業机を量産
- 原則：**1エージェント = 1worktree = 1ブランチ**
- 環境構築は順調だった。動作確認で `make up` するまでは…

<!--
トーク原稿（約35秒）:
やろうとしていたのは、Claude CodeやCursorを複数同時に走らせる並列開発です。git worktreeという機能で、1つのリポジトリから作業ディレクトリを複数生やせるので、エージェントごとに独立した作業机を1つずつ貸す。人間はメインのリポジトリに居座ってレビューとマージだけやる。この構成自体は順調に組めていました。問題は、worktree側で動作確認しようとDockerを起動した瞬間に起きます。
-->

---

<!-- _class: problem -->

# 諸悪の根源はたった1つ

<div class="columns">
<div class="box good">

## ✅ git管理下 → 引き継がれる
- ソースコード
- `compose.yml` / `Makefile`
- `.node-version`

</div>
<div class="box bad">

## ❌ git管理外 → 引き継がれない
- SSL証明書（pem）
- `vendor/` などの依存
- DBデータ（bind mount）

</div>
</div>

<div class="highlight">
worktreeは<strong>git管理外のものを引き継がない</strong>。<br>
「起動設定はあるのに、中身が無い」状態でDockerが立ち上がる。
</div>

<!--
トーク原稿（約40秒）:
三段障害の原因は、実は全部同じ一点に行き着きます。worktreeはgitの機能なので、当然gitが管理しているものしかコピーされません。コードやcompose.ymlは付いてくる。でも、git管理外のもの、つまり証明書、vendorディレクトリ、DBの実データは付いてこない。つまり「Dockerを起動する設定は完璧に揃っているのに、中身だけが無い」という状態が生まれます。この状態でmake upすると何が起きるか。1つずつ見ていきます。
-->

---

<!-- _class: problem -->

# 一段目：SSL証明書が「無い」

```text
myapp/certs/server.pem      ← メインには、ある
myapp-task-a/certs/         ← worktreeでは、空
```

- 証明書はgit管理外 → worktreeでは**空ディレクトリ**
- Apacheが証明書を読めず起動失敗 → **接続拒否**

<div class="highlight">
コンテナ群は同じ名前で1セットだけ。worktreeから起動した「証明書なし構成」が、メインの正常な環境を<strong>上書き</strong>した。
</div>

<!--
トーク原稿（約35秒）:
一段目、接続拒否の正体はSSL証明書です。証明書はgit管理外なので、worktree側では空ディレクトリ。Apacheは証明書が読めずに起動できません。そして重要なのがここで、Dockerのコンテナ名は1セットしかないので、worktreeから起動した壊れた構成が、それまで動いていたメインの環境をそのまま上書きしてしまった。worktreeの事故がメインに波及したのはこの構造のせいです。
-->

---

<!-- _class: problem -->

# 二段目：vendorが「別物」だった

- `vendor/` もgit管理外 → 過去の**別ブランチ作業の名残**がそのまま残存
- 中身はLaravel 7用、コードはLaravel 6 → **500エラー**

<div class="highlight">
しかも <strong>7週間起動しっぱなしのコンテナのopcache</strong> がこの不整合をずっと隠蔽していた。<br>
「再起動したら壊れた」のではなく、<strong>ずっと壊れていたのが再起動で見えた</strong>。
</div>

<!--
トーク原稿（約40秒）:
二段目の500エラーはもっと味わい深いです。vendorディレクトリもgit管理外なので、以前フレームワークのアップグレード検証をしたときの、新しいバージョン用のvendorがそのまま残っていました。コードは古いバージョンなので当然合わない。ではなぜ今まで動いていたのか。7週間起動しっぱなしだったコンテナのopcacheが古いコードをキャッシュしていて、不整合を隠し続けていたんです。再起動したから壊れたのではなく、ずっと壊れていたのが再起動で初めて見えた。これはヒヤッとしました。
-->

---

<!-- _class: problem -->

# 三段目：データベースが「空」＋ 復旧

- DBのbind mountが**リポジトリからの相対パス**
- worktree側の相対パスは空 → `Unknown database`

```text
復旧手順:
1. worktree側に生まれた残骸（空ディレクトリ）を削除
2. SSL証明書を再生成
3. メイン側で docker compose up し直す
4. composer install（1回目は不完全、2回目で復旧）
```

<!--
トーク原稿（約35秒）:
三段目、Unknown database。DBのデータはbind mountで、その指定がリポジトリからの相対パスでした。worktree側から起動すると相対パスの先は空なので、DBが空っぽで立ち上がる。復旧はこの4手順です。worktree側にできた残骸を消して、証明書を作り直して、メインから起動し直して、最後にcomposer install。これは1回目が不完全で2回叩いて完全復旧でした。トータルでけっこうな時間を溶かしています。
-->

---

<!-- _class: closing -->

# 教訓：worktreeでDockerを立てるな

- 動作確認は**メインの検証ステーションに一本化**する
- 根源は常に1つ：**git管理外のものはworktreeに引き継がれない**
- 逆にgit管理下は完璧だった：`.node-version` 共有でnodeズレゼロ
- pnpmはハードリンクで2個目以降のinstallが **22.3秒 → 12.6秒**（ダウンロード0）

worktree並列開発そのものはオススメです。ただし検証は母艦で 🙏

<!--
トーク原稿（約40秒）:
まとめです。教訓はシンプルで、worktreeでDockerを立てるな。動作確認はメインの検証ステーションに一本化する、です。そして今日の話の根っこは常に1つ、git管理外のものはworktreeに引き継がれない。逆に言うと、git管理下にあるものは完璧に引き継がれます。.node-versionのおかげでnodeのズレはゼロでしたし、pnpmはグローバルストアからハードリンクを張るので、2個目のworktreeのinstallは22.3秒が12.6秒、ダウンロード量ゼロでした。worktree並列開発自体はオススメです。ただし検証は母艦でどうぞ。ありがとうございました。
-->
