# slides

社内LTや勉強会のスライドをMarpで管理するリポジトリ。

## 構成

```
slides/
├── CLAUDE.md
├── README.md
├── .claude/
│   └── skills/
│       └── marp/
│           └── marp.md
├── .github/
│   └── workflows/
│       └── deploy.yml
└── talks/
    └── YYYY-MM-テーマ名/
        ├── slides.md
        └── assets/
```

## ローカルでの確認方法

VSCodeのMarp拡張機能を使う：

1. [Marp for VS Code](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode) をインストール
2. `slides.md` を開く
3. 右上のプレビューボタンでスライドを確認

## GitHub Pagesで公開

`main` ブランチにpushすると自動でビルドされてGitHub Pagesに公開される。

GitHubリポジトリの Settings → Pages → Source を `GitHub Actions` に設定しておくこと。

## スライド一覧

| 日付 | テーマ | リンク |
|---|---|---|
| 2026-04 | Jujutsu（jj）VCS入門 | - |
