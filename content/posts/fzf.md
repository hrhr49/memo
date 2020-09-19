---
title: "fzfの使い方"
date: 2020-09-19T23:45:20+09:00
draft: false
tags: ["fzf", "tool", "CLI"]
---

# fzfとは

インクリメンタルにあいまい検索で絞り込みを行うためのコマンドラインツール。
[GitHubのページ](https://github.com/junegunn/fzf)

以下の動画を見てもらえば、どういったものかわかりやすい。

<script id="asciicast-HZtIiHv7V6oeveTDzhBVHbWOc" src="https://asciinema.org/a/HZtIiHv7V6oeveTDzhBVHbWOc.js" async></script>

`fzf` コマンドは、標準入力に受け取ったテキストを絞り込んだ結果を標準出力に流す。
そのため、他のコマンドと簡単に連携できるのでとても強力。

なお、標準入力無しでコマンドを実行した際は、
（再帰的に）現在のディレクトリ配下のファイルすべて
の中から候補を絞り込むことができる。

上記の例では、 fzf で絞り込んだファイルをvimで開いている。

## インストール

### homebrewもしくはlinuxbrewでインストールする場合

```sh
brew install fzf

# シェルでのキーバインドや補完機能を使用する場合は以下を実行する
$(brew --prefix)/opt/fzf/install
```

### aptを使う場合

Ubuntu バージョン20以上だと、aptでインストールもできるようになっている。
ただ、こっちだとバージョンが古くなりがちなので最新のものを使いたい場合はbrewの方を使う。

```sh
sudo apt install fzf
```

キーバインドなどを有効化したいときにはbashrcやzshrcに以下を記載する。

bashの場合は~/.bashrcに以下を追加。

```bash
source /usr/share/doc/fzf/examples/completion.bash
source /usr/share/doc/fzf/examples/key-bindings.bash
```

zshの場合は~/.zshrcに以下を追加。

```zsh
source /usr/share/doc/fzf/examples/completion.zsh
source /usr/share/doc/fzf/examples/key-bindings.zsh
```

参考： https://github.com/junegunn/fzf/issues/1866


### キーバインド

* `<C-t>` : ディレクトリやファイル名補完
* `<C-r>` : コマンド履歴
* `<M-c>` : 選択したディレクトリへcd

### 検索に使える特殊記号

* `'hoge` : hogeを含む文字列
* `^hoge` : hogeが先頭にある文字列
* `hoge$` : hogeが末尾にある文字列
* `!hoge` : hogeを含まない文字列
* `!^hoge` : hogeが先頭にない文字列
* `!hoge$` : hogeが末尾にない文字列

### vimプラグイン

fzf自体vimプラグインになっている。
それに加えてfzf.vimというvimプラグインが用意されている。
この両方をインストールすれば、vimでいろいろなものを絞り込むことができる。

```vim
Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'
```

[fzf.vimのGitHubのページ](https://github.com/junegunn/fzf.vim)

## 参考

* [GitHubのページ](https://github.com/junegunn/fzf)
* [fzf.vimのGitHubのページ](https://github.com/junegunn/fzf.vim)


もう少し詳しいことを知りたい場合は、以下の記事が参考になる。

* [fzfを使おう](https://qiita.com/kompiro/items/a09c0b44e7c741724c80)
