---
layout: post
title: vr.org VPS 香港节点简单测试
categories: [VPS]
tags: [VR, VPS, 香港]
---
--------------------------------------------------------

我的推荐[购买链接](http://www.vr.org/?a=2306), 不甚感激。

身在伟大的祖国大陆，不管搭梯子还是放网站（不想备案党），都需要在境外买个vps。以前一直在捣鼓美帝西海岸的，但是地理上隔着太平洋注定了延迟差不多要200ms。幸运的是我们还有香港啊。

VR.ORG是一家比较有名的VPS提供商，在亚洲的香港和印度都有节点。经过在[V2ex](http://www.v2ex.com/t/65205)上好基友们的介绍，入手了[VR香港的512M类型的VPS](http://www.vr.org/?a=2306)，下面做一个简单评测，给想入手的朋友一个参考。


## 基本配置

> 512 MB 内存 ## 20 GB Raid存储

> 400 GB 出口流量 ## 无限的入口流量

> 1 IPv4 ## 1 IPv6 IP 地址

> Root权限，超过370种安装系统镜像

> XEN架构，保证不超售，全球15个节点

> 不满意三天退款


vr.org官方默认页面是英文，不过，他们有中文界面，专为中国客户设计的，非常棒。机房在NTT位于香港葵涌的HKNet数据中心，国内访问速度是无话可说的。另外，vr.org也是一个非常靠谱的主机商，我是用了四个多月，非常稳定，客服回复也很迅速。

---------------------------------------------------------------------

## 数据中心

VR在全球有15个数据节点可供选择，包括Amsterdam, NL, Chennai (Madras), India, Chicago, IL, Dallas, TX, Denver, CO, Hong Kong, HK, London, United Kingdom, Los Angeles, CA, Miami, FL, New York, NY, Paris, France, Reston, VA, San Jose, CA, and Seattle, WA.

我使用的是[VR香港](http://www.vr.org/datacenters/hong-kong-vps)

----------------------------------------------------------------------
## 后台操作界面


VR的界面是VPS提供商里面做的比较好看大气的，后台功能也较完善。

#### 重启等命令控制台

![](http://ww3.sinaimg.cn/large/a74ecc4cjw1e3mudb2vapj.jpg)

#### 资源使用量的Dashboard(有网络，使用量等多种展示)
![](http://ww4.sinaimg.cn/large/a74e55b4jw1e3mug93mdyj.jpg)

#### Web界面的终端
![](http://ww1.sinaimg.cn/large/a74eed94jw1e3mug5fquhj.jpg)


#### 如果不慎把VPS搞挂了，还有急救模式
![](http://ww4.sinaimg.cn/large/bfadf3bejw1e3muhekq5qj.jpg)

---------------------------------------------------------------------
## CPU 信息

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

---------------------------------------------------------------------
## 网络入口速度(应该是百兆网口)
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

------------------------------------------------------------------
## 磁盘IO（可以看到做了raid，还是很强的）
{% highlight bash linenos %}
$ dd if=/dev/zero of=test bs=64k count=4k oflag=dsync
4096##0 records in
4096##0 records out
268435456 bytes (268 MB) copied, 1.61833 s, 166 MB/s

$ dd if=/dev/zero of=test bs=64k count=16k conv=fdatasync
16384##0 records in
16384##0 records out
1073741824 bytes (1.1 GB) copied, 18.186 s, 59.0 MB/s
{% endhighlight %}

-------------------------------------------------------------------
## UnixBenckmark得分（单核300分，双核500分，足够了）
{% highlight bash linenos %}
  ##    ##  ##    ##  ##  ##    ##          ##########   ############  ##    ##   ########   ##    ##
   ##    ##  ####   ##  ##   ##  ##           ##    ##  ##       ####   ##  ##    ##  ##    ##
   ##    ##  ## ##  ##  ##    ####            ##########   ##########   ## ##  ##  ##       ############
   ##    ##  ##  ## ##  ##    ####            ##    ##  ##       ##  ## ##  ##       ##    ##
   ##    ##  ##   ####  ##   ##  ##           ##    ##  ##       ##   ####  ##    ##  ##    ##
    ########   ##    ##  ##  ##    ##          ##########   ############  ##    ##   ########   ##    ##

   Version 5.1.3                      Based on the Byte Magazine Unix Benchmark

   Multi-CPU version                  Version 5 revisions by Ian Smith,
                                      Sunnyvale, CA, USA
   January 13, 2011                   johantheghost at yahoo period com


1 x Dhrystone 2 using register variables  1 2 3 4 5 6 7 8 9 10

1 x Double-Precision Whetstone  1 2 3 4 5 6 7 8 9 10

1 x Execl Throughput  1 2 3

1 x File Copy 1024 bufsize 2000 maxblocks  1 2 3

1 x File Copy 256 bufsize 500 maxblocks  1 2 3

1 x File Copy 4096 bufsize 8000 maxblocks  1 2 3

1 x Pipe Throughput  1 2 3 4 5 6 7 8 9 10

1 x Pipe-based Context Switching  1 2 3 4 5 6 7 8 9 10

1 x Process Creation  1 2 3

1 x System Call Overhead  1 2 3 4 5 6 7 8 9 10

1 x Shell Scripts (1 concurrent)  1 2 3

1 x Shell Scripts (8 concurrent)  1 2 3

2 x Dhrystone 2 using register variables  1 2 3 4 5 6 7 8 9 10

2 x Double-Precision Whetstone  1 2 3 4 5 6 7 8 9 10

2 x Execl Throughput  1 2 3

2 x File Copy 1024 bufsize 2000 maxblocks  1 2 3

2 x File Copy 256 bufsize 500 maxblocks  1 2 3

2 x File Copy 4096 bufsize 8000 maxblocks  1 2 3

2 x Pipe Throughput  1 2 3 4 5 6 7 8 9 10

2 x Pipe-based Context Switching  1 2 3 4 5 6 7 8 9 10

2 x Process Creation  1 2 3

2 x System Call Overhead  1 2 3 4 5 6 7 8 9 10

2 x Shell Scripts (1 concurrent)  1 2 3

2 x Shell Scripts (8 concurrent)  1 2 3

========================================================================
   BYTE UNIX Benchmarks (Version 5.1.3)

   System: vr.pkudlib.org: GNU/Linux
   OS: GNU/Linux -- 3.4.0-cloud -- ##1 SMP Thu May 24 05:12:36 EDT 2012
   Machine: i686 (i386)
   Language: en_US.utf8 (charmap="UTF-8", collate="UTF-8")
   CPU 0: Intel(R) Xeon(R) CPU E5620 @ 2.40GHz (4800.2 bogomips)
          Hyper-Threading, MMX, Physical Address Ext, SYSENTER/SYSEXIT
   CPU 1: Intel(R) Xeon(R) CPU E5620 @ 2.40GHz (4800.2 bogomips)
          Hyper-Threading, MMX, Physical Address Ext, SYSENTER/SYSEXIT
   13:47:03 up  2:46,  1 user,  load average: 0.56, 0.46, 0.42; runlevel 2

------------------------------------------------------------------------
Benchmark Run: Fri Apr 12 2013 13:47:03 - 14:15:18
2 CPUs in system; running 1 parallel copy of tests

Dhrystone 2 using register variables       14383899.9 lps   (10.0 s, 7 samples)
Double-Precision Whetstone                     2592.4 MWIPS (10.0 s, 7 samples)
Execl Throughput                               1142.4 lps   (30.0 s, 2 samples)
File Copy 1024 bufsize 2000 maxblocks         96731.6 KBps  (30.0 s, 2 samples)
File Copy 256 bufsize 500 maxblocks           25455.0 KBps  (30.0 s, 2 samples)
File Copy 4096 bufsize 8000 maxblocks        350681.0 KBps  (30.0 s, 2 samples)
Pipe Throughput                              110935.2 lps   (10.0 s, 7 samples)
Pipe-based Context Switching                  22938.9 lps   (10.0 s, 7 samples)
Process Creation                               2357.2 lps   (30.0 s, 2 samples)
Shell Scripts (1 concurrent)                   3555.9 lpm   (60.0 s, 2 samples)
Shell Scripts (8 concurrent)                    629.6 lpm   (60.0 s, 2 samples)
System Call Overhead                         616613.9 lps   (10.0 s, 7 samples)

System Benchmarks Index Values               BASELINE       RESULT    INDEX
Dhrystone 2 using register variables         116700.0   14383899.9   1232.6
Double-Precision Whetstone                       55.0       2592.4    471.3
Execl Throughput                                 43.0       1142.4    265.7
File Copy 1024 bufsize 2000 maxblocks          3960.0      96731.6    244.3
File Copy 256 bufsize 500 maxblocks            1655.0      25455.0    153.8
File Copy 4096 bufsize 8000 maxblocks          5800.0     350681.0    604.6
Pipe Throughput                               12440.0     110935.2     89.2
Pipe-based Context Switching                   4000.0      22938.9     57.3
Process Creation                                126.0       2357.2    187.1
Shell Scripts (1 concurrent)                     42.4       3555.9    838.6
Shell Scripts (8 concurrent)                      6.0        629.6   1049.3
System Call Overhead                          15000.0     616613.9    411.1
                                                                   ========
System Benchmarks Index Score                                         321.4

------------------------------------------------------------------------
Benchmark Run: Fri Apr 12 2013 14:15:18 - 14:43:35
2 CPUs in system; running 2 parallel copies of tests

Dhrystone 2 using register variables       27445618.4 lps   (10.0 s, 7 samples)
Double-Precision Whetstone                     5085.1 MWIPS (10.1 s, 7 samples)
Execl Throughput                               2192.5 lps   (30.0 s, 2 samples)
File Copy 1024 bufsize 2000 maxblocks        142890.9 KBps  (30.0 s, 2 samples)
File Copy 256 bufsize 500 maxblocks           36172.8 KBps  (30.0 s, 2 samples)
File Copy 4096 bufsize 8000 maxblocks        510183.5 KBps  (30.0 s, 2 samples)
Pipe Throughput                              181152.6 lps   (10.0 s, 7 samples)
Pipe-based Context Switching                  60831.3 lps   (10.0 s, 7 samples)
Process Creation                               4141.7 lps   (30.0 s, 2 samples)
Shell Scripts (1 concurrent)                   4847.5 lpm   (60.0 s, 2 samples)
Shell Scripts (8 concurrent)                    650.0 lpm   (60.1 s, 2 samples)
System Call Overhead                        1098147.9 lps   (10.0 s, 7 samples)

System Benchmarks Index Values               BASELINE       RESULT    INDEX
Dhrystone 2 using register variables         116700.0   27445618.4   2351.8
Double-Precision Whetstone                       55.0       5085.1    924.6
Execl Throughput                                 43.0       2192.5    509.9
File Copy 1024 bufsize 2000 maxblocks          3960.0     142890.9    360.8
File Copy 256 bufsize 500 maxblocks            1655.0      36172.8    218.6
File Copy 4096 bufsize 8000 maxblocks          5800.0     510183.5    879.6
Pipe Throughput                               12440.0     181152.6    145.6
Pipe-based Context Switching                   4000.0      60831.3    152.1
Process Creation                                126.0       4141.7    328.7
Shell Scripts (1 concurrent)                     42.4       4847.5   1143.3
Shell Scripts (8 concurrent)                      6.0        650.0   1083.4
System Call Overhead                          15000.0    1098147.9    732.1
                                                                   ========
System Benchmarks Index Score                                         531.5
{% endhighlight %}

----------------------------------------------------------------------------
## 从大陆各地的访问速度(电信联通移动的线路都很好，基本上都在60ms以内)

![](http://ww2.sinaimg.cn/large/a74ecc4cjw1e3musf4mn3j.jpg)
![](http://ww3.sinaimg.cn/large/a74eed94jw1e3musk533gj.jpg)

---------------------------------------------------------------------------------
## 最近的好消息
最近VR将旗下的VPS内存免费加倍了，即你可以用以前买512M内存的前买到1G内存VPS的了。如果你有购买意向，可以使用我的[推荐链接](
http://www.vr.org/?a=2306), 不甚感激。

