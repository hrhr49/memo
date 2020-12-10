---
title: "Tensorflow Gpu"
date: 2020-12-03T08:00:18+09:00
draft: true
---

# Tensorflow with GPUの環境構築

## 環境

注意：現在はまだUbuntu 18.04までしかいろいろと対応していないけど、20.04に入れた。

Ubuntu 20.04
GPU: RTX2060

tensorflowのバージョン

```
>>> tensorflow.__version__
'2.3.1'
```

## すでにある環境を一旦アンインストール
いろいろと入れていたものがあってよくわからない状況だったのでアンインストールした。

`nvidia-smi` を実行したところ、CUDAのバージョンが11になっているので
変更する必要があった。

```
nvidia-smi
```

```
sudo apt-get --purge remove "nvidia-*"
sudo apt-get --purge remove "cuda-*"
sudo apt autoremove "nvidia-*"
sudo apt autoremove "cuda-*"
```

あと、 `/usr/local/cuda-11.0` が残っていたので削除した。
勝手に削除しちゃってよかったのかな？

## インストール

以下は現在 2020/12/03時点の情報

入れるドライバ、CUDA, CuDNNのバージョンを確認する。
https://www.tensorflow.org/install/source#common_installation_problems

今回の場合、以下のURLによると、CUDAのバージョンは10.1しかだめみたい?
https://www.tensorflow.org/install/gpu?hl=ja

### CUDAとGPUのドライバをインストール


```
sudo apt install nvidia-cuda-toolkit
sudo apt install nvidia-driver-455
sudo reboot # rebootしないとエラーが出た
```

### CuDNNのインストール

nvidiaのページでアカウントを作る必要がある。
Googleアカウントでもログインできるっぽい。

https://developer.nvidia.com/cudnn

今回は、以下のを入れることにした。
Download cuDNN v7.6.0 (May 20, 2019), for CUDA 10.1 の中にある
* cuDNN Runtime Library for Ubuntu18.04 & Power (Deb)
* cuDNN Developer Library for Ubuntu18.04 & Power (Deb)

これらをダウンロード後、 `sudo dpkg -i ...` をそれぞれ実行し、PCを再起動する。


## Dockerで使えるようにする

以下のURLの「Setting up NVIDIA Container Toolkit」のところからを実施する。

https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker

## GPUが使えるか確認

```python
from tensorflow.python.client import device_lib
device_lib.list_local_devices()
```

以下のようにdevice_type: "GPU"のがあればよい。
```
physical_device_desc: "device: XLA_GPU device"
, name: "/device:GPU:0"
device_type: "GPU"
memory_limit: 5264848992
locality {
  bus_id: 1
  links {
  }
}
```

[TensorFlowからGPUが認識できているかを2行コードで確認する - 動かざることバグの如し](https://thr3a.hatenablog.com/entry/20180113/1515820265)
[Ubuntu 20.04へのCUDAインストール方法 - Qiita](https://qiita.com/yukoba/items/c4a45435c6ee5d66706d)
