---
title: Github PagesでHugoサイトを公開
description: first post
date: "2021-12-12T18:24:05+09:00"
publishDate: "2021-12-12T18:24:05+09:00"
summary: Hugoで作成したサイトをGithub Pagesで公開した時のメモです
---

## Github Pages で公開する

公式の Doc に従い github actions で自動デプロイする構成にしました。

公式の Doc はこちら

- [GitHub](https://docs.github.com/ja/pages/getting-started-with-github-pages/about-github-pages)
- [Hugo](https://gohugo.io/hosting-and-deployment/hosting-on-github/)

### 手順

#### Repository 名を`<username>.github.io`に変更する

この Repository 名が以下のように URL になります。

`https://<username>.github.io`

#### Github Actions の Workflow ファイル を追加

今回は Hugo の Doc に記載の内容をそのままコピーしました

```yaml
name: github pages

on:
  push:
    branches:
      - main # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0 # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "latest"
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

#### config.toml を修正

baseURL を以下の形式で追記します。

```toml
baseURL = "https://<username>.github.io"
```

ここまでで一旦 Github の Remote Repository に Push

#### Github Pages の公開設定

Github のリポジトリ画面で設定を行います。

まず、Settings タブをクリック
{{< figure src="/posts/images/hugo-pages/settings.png" >}}

ページをスクロールして下部にある Github Pages の[Check it out here!]をクリック
{{< figure src="/posts/images/hugo-pages/githubpages-check.png" >}}

次に、Github Pages のページで Source ブランチを変更します。
ブランチは Github Actions でデプロイしている`gh-pages`ブランチに設定します。
{{< figure src="/posts/images/hugo-pages/githubpages-source.png" >}}

1~2 分待って以下のように`Your site is published at https://<username>.github.io/`と表示されれば反映完了
{{< figure src="/posts/images/hugo-pages/githubpages-published.png" >}}

URL にアクセスしてページが公開されている事を確認できれば反映完了です。

Github Actions で自動デプロイを行っているので、次回以降の反映は main ブランチに修正を push する度に自動的に反映されます。

### まとめ

Hugo で作成したサイトを Github Pages で公開したのでその際の手順をメモしてみました。

公式の Doc が充実していて、書いてある手順をそのまま実施するだけで公開＆自動デプロイ設定が行えたので簡単に構築ができました。
