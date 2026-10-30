# 2026-10 GitHub Universe 2026 LT — 出典・補足・想定問答

## 技術的な補足（スライドごと）

- **Slide 3**
  - 【以前から】Reviewers 欄から Copilot にレビューを依頼（Copilot code review は 2025-04-04 に GA）
  - 【新機能】Code Review Max（パブリックプレビュー）／ Copilot による承認（2026-09-01 プレビュー → 2026-10-28 GA）
  - 承認の判定はどの深さのレビューにも付く。Max を使わなくても承認機能は使える
- **Slide 4**
  - 【新機能】Agent Merge（失敗した Actions ワークフロー・マージ競合の修正、クラウドで実行）
  - 【以前から】auto-merge（必要なレビューとステータスチェックが通ったら自動でマージ）
- **Slide 5**
  - Copilot の承認：承認後に新しいコミットが push されると外れる（GitHub Docs / Changelog）
  - 人の承認：ブランチ保護の「Dismiss stale pull request approvals when new commits are pushed」を有効にした場合、PRの差分が変わると外れる（オプション）
  - 再レビュー：手動で再依頼。ルールセットで「Review new pushes」を有効にすると、新しい push ごとに Copilot が自動レビュー
  - **注意**：「Agent Merge の修正で承認が外れる」は、上の公式仕様（Agent Merge は修正コミットを追加する／新しいコミットで Copilot の承認は外れる）を組み合わせた説明。この組み合わせを直接書いた公式文書はない
- **Slide 6**
  - Bitbucket Cloud の「Reset approvals when the source branch is modified」とマージチェック（Atlassian Support）
  - 社内の契約プランや現在の設定は確認していない。事実として話さない

## 出典（2026-10-30 確認）

- GitHub Docs — [Using GitHub Copilot code review](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review)（Reviewers からの依頼、承認の取り消し、Review new pushes）
- GitHub Docs — [About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)（古い承認の取り消し）
- GitHub Docs — [Automatically merging a pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/automatically-merging-a-pull-request)（auto-merge）
- GitHub Changelog — [Copilot code review can now approve pull requests](https://github.blog/changelog/2026-09-01-copilot-code-review-can-now-approve-pull-requests)（2026-09-01）
- GitHub Changelog — [Copilot code review: Merge with confidence with Max and approvals](https://github.blog/changelog/2026-10-28-copilot-code-review-merge-with-confidence-with-max-and-approvals)（2026-10-28）
- GitHub Changelog — [Agent Merge watches and fixes pull requests in the cloud](https://github.blog/changelog/2026-10-28-agent-merge-watches-and-fixes-pull-requests-in-the-cloud)（2026-10-28）
- GitHub Changelog — [Copilot code review now generally available](https://github.blog/changelog/2025-04-04-copilot-code-review-now-generally-available/)（2025-04-04）
- GitHub Blog — [GitHub Universe 2026: Evolving every layer of the stack](https://github.blog/news-insights/company-news/github-universe-2026-evolving-every-layer-of-the-stack/)（2026-10-28）
- GitHub Blog — [Octoverse: Commits, pushes reach new highs in 2026 as developers build with agents](https://github.blog/news-insights/octoverse/octoverse-commits-pushes-reach-new-highs-in-2026-as-developers-build-with-agents/)
- Atlassian Support — [Suggest or require checks before a merge](https://support.atlassian.com/bitbucket-cloud/docs/suggest-or-require-checks-before-a-merge/)

### 質疑用メモ
- **Bitbucket のプラン：** 「承認リセット」と、マージチェックで実際にマージを止める機能は Premium プランです。それ以外のプランでは警告が出るだけで、マージはできます。「変更要求のリセット」は全プランで使えます。Premium には「差分が変わらなければ承認を残す」設定もあります。
- **GitHub の人の承認：** 古い承認を取り消すのはオプション設定です。差分が変わらない push では外れません。merge base が変わって新しい変更が入った場合は外れます。
- **Agent Merge の提供状況：** GitHub 上で GA かプレビューかは、Changelog に書かれていません。Copilot app と VS Code では先行提供されていて、今回PR画面に統合されました。有料の Copilot プランが対象で、組織や Enterprise のポリシーに従います。
- **Agent Merge の対象：** Actions 以外のCIは書かれていません。フォークからのPRやスタックPRの扱い、課金先も Changelog では確認できていません。
- **auto-merge：** 書き込み権限のない人が新しいコミットを push すると、auto-merge はオフになります。
- **CODEOWNERS：** Copilot の承認が CODEOWNERS のレビュー要件を満たすかは未確認です。Docs の記述は「必須承認数にカウントされる」までです。
- **Max：** かかる時間やクレジットの具体的な数値は公開されていません。
- **今回扱わなかった発表：** GitHub Agentic Workflows の GA など。
- **Code Review Max の選び方：** レビューの深さ（Lite / Balanced / Max）を、どの画面のどの操作で選ぶかは、公式資料ではまだ確認していません。聞かれたら「Copilot にレビューを頼むときの深さの選択肢の1つ」とだけ答えてください。
- **用語の補足：** 「AIエージェント」は、指示を受けて自分で作業を進めるタイプのAIです。Octoverse では、検出できたコーディングエージェントが最初の下書きを作ったPRを数えています。
