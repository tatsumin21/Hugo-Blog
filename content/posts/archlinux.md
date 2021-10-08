---
title: "Archlinux 及常用软件工具安装配置小记"
date: 2020-10-24T22:31:22+02:00
draft: false
toc: false
images:
tags:
  - Linux
  - InfoTech
---
## Keep It Simple, Stupid

我最初尝试使用Linux已经是2015年的事了。基于日渐增加的对Windows（以下简称Win）各种不满，在知乎上看到 只用Linux系统是一种怎样的体验 后决定装个双系统来试试。磕磕绊绊装好也用了一段时间后，却因为某次Win10的系统升级，Arch Linux被莫名其妙干掉后就也放弃了再从头装一遍。更何况专业对Linux没有刚需加上舍不得游戏，便就着Win10也用了下去。直到一个月前无意中看到QQ for Linux 时隔10年居然更新了，无聊中便生出了再次拥抱Arch Linux的想法。

Keep It Simple, Stupid 是Arch Linux发行版的哲学。根据中文Wiki，可以理解为“保持简单，且一目了然”[1]

作为非计算机专业，不会程序语言的外行，一开始并不能理解从硬盘分区开始就是全程手动命令行操作的Arch Linux到底simple在哪。但经过这次的再见面，在一次搞定顺利安装的基础上，似乎稍微了解了一些其中的Simple的含义。

虽然这个词字面翻译来就是简单，但以我个人的理解来看，这个“简单”所对应的反意并不是“困难”，而是“复杂”。具体来说便是从安装到配置，Arch Linux给予了用户相当高的操作控制权，不会给你任何你不想要的东西。而一旦遇到问题时的解决也不会像Win一样给人隔山观海之感，而是能够较为直观地看到并触及到问题的关键所在。

但不可否认的是，这一切仍是需建立在对Linux系统以及命令行操作有一个最基本的了解的基础之上的。也正如Arch Linux的Wiki中所说，这并不是一个“用户友好”的发行版，而是“以用户为中心”的发行版。

## Desktop | 桌面

因为最初尝试Linux时便试过了如KDE，GNOME，XFCE等桌面环境但结果并不十分满意后，这次再来便一开始就决定放弃Desktop Environment（DE）直接上Window Manager（WM）。当然，这二者之间也并无所谓绝对的好与坏，各有所长而已。

简单来说，桌面环境所给的东西更全面，一整套工具开箱即用无需过多细节配置，但不可避免地便会有一些用不到或是不满意的部件需要自行删减或是更改替换。而WM则是一个简单的窗口排布的工具，除此之外再无其他，系统资源消耗低的同时也意味着从虚拟终端到浏览器到邮箱客户端等等一切所需软件工具都需要自己另外安装配置。

按需求和喜好来选择就好。

### Window Manager | 窗口管理器

#### DWM

DWM，Dynamic Window Manager的缩写，Suckless出品的极简主义窗口管理器。

轻量自是不用多说，它最让我喜欢的一点是它的栈式窗口管理逻辑（Stacking）。即，新打开的窗口默认置于顶栈，而越接近栈顶的区域屏幕面积越大。输出到显示器上便体现于新窗口默认打开占据显示器左半，之前打开的窗口被推到屏幕右半与其他旧窗口排布在一起。

Suckless的极简最直观便体现在代码了。与其他众多WM不同，DWM不仅代码精简，甚至连配置文件都没有，所有特性配置直接写在了源代码里，需要什么直接修改头文件甚至C的源代码来实现。而这所带来的对于新手的一大问题便是，它需要自己手动编译安装。
虽然官方网站上提供了相当数量各个功能的补丁，编译安装也就是个`sudo make clean install`的事，但仍是需要至少能看懂个C语言头文件和代码的大概的。

这里非常感谢B站up主theCW的教学视频

