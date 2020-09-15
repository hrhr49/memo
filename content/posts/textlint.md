---
title: "Textlintの使い方"
date: 2020-09-15T21:52:48+09:00
draft: false
tags: ["tool", "JavaScript"]
---


# textlintメモ

文章の校正ツール。Node.jsの環境で動く。

# インストール
npmでインストールできる。

```sh
npm -g install textlint
# プリセットもインストールしておくと良い
npm -g install textlint-rule-preset-ja-technical-writing
npm -g install textlint-rule-spellcheck-tech-word
```

# 使い方

作業ディレクトリで `textlint --init` コマンドを実行すると、設定ファイル `.textlintrc` が生成されるので、それに設定を追加する。

```json
{
  "filters": {},
  "rules": {
      "preset-ja-technical-writing": true,
      "spellcheck-tech-word": true
  }
}
```

その後、以下のコマンドで対象のファイルの校正結果を確認できる。

```
textlint {ファイル名}
```


