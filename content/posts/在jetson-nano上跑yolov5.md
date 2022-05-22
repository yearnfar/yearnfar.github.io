---
title: "在Jetson Nano上跑yolov5"
date: 2022-05-22T18:16:32+08:00
lastmod: 2022-05-22T18:16:32+08:00
description: "在Jetson Nano设备上，搭建PyTorch环境，并运行yolov5项目。"
tags: ["jetson nano", "yolov5"]
featured_image: "https://cdn.yearnfar.com/blog/22/05/491ccebbdafadababd79947566ffbb7b.jpg"
# images is optional, but needed for showing Twitter Card
images: []
categories: "jetson-nano"
comment: false
draft: false
author: "yearnfar"
---

## Jetson Naon搭建

参考官方文档：[Jetson Nano 开发者套件入门](https://developer.nvidia.com/zh-cn/embedded/learn/get-started-jetson-nano-devkit)

按照官方文档搭建好Jetson Nano环境后，可以看到/usr/local/cuda目录，将CUDA加入环境变量：

```shell
export CUDA_HOME=/usr/local/cuda
export PATH=$CUDA_HOME/bin:$PATH
export LD_LIBRARY_PATH=$CUDA_HOME/lib64:$LD_LIBRARY_PATH
```

查看CUDA信息

```shell
➜  ~ nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2021 NVIDIA Corporation
Built on Sun_Feb_28_22:34:44_PST_2021
Cuda compilation tools, release 10.2, V10.2.300
Build cuda_10.2_r440.TC440_70.29663091_0
```

## 准备Python

以前一直使用Anaconda管理Python环境，但是在Jetson Nano安装Anaconda或Miniconda时都出现了"core dumped"，最后发现了miniforge 这个项目。

我这里安装的是4.12.0版本：[Mambaforge-Linux-aarch64.sh](https://github.com/conda-forge/miniforge/releases/download/4.12.0-0/Mambaforge-4.12.0-0-Linux-aarch64.sh)

```shell
➜  ~ sudo ./Mambaforge-Linux-aarch64.sh
```

安装好Mamba后就是创建Python环境了。

因为PyTorch官方不提供ARM aarch64架构的CUDA版本，所以我下载Nvida官网编译好的二进制包。目前提供PyTorch1.0.0 ~ PyTorch1.12.0 总计13个版本可供选择。

[PyTorch for Jetson](https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-11-now-available/72048)

怎么选择PyTorch版本呢？我们可以看到不同的PyTorch版本对JetPack版本要求是不一样，先查看Jetson Nano的JetPack版本。

````shell
➜  ~ cat /etc/nv_tegra_release
# R32 (release), REVISION: 7.2, GCID: 30192233, BOARD: t210ref, EABI: aarch64, DATE: Wed Apr 20 21:34:48 UTC 2022
````

可以看到默认安装的是JetPack R32，JetPack R32支持的PyTorch最高的版本是v1.10.0，对应的Python版本是3.6。

![nvidia](https://cdn.yearnfar.com/blog/22/05/f2797550a253297f6de428e927cd81ed.jpg)

创建Python3.6环境：

```shell
mamba create -n yolov5 python=3.6
```

## 安装PyTorch

下载[PyTorch](https://developer.nvidia.com/zh-cn/embedded/learn/get-started-jetson-nano-devkit)，并运行下面命令安装。

```shell
sudo apt-get install libopenblas-base libopenmpi-dev libomp-dev
pip install Cython
pip install torch-1.10.0-cp36-cp36m-linux_aarch64.whl
```

检查PyTorch安装情况：

```shell
(yolov5) ➜  python
Python 3.6.15 | packaged by conda-forge | (default, Dec  3 2021, 19:12:04)
[GCC 9.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> print(torch.cuda.is_available())
True
>>> print(torch.backends.cudnn.version())
8201
```

## 安装torversion 

因为PyTorch选择的是v1.10.0，所以torchvision需要安装版本0.11.0。因为torchvision依赖pillow，安装时默认会安装最新的pillow。但是最新的pillow会产生其他问题，所以这里先安装好pillow版本8.4.0。

[Torchvision项目地址](https://github.com/pytorch/vision)

先安装依赖

```shell
(yolov5) ➜  ~ sudo apt-get install libjpeg8 libjpeg62-dev libfreetype6 libfreetype6-dev
(yolov5) ➜  ~ pip install pillow==8.4.0
```

下载torchvision源码，并安装：

```shell
python setup.py install
```

检查torchvision安装情况：

```shell
(yolov5) ➜  ~ python
Python 3.6.15 | packaged by conda-forge | (default, Dec  3 2021, 19:12:04)
[GCC 9.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torchvision
>>> print(torchvision.__version__)
0.11.0a0
```

## 安装Yolov5

[Yolov5项目地址](https://github.com/ultralytics/yolov5)

下载项目源码，并安装依赖：

```shell
(yolov5) ➜  ~ pip install -r requirements.txt
```

运行yolov5：

```
(yolov5) ➜  yolov5 python detect.py --source 0
```

效果如下：

![23-07-36屏幕截图](https://cdn.yearnfar.com/blog/22/05/7e866c29da33e98d7d2eeea6d25b3af6.png)







