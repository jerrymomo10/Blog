title: VPS服务器和Shadowsocks
date: 2015-11-12 11:40:51
tags: [服务器，SS]
categories: [服务器]
---
大学的时候用过Goagent代理，并不理解具体的原理，以后陆续用过Chrome的插件红杏和时光隧道，随着红杏的关闭，SS在GitHub上的源码删除，自己尝试购买了LA的VPS。
 - **VPS选择**
     VPS选择比较多 就是一个小型的服务器，和国内的阿里云是一样，主要是看中国外的VPS的外网IP和带宽资源，所以存储，处理器性能要求很低，当然如果是做其他的用途配置就不同了。我用的是Ramnode 128M ram,单核，网络500GB，包年15刀的，优惠以后13.5刀，个人觉得比较便宜，毕竟当时用红杏的时候包年是100+，时光隧道包月20,。
最初选择的VPS是著名的搬瓦工，但是没有满意的价格。据说DigitalOcean也不错。
RamNode 付费需要用到Paypal。开通以后会Email你服务器的登录方式，RamNode基本都是通过Email与你沟通，后续会跟你要具体的地址什么的。我选择的OS是ubuntu14.04_32，用SSH通信，修改密码，以前在阿里云上搭建过，所以使用没有什么问题。
 - **SS安装配置**
 先跟新软件库
sudo apt-get update

SS有很多语言的实现Python,Go,C 等等。
这里为了方便用的Python的。
apt-get install python-pip
pip install shadowsocks

使用配置文件进行配置，方法创建etc/shadowsocks.json文件，填入如下内容：

{
    "server":"my_server_ip",
    "server_port":8000,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"rc4-md5"
}
然后使用配置文件在后台运行：

ssserver -c /etc/shadowsocks.json -d start
如果要停止运行，将命令中的start改成stop。
加入开机自动启动，rc.loacl。
服务器上有很多加速优化的方法，没有研究，具体在
http://wuchong.me/blog/2015/02/02/shadowsocks-install-and-optimize/
感谢博主分享，服务器端基本就是这样了。

在客户端的安装
http://www.jianshu.com/p/08ba65d1f91a
不过在Linux下用的QT5没成功，可能是我的配置有问题，只能也用Python版本的，目前还不知道能不能配置PAC，IPv6代理没有用过。
原来Linux下还需要浏览器安装代理插件设置，Chrome下选择SwitchSharp
