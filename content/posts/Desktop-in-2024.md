---
title: "Desktop in 2024"
date: 2024-09-05T20:32:11+02:00
draft: false
showDate: ture
tags: - Linux
- Life
---
又是好久没更新了，懒了很久一直都没动力。直到上周实在是自己看不下去自己了才狠下心逼自己动起来。

至于为什么又又又又要讲Desktop的东西，其实是因为，大概在半年前我为了FF14曾经把系统全换成了Windows 11（没错就是那个该死的7.0！）

然后上周为什么又又又又又换回Arch Linux了，原因大概是下面两个：

 1. 我发现了Linux下也能用的效果还可以的加速器。
 2. 待在Windows下还是很难受，我还是很怀念dwm的栈式平铺逻辑和很多时候可以不需要碰鼠标的体验。

于是我就再次把又把windows 11 格式化了然后装回了Arch Linux。
但是毕竟距离上回也隔了起码一年多，虽然很多配置文件当时做了备份，但也有不少变动。

于是又可以久违地水一篇了。

## 系统安装

这次和上次装系统最大的区别大概是文件系统和内核。

文件系统从ext.4 换成了Btrfs。而内核直接用的Linux-Zen，反正我也就是日常使用不做生产力工具。

### 内核

内核没什么可讲的，pactrap的时候直接没选linux 而是选的Linux-Zen，连我个技术小白都不需要找教程的程度。

但有一点或许可以稍稍提一下：

在装驱动的时候，比如我这个用Nvidia的倒霉蛋，如果有使用Wayland 的需求或者需要将显卡加进内核模块加载的话，Linux-Zen内核的组合应该是

`Linux-Zen` + `Linux-Zen-headers` + `nvidia-dkms` 这样的组合。

### 文件系统

都2024年了，对吧。
Btrfs那么好用的快照功能不用白不用！

之前的文里应该有提到过，我的硬盘是一块1T 的SSD 和一块2T的HDD。
系统一定是装在SSD的，但2T我想分给`/home`。

分区基本还是和普通ext.4没什么区别：

- SSD部分：EFI分区，swap分区，` / ` 根目录分区
- HDD部分：一整块！

因为Btrfs特殊的子卷功能特性，快照只照单独子卷，不会照到全硬盘，而如果直接把snapshot放根目录，备份的时候就会把快照也照进去。好像没什么必要？

于是子卷分配如下：

- SSD：
  - ` @ `: 用来之后挂载根目录
  - `@efi`：efi分区挂载用
  - `@swap`：swap分区/文件用
  - `@snapshot`：为了不把快照照进快照，专门分出来的快照分区

然后因为`/home`我要放在另一块HDD上，于是整个HDD就只创建一个子卷`@home`就ok了。


## 桌面

### dmw & st & dmenu 

原本是想之前备份的打好补丁的DWM 6.3可以直接拿来用的。

其实也确实可以用，但当我以为都弄好了也用着好像没什么问题的时候，问题来了：dmenu 用不了！！会给出 BadMatch 的报错！
一开始在网上搜了很久也没有找到相同问题的解决方案，准备换成rofi的时候，无意中又打开suckless官网看到她们今年3月更新的内容里有提到`Fix: BadMatch`。

于是灵光一闪（不是）想到或许是旧版本本来就有的bug。便一咬牙决定git clone 下最新的重新打个补丁重新来一遍。

于是问题真就没了...

然后想着反正dwm都重来了，以防万一st也有啥问题，便干脆也一起重来了一遍。

但之前的备份什么的都在，快捷键什么的都按照原来的来配的，弄好后用起来也完全不会有什么区别。

### Steelseries GameDac & Pipewire

这次也是跳过了pulseaudio直接上了pipewire，但比之前多了个GameDac，于是又有一个问题了：

之前在Windows下插上就有的双通道GameDac Game 和 GameDac Chat 去哪了？
更准确说是只有Chat，Game消失了。

但这个问题倒是不大，也不用装驱动。

pipewire自带一个pro 的profile，pavucontrol里Configuration标签下的GameDac设备有个`profile`下拉菜单，找到`Pro Audio`，改成这个就好了。

我这里这样设置后，会多出来一个 GameDac Pro 和 GameDac Pro1 两个音频通道，分别对应的 Chat和Game。

这样这个小破GameDac也算是没浪费了（笑）。

## 最后

暂时就想到这些比较大一点的改动了。
最后放一张随便一截的桌面吧。

![Desktop in 2024](/img/desktop.png)






