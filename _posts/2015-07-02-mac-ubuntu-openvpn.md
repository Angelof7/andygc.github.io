---
layout: post
title: Mac & Ubuntu 配置openvpn客户端
categories: [server]
tags: [vpn]
---

## Mac
---------------
**tunnelblick**


## Ubuntu
---------------
1. >\# sudo apt-get install openvpn

2. >\# cd /etc/openvpn

3. >\# cp ~/Downloads/client.ovpn .

4. >\# sudo openvpn /etc/openvpn/client.ovpn

5. 后台连接:  
>\# sudo openvpn /etc/openvpn/client.ovpn > /dev/null &

6. 开机启动:  
将上述命令加入:
> /etc/rc.local
