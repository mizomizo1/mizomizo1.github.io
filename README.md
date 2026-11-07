# mizomizo1's Blog

HugoとBlowfishで構築する日本語の個人ブログです。
公開URL: <https://mizomizo1.github.io/>

## ブログの構成

- ホーム `/`：ブログの紹介と新着記事5件、記事一覧へのリンク
- 記事一覧 `/posts/`：新しい順にタイトル・日付・要約を表示（1ページ10件）
- 記事 `/posts/記事名/`：本文、目次、カテゴリー・タグ、前後の記事へのリンク
- プロフィール `/about/`、カテゴリー `/categories/`、タグ `/tags/`
- RSS `/index.xml`、サイト内検索、ライト／ダークモード切り替え

## ディレクトリ構成

```text
.
├── .github/workflows/hugo.yaml   # mainへのPushでビルド・公開
├── archetypes/default.md        # 新規記事のテンプレート
├── config/_default/
│   ├── hugo.toml                # URL・言語・ページ分割など
│   ├── languages.ja.toml        # 日本語表記・著者情報
│   ├── menus.ja.toml            # ヘッダー・フッターのメニュー
│   ├── params.toml              # Blowfishのレイアウト・表示設定
│   ├── markup.toml              # Markdown・コード・目次の設定
│   └── module.toml              # Hugoモジュール設定
├── content/
│   ├── _index.md                # ホームのタイトル・紹介文
│   ├── about.md                 # プロフィール
│   ├── categories/_index.md     # カテゴリー一覧の日本語タイトル
│   ├── tags/_index.md           # タグ一覧の日本語タイトル
│   └── posts/
│       ├── _index.md            # 記事一覧のタイトル・説明
│       └── hello-world.md       # ブログ記事
├── assets/                     # Hugoで処理するCSS・画像など
├── static/                     # そのまま公開する画像・ファイル
├── layouts/                    # テーマを上書きするテンプレート
├── data/                       # テンプレート用の構造化データ
├── i18n/                       # 翻訳の追加・上書き
├── themes/blowfish/             # Git Submoduleで管理するテーマ
├── public/                     # ビルド結果（Git管理対象外）
└── resources/_gen/              # 生成キャッシュ（Git管理対象外）
```

**編集するのは `content/` や `config/` などの元ファイルです。**
`public/categories/index.html` と `index.xml` などは自動生成されるため、直接編集しても次回ビルドで上書きされます。カテゴリーやタグは各記事のフロントマターから生成されます。テーマ本体は変更せず、設定は `config/_default/`、独自テンプレートは `layouts/` に置きます。

## ローカル環境

Gitと **Hugo Extended 0.169.0**（GitHub Actionsと同じバージョン）を使用します。

```sh
git clone --recurse-submodules https://github.com/mizomizo1/mizomizo1.github.io.git
cd mizomizo1.github.io
# すでにclone済みの場合も、テーマを取得する
git submodule update --init --recursive
hugo version
hugo server -D
```

表示されたローカルURL（通常 `http://localhost:1313/`）で確認します。`-D` は下書きも表示する指定です。公開対象だけを確認する場合は `hugo server` を使います。

## 記事を書く

```sh
hugo new content posts/my-first-post.md
```

生成されたMarkdownを編集します。カテゴリーは大きな分類、タグは記事のキーワードとして使います。

```toml
+++
title = '記事のタイトル'
date = 2026-11-07T15:00:00+09:00
draft = true
description = '記事の説明（検索結果などに使用）'
tags = ['Hugo']
categories = ['技術']
+++

記事一覧に表示する導入文を書きます。

<!--more-->

## 見出し

ここから本文を書きます。
```

`<!--more-->` より前が一覧の要約になります。省略した場合は本文から自動生成されます。公開するときは `draft = false` にします。未来の日時の記事はその日時になるまでビルド対象になりません。日時を過ぎたら再ビルドが必要です。

画像を記事とまとめる場合は `content/posts/記事名/index.md` と同じフォルダーに画像を置き、`![説明](image.jpg)` で参照できます。共通の画像は `static/images/` に置き、`/images/ファイル名` で参照します。

## ビルドと公開

```sh
hugo --gc --minify
```

生成先は `public/` です。GitHubリポジトリの **Settings → Pages → Build and deployment → Source** を **GitHub Actions** に設定してください。

変更を `main` にPushすると、`.github/workflows/hugo.yaml` がビルドしGitHub Pagesに公開します。Actionsからの手動実行にも対応しています。`public/` をコミットする必要はありません。
