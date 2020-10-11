---
title: "Dockerの使い方"
date: 2020-09-19T17:55:26+09:00
draft: false
tags: ["docker"]
---

# Dockerメモ

## Dockerのインストール
どうやら、種類がいろいろあってdocker-ceはdockerの公式がサポートしていて、
docker.ioはUbuntuがサポートしているみたい。
docker.ioに関しては以下のようにしてインストールできた。

```sh
sudo apt install docker.io
```

Dockerに合わせてdocker-composeも一緒にインストールしておくと便利。
これを入れておくと、コンテナの構成管理などが便利らしい。

```sh
sudo apt isntall docker-compose
```

## sudoをつけずに実行するようにする

注意：セキュリティ上のリスクを理解した上で実行する必要あり

dockerグループに対象のユーザを所属させる。
以下を実行した後に再ログインする。

```sh
sudo groupadd docker
sudo gpasswd -a $USER docker
sudo systemctl restart docker
```

参考： https://qiita.com/DQNEO/items/da5df074c48b012152ee

## DockerでHello World

以下のコマンドで `hello-world` のDockerイメージからコンテナを起動できる。
もしこのイメージがまだローカルにダウンロードされていない場合、
[Docker Hub](https://hub.docker.com/)からイメージをダウンロードした後に実行される。

```sh
docker run hello-world
```

このコマンドを実行すると以下のようなメッセージが表示される。

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
略
```


## 簡単なDockerイメージを作る

自分用のdotfiles(各種設定ファイル)をインストールするスクリプトをコンテナ内で実行するための
Dockerfileを作って動かしてみたのでその時のメモ。
(インストールスクリプトといっても、各種設定ファイルを、
ホームディレクトリなどにシンボリックリンク張るだけですが...)

以下のURLにおいてある。(注意：将来的に内容が変わっている可能性あり。)

https://github.com/hrhr49/dotfiles/blob/master/Dockerfile

補足：
Dockerfileを使わずに、Dockerコンテナからイメージを作成することも可能。
しかし、この方法だと設定に何を行ったかがわかりづらくなってしまうので今回はこの方法を採用しなかった。

1. Dockerfileを作る

最終的に以下のような内容のものになった。

```docker
FROM ubuntu:20.04

# インタラクティブな選択画面を使用しない
ENV DEBIAN_FRONTEND=noninteractive

# 先にtzdataを入れないと、タイムゾーンの選択画面が出てしまう(Ubuntu18.04以降)
# https://qiita.com/yagince/items/deba267f789604643bab
RUN apt-get -y update && \
    apt-get -y install \
    tzdata

# Build Environment
RUN apt-get -y update && \
apt-get -y install \
    language-pack-ja-base \
    language-pack-ja \
    sudo

# Utility
RUN apt-get -y update && \
apt-get -y install \
	python3 \
	wget \
    curl \
	zip \
    unzip \
	git

# 日本語の文字化けなどを回避
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8

ENV USER user
ENV HOME /home/${USER}
ENV SHELL /bin/bash

# 一般ユーザアカウントを追加
# -m オプションで、user:userでホームディレクトリを作る
# -s オプションでログインシェルを指定
RUN useradd -G video -ms /bin/bash ${USER}

# 一般ユーザにsudo権限を付与
RUN gpasswd -a ${USER} sudo

# 一般ユーザのパスワード設定
RUN echo "${USER}:${USER}" | chpasswd

# 以降のRUN/CMDを実行するユーザ
USER ${USER}

WORKDIR ${HOME}
```

### Dockerfileの注意点

#### 人間用の選択画面を回避する

どうやらUbuntu18.04以降のバージョンだと、途中でインタラクティブな画面が
出てくるため、ちゃんと動作しないらしい。そこで、以下の内容をDockerfileの
頭の方に入れている。

参考: https://qiita.com/yagince/items/deba267f789604643bab

```docker
# インタラクティブな選択画面を使用しない
ENV DEBIAN_FRONTEND=noninteractive

# 先にtzdataを入れないと、タイムゾーンの選択画面が出てしまう(Ubuntu18.04以降)
RUN apt-get -y update && \
    apt-get -y install \
    tzdata
```

#### 日本語関連

以下の記載をすることで、日本語環境に対応する。

```docker
# 日本語の文字化けなどを回避
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8
```

#### ユーザ設定

ユーザを作成してその設定を使うために以下の内容を記載する。

```docker
ENV USER user
ENV HOME /home/${USER}
ENV SHELL /bin/bash

# 一般ユーザアカウントを追加
# -m オプションで、user:userでホームディレクトリを作る
# -s オプションでログインシェルを指定
RUN useradd -G video -ms /bin/bash ${USER}

# 一般ユーザにsudo権限を付与
RUN gpasswd -a ${USER} sudo

# 一般ユーザのパスワード設定
RUN echo "${USER}:${USER}" | chpasswd

# 以降のRUN/CMDを実行するユーザ
USER ${USER}

WORKDIR ${HOME}
```

2. Dockerイメージをビルドする

以下のコマンドを実行することで、DockerfileからDockerイメージを作成する。

```
docker image build -t {タグ名} .
```

今回の場合は、 `hrhr49-dotfiles` というタグ名にしたため、以下のコマンドを実行した。

```
docker image build -t hrhr49-dotfiles .
```

なお、以下のコマンドでローカルにあるDockerイメージ一覧を確認できる。

```
docker image ls
```

### Dockerイメージからコンテナを実行

先程ビルドしたDockerイメージからコンテナを実行するには以下のコマンドを実行する。

```
docker run --name {コンテナ名} --rm -v {ホスト側のパス}:{コンテナ内のパス} {Dockerイメージのタグ名} {実行するコマンド}
```

補足：
上記のオプションはよく使うので記載している。
詳細は以下の通り。

* `--name` : 実行されるコンテナに名前をつける。
* `--rm` : コンテナの実行が終わったら、自動でコンテナを削除する。これをやらないと、コンテナ実行終了後もずっとゴミが残る。
* `-v {ホスト側のパス}:{コンテナ内のパス}` : ホスト側のディレクトリをコンテナ側にマウントする。ホスト側とコンテナ内でファイルを共有したいときに便利。

今回の場合は、カレントワーキングディレクトリ`install.py` というPythonスクリプトをコンテナ内で実行したかったため
以下のようなコマンドを実行するようにMakefileに書いている。

```
docker run --name dotfiles-test --rm -v ${PWD}:/home/user/dotfiles ${DOCKER_TAG_NAME} python3 /home/user/dotfiles/install.py
```

## コマンドめも

### よく使うパターン

インタラクティブモードで動かしたいときには `-it` オプションをつける。

```sh
`docker run -it -d -p <ホスト側ポート>:<コンテナ側ポート> -v <ホスト側ディレクトリ>:<コンテナ側ディレクトリ> --name <コンテナ名> <Dockerイメージ名>`
```

* `docker help` : ヘルプ。多分サブコマンドでも行ける

### コンテナ操作
* `docker container run (container-name)` : コンテナの実行(containerは省略可)
    - `-d` : バックグラウンドで実行
    - `-p 9000:8080` : ポートフォワーディング(ホストの9000番ポートをコンテナの8080番ポートにつなぐ)
    - `--rm` : 実行終了時に自動的に削除する(デフォだと停止してもずっと残る)
    - `--name` : 名前をつける
    - `-P` : ポートをランダムに割り当てる
    - `-v` : ボリュームを指定することでホスト側とのディレクトリの共有する。以下のように指定する。

```
-v (ホスト側のディレクトリ):(コンテナ側のディレクトリ)
```

たとえば、`-v $(pwd)/logs:/share/logs`とするとホスト側の`$(pwd)/logs`がコンテナ側の`/share/logs`と 共通になる。



* `docker ps` : 動作中のコンテナ一覧
    - `-a` : 停止しているコンテナも表示(このオプションは常につけたほうがいい)

* `docker container prune` : 停止済みのコンテナを削除
* `docker container stop $(docker container ls --filter "ancestor=example/echo" -q)` : コンテナ停止
* `docker port (container-name)` : コンテナが使用しているポートを表示
* `docker rm (コンテナ名またはID)` : コンテナの削除(動作中のものは`-f`つけないと削除できない)
* `docker cp (コピー元ディレクトリ) (コピー先ディレクトリ)` : ファイルのコピー。
    ホストのディレクトリの場合は通常のパスを指定。コンテナのディレクトリの場合は`(コンテナ名):(パス)`の形式
    で指定する。

**例**

```docker
docker cp $(pwd)/.vimrc hoge-container:/home/fuga/ # ホスト側のvimrcをhogeコンテナのfugaホームディレクトリへ
```

* `docker exec -it (コンテナ名) bash` : (コンテナ名)のbashをインタラクティブモードで実行できて便利
* `docker logs -f (コンテナ名)` : 実行したコンソールログを表示

### イメージ操作

* `docker image build -t (image-name)[:tag-name] Dockerfileのあるディレクトリ` : イメージのビルド
* `docker search (keyword)` : Docker Hubでイメージを検索
* `docker image pull (repository-name)[:tag-name]` : イメージの取得
* `docker image ls [repository-name)[:tag-name]` : イメージの一覧
* `docker images` : イメージ一覧
* `docker image tag (org-name) (new-name)` : イメージのタグ付け
* `docker rmi (イメージ名またはID)` : イメージの削除
* `docker pull (イメージ名)` : イメージをDockerHubから落としてくる

### 全コンテナ削除

* `docker rm $(docker ps -aq)` : ストップ中のを削除
* `docker rm -f $(docker ps -aq)` : 全部削除(動作中のものも)

## コンテナからイメージを作成
ホスト側で以下のコマンドを実行する。

```sh
docker commit (コンテナ名) (作成するイメージ名)
# 例: docker commit tomcat tomcat-image
```

## Dockerfileの書き方

基本は以下の内容を記載する。

```docker
FROM (イメージ名)
RUN (イメージ作成において予め実行するコマンド。yumとかaptなどでのinstallを書くと便利)
ADD (ホスト側のコピー元ディレクトリ) (イメージ内でのコピー先ディレクトリ) # ファイルのコピー。tar.gzなどは自動で展開される。COPYも似てるが展開はされない
CMD ["コマンド", "コマンド引数", ... ] # コンテナ起動時に実行されるコマンド
```

簡単な例。 `alpine linux` というイメージはサイズが小さいので、良くDockerで使われている。

```
## alpine linuxがサイズが小さいらしいので使ってみた
FROM alpine:3.4

RUN echo "今イメージ作成中です..."

## hoge.txtに適当な内容を書いておく
ADD ./hoge.txt /

## hoge.txtの内容を表示する
CMD ["cat", "/hoge.txt"]
```

注意: RUNコマンドは各コマンドが前回の実行状態になっているわけではない。
例えば、`RUN cd /hoge`のあとに`RUN touch fuga.txt`などがあっても想定通りにならない。
必要なら`&&`で連結する必要がある。

また、コマンドごとに中間的なイメージがキャッシュされる。
したがって、あまり変わらない部分を頭に配置してまとめておくと良い。


# 参考リンク

* [チュートリアル(英語)](https://docker-curriculum.com/#introduction)
* [さくらナレッジ](https://knowledge.sakura.ad.jp/15253/)
