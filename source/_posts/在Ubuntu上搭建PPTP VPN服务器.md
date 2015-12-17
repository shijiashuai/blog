title: 在Ubuntu上搭建PPTP VPN服务器
id: 58
categories:
  - 教程
date: 2014-12-27 23:08:15
tags: 
 - Linux
 - VPN
 - 科学上网
---

如果你有一台在境外的VPS，按照本教程设置好PPTP服务器就可以通过它访问任何你想去的网站啦。

<!--more-->

### 1.安装pptpd
输入以下命令即可
<pre>sudo apt-get install pptpd</pre>
### 2.设置pptp
打开`pptpd-options`
<pre>sudo vim /etc/ppp/pptpd-options</pre>

找到以下三行
```plain
refuse-pap
refuse-chap
refuse-mschap
```
加“#”号注释掉，像这样
```plain
#refuse-pap
#refuse-chap
#refuse-mschap
```
在文件最后添加下面的两行，用来配置DNS服务器，这里我选用了Google的DNS，也可以改成自己喜欢的
```plain
ms-dns 8.8.8.8
ms-dns 8.8.4.4
```
保存退出

### 3.设置网络地址
打开`pptpd.conf`
<pre>sudo vim /etc/pptpd.conf</pre>
在最后加上以下内容
```plain
localip YOUR_IP_ADDRESS
remoteip 192.168.0.100-254
```
把`YOUR_IP_ADDRESS`替换成你的服务器的ip地址，比如我的是`104.236.156.229`

`remoteip`是你要分配出去的ip地址范围，按自己喜好填就好

添加完成以后保存退出

### 4.添加自己的vpn账户
打开`chap-secrets`
<pre>sudo vim /etc/ppp/chap-secrets</pre>
按以下格式添加账户，一行一个，注意空格
<pre>[Username] [Service] [Password] [Allowed IP Address]</pre>
比如
<pre>ss pptpd sspwd *</pre>
保存退出，然后重启pptpd
<pre>sudo /etc/init.d/pptpd restart</pre>
### 5.设置ipv4包转发
打开`sysctl.conf`
<pre>sudo vim /etc/sysctl.conf</pre>
找到这一行，把注释的“#”号去掉
<pre>#net.ipv4.ip_forward=1</pre>
改成像这样
<pre>net.ipv4.ip_forward=1</pre>
保存退出，然后重新加载这个系统服务
<pre>sudo sysctl -p</pre>
### 6.设置NAT规则
<pre>sudo iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -o eth0 -j MASQUERADE</pre>
注意：如果你在第三部设置了跟我不一样的remoteip，那么这里也要作相应的更改。

**至此，PPTP VPN服务器就已经架好了。**