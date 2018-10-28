---
layout:     post
title:      在centos7上部署python3并运行自己做的小脚本
subtitle:   找答案也是学问
date:       2018-10-27
author:     inertia1p
header-img: img/post-bg-github-cup.jpg
keywords_post:  "centos 网路配置"
catalog: true
tags:
    - python
    - centos
---

## Summary

因为在寝室用的台式机，有时候忘记充电费就会突然暴毙，所以想做一个自动访问电费网站然后发邮件给自己的小脚本，脚本写起来简单，但是因为是第一次玩服务器，遇上不少坑卡了我很久。有些都是些很简单的坑。见笑~

## 坑1 使用yum失败

首先，默认的yum是基于centos7（我用的centos7，这里只代表这个版本）中默认安装的python2。所以如果想下载python3那么要记得把yum的依赖改成pyhton2。
其次，我在使用yum的时候遇到了无法识别地址（baseurl），这是因为DNS没有配置  
使用命令  
```
nmcli  connection show
nmcli con mod 接口名 ipv4.dns     xxxxxxx   更改dns
nmcli con up 接口名  配置生效
```

## 坑2 需要前提配置多种命令

1. wget yum -y install wget
2. 依赖环境 # yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
3.  ping error：Name or service not known  这个其实和坑1是一样的，是因为DNS没有配置，我这里又出现了这个问题是因为我测试是使用的VM虚拟机，在配置网卡时用了主机模式，需要换成NAT模式
4. gcc yup -y install gcc


>''
