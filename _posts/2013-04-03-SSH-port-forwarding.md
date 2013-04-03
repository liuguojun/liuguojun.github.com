---
layout: post
title: SSH端口转发以及应用实例 
categories: [Trick]
tags: [SSH, 端口转发]
---
---------------------------------------

SSH是我们常用的远程连接管理*nix服务器的居家旅行必备利器。Linux上常用的Openssh则由[OpenBSD](http://www.openssh.org/openbsd.html)（好奇怪，为啥不是Apache基金会或者FSF呢？）维护。

我今天着重介绍SSH的端口转发的功能。Google一搜相关主题，文章已经很多了。但是我觉得他们都没怎么讲清楚转发时哪一段连接是加密的。

一般转发过程会涉及到3台机器：客户端client， 待访问机器host， 中转服务器server。 当然这3个角色中的某两个可能会处在同一台机器上，比如host和server是同一台机器。

---------------------------------------
* Local Forwarding 本地转发
![](http://ww4.sinaimg.cn/large/a74ecc4cjw1e3cdfhhrkkj.jpg)

顾名思义是将本地机器的某个端口流量转发到某台机器上去。

语法格式

*ssh -L [bind_addr]:port:host:hostport  user@server*

（其中如果bind_addr不指定的话，默认是localhost）

含义是将client机器上port端口流量通过server机器中转，转发到host机器的hostport端口。

那么这里面，只有client与server机器之间的连接是通过ssh加密的， 而后来server与host机器之间的连接则是明文的。要注意甄别。

举个例子，你在本地搭了一个squid做代理，但是由于伟大的qiang存在，需要把比如Google之类的网页流量让海外VPS上的父Squid来承载。但是如果只是在本地Squid里利用cache_peer连到海外的父Squid， 由于HTTP流量是明文的，那么几乎100%会被大qiang拦截。 

这里面还有一个安全隐患是这时的父Squid需要监听一个公网地址，容易被人扫端口扫到，不太安全。

假设海外的VPS的ip是x.x.x.x, 那么你可以将海外的父Squid监听 127.0.0.1:3128, 然后做一个本地端口转发  

**ssh -L 3129:127.0.0.1:3128   user@x.x.x.x**

那么这时候连接本地机器的3129端口，就相当于加密后连接父Squid。

还有一个需要注意的是，host的地址是在server端解析的（很自然啊，是server去连接host的）。 


---------------------------------------
* Remote Forwarding 远程转发
![](http://ww3.sinaimg.cn/large/a74eed94jw1e3cdg6zr0pj.jpg)

语法格式

*ssh -R [bind_addr]:port:host:hostport  user@server*

含义是：将server的port端口流量（通过client）转发到host机器的hostport。

看起来跟local forwarding一样，但是意义却差的很大。

设想这么一个场景：你在公司开发机是内网ip：10.x.x.x , 公司服务器有个公网ip: y.y.y.y,   回家之后你想连接公司的开发机。 那么你可以（回家之前在开发机上）做一个远程转发 

**ssh  -R   port:10.x.x.x:hostport    user@y.y.y.y **   

这样你连接 y.y.y.y:port时就相当于访问内网10.x.x.x:hostport 了。  

如果对照示意图的话，这里面的client和host机器合二为一，就对应你的公司的开发机。

这个过程里面，server <----> client 是加密的， client <----->  host 不加密


---------------------------------------

* Dynamic Forwarding 动态转发

语法格式是

*ssh -D [bind_addr]:port  user@server*

这样将在 [bind_addr]:port 监听请求（这个是本机的地址和端口）， 对于不同类型的请求过来，都将会被加密发到server机器上，调用对应的服务来执行：该是ftp的走ftp协议，该是HTTP的走HTTP协议。 这也就是常见的socks代理。

如果server机器是海外的VPS，那么这个socks代理就可以cross the great fire wall了。

以Chrome为例， 具体用法是在SwitchySharp插件里新建一个代理。socks4代理是在本地dns解析，socks5实现了socks4的所有功能，并且dns在远程解析。

![](http://ww3.sinaimg.cn/large/a74e55b4jw1e3ceisa4jfj.jpg)

毫无疑问，这里面client <---->  server 是加密的， 而client就是本机。

firefox用户的Autoproxy插件里使用方法类似。

---------------------------------------

Reference：

* 图片[来源](http://www.dirk-loss.de/ssh-port-forwarding.png)
* 参考这篇[wiki](http://linux-wiki.cn/wiki/zh-hans/SSH%E7%AB%AF%E5%8F%A3%E8%BD%AC%E5%8F%91%EF%BC%88%E9%9A%A7%E9%81%93%EF%BC%89)

