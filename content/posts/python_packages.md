---
title: "Pythonのバッケージいろいろ"
date: 2020-11-15T14:02:36+09:00
draft: false
tags: ["python", "package", "tool"]
---

Pythonのパッケージをとりあえずまとめておく。
まだ試せていないものもあるけど、
今後触っていけたらいいなと思っているものもどんどん追加していく予定。

## データ可視化

* matplotlib
* seaborn
* vpython: 3D図形の表示
* pygraphviz: グラフ(点と辺で構成されるやつ)を表示

## 数学

### グラフ理論
* networkx

### 数理最適化
* pulp: 線形計画問題や、混合整数計画問題を解く
* cvxpy: 非線形最適化の場合に使う
* pyopt: 非線形最適化の場合に使う

## 機械学習
* jupytext

### 自然言語
* gensim: 簡単にトピック分析できるらしい

## データ抽出ツール
* dateparser: 時間を表す文字列("two weeks ago" or "tomorrow"などもつかえる！)からdatetimeオブジェクトを作成

## ファイル
* csvkit: CSVファイルの操作コマンド群
* csvtotable: csvファイルから、ソートやフィルタ可能ないい感じのHTMLを生成
* watchdog: ファイルの変更を監視
* pyyaml: YAMLファイルの読み書き
* yq: jqのyaml用ラッパー
* jinja2: テンプレートエンジン

* ranger-fm: ファイルマネージャ

## コンソール
* rich: コンソールでリッチテキスト的な表示

## 開発環境
### 環境を管理
* pipenv
* poetry: こっちだとパッケージの作成とかもできるっぽい

### エディタ
* neovim
* pynvim

### 静的解析
* flask
* pyflakes
* flake8
* jedi
* autopep8
* mypy
* pylint
* python-language-server

### デバッグ
* better_exceptions

## Web
* requests: httpするためのいい感じのラッパー
* beautifulsoup4: HTMLからデータ抽出
* dominate: htmlを作るためのDSL
* flask: 軽量Webフレームワーク
* peewee: ORマッパー


## 音声関連
* librosa: 音声信号処理
* pyworld: 音声解析
* simpleaudio: 音声再生

