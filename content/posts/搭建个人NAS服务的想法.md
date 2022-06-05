---
title: "搭建个人NAS服务的想法"
date: 2022-06-05T12:59:33+08:00
lastmod: 2022-06-05T12:59:33+08:00
description: "最近又搭建个人NAS服务的想法，分析下各种方案的利弊和价格。"
tags: ["个人NAS服务器"]
featured_image: "https://cdn.yearnfar.com/blog/22/06/29271329604ed7c41b3cbc5fbd435c21.webp"
# images is optional, but needed for showing Twitter Card
images: []
categories:
comment: false
draft: false
author: "yearnfar"
---

最近有点想搭建个人NAS服务。原因有以下几点：

1. NAS服务器可以用于存储个人数据，因为我已经解决了IPv6问题，再外面访问家里的网也不是问题，即使可能遇到IPv6无法访问的情况（比如我连了不支持IPv6路由）也可以通过内网穿透搞定。
2. 一直开着一台服务器可以对外提供服务，比如IPv6下载站点。毕竟使用各种云服务商的存储和流量是需要花钱的，比如阿里云OSS 5G以内不收存储费用，流量另外计费0.8元/G，而家里的网络不用另外交费用。

### 个人NAS方案

1. 选择群辉、威联通、绿联之类的品牌
2. DIY一套系统

这里以2个盘，不同方案的费用进行对比，可以看到这些NAS都不便宜。

![nas对比](https://cdn.yearnfar.com/blog/22/06/0a0984aedb18129df96b88f91b2b350c.png)

考虑到可玩性，我更倾向于自己DIY一个NAS系统，DIY只需要一个mini主机 + 硬盘柜子。

mini主机可以选择各种pi。比如 树莓派、rock pi、香蕉pi等等。最近树莓派价格高的离谱，直接pass。顺便也可以支持下国产芯片厂商，比如 晶晨、瑞芯微等。当下晶晨的比较好的芯片有A311D、瑞芯微的rk3399、rk3588等。

![开发板对比](https://cdn.yearnfar.com/blog/22/06/59908f054207c33e856c4b10b27f68b0.png)

硬盘柜

![硬盘柜](https://cdn.yearnfar.com/blog/22/06/460c9395d867cfb64710f7912f12b8ba.png)

可以看出DIY的费用在 1409 ~ 2134 之间。当然，还想更低也不是不可以，因为除了用作NAS，我还希望它能承担一点服务器的作用。所以我倾向上面三种。

关于硬盘

![硬盘](https://cdn.yearnfar.com/blog/22/06/ae43a15f494bfcebeebdfc63daea53c7.png)

### 写在最后

DIY的成本大概是购买品牌上的47%~77% 之间。但是即使使用rock pi + 铁威马 + 东芝硬盘(2个硬盘)的最便宜方案，也需要2247元。我的内心更倾向于 Khadas VIM3 + 绿联 + 西部数据（2个硬盘）的方案，成本 3192 元。

看到这个费用我重新思考了下，我对NAS的需求，可能是个伪需求吧。。。



