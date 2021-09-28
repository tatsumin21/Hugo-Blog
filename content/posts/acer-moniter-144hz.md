---
title: "Acer 宏碁XB273K 显示器解锁超频4K 144hz 10bit"
date: 2021-09-19T01:52:32+01:00
draft: false
toc: false
images:
tags:
  - InfoTech
---

## 前言

大约半年前买新电脑时一起把显示器也换了。因为显卡直接上了RTX3080，所以显示器也咬牙在我承受范围内选择了支持4K 144hz的Acer XB273K。但是这个型号的144hz需要overclocking，并且如果想开144hz的话，HDR和G-Sync都需要关闭。

最开始组装的时候折腾了好一阵也没闹明白到底怎么开，上网中英文搜索了半天也只找到需要两根DP线，但尝试之后仍是无果，便放置了。前段时间又想起这么回事，找了好几个小时终于找到了解锁方法，特此记录一下。

## 步骤

1. Win10/11系统和显示器硬件panel的HDR以及G-Sync开关都要先关闭，并打开支持144hz Mode。
2. 两根DP1.4的线。这边需要注意的是，两根线在显卡上的接口和显示器上的要对应。即：显卡接口OUTPUT1 - 显示器INPUT1，OUTPUT2-INPUT2。不能乱接，否则开启144hz后会显示`not supported Input`。
3. 如果要开10bit色彩，在Nvidia Control Panel 里先开10bit，然后再开144hz。

其实没啥特别难的，但是如果第一次弄或者不知道细节还是挺费时间的。

权当做个备忘吧。
