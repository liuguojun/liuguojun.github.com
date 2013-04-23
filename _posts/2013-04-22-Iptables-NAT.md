---
layout: post
title: 谈谈iptables的NAT
categories: [Trick]
tags: [iptables, 端口转发]
---
---------------------------------------

最近一段时间折腾iptables的NAT功能，才发现iptables如此神器的东西以前竟然所知甚少。恶补了一下这方面的知识。

---------------------------------------
## iptables背景知识简要介绍

* iptables有table, chain, rules 三个概念。
* iptables的基本语法规则
    iptables  <option> <chain> <matching criteria> <target>
 

## iptables的NAT表
---------------------------------------
* NAT在iptables中的位置图示
![](http://ww4.sinaimg.cn/large/a74ecc4cjw1e3ypso4b3tj20dx0760t9.jpg)

* 包括
  * SNAT - Source NAT，扮演网关角色的机器和其他机器共享一个网络连接时使用，
  * DNAT - Destination NAT，将网络内部服务细节掩藏起来 
  * MASQUERADE - SNAT的一种特殊形式，适用于像adsl这种临时会变的ip上
  * REDIRECT - DNAT的一种特殊形式，将网络包转发到本地host上（不管IP头部指定的目标地址是啥）

* 首先需要启用NAT需要在Linux上打开内核对IP包的转发支持，linux上编辑 /etc/sysctl.conf 文件:


    net.ipv4.ip_forward = 1

然后执行

    sysctl -p 

* SNAT最主要是用在本地机器作为网关时，和其他机器共享网络连接是使用。比如，我们常见的pptp或者openvpn服务器:
  * 假设vpn网段是192.168.0.1/24
  * 本地机器外网ip是a.d.c.d
  * vpn网口为tun0. 那么需要:
{% highlight bash linenos %}  
    # 将来自于192.168.0.1/24 网段的流量通过a.b.c.d出口出去
    iptables -t nat -A POSTROUTING -s 192.168.0.1/24 SNAT --to-destination a.b.c.d
    
    # FORWARD链相应设置打开
    iptables -A FORWARD -i eth0 -o tun0 -m state -state RELATED,ESTABLISHED -j ACCEPT
    iptables -A FORWARD -i tun0 -o eth0 -j ACCEPT
{% endhighlight %}

如果是动态ip，则第一句话需要改成：
    iptables  -t nat -A POSTROUTING -s 192.168.0.1/24 MASQUERADE

* DNAT主要用在网络内部的机器提供服务，但是又不想暴露在网络上，那么需要在一台足够安全、可以暴露在互联网上的机器来提供NAT服务。
  * 比如本地机器有公网ip：1.1.1.1， 内网ip：192.168.0.1
  * 内网另一台机器192.168.0.2:8080 提供了web服务。
  * 我们可以做DNAT使得看起来访问1.1.1.1:80的时候，就跟访问192.168.0.1:8080一样：
    {% highlight bash linenos %}
    iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.0.2:8080
    
    # 关键一步，需要加上SNAT
    iptables -t nat -A POSTROUTING -d 192.168.0.2 --dport 8080 -j SNAT --to-source 1.1.1.1
{% endhighlight %}

  * 这里面其实就是做了端口转发， 将本机1.1.1.1:80端口转发到192.168.0.2:8080端口
  * DNAT是在进入路由层面的route之前，将IP包中的目的地址改掉，所以是PREROUTING链起作用
  * SNAT的目的是IP包经过route之后、出本地的网络栈之前，将源地址改成本机的（否则192.168.0.2机器响应请求的返回地址就不对）， 所以在POSTROUTING链。

* 此处延伸一下。端口转发有很多种实现方案，比如可以自己手写， 给一个python版的[代码](https://github.com/surfly/gevent/blob/master/examples/portforwarder.py):
{% highlight python linenos %}   
"""Port forwarder with graceful exit.

Run the example as

  python portforwarder.py :8080 gevent.org:80

Then direct your browser to http://localhost:8080 or do "telnet localhost 8080".

When the portforwarder receives TERM or INT signal (type Ctrl-C),
it closes the listening socket and waits for all existing
connections to finish. The existing connections will remain unaffected.
The program will exit once the last connection has been closed.
"""
import sys
import signal
import gevent
from gevent.server import StreamServer
from gevent.socket import create_connection, gethostbyname


class PortForwarder(StreamServer):

    def __init__(self, listener, dest, **kwargs):
        StreamServer.__init__(self, listener, **kwargs)
        self.dest = dest

    def handle(self, source, address):
        log('%s:%s accepted', *address[:2])
        try:
            dest = create_connection(self.dest)
        except IOError, ex:
            log('%s:%s failed to connect to %s:%s: %s', address[0], address[1], self.dest[0], self.dest[1], ex)
            return
        gevent.spawn(forward, source, dest)
        gevent.spawn(forward, dest, source)
        # XXX only one spawn() is needed

    def close(self):
        if self.closed:
            sys.exit('Multiple exit signals received - aborting.')
        else:
            log('Closing listener socket')
            StreamServer.close(self)


def forward(source, dest):
    source_address = '%s:%s' % source.getpeername()[:2]
    dest_address = '%s:%s' % dest.getpeername()[:2]
    try:
        while True:
            data = source.recv(1024)
            log('%s->%s: %r', source_address, dest_address, data)
            if not data:
                break
            dest.sendall(data)
    finally:
        source.close()
        dest.close()


def parse_address(address):
    try:
        hostname, port = address.rsplit(':', 1)
        port = int(port)
    except ValueError:
        sys.exit('Expected HOST:PORT: %r' % address)
    return gethostbyname(hostname), port


def main():
    args = sys.argv[1:]
    if len(args) != 2:
        sys.exit('Usage: %s source-address destination-address' % __file__)
    source = args[0]
    dest = parse_address(args[1])
    server = PortForwarder(source, dest)
    log('Starting port forwarder %s:%s -> %s:%s', *(server.address[:2] + dest))
    gevent.signal(signal.SIGTERM, server.close)
    gevent.signal(signal.SIGINT, server.close)
    server.start()
    gevent.wait()


def log(message, *args):
    message = message % args
    sys.stderr.write(message + '\n')


if __name__ == '__main__':
    main()
{% endhighlight %}

* SSH的端口转发也是比较成熟的端口转发方案，详情见[上一篇博客](http://www.liugj.com/2013/04/SSH-port-forwarding/)

* 这些方法的区别在于，Python版本的和SSH的端口转发工作在应用层， 而iptables能在网络层就实现，理论上来说效率应该更高（我没测试过）


* REDIRECT是DNAT的一个特例。方便在本机做端口转发。
  * 比如你有一个Squid这样的HTTP代理，监听网关的8888端口。那么你可以如下设置将它做成一个透明代理：

{% highlight bash linenos %}
    iptables -t nat -A PREROUTING -i eth0(假设从eth0进流量)  -p tcp --dport 80 -j REDIRECT  --to-port 8888
{% endhighlight %}
  * 这么做相当于将80端口的流量劫持到8888，并且不会被用户察觉


