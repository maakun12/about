---
title: 初投稿
description: first post
date: "2021-12-11T18:24:05+02:00"
publishDate: "2021-12-11T18:24:05+02:00"
---

技術系の学習をアウトプットするためのブログを作成してみました。

気軽に投稿できるブログで開始しようと思い色々なブログサービスを調べましたが、どれも自分にはしっくりこなかったため前々から気になっていた[Hugo](https://gohugo.io/)で作成してみました。

簡単にサイトが作成できるのでオススメです。

Theme は[personal-web](https://github.com/bjacquemet/personal-web)を使ってます。
{{< figure src="/post/images/personal-web-example.png" caption="Example of Portfolio page" >}}

以下導入手順を簡単にメモ

#### Install Hugo(for Mac)

```bash
brew install hugo
```

#### サイトの作成

```bash
Hugo new site my-site
```

#### テーマのインストール

```bash
cd ./my-site/theme
git clone ...theme.git
```

テーマは[こちら](https://themes.gohugo.io/)から選びました。

#### config.toml に theme を追記

```bash
theme = "personal-web"
```

#### サーバー起動

```bash
hugo server
```

http://localhost:1313 にアクセスするとインストールしたテーマの内容が表示されます。

簡単にサイト作成ができました。
今後も色々弄ってみて気づきがあれば記事にしてみようと思います。
