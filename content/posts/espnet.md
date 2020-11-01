---
title: "ESPnet"
date: 2020-11-01T17:35:53+09:00
draft: true
---

# ESPnetメモ

音声合成(テキスト読み上げ)関連のツールを探しているときに見つけたいい感じのツール。
end-to-endのスピーチ処理のツールキットらしい。
どうやら、ESPnet2というのがあってこっちのが新しいみたい。同一のリポジトリで管理してるっぽい。

公式ページ
* https://espnet.github.io/espnet/

GitHubのページ
* https://github.com/espnet/espnet

## 紹介されていたデモページ

Google ColaboratryでTTS(Text to Speech)を動かせるデモがあった(ESPnet2のもの)。
日本語の音声合成をやってみた感じ、とてもなめらかに発音できてそうだった！

https://colab.research.google.com/github/espnet/notebook/blob/master/espnet2_tts_realtime_demo.ipynb

## 自分の環境で動かすやり方

現在調査中。適宜分かり次第、追記していく予定。

### 環境

* Ubuntu 20.04.1 LTS x86_64
* NVIDIA GeForce RTX 2060 Rev. A
* ESPnet v.0.9.5

### 方針

とりあえず、Dockerで動かせそうなので、それを目標にする。

https://espnet.github.io/espnet/docker.html#

上記のドキュメントを参考に、 `docker` ディレクトリでコマンドを実行した。

### 問題1 `could not select device driver "" with capabilities: [[gpu]]` 

うまく行かなかった。
(なんか`could not select device driver "" with capabilities: [[gpu]]` みたいなのが
表示されてエラーになった)

以下のissueによると、
https://github.com/espnet/espnet/issues/2041

これが原因らしい。
https://github.com/NVIDIA/nvidia-docker/issues/1034

議論をいろいろ見ていくと、どうやら `nvidia-container-toolkit` なるものを
インストールした後、 `systemctl restart dockerd` をやる必要があるみたいだった。

`nvidia-container-toolkit` と言うやつは、dockerのコンテナ上でcudaを動かすやつらしい。
とのことで、以下の記事を参考(もともとは公式ページの手順から引用しているみたい)に`nvidia-container-toolkit` 
を入れたら、このエラーは解決した。

https://qiita.com/Hiroaki-K4/items/c1be8adba18b9f0b4cef

↓上記URLの内容から引用

```sh
# Add the package repositories
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```

### 問題2 `./run.sh: invalid option --dlayers`

以下のようなエラー内容が表示された。

```sh
./run.sh: invalid option --dlayers
```

https://github.com/espnet/espnet/issues/1619
の議論の中で同じことに触れられているっぽい。

とりあえず、この問題は置いといて、TTSのプログラムを動かすためには
以下を実行すれば良かったっぽいので今回は深入りしない。

```sh
./run.sh --docker_gpu 0 --docker_egs ljspeech/tts1 --docker_folders /export/ljspeech/tts1 -ngpu 1
``

この詳細はまだ調査中・・・・
