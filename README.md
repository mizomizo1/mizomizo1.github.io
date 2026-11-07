# mizomizo1's Blog

HugoとBlowfishで構築した日本語の個人ブログです。

**公開URL:** <https://mizomizo1.github.io/>

## 記事の投稿

`content/posts/` にMarkdownファイルを追加し、`main` ブランチへPushすると、GitHub ActionsがビルドしてGitHub Pagesへ公開します。

```sh
hugo new content posts/my-first-post.md
```

作成した記事のフロントマターで `draft: false` にすると公開対象になります。ローカルで確認する場合は次を実行します。

```sh
hugo server
```

## ローカルビルド

Hugo Extended 0.169.0とGitが必要です。サブモジュールも含めて取得し、ビルドします。

```sh
git clone --recurse-submodules https://github.com/mizomizo1/mizomizo1.github.io.git
cd mizomizo1.github.io
hugo --gc --minify
```

サイトは `public/` に生成されます。このディレクトリはGit管理対象外です。

## GitHub Pagesの初回設定

GitHubリポジトリの **Settings → Pages → Build and deployment → Source** を **GitHub Actions** に設定してください。以降、`main` へのPushまたはActionsからの手動実行でデプロイされます。

## 構成

- `config/_default/` — HugoとBlowfishのサイト設定
- `content/` — Markdownの記事とプロフィール
- `themes/blowfish/` — Git Submoduleで管理するBlowfishテーマ
- `.github/workflows/hugo.yaml` — GitHub Actionsによるビルドとデプロイ

Blowfish本体は変更せず、カスタマイズはサイト側の設定・コンテンツで行います。
