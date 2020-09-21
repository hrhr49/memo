---
title: "Noto Sansフォントを使ってみた"
date: 2020-09-21T09:02:15+09:00
draft: false
tags: ["font"]
---

~~このブログで使用しているフォントを `Noto Sans CJK JP` に変更したので、その時のメモ。~~

追記：2020/09/22時点だと、Webフォントを使用しないように変更した。

# Noto Sans CJK JPとは

Googleが作っているフォント。日本語がきれい。

https://www.google.com/get/noto/#mono-mono

# 注意

このフォントをそのまま使用すると、ファイルサイズが大きすぎる。
この問題を解消するため、サブセット化済みのフォントを用いる。

## サブセット化とは

必要ない文字を削除してフォントのサイズを軽量化すること。
とりあえずは JISの第一水準の漢字が使えればほとんどのケースで困らない。

## サブセット化済みのフォント

以下のURLでサブセット化済みの `Noto Sans` が提供されていたので、ありがたく使わせてもらえばOK。

https://github.com/axcelwork/Noto-Snas-subset

