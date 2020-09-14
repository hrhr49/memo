---
title: "Hugoの使い方メモ"
date: 2020-09-14T22:45:24+09:00
draft: false
tags:
- hugo
---

# [Hugo](https://github.com/gohugoio/hugo)とは

* 静的サイトジェネレータと呼ばれるソフトウェアの一種
* Go言語で作られているため、ビルドが高速らしい
* この技術ブログもHugoで作成したもの([Clarity](https://github.com/chipzoller/hugo-clarity)テーマを使用)


## インストール

MacやLinuxなら、
homebrew(linuxbrew)でインストールできる。

```sh
brew install hugo
```

## サイトの作成方法

`hugo new site {サイト名}` で新規にサイトを作成できる。
このコマンドを実行すると、
ここで指定したサイト名と同名のディレクトリが作成される。

例

```sh
hugo new site memo
```

## テーマの追加

以下のコマンドを実行することで、すでに誰かか作ってくれているテーマを追加できる。

```sh
cd {サイト名} # サイトのディレクトリへ移動
git init
git submodule add {テーマがあるリポジトリのURL} themes/{テーマの名前}
```

追加したテーマを使用するためには、サイトのディレクトリのルートにある `config.toml` (Hugoの設定ファイル)
に以下の内容を追記する。

```
theme = "{テーマ名}"
```

今回は[Clarity](https://github.com/chipzoller/hugo-clarity)というテーマを使ったので
以下の手順を行った。

参考: https://github.com/chipzoller/hugo-clarity#getting-up-and-running

```
cd memo
git init
git submodule add https://github.com/chipzoller/hugo-clarity themes/hugo-clarity
cp -a themes/hugo-clarity/exampleSite/* . # ← config.tomlとかもexampleSiteをそのまま流用した
```

## コンテンツの追加

以下のコマンドを実行すると、 `content/posts/{ファイル名}.md` というファイルが自動生成される。
このファイルを編集していけば良い。

```sh
hugo new posts/{ファイル名}.md
```

## サーバの起動

以下のコマンドで、ビルド結果の確認用のサーバを起動できる。

```sh
hugo server -D
```

コマンド実行後、
`Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)`
みたいなメッセージがコンソールに表示されているので、このURLにブラウザからアクセスすれば
ページを確認できる。

## ビルド

以下のコマンドを実行すると、 `public/` ディレクトリにビルド生成物ができあがる。

```sh
hugo
```

## Github Pagesを使用してデプロイ

`confit.toml` を修正して `baseurl` を修正する必要がある。
今回は、`https://hrhr49.github.io/memo/` URL以下にファイルが配置されるので
以下の設定にした。

```
baseurl = "https://hrhr49.github.io/memo/"  # Include trailing slash
```

**注意：**
ここでは、後述のGithut Actionsの設定により、 `public/` ディレクトリではなく、
ルートディレクトリにビルド生成物を配置するようにしたので `baseurl` には `public/` を含めていない。

## TODO

この記事の続きを書く

## つまずいたこと

`content/posts/` に追加したはずのファイルがビルド後の `public/` フォルダには
反映されない現象に悩まされた。

よくソースを見直してみると、 `content/post/_index.md` というファイル(clarityテーマのexampleからコピってきたファイル)が存在しており、
このファイルの中で以下の記載があったため、 `content/posts/` の内容が反映されていないみたいだった・・・

```
+++
# ↓ この設定のせいで、postsがpostにエイリアスされているため、
# postsディレクトリの内容が反映されなかった。
aliases = ["posts", "articles", "blog", "showcase", "docs"]
title = "Posts"
author = "Hugo Authors"
tags = ["index"]
+++
```

## 参考

* [Hugo公式ドキュメントのQuick Start](https://gohugo.io/getting-started/quick-start/)
* [Clarityテーマ](https://github.com/chipzoller/hugo-clarity)
