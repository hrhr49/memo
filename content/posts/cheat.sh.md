---
title: "Cheat.shでコマンドのヘルプを表示"
date: 2020-11-17T20:50:07+09:00
draft: false
tags: ["cheat.sh", "tool", "CLI"]
---

チートシートを表示してくれるサービス。

以下のコマンドを叩くと、対応するコマンドのチートシートが表示される。

```sh
curl cheat.sh/<コマンド名>
```

## 例

```sh
$ curl cheat.sh/ls

# ↓ 実行結果
# To display everything in <dir>, excluding hidden files:
ls <dir>

# To display everything in <dir>, including hidden files:
ls -a <dir>

# To display all files, along with the size (with unit suffixes) and timestamp
ls -lh <dir>

# To display files, sorted by size:
ls -S <dir>

# To display directories only:
ls -d */ <dir>

# To display directories only, include hidden:
ls -d .*/ */ <dir>
```

## 似たようなツール

TODO: 気が向いたらメモを作る。

* `tldr`: コマンド
* `cheat`: コマンド

