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

## 配色・見た目・アイコンの管理

テーマ本体を変更せず、サイト側のファイルで管理しています。

| 変更したいもの | 編集するファイル |
| --- | --- |
| テーマ全体のアクセント色（RGBの段階色） | `assets/css/schemes/journal.css` |
| 背景・カード・文字色、角丸・余白 | `assets/css/custom.css` の変数とコンポーネント定義 |
| ライト／ダークそれぞれの色 | `custom.css` の `:root` ／ `html.dark` |
| 使用する配色・記事カードの有効化 | `config/_default/params.toml` |
| メニューの名前・順番・アイコン | `config/_default/menus.ja.toml` |
| トップのヒーローセクションの構造 | `layouts/partials/home/hero.html` |
| トップのタイトル・紹介文 | `content/_index.md` |

青緑をアクセントに、ライトとダークの両方に対応しています。色はCSS変数に集約し、画面幅に応じて余白やリンク配置を調整しています。

アイコンは公式の [Bootstrap Icons](https://icons.getbootstrap.com/) **v1.13.1** のSVGを使用しています。必要な8点を `assets/icons/bi-*.svg` に保存し、閲覧時のCDNやアイコンフォントの読み込みを不要にしています。Bootstrap IconsはBootstrap本体なしで利用できるため、Blowfishの既存スタイルと組み合わせています。

メニューでは `pre = "bi-house"` のようにSVG名（拡張子なし）を指定します。テンプレートでは `{{ partial "icon.html" "bi-house" }}` で利用できます。アイコンは文字ラベルと組み合わせ、装飾SVGには `aria-hidden="true"` を付けています。

取得元・バージョン・更新方法は `licenses/bootstrap-icons/README.md`、MITライセンス原文は同ディレクトリの `LICENSE` に保存しています。追加時も `bi-` 接頭辞を使い、テーマ側のアイコンと区別してください。

サイトの横幅は `assets/css/custom.css` の `--blog-max-width`（最大1600px）、左右の余白は `--blog-gutter` で管理します。トップは `params.toml` の `homepage.layout = "hero"` でサイト独自のヒーローを表示します。本文の行幅は読みやすさのためテーマの制限を維持し、スマートフォンではヒーローの装飾を省いて1列で表示します。

## フォント

本文と見出しは **BIZ UDPゴシック（BIZ UDPGothic）** を使用します。`layouts/partials/extend-head.html` でGoogle Fontsの400・700ウェイトを読み込み、`assets/css/custom.css` の `--blog-font` でフォントを管理します。`display=swap` により読み込み中も代替フォントで表示します。外部フォントを取得できない場合は端末のフォントに切り替わります。コードブロックは等幅フォントを維持します。

文章の読みやすさは `custom.css` の `--blog-text-leading`（本文行間1.2倍）、`--blog-text-tracking`（字間0）、`--blog-paragraph-gap`（段落間0.35em）、`--blog-reading-width`（本文の最大幅44rem）で調整できます。本文はPCで18px、スマートフォンで16pxです。見出し・箇条書き・コードにはそれぞれの間隔を設定しています。
