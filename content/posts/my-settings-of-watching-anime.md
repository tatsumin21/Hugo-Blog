---
title: "我的Linux下简易（海外）看番配置备忘"
date: 2025-07-13T19:01:17+02:00
draft: false
showDate: true
Tags
 - Linux
 - Life
---

## 事情是这样的

又是好~久没有写东西了（拍脑袋。

2.5次元或3次元认识我很久的人应该知道我其实是不太爱看动漫的。

我也不清楚具体问题出在哪。但之前也看了好几部人气比较高／朋友力荐的作品，都是要么强行看完内心毫无波澜要么甚至都坚持不到看完。于是我甚至一度觉得我和二次元大概是无缘了。
直到去年年底。
因为毫无预兆地跳进了 _milet_ 的大坑，又在和基友聊天的时候得知她的那首 ___Anytime Anywhere___ tie了一部很火评价很高的动漫，而那部动漫叫 ___葬送的芙莉莲(日：葬送のフリーレン)___ 。
出于好奇和不信邪，心想反正最差也就是看两集不行再删嘛，便又试着下载来试试看看。

结果意外地非常合我口味！
第一集的时候还觉得稍显平淡了点，但看完两集后我的手开始自己去打开了第三集，然后是第四集，第五集...
于是乎我甚至在工作日熬了两个大夜一口气把28集看完了。

然后我甚至去Amazon JP上把14卷漫画也买回来kindle上看了（！）

再然后，又看了和芙莉莲同一个监督，同样人气也挺高的 ___孤独摇滚（日：ぼっち・ざ・ろっく!）___ 。
虽然小孤独没让我像看芙莉莲时感觉那么惊艳，但也算好好看完了。

STOP！

其实要写的是这个过程中我自己用来还算舒服的资源和配置。

## 资源

咳咳！各位如果有条件的还是要多支持正版啊！

### 在线观看

- [Anime1.me](https://anime1.me/)

![Anime1.me](/img/img-2025-07-13-20-07-51.png)

应该是个台湾的网站，内文使用繁体中文，搜索的时候可能需要切换一下输入法。
内容还算全。里面也有当季度新番表，更新时间。

画质如果放大到全屏观看可能会有点不那么高清，但也不那么影响体验。

### 下载到本地

- [Nyaa.si](https://nyaa.si/)

![Nyaa.si](/img/img-2025-07-13-21-04-12.png)

挺大的日系资源站，里面有各种漫画，动漫，字幕，书，甚至是有损/无损音乐。
提供BT种子或者磁力链接下载。

- [东京图书馆/東京図書館](https://www.tokyotosho.info/)

![東京図書館](/img/img-2025-07-13-21-15-49.png)

也是个很大的日系资源的BT种子下载站。
搜索需要用日文原文或罗马音或英文。
一眼望去似乎也有不少R18内容，请自行注意辨别。

- [动漫花园](https://dmhy.anoneko.com/)

![动漫花园](/img/img-2025-07-13-21-22-14.png)

各种动漫及其音乐资源，多为字幕组制作后带字幕的形式。需要熟肉的朋友会比较开心。
下载为磁力链接形式。

- [VCB-Studio](https://vcb-s.com/)

![VCB-Studio](/img/img-2025-07-13-21-30-21.png)

专门做高清修复的压制组，出品不会是最新，但胜在有修复，画质够好。适合拿来收藏用。
下载多为BT资源。

## 本地配置

### mpv + yt-dlp + Anime4K 

播放器为mpv。
虽然没有GUI操作界面，需要自行配置一下配置文件。但轻量 + 支持格式多 + 支持一些自定义扩展，所以只要稍稍花时间配置好了以后，使用体验会相当舒适。

Arch Linux 下用户级配置文件位于 `~/.config/mpv/`，但默认不存在。
可通过如下命令复制模板。

```bash
$ cp -r /usr/share/doc/mpv/ ~/.config/
```
Arch Wiki 里有详细的配置说明，可以参考并自行修改。

#### yt-dlp 的支持

mpv可以通过配置yt-dlp来观看YOUTUBE/Bilibili等在线视频，但画质和清晰度需要导入浏览器Cookies

在mpv 的配置文件 `~/.config/mpv/mpv.conf` 中加入如下内容即可

```
ytdl-raw-options=cookies-from-browser=<browser>
```
如果所使用的浏览器为Firefox，则将`<browser>`替换为`firefox`即可。

```
ytdl-raw-options=cookies-from-browser=firefox
```
Chrome同理。

如果所使用的浏览器为Librewolf这类基于Firefox内核所修改，则指定cookies的本地路径：

```
ytdl-raw-options=cookies-from-browser=firefox:/path/to/your/librewolf/cookies/
```

同理，如浏览器为基于Chrome内核的qutebrowser，则为：

```
ytdl-raw-options=cookies-from-browser=chrome:/path/to/your/qutebrowser/cookies/
```

其他以此类推，找到自己浏览器的cookies存放地址后进行替换即可。

这个组合也同样可以用来配合newsboat这类RSS订阅软件来使用。可以以此在不开浏览器网页的情况下观看所（在RSS软件内）订阅的YOUTBE 频道的视频。

#### Anime4K

Anime4K是个github上的开源项目，提供实时动漫高质量升级算法。
可参考github上项目页面选择使用/配置。

[bloc97/Anime4K](https://github.com/bloc97/Anime4K)

配置方法也很简单，把glsl-shader下载下来解压缩到mpv的配置文件夹下即可。
是否写进配置文件就需要根据需求（是否默认开启shader）来自行决定了。

## 结束前有件事要解释

我并不是两个番看了半年！
年初回国和家人过了年，再回来后又因为FF14的零式攻略等占用了大量休闲时间。
以至于Blog大半年甚至都没更新了。

游戏误事啊~



