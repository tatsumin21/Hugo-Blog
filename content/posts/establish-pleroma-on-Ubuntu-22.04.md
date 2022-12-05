---
title: "在Ubuntu 22.04上搭建Pleroma实例"
date: 2022-12-05T18:59:38+01:00
draft: false
showDate: true
tags: 
  - InfoTech
  - Linux
---

## 前言

首先，这不是个详细的手把手教程！只是给我自己的笔记，以及给如果看到这篇文章的人一个引路。

## 准备

- 一台墙外的来自可靠服务商的VPS

    我用的是Hetzner的CX11, AMD64,2G内存40GB SSD硬盘。因为只打算作个人使用，因此这个配置足够跑Pleroma了。毕竟Pleroma默认不缓存站外媒体文件，相比Mastodon能剩下不少硬盘空间。

- 一个域名

    我的域名是在Namecheap买的。如果已有一个域名正在使用，使用此域名的二级域名也可。

- 接入CloudFlare

    作为DNS服务和提升网站的安全防护以及CDN加速（视情况而定某些地区反而有减速效果）

- SSH远程连接VPS

    用Linux的人就不用我说这个了吧。
    Windows用户可以用Bitvise SSH Client 或 PuTTY。

## 安装过程

我是装的OTP release。另有From Source release可选，两种不同安装方法官方文档里都有详细介绍。能读英语的强烈建议参照[官方Document](https://docs-develop.pleroma.social/backend/installation/otp_en/)！！

我是用的OTP版本，因为简单。

中文安装教程 --> [在 Debian 10 / Ubuntu 20.04 上安装 Pleroma](https://suicablog.cobaltkiss.blue/posts/pleroma-installation-on-linux-using-otp-releases/)

这篇教程写的非常非常详细，几乎是喂饭级别。我这里只特别提一下我安装途中遇到的上文没提及的问题：

### libcrypto.so.1.0(or 1.1) not found

在生成config配置文件时，如果成功会出现一系列问题需要回答，但我初次尝试时碰到了 缺少 `libcrypto.so.1.0` 的报错。

如果碰到同样error，可以直接去Ubuntu源码处找到所需文件下载解压：

```bash
wget http://nz2.archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2.16_amd64.deb
sudo dpkg -i libssl1.1_1.1.1f-1ubuntu2.16_amd64.deb
```

如果执行第一步报错地址无效，或如果你的VPS非AMD64架构，则去以下链接处找到合适并有效的版本替换上面命令中的链接就好：

http://nz2.archive.ubuntu.com/ubuntu/pool/main/o/openssl/?C=M;O=D

解压缩成功后再尝试一次生成Pleroma config文件，应该就可以成功了。

### Port 80 已被占用

如果在用Certbot 注册 Let's Encrypt 证书时出现这种报错，那多半就是被Nginx占用了
先停掉Nginx 服务就好，之后注册完再打开。

停止Nignx服务

```bash
systemctl stop nginx
```
开启Nginx服务

```bash
systemctl start nginx
```

### CloudFlare

A Record/CNAME Record 的具体设置方法请参照所用域名商的帮助说明。

最后，如果一切都弄好了但是网站就是打不开，记得去Cloudflare看看SSL/TLS 标签下加密等级有没有从flexible改成full。

这样应该就没问题了。

## 配置你的pleroma！

### 个性化/设置主题

按照官方Doc，OTP release 的自定义主题应该放在 `$STATIC_DIR/static/themes`里，而默认设置 $STATIC_DIR的路径是 `/var/lib/pleroma/static`

于是乎，`styles.json` 文件所应该放的实际目录是 `/var/lib/pleroma/static/static`，而自定义主题所在目录是 `/var/lib/pleroma/static/static/themes/`。

注意这里是叠了两层名为static的目录，而我一开始忽略了这一点，只放在了第一层下。然后前端设置里就理所当然找不到了。

如果 `/var/lib/pleroma/static` 目录下没有这些目录，那就创建一个！

```bash
mkdir -p /var/lib/pleroma/static/static/themes
```

然后再往里塞东西就好。

### 加入Fediverse

刚建好的站点的世界时间线一定是什么都没有的。它会随着你Follow的好友和于其他人的互动而慢慢丰富起来，但作为最开始可以先通过下面两种方法让里面内容更多一些更快和Fediverse的大家互动起来

- Follow a Relay / 加入中继

    很不幸，Pleroma和目前常见的relay实现的兼容性似乎有点问题，能不能加的上是个玄学。

- Follow单个Pleroma实例

    pleroma实例之间可以互相把对方当作一个relay来follow（pleroma实例URL/relay），这样就可以关注到整站的时间线。

    中文Pleroma实例可以去下面的list里找。本来就没几个，都加上也不用多久。
    
    - https://the-federation.info/pleroma
    - https://fedidb.org/network

## 后记

还有这些文章可以参考：

[我的pleroma搭建笔记](https://dasgelobteland.github.io/posts/22pleroma/)

[宝宝的活动板房](https://blog.debula.ml/index.php/archives/5/)
