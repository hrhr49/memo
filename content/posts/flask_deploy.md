---
title: "Flaskアプリを簡単に本番環境"
date: 2021-08-07T20:19:48+09:00
draft: false
---

Flaskのアプリを簡単に本番環境で動かす方法を探していたとき、
`waitress` なるツールを知ったのでメモ

どうやら、Flaskのページでも紹介されているツールみたい
https://msiz07-flask-docs-ja.readthedocs.io/ja/latest/tutorial/deploy.html

例えば、以下のflaskアプリを動かすとする。(ファイル名は `my_app.py` とする)

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return 'hello'
```

NOTE: waitressはFlask専用ではないので、他のフレームワークとかでも良いいと思う

`waitress` のインストールと実行

インストールは以下の通り
```sh
pip install waitress
```

先程の `myapp.py` の中の `app` を実行する場合は以下のコマンドを実行する。

```sh
# ポート 54321でサーバーを立てる場合
waitress-serve --port 54321 myapp:app
```
