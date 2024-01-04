---
title: "Arch Linux 音频服务器从PulseAudio 切换到 Pipewire"
date: 2023-08-14T22:24:01+02:00
draft: false
showDate: true
Tags:
  - Linux
  - InfoTech
---

## 什么是Pipewire

> PipeWire是一个新的底层多媒体框架。它专注于同时为音频和视频提供低延迟的录制和回放，Pipewire支持所有接入PulseAudio，JACK，ALSA和GStreamer的程序。

## 为什么

最开始注意到这个词是在尝试 `OBS Studio` 在Wayland下直播的时候，发现其屏幕捕捉用的就是Pipewire。再然后去简单搜了一下，发现这是个新的多媒体流处理系统。比PulseAudio处理治疗要更好，劣于JACK。但易用性要大于JACK。大概是介于PulseAudio和JACK中间的位置。按我的理解来说，如果不是有特别专业的需求必须要用到JACK，则它会是个好选择。

好了我坦白，其实就只是因为想试试新东西。

## 怎么切换

1. 如果现在正在用PulseAudio，停掉Systemd启用了的Service和Socket。
2. 安装相关包，以Arch Linux为例：

```bash
# pacman -S pipewire pipewire-pulse pipewire-alsa wireplumber
```

安装的时候会提示Pipewire和PulseAudio冲突，需要卸载PulseAudio，下定决心了的话确认就好。

如果有需要还可以安装multilib仓库里的 `lib32-pipewire`，来提供对32位的支持。

3. enable and start Pipewire相关service和Socket，然后重启一次，简单日常使用这样应该就可以了。

## 更专业一点的处理

[Audio后期处理](https://wiki.archlinuxcn.org/wiki/PipeWire#Audio%E5%90%8E%E6%9C%9F%E5%A4%84%E7%90%86)

## 更专业一点的介绍

- [概览Linux音频系统 - Leo's Field](https://szclsya.me/zh-cn/posts/linux/audio-system/)

- [用了200天的PipeWire到底好在哪？ - 啦哆咪](https://lado.me/2022/04/16/whats-good-about-pipewire/)


