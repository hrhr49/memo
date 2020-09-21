---
title: "Hugoの使い方メモ"
date: 2020-09-14T22:45:24+09:00
draft: false
tags: ["hugo"]
---

## 注意

2020/09/22時点で、テーマをClarityから、自作のものに変更したため、現在はClarityテーマを使用していない。
しかし、Clarityテーマの導入方法としての情報を残すために、記事の内容は一部そのままにしてある。



# [Hugo](https://github.com/gohugoio/hugo)とは

* 静的サイトジェネレータと呼ばれるソフトウェアの一種
* Go言語で作られているため、ビルドが高速らしい
* この技術ブログもHugoで作成したもの ~~([Clarity](https://github.com/chipzoller/hugo-clarity)テーマを使用)~~ (自作テーマを使用)


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

例)

```sh
hugo new site memo
```

## テーマの追加

以下のコマンドを実行することで、すでに誰かが作ってくれているテーマを追加できる。

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

注意)

ファイルの先頭にあるヘッダ([Front-matter](https://qiita.com/amay077/items/e27f9b4e2374b70a5dfb)という)
で `draft` を `false` にしておかないと、ビルド時に反映されない。

```
---
title: "Hugoの使い方メモ"
date: 2020-09-14T22:45:24+09:00
draft: false
tags: ["hugo"]
---
↑ ファイル先頭にこんなのがある。
```

## サーバの起動

以下のコマンドで、ビルド結果の確認用のサーバを起動できる。

```sh
hugo server -D
```

コマンド実行後、以下のようなメッセージがコンソールに表示されている。

```
                   | EN
-------------------+-----
  Pages            | 17
  Paginator pages  |  0
  Non-page files   |  0
  Static files     | 61
  Processed images |  0
  Aliases          |  6
  Sitemaps         |  1
  Cleaned          |  0

Built in 52 ms
Watching for changes in /home/hrhr49/ghq/github.com/hrhr49/memo/{archetypes,content,data,layouts,static,themes}
Watching for config changes in /home/hrhr49/ghq/github.com/hrhr49/memo/config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/memo/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

このURL(下から二行目)にブラウザからアクセスすればページを確認できる。

## ビルド

以下のコマンドを実行すると、 `public/` ディレクトリにビルド生成物ができあがる。

```sh
hugo
```

## GitHub Pagesを使用してデプロイ

GitHub Pagesを使用すれば、GitHubで管理している静的ファイル(HTMLやJavaScriptなど)をホスティングできるので便利。

補足)

静的サイトのホスティングは[netlify](https://www.netlify.com/)の方が色々と便利そう。
しかし、今回はとりあえずGitHubで完結できるメリットを考慮してGitHub Pagesを使うことにした。

### ベースURLの設定

`confit.toml` を修正して `baseurl` を修正する必要がある。
今回は、`https://hrhr49.github.io/memo/` URL以下にファイルが配置されるので
以下の設定にした。

```
baseurl = "https://hrhr49.github.io/memo/"  # Include trailing slash
```

**注意：**

今回は後述のGithut Actionsの設定により、 `public/` ディレクトリではなく
ルートディレクトリにビルド生成物を配置するようにした。
このような理由から、 `baseurl` には `public/` を含めていない。

### GitHub Actionsの設定

以下のGitHub Actionsでは、Hugoのビルド結果をgh-pagesブランチの
ルートディレクトリに自動で配置してくれるっぽい。

https://github.com/peaceiris/actions-hugo

ただし、今回の場合はワークフローファイルの設定を以下のように変更した。

* アクションのトリガとなるブランチ名を `main` から `master` に変更した(masterブランチで管理していたため)。
* `actions-hugo@v2` のオプションとして、 `extended` を `true` に変更した。

以上の設定すると、以下の作業を自動でできるようになった。

1. masterブランチにpushされると、アクションが起動
2. Hugoのビルドを行い、その生成物を `gh-pages` ブランチのルートディレクトリに配置 & コミットする。
3. 2. で `gh-pages` ブランチの内容を変更したので、 `GitHub Pages` でホスティングしているページも自動で更新する。

### GitHub Pagesの設定

以下の内容を実施すればよい。
[GitHub Pages サイトの公開元を設定する](https://docs.github.com/ja/github/working-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)

## つまずいたこと

`content/posts/` に追加したはずのファイルがビルド後の `public/` フォルダには
反映されない現象に悩まされた。

よくソースを見直してみると、 `content/post/_index.md` というファイル(clarityテーマのexampleからコピってきたファイル)が存在していた。
このファイルの中で以下の記載があったため、 `content/posts/` の内容が反映されていないみたいだった...。

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
