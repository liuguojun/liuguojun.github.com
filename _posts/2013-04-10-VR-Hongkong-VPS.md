---
layout: post
title: vr.org VPS 香港节点简单测试
categories: [VPS]
tags: [VR, VPS, Hongkong]
---
---------------------------------------

身在伟大的祖国大陆，不管搭梯子还是放网站（不想备案党），都需要在境外买个vps。以前一直在捣鼓美帝西海岸的，但是地理上隔着太平洋注定了延迟差不多要200+ms。幸运的是我们还有香港！

VR.ORG是一家比较有名的VPS提供商，在亚洲的香港和印度都有节点。经过在[V2ex](http://www.v2ex.com/t/65205)上好基友们的介绍，入手了[VR香港的512M类型的VPS](http://www.vr.org/?a=2306)，下面做一个简单评测，给想入手的朋友一个参考。

+ 基本配置

> 512 MB 内存 + 20 GB Raid存储

> 400 GB 出口流量 + 无限的入口流量

> 1 IPv4 + 1 IPv6 IP 地址

> Root权限，超过370种安装系统镜像

> XEN架构，保证不超售，全球15个节点

> 不满意三天退款


vr.org官方默认页面是英文，不过，他们有中文界面，专为中国客户设计的，非常棒。机房在NTT位于香港葵涌的HKNet数据中心，国内访问速度是无话可说的。另外，vr.org也是一个非常靠谱的主机商，这点从赵容部落的几位朋友亲身体验得知，非常不错。

+ CPU 信息

{% highlight console linenos %}
processor   : 0
vendor_id   : GenuineIntel
cpu family  : 6
model   : 44
model name  : Intel(R) Xeon(R) CPU   E5620  @ 2.40GHz
stepping: 2
microcode   : 0x15
cpu MHz : 2400.084
cache size  : 12288 KB
physical id : 0
siblings: 2
core id : 0
cpu cores   : 1
apicid  : 0
initial apicid  : 18
fdiv_bug: no
hlt_bug : no
f00f_bug: no
coma_bug: no
fpu : yes
fpu_exception   : yes
cpuid level : 11
wp  : yes
flags   : fpu de tsc msr pae cx8 sep cmov pat clflush mmx fxsr sse sse2 ss ht nx constant_tsc nonstop_tsc pni ssse3 pcid sse4_1 sse4_2 popcnt hypervisor
bogomips: 4800.16
clflush size: 64
cache_alignment : 64
address sizes   : 40 bits physical, 48 bits virtual
{% endhighlight %}

+ 网络入口速度
{% highlight bash linenos %}
$ wget http://cachefly.cachefly.net/100mb.test
--2013-04-11 13:46:24--  http://cachefly.cachefly.net/100mb.test
Resolving cachefly.cachefly.net (cachefly.cachefly.net)... 204.93.150.151
Connecting to cachefly.cachefly.net (cachefly.cachefly.net)|204.93.150.151|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 104857600 (100M) [application/octet-stream]
Saving to: `100mb.test'

100%[=====================================>] 104,857,600 8.15M/s   in 11s     

2013-04-11 13:46:36 (8.82 MB/s) - `100mb.test' saved [104857600/104857600]
{% endhighlight %}

