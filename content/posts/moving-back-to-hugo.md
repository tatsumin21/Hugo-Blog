---
title: "博客迁移回Hugo以及域名更改"
date: 2021-09-28T23:51:32+02:00
draft: false
toc: false
images:
tags:
  - Thoughts
---

## Moving back to Hugo

是的，我的博客又又又又搬家了。

不过这回倒是没有过多折腾，而是迁移回到了老朋友Hugo来。大致原因以下几点：

1. Windows Terminal的使用体验改善了不少，
2. 微软的WSL项目已经更新到了WSL2，windows上也能很平滑无障碍地安装并使用Linux内核系统的环境和Shell，
3. 通过Netlify免费服务能实现自动生成并部署网页，能够省掉每次生成部署的麻烦，用Git来管理，并存放在github上也能在更换设备后通过简单的 `git pull` 直接拉下来然后接着写，不用担心备份的问题。
4. 能省掉一份租VPS的钱！

其实吧，还有一个原因是，我果然还是更喜欢这种简单的静态网页。

早前折腾博客总是到最后没更新多少就直接杀掉了，多半是因为静态网页生产在需要在本地搭建环境生成然后部署。我想玩游戏就必须得用回Windows，然而 Windows 下各种环境搭建麻烦不说，Powershell一直以来也不是那么好用，因此便总是更新几篇就没然后了。

前段时间久违地翻腾了不少人的博客后发现了Netlify自动部署这么个好东西，本地只需要写就好，而就算Powershell再难用，也就是用个Git而已。

于是我就又回来啦！

## 新域名

首先，其实本来是想用 `tatsumi.dev` 的，但是搜索的时候发现已经被占用了，于是就根据日语的习惯取了tatsumin这个读法。

大概类似昵称的感觉吧。

至于tatsumi，汉字是巽。巽卦为风。

其实没有太多深层含义，纯粹是我想换网名了。虽然严格来说是只换了一半，Noris和Norihiro依旧在很多平台上都用着。

至于域名，选择了 `.dev` 而不是 `.com`，是因为 dev 域名默认开启了 `HSTS (HTTP Strict Transport Security)` 并因此拥有更高的安全性。同时我也在DNS服务中开启了 `DNSSEC`。

- 网站的 `HSTS Preload` 状况可以在这里检查：[hstspreload.org/](https://hstspreload.org/)

还是那句老话，虽然只是个没什么人来看的小破站，但我仍然希望能在我的知识和能力范围内提高它的隐私保护和安全性。

## 其他碎碎念

评论暂时没折腾。Disqus的安全性存疑，其他实现我还得再查查看Netlify上能不能跑。

先这样吧。