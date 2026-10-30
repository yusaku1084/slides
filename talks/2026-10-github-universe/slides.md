---
marp: true
theme: default
paginate: true
style: |
  /* このLT専用のスタイル（白×青のシンプルな社内勉強会デザイン） */
  @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;700&display=swap');
  section {
    --accent: #2f6fde;
    --accent-soft: #e8f0fd;
    --panel: #f4f6f9;
    --line: #d8dee7;
    --muted: #6b7280;
    background: #ffffff;
    color: #1f2937;
    font-family: 'Noto Sans JP', sans-serif;
    font-size: 18px;
    padding: 60px 80px 56px;
    justify-content: flex-start !important;
    align-content: flex-start !important;
  }
  h1 {
    font-size: 36px;
    font-weight: 700;
    line-height: 1.3;
    margin: 4px 0 30px;
    padding: 0 0 0 18px;
    border: none;
    border-left: 7px solid var(--accent);
    color: #1f2937;
  }
  footer { left: 80px; right: 80px; bottom: 22px; font-size: 13px; color: var(--muted); }
  section::after { right: 80px; bottom: 22px; font-size: 13px; color: var(--muted); }
  b { color: var(--accent); }

  /* タグ（公式 / 個人の考察） */
  .tags { position: absolute; top: 66px; right: 80px; display: flex; gap: 8px; }
  .tag { display: inline-block; padding: 3px 12px; border-radius: 999px; font-size: 14px; font-weight: 700; white-space: nowrap; }
  .tag.fact { background: var(--accent); color: #fff; }
  .tag.pred { border: 1.5px dashed var(--muted); color: var(--muted); background: #fff; }
  .tag.plain { border: 1.5px solid var(--line); background: #fff; }

  /* ボックスと矢印 */
  .row { display: flex; align-items: center; }
  .box { border: 1.5px solid var(--line); border-radius: 10px; background: #fff; padding: 10px 12px; font-size: 17px; font-weight: 500; line-height: 1.45; text-align: center; }
  .box small { display: block; font-size: 14px; font-weight: 400; color: var(--muted); margin-top: 2px; }
  .box.blue { border-color: var(--accent); background: var(--accent-soft); }
  .box.fill { border-color: var(--accent); background: var(--accent); color: #fff; }
  .box.dash { border-style: dashed; border-color: #9aa3b1; }
  .box.warn { border-color: #d0574e; background: #fdf1f0; color: #b3362c; }
  .arrow { flex: none; width: 32px; text-align: center; font-size: 22px; font-weight: 700; color: var(--accent); }
  .loop { position: absolute; border: 2px dashed #9aa3b1; border-top: none; border-radius: 0 0 12px 12px; height: 38px; }
  .loop span { position: absolute; left: 50%; bottom: -12px; transform: translateX(-50%); background: #fff; padding: 0 10px; font-size: 15px; color: var(--muted); white-space: nowrap; }

  /* カード */
  .grid2 { display: grid; grid-template-columns: 1fr 1fr; gap: 32px; }
  .grid3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 20px; }
  .card { background: var(--panel); border-radius: 14px; padding: 20px 24px; }
  .card h3 { font-size: 22px; margin: 0; color: #1f2937; }
  .card h4 { font-size: 18px; margin: 0; }
  .card p { font-size: 16px; line-height: 1.6; margin: 6px 0 0; }
  .card .lead { font-size: 17px; font-weight: 500; }
  .card ul { margin: 12px 0 0; padding-left: 1.2em; font-size: 16px; line-height: 1.7; }
  .label { font-size: 14px; font-weight: 700; color: var(--accent); margin: 0; }
  .note { font-size: 14px; color: var(--muted); }
  .top { display: flex; justify-content: space-between; align-items: center; gap: 10px; }

  /* 表紙 */
  section.cover { padding-left: 110px; justify-content: center !important; align-content: safe center !important; }
  section.cover::before { content: ""; position: absolute; left: 0; top: 0; bottom: 0; width: 16px; background: var(--accent); }
  section.cover .event { font-size: 20px; font-weight: 700; color: var(--accent); margin: 0; }
  section.cover h1 { font-size: 68px; border: none; padding: 0; margin: 10px 0 0; }
  section.cover .sub { font-size: 26px; font-weight: 500; color: var(--muted); margin: 18px 0 40px; }
  section.cover .row .box { width: 250px; }
  section.cover .row .box.dash { width: 320px; font-weight: 700; color: #1f2937; }
  section.cover .meta { position: absolute; right: 80px; bottom: 44px; font-size: 17px; color: var(--muted); margin: 0; }
---

<!-- _class: cover -->
<!-- _paginate: false -->

<p class="event">GitHub Universe 2026</p>

# AIがコードを書く、<br>その先へ。

<p class="sub">GitHub Universe 2026 から見る開発の未来</p>

<div class="row">
  <div class="box blue">Copilot が<br>PRを「承認」できる</div><div class="arrow">＋</div>
  <div class="box blue">落ちたチェックを<br>Copilot が直す</div><div class="arrow">→</div>
  <div class="box dash">全部つなげたら、<br>マージまで自動？</div>
</div>

<p class="meta">社内LT ・ 2026.10 ・ 5分</p>

<!--
（約0:35）
こんにちは。GitHub には、GitHub 自身が提供している AI アシスタント「Copilot」があります。その Copilot が、PR を「承認」したり、落ちたテストを直したりできるようになりました。
じゃあ、全部つなげたらマージまで自動になるのか。10月の GitHub Universe 2026 の発表を調べながら、確かめてみました。
-->

---

<!-- _footer: 出典：引用＝GitHub Blog「GitHub Universe 2026: Evolving every layer of the stack」／ グラフ＝Octoverse 2026（2026-10-28） -->

# たたき台は、AIが書くのが当たり前に

<div class="tags"><span class="tag fact">公式発表</span></div>

<div class="grid2" style="gap:56px">
  <div>
    <p class="note" style="font-size:17px;margin:0">10月28〜29日 ・ サンフランシスコ＋オンライン</p>
    <div style="margin-top:16px;padding:20px 22px;border-radius:12px;background:var(--accent-soft)">
      <p style="margin:0;font-size:24px;font-weight:700;line-height:1.5">「コードの量」より<br>「完了した仕事」が大事</p>
      <p class="note" style="margin:8px 0 0">基調講演ブログより（“Completed work, not more code, is the output that matters.”）</p>
    </div>
    <div class="row" style="margin-top:28px">
      <div class="box" style="flex:1">書く<small>AIが一般的に</small></div><div class="arrow">→</div>
      <div class="box blue" style="flex:1">レビュー・修正<small>今回の発表の中心</small></div><div class="arrow">→</div>
      <div class="box" style="flex:1">マージ</div>
    </div>
  </div>
  <div>
    <p style="margin:0;font-size:20px;font-weight:700">AIエージェントの下書きから始まったPRの割合</p>
    <p class="note" style="margin:4px 0 0">GitHub上のPR全体に占める割合（検出できた分のみ＝下限値）</p>
    <div style="display:flex;justify-content:space-around;align-items:flex-end;height:250px;margin-top:20px;border-bottom:2px solid #1f2937">
      <div style="width:110px;height:87px;background:#a9c3f2;border-radius:8px 8px 0 0;position:relative"><span style="position:absolute;left:-20px;right:-20px;top:-42px;text-align:center;font-size:32px;font-weight:700">17%</span></div>
      <div style="width:110px;height:205px;background:var(--accent);border-radius:8px 8px 0 0;position:relative"><span style="position:absolute;left:-20px;right:-20px;top:-42px;text-align:center;font-size:32px;font-weight:700">40%超</span></div>
    </div>
    <div style="display:flex;justify-content:space-around;margin-top:8px;font-size:16px;color:var(--muted)"><span style="width:110px;text-align:center">2026年3月</span><span style="width:110px;text-align:center">2026年9月</span></div>
  </div>
</div>

<!--
（約0:30）
まず前提です。GitHub のレポートでは、AIエージェント、つまり自分で作業を進めるタイプの AI が下書きした PR が、3月の17%から、9月には40%超に増えました。
たたき台を AI が書くのは、もう普通になりつつあります。次の焦点は、その PR のレビューと修正です。今回の発表もそこが中心でした。
-->

---

<!-- _footer: 出典：GitHub Docs「Using GitHub Copilot code review」／ GitHub Changelog（2026-09-01、2026-10-28） -->

# AIコードレビュー：新機能と正式提供

<div class="tags"><span class="tag fact">公式発表</span></div>

<div class="row" style="gap:14px;padding:10px 18px;border:1.5px solid var(--line);border-radius:12px;font-size:16px">
  <span class="tag plain">以前から</span>
  <span style="background:var(--panel);border-radius:8px;padding:4px 12px">PR画面の右側「Reviewers」→ Copilot <span style="border:1.5px solid var(--accent);border-radius:6px;padding:0 10px;color:var(--accent);font-weight:700">Request</span></span>
  <span class="arrow">→</span>
  <span>Copilot がPRにレビューコメントを書いてくれる</span>
</div>

<div class="grid2" style="margin-top:20px">
  <div class="card">
    <div class="top"><h3>① Code Review Max</h3><span class="tag plain">新機能・プレビュー</span></div>
    <p class="lead">レビューの「深さ」に、いちばん深い Max が追加</p>
    <div class="row" style="margin-top:14px">
      <div class="box" style="flex:1;font-size:15px">複数のAIが<br>別々にレビュー<small>複数社のモデル</small></div><div class="arrow">→</div>
      <div class="box" style="flex:1;font-size:15px">結果を<br>突き合わせ</div><div class="arrow">→</div>
      <div class="box blue" style="flex:1;font-size:15px">1つの<br>レビューに</div>
    </div>
    <ul><li>認証まわりなど、重要な変更向け</li></ul>
  </div>
  <div class="card">
    <div class="top"><h3>② Copilotによる承認</h3><span class="tag plain">9月プレビュー → 今回GA</span></div>
    <p class="lead">Copilotの「承認」を、必須承認の1人分として数えられる</p>
    <div class="row" style="margin-top:14px">
      <div class="box" style="flex:1;font-size:15px">レビュー<small>深さは問わない</small></div><div class="arrow">→</div>
      <div class="box" style="flex:1;font-size:15px">マージ可否を<br>判定</div><div class="arrow">→</div>
      <div class="box blue" style="flex:1;font-size:15px">承認<small>有効時のみ</small></div>
    </div>
    <ul><li>初期設定はOFF（管理者が有効にする）</li><li><b>新しいコミットが入ると、承認は外れる</b></li></ul>
  </div>
</div>

<p style="text-align:center;font-size:17px;color:var(--muted);margin:18px 0 0">2つは別々の機能です。<strong style="color:#1f2937">AIが承認できる ＝ 人の確認がいらない、ではありません。</strong></p>

<!--
（約1:05）
まずレビューです。PR 画面の右側に、レビュアーを指定する「Reviewers」欄がありますよね。ここで人の代わりに Copilot を選ぶと、PR にレビューコメントを付けてくれます。ここまでは以前からある機能です。
今回の1つ目は「Code Review Max」。このレビューに、いちばん丁寧なモードが加わりました。複数の会社の AI が別々にレビューして、結果を1つにまとめます。まだプレビュー版です。
2つ目は「Copilot による承認」の正式提供です。管理者が有効にすると、Copilot が Approve を付けられて、必要な承認の1人分として数えられます。
そして、ここが後で効いてきます。承認のあとに新しいコミットが入ると、Copilot の承認は外れます。
-->

---

<!-- _footer: 出典：GitHub Changelog「Agent Merge watches and fixes pull requests in the cloud」（2026-10-28） -->

# Agent Merge：落ちたチェックをAIが直す

<div class="tags"><span class="tag fact">公式発表</span></div>

<div style="position:relative;width:896px;margin:24px auto 0">
  <div class="row">
    <div class="box" style="width:200px;height:96px;display:flex;flex-direction:column;justify-content:center">マージボックスで<br>「Watch and fix」</div><div class="arrow">→</div>
    <div class="box blue" style="width:200px;height:96px;display:flex;flex-direction:column;justify-content:center">Copilotが<br>PRを見守る</div><div class="arrow">→</div>
    <div class="box blue" style="width:200px;height:96px;display:flex;flex-direction:column;justify-content:center">テスト失敗・<br>競合を見つける</div><div class="arrow">→</div>
    <div class="box fill" style="width:200px;height:96px;display:flex;flex-direction:column;justify-content:center">修正コミットを<br>PRに追加</div>
  </div>
  <div class="loop" style="left:332px;width:464px;top:96px"><span>直したら、また見守る</span></div>
</div>

<div class="grid3" style="margin-top:76px">
  <div class="card"><h4>たとえば</h4><p>Actions のテストが1件落ちた<br>main ブランチと競合した</p></div>
  <div class="card"><h4>マージは auto-merge</h4><p>条件を満たしたら GitHub がマージ<br>（以前からある機能）</p></div>
  <div class="card" style="background:#fff;border:1.5px dashed var(--accent)"><h4 style="color:var(--accent)">ここで気になること</h4><p>修正は「新しいコミット」として<br>PRに積まれる → 承認は？</p></div>
</div>

<!--
（約0:55）
次は新機能の「Agent Merge」です。PR の下のほう、マージボタンがある枠で「Watch and fix」を押すと、Copilot が PR を見張り始めます。
たとえば Actions のテストが落ちた、main と競合した。すると Copilot がクラウド上で修正を試みて、修正コミットを PR に追加します。以前からある auto-merge、つまり必要な条件がそろったら自動でマージする機能とも組み合わせられます。ただし、その条件がちゃんとそろうかがポイントです。
便利ですよね。でも、ここで引っかかりました。修正は新しいコミットとして積まれます。じゃあ、さっきの承認はどうなるのか。
-->

---

<!-- _footer: 出典：GitHub Docs「Using GitHub Copilot code review」「About protected branches」／ GitHub Changelog（2026-10-28） -->

# 発見：AIの承認は、AIの修正で外れる

<div class="tags"><span class="tag fact">公式の仕様</span><span class="tag pred">個人の考察</span></div>

<div style="position:relative;width:1028px;margin:8px auto 0">
  <div class="row">
    <div class="box" style="width:180px;height:88px;display:flex;flex-direction:column;justify-content:center;font-weight:700">Copilotが<br>レビュー</div><div class="arrow">→</div>
    <div class="box blue" style="width:180px;height:88px;display:flex;flex-direction:column;justify-content:center;font-weight:700">Copilotが<br>承認 ✓</div><div class="arrow">→</div>
    <div class="box fill" style="width:180px;height:88px;display:flex;flex-direction:column;justify-content:center;font-weight:700">Agent Mergeが<br>修正コミット</div><div class="arrow">→</div>
    <div class="box warn" style="width:180px;height:88px;display:flex;flex-direction:column;justify-content:center;font-weight:700">承認が<br>外れる ✕</div><div class="arrow">→</div>
    <div class="box dash" style="width:180px;height:88px;display:flex;flex-direction:column;justify-content:center;font-weight:700">もう一度<br>レビューが必要</div>
  </div>
  <div class="loop" style="left:302px;width:636px;top:88px"><span>承認をもらい直す</span></div>
</div>

<div class="grid3" style="margin-top:56px">
  <div class="card" style="padding:16px 20px"><h4>Copilot の承認</h4><p>承認後に新しいコミットが入ると<br>外れる（Copilot の仕様）</p></div>
  <div class="card" style="padding:16px 20px"><h4>人の承認</h4><p>外れるかどうかは、<br>ブランチ保護の設定しだい</p></div>
  <div class="card" style="padding:16px 20px"><h4>再レビュー</h4><p>手動で再依頼。設定すれば<br>新しいpushごとに自動レビュー</p></div>
</div>

<div style="margin-top:22px;padding:16px 26px;border-radius:14px;background:var(--accent-soft)">
  <span class="tag pred">個人の考察</span>
  <p style="margin:8px 0 0;font-size:21px;font-weight:700;line-height:1.5">AIの力不足ではなく、「変更後の承認をそのまま使わない」安全のための設計。<br>各工程をAIが担当できても、自動で一直線にはつながらない。</p>
</div>

<!--
（約1:00）
調べてみると、外れるんです。Copilot が承認したあとに Agent Merge が修正コミットを足すと、その承認は外れて、もう一度レビューが必要になります。push のたびに自動で再レビューさせる設定もありますが、修正したら終わりではなく、もう一度承認条件を満たす必要があります。
最初は「AI 同士なのに不便だな」と思いました。でも、承認したあとにコードが変わったら、その承認はもう古いんですよね。人の承認も、設定によっては同じように外れます。
つまり、AI の力不足ではなく、安全のための仕組みです。AI が1つ1つの作業をこなせても、作業のつなぎ目にはルールが要る。これが今回いちばん驚いた発見でした。
-->

---

<!-- _footer: 出典：Atlassian Support「Suggest or require checks before a merge」 -->

# まとめ：任せる範囲と、受け入れる条件

<div class="tags"><span class="tag pred">個人の考察</span></div>

<div style="padding:26px 36px;border-radius:14px;background:var(--accent-soft)">
  <p class="label" style="font-size:16px">私の考え</p>
  <p style="margin:8px 0 0;font-size:29px;font-weight:700;line-height:1.5">AIに「何を任せるか」だけでなく、<br>「どんな条件で結果を受け入れるか」の設計が大事になりそう</p>
</div>

<div class="grid2" style="margin-top:26px">
  <div class="card">
    <p class="label">Bitbucket Cloud にも似た仕組みがある</p>
    <ul><li>ソースブランチが変わったら承認をリセット</li><li>マージチェック（最低承認数・ビルド成功など）</li></ul>
    <p class="note" style="margin-top:8px">※ 承認リセットや、チェックでマージを止めるのは Premium プラン</p>
  </div>
  <div class="card">
    <p class="label">持ち帰り：自分たちのリポジトリでは？</p>
    <p style="font-size:20px;font-weight:700;line-height:1.5;margin-top:10px">承認のあとに変更が入ったら、<br>その承認はどう扱われる設定？</p>
    <p class="note" style="margin-top:8px">※ 社内のプランや現在の設定は、まだ確認していません</p>
  </div>
</div>

<!--
（約0:45）
ここからは私の考えです。これからは、AI に何を任せるかだけでなく、AI の結果をどんな条件で受け入れるかを決めることが大事になりそうです。
Bitbucket Cloud にも、ブランチに変更が入ったら承認をリセットする設定や、マージ前に承認数などを確認するマージチェックがあります。
なので、持ち帰りを1つ。自分たちのリポジトリでは、承認のあとに変更が入ったらどうなる設定なのか。一度見てみてください。ありがとうございました。
-->
