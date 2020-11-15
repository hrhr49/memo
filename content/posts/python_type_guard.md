---
title: "TypeguardでPython実行時に型チェック"
date: 2020-11-15T13:01:16+09:00
draft: false
tags: ["python", "typeguard"]
---

# Typeguardとは

Pythonのスクリプト実行時に型アノテーションの情報を使った
型チェックをしてくれるパッケージ

## インストール

pipコマンドでインストールできる。

```sh
pip install typeguard
```

## 使い方

自分が使いそうな機能だけ書いておく。他にも便利なのがいろいろあるので
詳細は[User guide — Typeguard 2.10.0.post3 documentation](https://typeguard.readthedocs.io/en/latest/userguide.html)を参照。

### デコレータを使ったやり方

以下のように、typecheckedデコレータをつけた関数は実行時に型チェックされる。

```python
from typeguard import typechecked

# typecheckedデコレータがついた関数は、スクリプト実行時に型チェックされる
@typechecked
def f(x: int):
    print(x)

f('これは文字列だよ')
# 型があっていない場合、以下のエラーが投げられる。
# TypeError: type of argument "x" must be int; got str instead
```

### pytestのプラグインとして使う


以下のように、`--typeguard-packages` にチェックするパッケージ名を指定することで、
そのパッケージをチェック対象にしてpytestを実行できる。

```sh
# foo, bar, xyzパッケージをチェック対象にする。
pytest --typeguard-packages=foo.bar,xyz
```


## 参考

* [公式ドキュメント](https://typeguard.readthedocs.io/en/latest/index.html)
* [GitHubのリポジトリ](https://github.com/agronholm/typeguard)
