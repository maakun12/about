---
title: 初投稿
description: first post
date: "2021-12-12T18:24:05+09:00"
publishDate: "2021-12-12T18:24:05+09:00"
summary: Hugoを使って自分用のサイトを構築しました。
---

技術系の学習をアウトプットするために本サイトを作成してみました。

気軽に投稿できるブログで開始しようと思い色々なブログサービスを調べましたが、どれも自分にはしっくりこなかったため前々から気になっていた[Hugo](https://gohugo.io/)で作成してみました。

簡単にサイト作成ができるのでオススメです。

Theme は[hugo-coder](https://github.com/luizdepra/hugo-coder)を使ってます。
{{< figure src="/posts/images/hugocoder_sample.png" caption="Example of Portfolio page" >}}

以下導入手順を簡単にメモ

#### Hugo をインストール(for Mac)

```bash
brew install hugo
```

#### サイトの作成

```bash
Hugo new site my-site
```

#### テーマのインストール

```bash
git submodule add https://github.com/luizdepra/hugo-coder.git themes/hugo-coder
```

テーマは[こちら](https://themes.gohugo.io/)から選びました。

#### config.toml に theme を追記

```bash
theme = "hugo-coder"
```

#### サーバー起動

```bash
hugo server
```

http://localhost:1313 にアクセスするとインストールしたテーマの内容が表示されます。

簡単にサイト作成ができました。
今後も色々弄ってみて気づきがあれば記事にしてみようと思います。
