---
title: xshell 连接 virtualbox 下的ubuntu 虚拟机问题
img: /medias/blog/201905/xshell-overview-1.png
categories:
  - Linux
tags:
  - Xshell
date: 2019-05-25 20:55
---

    之前在virtualbox安装ubuntu 后一直用的图形界面。
    电脑配置不行，还是太卡了，想着用Xshell连接算了。
    准备：

## 1.virtualbox 设置网络使用 仅主机(hostonly)

![][1]

## 2.打开电脑的网络连接确认虚拟机的网卡启用

![][2]

![][3]

![][4]

## 3.打开虚拟机查看自动分配的网路地址：

![][5]

## 4.windows物理机Ping虚拟机测试网络连通状态：

![][6]

## 5.Xshell尝试连接：

![][7]

## 出错：连接不上

![][8]

## 6.关闭linux 防火墙

![][9]

## 7.登录成功

![][10]

![][11]


  [1]: /medias/blog/201905/4282315553.png
  [2]: /medias/blog/201905/621480926.png
  [3]: /medias/blog/201905/2778096511.png
  [4]: /medias/blog/201905/366886625.png
  [5]: /medias/blog/201905/2193637862.png
  [6]: /medias/blog/201905/2485706882.png
  [7]: /medias/blog/201905/412057713.png
  [8]: /medias/blog/201905/2620989300.png
  [9]: /medias/blog/201905/766252927.png
  [10]: /medias/blog/201905/619433018.png
  [11]: /medias/blog/201905/2493423827.png