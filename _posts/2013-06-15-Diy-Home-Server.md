---
layout: post
title: Diy Home Server
categories: [玩]
tags: [diy, home server]
---
---------------------------------------

一直想有一个自己的server，加上苦逼学生党囊中羞涩，所以一直想着自己diy一台server放在家里，只装linux，满足自己喜欢源码编译和各种折腾的爱好。

## CPU：[Intel Xeon E3 1230-v2](http://ark.intel.com/products/65732)
![]()

## 散热器：[采融B45](http://bbs.pceva.com.cn/thread-81790-1-1.html)
![]()

## 主板：[华擎B75 pro3](http://www.pcpop.com/doc/0/801/801822_all.shtml)
![]()

## 内存：[镁光](http://item.51buy.com/item-453739.html)
![]()

## 机箱：[恩杰H2](http://we.pcinlife.com/thread-1661360-1-1.html)
![]()

## 电源：[全汉蓝暴节能版275w](http://item.jd.com/375587.html)
![]()


## iptables背景知识简要介绍

* iptables有table, chain, rules 三个概念。
* iptables的基本语法规则
    iptables  <option> <chain> <matching criteria> <target>


## iptables的NAT表

* NAT在iptables中的位置图示
![](http://ww4.sinaimg.cn/large/a74ecc4cjw1e3ypso4b3tj20dx0760t9.jpg)

* 包括
  * SNAT - Source NAT，扮演网关角色的机器和其他机器共享一个网络连接时使用，
  * DNAT - Destination NAT，将网络内部服务细节掩藏起来
  * MASQUERADE - SNAT的一种特殊形式，适用于像adsl这种临时会变的ip上
  * REDIRECT - DNAT的一种特殊形式，将网络包转发到本地host上（不管IP头部指定的目标地址是啥）

* 首先需要启用NAT需要在Linux上打开内核对IP包的转发支持，linux上编辑 /etc/sysctl.conf 文件

    net.ipv4.ip_forward = 1
然后
    sysctl -p /etc/sysctl.conf
