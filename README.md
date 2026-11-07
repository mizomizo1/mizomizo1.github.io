# mizomizo1.github.io

GitHub Pagesで公開する個人サイトです。

**公開URL:** <https://mizomizo1.github.io/>

## 公開方法

このリポジトリはビルド不要の静的サイトです。`main`ブランチのルートにあるファイルが公開対象です。

初回のみ、GitHubリポジトリの **Settings → Pages → Build and deployment** で以下を設定してください。

1. **Deploy from a branch** を選択
2. ブランチを `main`、フォルダーを `/(root)` にして **Save**

設定後、`main`ブランチにプッシュするとGitHub Pagesが自動で更新されます。公開には数分かかることがあります。

## ローカルで確認する

リポジトリのルートで次のコマンドを実行し、表示されたURLをブラウザーで開きます。

```sh
python3 -m http.server 8000
```

通常は <http://localhost:8000/> で確認できます。

## ファイル構成

- `index.html` — サイトのトップページ
- `README.md` — このプロジェクトの説明と公開手順
