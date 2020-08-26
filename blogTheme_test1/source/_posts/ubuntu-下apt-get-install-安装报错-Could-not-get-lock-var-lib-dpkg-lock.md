---
title: ubuntu 下apt-get install 安装报错 Could not get lock /var/lib/dpkg/lock
img: /medias/blog/201905/375px-Logo-ubuntu_no(r)-black_orange-hex.svg.png
categories:
  - Linux
tags:
  - ubuntu
date: 2019-05-22 20:33
---

## ubuntu 常见错误

```bash
Could not get lock /var/lib/dpkg/lock
```

## 通过终端安装程序sudo apt-get install xxx或者apt-get update时出错：

```bash
E: Could not get lock /var/lib/dpkg/lock - open (11: Resource temporarily unavailable)
E: Unable to lock the administration directory (/var/lib/dpkg/), is another process using it?
```

出现这个问题可能是有另外一个程序正在运行，导致资源被锁不可用。而导致资源被锁的原因可能是上次运行安装或更新时没有正常完成，进而出现此状况，解决的办法其实很简单：
## 在终端中敲入以下两句

```bash
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock
```

再试着安装，问题解决。
