---
title: "i3wmでシステムのフォントサイズを変える方法"
date: 2020-11-02T16:29:40+09:00
draft: false
---

Google Chromeのブックマークバーなどの文字が小さくて
困っていたのを改善できたのでメモ。

### 環境

* OS: Ubuntu 20.04
* ウィンドウマネージャ: i3wm

### やり方

1. lxappearanceパッケージをインストールする。

```sh
sudo apt install lxappearance
```

2. 先程インストールしたlxappearanceを起動する。

```sh
lxappearance
```

3. 設定でフォントのサイズを変更する。

以上で完了。


### 参考

https://www.reddit.com/r/i3wm/comments/di6ubw/font_size_inside_applications_i3/
