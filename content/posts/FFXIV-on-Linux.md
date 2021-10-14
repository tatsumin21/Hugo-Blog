---
title: "通过KCPTUN 和 Shadowsocks 给 Linux 下的网游加速"
date: 2021-10-04T19:52:52+02:00
draft: false
showDate: true
toc: false
images:
tags:
  - Gaming
  - InfoTech
  - Linux
---
## 前言

来来回回在Win和ArchLinux之间换了好几次，最终还是下定决心回到Linux怀抱，毕竟我目前的日常需求除了游戏几乎也没有什么一定要用到Win的地方，而游戏目前又几乎只在玩FF14。

Linux下跑FF14本不是个难事，很早之前Steam的Proton和Lutris就都能做到这一点了。但我的需求稍微有些特殊：

> 我人在欧洲，但我的FF14角色在日服。而Linux下几乎没有像Win下那样的游戏专用加速器。

于是在换回ArchLinux并配置好基本的桌面及软件后，便开始想办法寻找Linux下给网游加速降低延迟的方法。经过搜索后决定试试`Kcptun` 和 `Shadowsocks`。

`kcptun` 加速流量的原理根据Github上的介绍，如下图所示：

![KCPTUN](https://raw.githubusercontent.com/xtaci/kcptun/master/kcptun.png)

需要注意的是，FFXIV的流量是全部走TCP格式传输的，而KCPTUN正好是加速的TCP流量，因此这个方法具有可行性。

而如果是其他游戏如PUBG(吃鸡)，只有游戏大厅内的传输流量是TCP,进入游戏开始对战后为UDP流量。而KCPTUN是不对UDP流量进行处理的，因此将不会有加速效果。

所以请在准备使用此方法之前确认所需加速的流量的传输格式，以免做无用功。

## 准备

- 一台带宽尚可的Linux系统的VPS（理论上发行版不限，Ubuntu，Cent OS或其他熟悉的`x64`发行版都可）

## 安装及配置服务端

### KCPTUN Server Client

远程登入VPS的shell后，在根目录下新建一个文件夹用来放`kcptun` 服务端，这里文件夹名就以`kcptun` 为例。

```bash 
mkdir kcptun
cd kcptun
```
然后下载[Github Release](https://github.com/xtaci/kcptun/releases) 页面上最新的`.tar.gz` 压缩包，解压缩并新建一个服务端的配置文件。
这边配置文件以 `config.json` 命名。

```bash
wget https://github.com/xtaci/kcptun/releases/download/v20210922/kcptun-linux-amd64-20210922.tar.gz
tar -zxvf kcptun-linux-amd64-20210922.tar.gz
vim $HOME/kcptun/config.json
```

这边压缩包的名称里的`amd64`便是x64 Linux系统用，如果是其他系统作为服务端，请根据系统选择对应的包。
后面的日期会根据作者维护更新有所不同，这里及后文都请注意更改，请勿照搬。

然后在配置文件里写入如下内容：

```json
{
"listen": ":6666",
"target": "127.0.0.1:2345",
"key": "1234",
"crypt": "salsa20",
"mode": "fast3",
"mtu": 1400,
"sndwnd": 2048,
"rcvwnd": 2048,
"datashard": 70,
"parityshard": 30,
"dscp": 46,
"nocomp": false,
"acknodelay": false,
"nodelay": 0,
"interval": 40,
"resend": 0,
"nc": 0,
"sockbuf": 4194304,
"keepalive": 10
}
```
- listen是Kcptun监听的端口，也就是Kcptun的服务端口，这里可以自行修改。
- target是你准备加速的端口，因为我们是要给ss加速，所以这里一定要填写你的ss端口号，这里以2345为例。
- key是通讯密钥，请修改为你自己的。
- crypt是加密方式，默认是salsa20，同时还支持aes, aes-128, aes-192, salsa20, blowfish, twofish, cast5, 3des, tea, xtea, xor, none。可以自行修改。
- mode是传输模式，这里建议使用fast3。
- sndwnd以及rcvwnd是你的本地带宽速度。我的本地宽带是Telekom 200M，这里就填写2048。也请根据自身情况更改。
- 其他的一些参数可以保持原样，当然也可根据需求更改。

然后查看一下文件夹里目前所有的文件，应当包含 `client_linux_amd64` 和 `server_linux_amd64` 这两个。

其中 `client_linux_amd64` 是客户端用，`server_linux_amd64` 是服务端用。

运行一下命令尝试启动服务：

```bash
./server_linux_amd64 -c $HOME/kcptun/config.json
```
如果看到反馈刚刚在配置文件里所填写的参数且无报错的话，即说明服务端可正常运行。`Ctrl`+`c`中断运行。

因为不能每次要开加速时临时进VPS开，那太麻烦了。所以可以给kcptun写个自动执行脚本并设定成systemd service使其在后台开机自动运行。
同样以kcptun命名：

```bash
vim kcptun.sh

#!/bin/bash
cd /root/lala/
./server_linux_amd64 -c $HOME/kcptun/config.json 2>&1 &
echo "KcpTun Started!"
```

并赋予其可执行权限

```bash
chmod +x $HOME/kcptun/kcptun.sh
```
添加systemd启动服务,注意将 `/path/to/yout/kcptun.sh` 替换成之前建的脚本的实际路径。

```bash
# /etc/systemd/system/kcptun.service
[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/path/to/your/kcptun.sh

[Install]
WantedBy=multi-user.target
```

然后 

```bash
sudo systemctl enable kcptun.service
sudo systemctl start kcptun.service
```
启用即可。

### Shadowsocks

Shadowsocks的搭建就简单很多，很多Linux发行版的包管理器里都有现成打包好的。

按照Github [Shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev) 的Repo的说明，Ubuntu的话

```
sudo apt install shadowsocks-libev
```

安装。

Shadowsocks的配置文件同样用JSON格式，具体如下：

```json
{
    "server":"your_server_ip",
    "server_port":2345,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"chacha20-ietf-poly1305",
    "fast_open": true,
    "workers": 1,
    "prefer_ipv6": false
}
```
这边Shadowsocks的侦听端口就是前面kcptun配置文件里的目标端口，如例子里便是2345。请根据前面的配置对应填写。

修改完成后保存退出。服务器上的部署部分就完成了。

## 本地客户端

### KCPTUN

方法和服务端的部署基本一样，只是配置文件和执行上稍有不同。

```json
{
"listen": ":7426",
"target": "your.vps.public.ip:6666",
"key": "1234",
"crypt": "salsa20",
"mode": "fast2",
"mtu": 1400,
"sndwnd": 2048,
"rcvwnd": 2048,
"datashard": 70,
"parityshard": 30,
"dscp": 46,
"nocomp": false,
"acknodelay": false,
"nodelay": 0,
"interval": 40,
"resend": 0,
"nc": 0,
"sockbuf": 4194304,
"keepalive": 10
}
```
大致和服务端的配置文件参数一样，需要变动的只有监听端口和目标端口。

监听端口可任意填写，只要不和本地shadowsocks客户端所使用的端口冲突就行。
目标IP和端口填写为你的vps的公网ip和服务端所填写的监听端口，如上即是6666。本地带宽，加密方式等其他参数与服务端保持一致。

配置完成后按服务端同样的方法给客户端在本地也添加到systemd服务中。

脚本内容大致一致，只需相应地把执行文件由 `server_linux_amd64` 变为 `client_linux_amd64`即可。

### Shadowsocks

Shadowsocks客户端实现就更多了，随便一个用的顺手的就行。

配置参数如下：

```
服务器IP：127.0.0.0
服务器端口：7426
```

其他如密码和加密方式与服务端保持一致即可。

如果不出意外，连上shadowsocks后，流量便是通过kcptun加速过的了。

## 实际效果

进游戏里试的时候太激动忘了截图，这里就简单写一下数据对比吧。

由于FF14特殊的服务器计算的底层原理，游戏本身就自带一定“延迟”，所以我测的是实际操作起来的变化。

方法：先让角色自动奔跑，在跑动中选择任意一个传送点传送，然后看传送读条被自动打断时所剩余的时间。

实际断读条时的剩余时间数据如下：

- 本地直连： 4.1s～3.9s
- KCPTUN+SS：几乎稳定在4.45s
- Win下加速器：4.5s左右

可以说至少比直连效果还是很明显的，甚至不比win下需要每个月订阅的加速器效果差多少。如果手上刚好有闲置vps，又有游戏加速需求的话，还是值得一试的。

## 参考
- [荒岛：配置使用Kcptun来暴力加速shadowsocks代理](https://lala.im/1569.html)
- [Github: xtaci/kcptun](https://github.com/xtaci/kcptun)
- [ArchLinux Wiki: Shadowsocks](https://wiki.archlinux.org/title/Shadowsocks_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