-[【高效神器】dwm安装与配置（1） —— 极简的窗口管理器 【Suckless的极简主义01】](https://www.bilibili.com/video/BV11J411t7RY)

-[【高效神器】dwm安装与配置（2） —— 极简的窗口管理器 【Suckless的极简主义02】](https://www.bilibili.com/video/BV1dJ411t7Hn)

具体步骤讲的非常详细好懂，包括打补丁的方法也都有具体演示。只要是能自己参照Wiki独立成功安装Arch Linux的，照着这个视频弄好DWM应该是没多大问题的。
这里具体方法就不再赘述了，只单独提两点我自己在安装配置过程中花费了较多时间精力才解决的，新手容易被困住的问题。

1. alpha patch 和systray patch 的冲突
2. IPC patch

##### alpha patch 和systray patch 的冲突

DWM安装配置好后，状态栏默认是没有常见的时间音量等系统状态显示的，因此常见方法是

1. 通过DWM的autostart patch所提供的功能来自动执行一些脚本来输出所需内容到状态栏上
2. 同样出品自Suckless的Slstatus
3. 使用Conky往状态条输出信息

但以上方法有一个小缺陷：无法显示使用系统托盘。实际上已经有人提供了解决方案。

去Suckless的网站里能找到一个名为systray的patch。但我在打上补丁后却发现DWM无法正常运行了。一顿搜索后发现了问题所在：

名为alpha的提供半透明功能的patch和systray patch有冲突。

这个问题在Reddit上有部分人提了出来，并且暂时成的完美解决方案。当然如果懂C语言的可以直接去源代码自己改。但对于不懂程序语言的我来说，便只好放弃systray patch的实现。

##### IPC patch

按照IPC patch作者的github上该repository dwm-IPC的说明，安装该补丁需要用到一个名为yajl的C JSON的库。

但在安装好yajl并按照普通方法打好补丁后编译dwm时却总是提示缺少yajl的定义。

不懂编程于我便用上了最笨的办法：在github上找其他也用到了polybar dwm module的各个repository。在比对了几个主要文件前面函数声明部分后发现，仅仅安装好yajl库是不够的，编译前同时还需要对dwm的config.mk文件的库的地址里添加指定出yajl库的具体位置。

具体修改后的内容如下：

在config.mk文件中添加：

```make
YAJLLIBS = -lyajl
YAJLINC = /usr/include/yajl

# inlucdes and libs
INCS = -I${X11INC} -I${FREETYPEINC} -I${YAJLINC}
LIBS = -L${X11LIB} -lX11 ${XINERAMALIBS} ${FREETYPELIBS} ${YAJLLIBS}-lXrender
```
如此修改保存后，再对dwm进行重新编译安装，便应当可以成功了。

#### XMonad
XMonad以及之后提及的XMobar，都是属于以Haskell语言编写的窗口管理器和状态栏。在Arch Linux下的安装并不难，pacman或AUR里都有相应源，它的难点并不在此，而在安装完成后的具体配置。

Haskell是一个纯函数语言（purely functional language）。它不易学，又相对较为冷门，很多时候碰到问题只能去官方的Wiki上去自己翻。因此在我配置XMonad时，即便是参考了各种教学视频并对着wiki来操作，也仍然有许多难以解决的问题。因此在边抄网上其他人的配置边改一周后，总算是将它配置到基本功能可以正常使用了。但由于难度过大，以及第二个来自于XMobar的问题，使得我最终还是放弃了XMonad和XMobar回到了dwm上。

### Status Bar | 状态栏
#### polybar

polybar是一个单纯的状态栏，它通常更多与bspwm和i3wm一起组合使用。原先我的dwm只是使用的自带的状态栏，但在systray patch和alpha patch冲突无法解决的情况下，想起有一部分人是使用polybar代替i3bar在使用的，因此开始尝试在dwm下配合polybar来使用。

最终找到了一个名为polybar-dwm-module的polybar的fork并加了dwm支持功能的版本。

polybar-dwm-module 可以从github上通过git clone和 sudo make install来编译安装，Arch Linux下也可以直接用AUR源里的软件包polybar-dwm-module。

唯一的难点便是它所需的之前提到的dwm的IPC patch，这个问题解决后，其他内容按照README.md里的说明来安装配置就基本没有大问题了。

#### Slstatus
Slstatus同样是Suckless出品，与dwm和st一样，都是直接修改源文件后编译安装，它实现后的效果和在dwm里用autostart来运行各个脚本输出的所需监控内容基本没有太大差别，二选一即可。

然而问题也同样的，它不支持显示系统托盘。

#### XMobar

XMobar在Arch Linux下可以用AUR源来编译安装，而问题也来源于此。

由于它是以Haskell语言编写的，一旦pacman -Syu的时候更新了pacman源里的Haskell的内容，再次启动XMobar时便会发现它无法正常工作了，只能删除并重新编译安装后才能再次正常使用。

而Arch Linux又是一个更新相对较为激进的发行版，pacman的源里的Haskell几乎是每天都会有更新。对于经常滚更新的人来说几乎等于每次更新系统后都需要手动删除再安装一遍XMobar。这个操作过于繁琐，加上本身Haskell对我来说难度过高，即便编写修改配置文件都是很消耗精力的事情，因此便只好作罢。

### Applications | 软件工具
#### Vim

vim本身不需多说什么，讲多了又会变成无谓的争吵。各个编辑器，以及编辑器和IDE，都各有千秋各有适合的应用场所，喜欢什么用什么，习惯什么用什么便是。

这次再战Linux，我试着好好配置了一下vim，包括一些常用的插件和alias等。

诚然vim学习曲线是稍微有些陡峭，但只是修改编辑一下文件啥的其实也真没那么难。

esc和:wq还能走天下呢不是？（大雾

#### Ranger

ranger是个支持vim快捷键的终端下的文件管理器，本身自带的功能虽然有限，但配合例如软件包和工具可以在终端下实现很多功能，如图片pdf预览等。配置也不难。文字和视频的教程也不少，参考着来一般问题不大。

## 后记
这篇blog最开始其实也只是为了给自己做个笔记，以免过了一段时间后又忘记了。虽然写到后面越来越短，但是整篇看下来也还是出乎意料地长。

很多内容我都没有做过多过于详细的描述，比如具体安装过程那些。因为那些内容随手一搜已经很多内容，重复性叙述的意义并不大，便只写了写自己遇到的问题和比较主观的看法。

另外再说起来，其实Win和Linux也并没有个孰优孰劣，一个商业一个开源，其内在哲学和构建的根本逻辑也不同，其实不好怎么作比较。

就还是那句话，有什么需求就用什么，喜欢哪个用哪个。

原大可不必争论个高下。
