# 検索結果のタイトルと説明

## 設定と役割

| 項目 | 編集先 | 役割 |
| --- | --- | --- |
| サイト名 | `config/_default/hugo.toml` と `languages.ja.toml` の `title` | トップのtitle、各ページのタイトル末尾、サイト名 |
| サイト共通の説明 | `languages.ja.toml` の `[params].description` | ページ固有の説明がない場合の補完 |
| トップの説明 | `content/_index.md` の `description` | トップを説明するmeta description |
| プロフィールの説明 | `content/about.md` の `description` | 人名・所属・掲載する活動を説明 |
| 記事のタイトル・説明 | `content/posts/記事名.md` の `title` と `description` | その記事の内容を具体的に説明 |
| 説明の優先順位 | `params.toml` の `[seo].metaDescriptionOrder` | `description` → 明示された`summary` → サイト共通説明 |
| 正規URL | `hugo.toml` の `baseURL` と各ページURL | canonicalの生成元 |

タイトル・metaタグ・canonical・サイトマップはHugoとBlowfishが生成する。`public/` のHTMLを直接編集する必要はない。見え方の変更だけでなく、記事一覧の要約にも `description` が使われる。

## 今回の方針

1. ブログ名やハンドル名で探す人にはトップを、実名で探す人にはAboutを対応させる。
2. 研究テーマで探す人には、各記事で実際に扱った課題・技術を示す。記事にない内容を検索向けに盛らない。
3. 各ページに同じキーワードを並べるのではなく、そのページを読むと何が分かるかを文章にする。
4. トップのtitleは短いサイト名を維持し、ハンドル名・関心分野を説明に入れる。実名はAboutのtitleと説明に限定する。

### 設定値に基づく表示例

これは実際のGoogleの検索結果を取得したものではない。

**mizomizo.log**
`https://mizomizo1.github.io/`
mizomizo1の個人ブログ。医療AI、画像解析、バイオインフォマティクス、数理最適化を中心に、研究や開発で学んだことを記録します。

**溝上 秀明 · mizomizo.log**
`https://mizomizo1.github.io/about/`
溝上秀明（Hideaki Mizoue）のプロフィール。関西医科大学医学部医学科に在籍。医療画像解析、ゲノム解析、数理最適化に関する研究・開発、論文、主な受賞を紹介します。

## 新しい記事での書き方

```toml
+++
title = 'Evo2によるゲノム変異スコアリングの実装メモ'
description = 'この記事で実際に紹介する手順と、読者が確認できる内容を1〜2文で書きます。'
+++
```

上のタイトルは書き方の例であり、現在その記事が公開されているという意味ではない。タイトルは主題、説明は扱う内容と範囲にする。定型の「初心者必見」「完全解説」や根拠のない効果を付け足さない。

## Google側で変わる点

[Googleのスニペット公式ガイド](https://developers.google.com/search/docs/appearance/snippet?hl=ja)によると、説明文は主に本文から生成され、meta descriptionが採用されることもある。検索語ごとに変わる可能性があり、設定した文の表示は保証されない。文字数の固定上限を設けず、表示幅によって省略されることを前提に重要な情報を先に書く。

[タイトルリンクの公式ガイド](https://developers.google.com/search/docs/appearance/title-link?hl=ja)では、具体的で簡潔なtitle、ページとの整合、キーワードの乱用を避けることが説明されている。titleの設定値も、Googleが別の情報から作った表示へ変わる場合がある。

[サイト名の公式ガイド](https://developers.google.com/search/docs/appearance/site-names?hl=ja)に対応するWebSite構造化データは、テーマがトップに生成する。OGPは主にSNSなどでの共有情報であり、Googleでのタイトル・説明や画像表示を固定する機能ではない。

## 公開後の確認

ローカル変更だけではGoogleへ反映されない。GitHub Pagesへ公開後、Google Search ConsoleのURL検査でトップとプロフィールのインデックス登録状況を確認する。サイトマップの公開先は `https://mizomizo1.github.io/sitemap.xml`。再クロールや掲載順位、表示の更新時期は保証できない。

今回、Googleの実検索結果・インデックス状況は確認していない。公開用baseURLを指定した別出力で6ページのtitle・説明・canonicalが各1つあること、構造化データのJSON構文、サイトマップの生成を確認した。JSON構文の検証はリッチリザルトへの適合や表示保証を意味しない。

著者設定・フッター・トップと記事のmeta情報は `mizomizo1` に統一する。Aboutの実名・所属・論文情報は維持する。Aboutを対象とするサイト内検索インデックスには、そのページの実名が含まれる。トップのRSSは記事のみを配信し、About本文を配信しない。
