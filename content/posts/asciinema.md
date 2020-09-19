---
title: "Asciinemaの使い方"
date: 2020-09-20T00:42:49+09:00
draft: false
tags: ["Asciinema", "tool", "CLI"]
---

# Asciinemaとは

コンソールの操作を記録・再生するためのツール。
記録した内容を、以下のようにHTMLファイルに動画として埋め込むこともできる。

<script id="asciicast-IA8RYSDRRmJ8MksKYIuwERiyv" src="https://asciinema.org/a/IA8RYSDRRmJ8MksKYIuwERiyv.js" async></script>

## インストール

homebrewでインストールできる。

```sh
brew install asciinema
```

## アカウントの作成・使い方

以下のURLを参照。

[Asciinemaを試す](https://qiita.com/sotoiwa/items/aefee55d76c0b6b4ed0c)

## 詰まったところ

### HugoでAsciinemaの埋め込み動画が表示されない

このブログはHugoという静的サイトジェネレータを使用しているが、Asciinemaの埋め込み動画を使用するには設定が必要。

Markdownファイル内でHTMLタグを有効化するために、設定ファイルを修正する。
以下の内容を `config.toml` に加える。

```toml
[markup]
  [markup.goldmark]
    [markup.goldmark.renderer]
      unsafe = true
```

参考: https://budougumi0617.github.io/2020/03/10/hugo-render-raw-html/
