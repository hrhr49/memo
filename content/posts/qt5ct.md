---
title: "UbuntuでQt5のアプリのフォント設定 Qt5ct"
date: 2021-01-25T17:32:28+09:00
draft: false
tags: ["Ubuntu", "Linux", "tool"]
---

UbuntuでQtベースのアプリを使用していると、どうもフォントが小さくて見づらかったが、解決したのでメモ

自分の環境

* OS: Ubuntu 20.04
* ウィンドウマネージャ: i3wm 4.17.1

といっても、基本的には以下のURLの内容を行うだけでOKだった。

[Ubuntu 17.04 で qt5 アプリケーションのフォントなどを変更する](https://sicklylife.hatenablog.com/entry/2017/07/09/190447)

一応、私の環境でやった内容も書いておく。

1. qt5ctをインストールする
Ubuntu 20.04ではppaを使わなくてもaptでインストールできた

```
sudo apt install qt5ct
```

2. Qtの環境変数を以下のように設定する

`~/.profile` に設定を入れる場合は以下のコマンドを実行すれば良い
(私の場合は `~/.xprofile` に入れたけど、良かったのかな？)

```sh
echo 'export QT_QPA_PLATFORMTHEME="qt5ct"' >> ~/.profile 
```

3. ログインし直す
4. `Qt5 Setting` というアプリを起動し、 `Fonts` のタブでフォントの設定ができる。フォントの大きさもここで変更できる。

以上で終了

ちなみに、Gtkベースのアプリについては `LXAppearance` で設定ができたはず

