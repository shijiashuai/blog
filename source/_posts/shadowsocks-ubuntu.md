title: 在Ubuntu上搭建Shadowsocks服务器以及优化
categories: 教程
date: 2015-12-16 23:02:59
tags: [科学上网,Shadowsocks]
---
从前科学上网一直使用的是[vpnso](http://vpnso.com),打开网页速度尚可，但是感觉带宽只有1Mbps左右，看YouTube视频和下载就有些吃力了。
毕竟对于科学上网来说，VPN还是有些大材小用。

 Shadowsocks这种轻量级的代理更适合科学上网。要搭建Shadowsocks服务器，首先你要有一台国外的VPS，这里我选用了DigitalOcean的Sinapore节点，在优化以后可以达到8Mbps~10Mbps的带宽，还是很可观的。
<!--more-->
## 1. 安装Shadowsocks
安装Shadowsocks很简单，首先安装`pip`

 	apt-get install python-pip

然后使用`pip`安装Shadowsocks

	pip install shadowsocks

安装完成

## 2. Shadowsocks的配置和使用
推荐使用配置文件的方式启动，方便以后的查看和修改`vim /etc/shadowsocks.json`

配置文件这样写
```python
{
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```
>完整的配置文件写法见[这里](https://github.com/shadowsocks/shadowsocks/wiki/Configuration-via-Config-File)

启动和停止Shadowsocks

	shadowsocks -c /etc/shadowsocks.json -d start
	shadowsocks -d stop

`-d`参数的作用是把shadowsocks作为后台daemon程序运行

>shadowsocks目前支持Windows/OSX/iOS/Android，客户端在[这里](https://github.com/shadowsocks/shadowsocks/wiki/Ports-and-Clients)

## 3. Shadowsocks的优化
推荐使用**[锐速](http://my.serverspeeder.com/w.do?m=lsl)**
>锐速（ServerSpeeder）加速软件是一种基于ZETATCP加速引擎的软件，可以起到显著加速效果的TCP加速技术.

首先确认你的Linux内核在锐速的[支持列表](http://my.serverspeeder.com/ls.do?m=availables)里，然后去注册一个锐速帐号

安装

	wget http://my.serverspeeder.com/d/ls/serverSpeederInstaller.tar.gz
	tar -xzf serverSpeederInstaller.tar.gz
	bash serverSpeederInstaller.sh

输入用户名和密码以后一路enter确认用默认参数，最后会问你是否要开机启动和现在启动，推荐打开。

输入`lsmod`看到有`appex0`模块就是加速成功了。

我们继续修改一下锐速的参数`vim /serverspeeder/etc/config`

其中`advinacc`,`maxmode`都改为1，DigitalOcean的网卡是支持`rsc`和`gso`加速的，我们也把它打开

最后重新加载一下配置

	service serverSpeeder reload

打开YouTube测一下速，1080p视频几乎不需要缓冲
![youtube](/img/youtube1080p.png)




