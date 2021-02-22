---
title: "HugoのMarkdownで画像を表示"
date: 2021-02-23T07:44:13+09:00
draft: false
---

HugoのMarkdownで画像表示しようとして、ちょっと調べないとわからなかったのでメモ

## 結論

以下のようなディレクトリ構成にすることにした。

```
ルート
    content
        posts
            記事名
                index.md
                画像ファイル
```

要点
* postsの下に、記事ごとにディレクトリを作る
* 記事の内容はindex.mdに書く
* 画像ファイルは、同一のディレクトリに配置する
* Markdown内での画像の指定は普通に`![](画像ファイル)`でOKだった(特に`./画像ファイル` とか記載しなくても良かった)

なお、記事を生成するコマンドは以下のような感じで行けた。

↓ 本記事を生成したときのコマンド
```sh
hugo new posts/hugo_image/index.md
```

## 参考
[画像ファイルを Markdown ファイルと同じディレクトリに置く (Page Bundle) | まくまくHugo/Goノート](https://maku77.github.io/hugo/misc/page-bundle.html)
