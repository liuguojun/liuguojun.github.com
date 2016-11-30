---
layout: post
title: 给博客加上HTTP/2支持
categories: [Trick]
tags: [HTTP2,nginx]
---
---------------------------------------

码农就喜欢折腾，更高更快更强是我们的目标。当HTTP/2协议已经被大多数服务器和浏览器支持的时候，是时候给自己的blog加上了。

这里有个[demo](https://http2.akamai.com/demo)可以看一下加载300多张小图片，HTTP/2的带来的速度上的快感

HTTP/2的历史可以参见[维基百科](https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE) 在这不多做介绍

一张图片概述HTTP协议的演化进程 ![](https://http2.akamai.com/resources/HTTP2-graphic.png)

下图可以看到不同浏览器从哪个版本开始支持HTTP/2 ![](https://sfault-image.b0.upaiyun.com/399/641/3996415863-583d40caae071_articlex)


---------------------------------------
## 简要介绍如何让你的博客支持HTTP/2

本站后台是nginx。nginx从1.9.5开始就支持HTTP/2.
要求：
* openssl完全安装
* HTTP/2强调安全性，采用了https,需要准备一个浏览器信赖的证书，可以用收费的，当然免费的[Let‘s Encrypt](https://letsencrypt.org/) 是我们的最爱。
* nginx需要开启http_v2_module 和 http_ssl_module，编译的时候注意下



## nginx 配置的时候需要注意
---------------------------------------
* 把监听在80端口的http请求重定向到443端口的https请求
{% highlight bash %}
server {
  listen 80;
  location / {
    return 301 https://$host$request_uri;
  }
}
{% endhighlight %}

* 然后开启HTTP/2

{% highlight bash %}
server {
  listen 443 ssl http2 default_server;

  ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
}
{% endhighlight %}

然后就是重启nginx



## 如何验证HTTP/2生效
---------------------------------------
Chrome上插件[HTTP/2 and SPDY indicator](https://chrome.google.com/webstore/detail/http2-and-spdy-indicator/mpbpobfflnpcgagjijhmgnchggcjblin)，蓝色的小图标亮起，就表示该网站使用了 HTTP/2 协议。
