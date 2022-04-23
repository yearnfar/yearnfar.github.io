# yearnfar.github.io

## git 中文乱码问题

markdown文件名使用中文，git status显示出现乱码，运行下列命令即可解决问题。
```
git config --global core.quotePath false
```

## 安装

首次拉代码, 为了将submodule拉到本地，需要加上 --recursive
```
git clone --recursive git@github.com:yearnfar/yearnfar.github.io.git

```