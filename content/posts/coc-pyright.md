---
title: "Coc-Pyright"
date: 2020-11-29T12:57:59+09:00
draft: false
---

[coc-pyright](https://github.com/fannheyward/coc-pyright)のメモ

## 概要
Vim用のプラグインであるcoc.nvimの拡張機能。
Pythonの静的解析ツールであるPyrightをcoc.nvimで使えるようにしたやつ。

## インストール

基本的な使い方は公式リポジトリの説明を参照

https://github.com/fannheyward/coc-pyright

## 設定

`pyrightconfig.json`をプロジェクトのルートに配置する。
グローバルで共通設定を用意することはできないっぽい？

PipenvやPoetryなどでの仮想環境化でも入力補完できるようにするために
設定が必要だったのでメモする。

私の環境の場合、以下の環境変数によって、仮想環境をプロジェクトのルートに`.venv`ディレクトリとして配置するようにしている。


```sh
# pipenvの仮想環境の場所をプロジェクト直下にする
export PIPENV_VENV_IN_PROJECT=true

# poetryの仮想環境の場所をプロジェクト直下にする
export POETRY_VIRTUALENVS_IN_PROJECT=true
```

したがって、この環境に対応するため、`pyrightconfig.json`に以下の内容を入れておく必要がある。

```json
{
    "venvPath": "",
    "venv": ".venv"
}
```

このとき、各設定の詳細は以下の通り

* `venvPath` : 仮想環境たちを入れておくためのディレクトリ。
といっても今回の場合は、プロジェクトのルートにひとつだけ仮想環境があるだけ。そのため `""` にしている。
ちなみに、この行自体を削除してしまうとうまく行かなかった。

* `venv` : 仮想環境の名前。つまり仮想環境が入っているディレクトリ名。今回の場合は`.venv`というディレクトリが対応する。

