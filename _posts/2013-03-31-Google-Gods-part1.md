---
layout: post
title: (译) Google众神 -- If Xerox PARC Invented the PC, Google Invented the Internet Part 1.
categories: [IT History]
tags: [Google, Jeff Dean]
---

- 原文[地址](http://www.wired.com/wiredenterprise/2012/08/google-as-xerox-parc/all/)
   , 第一部分在[这里](http://www.liugj.com/2013/03/Google-Gods-part2/) 
- 花了好大力气翻译了这篇文章， 水平有限很多英语习语不知道该如何翻译，有错误的地方请留言指出，谢谢
-  题目 Google众神是我自己取得(以前有本书叫做美国众神...)


---------------------------------------

## 如果说施乐帕克研究中心发明了PC， 那么Google创造了互联网 ##



![](http://ww4.sinaimg.cn/large/a7480316jw1e39e2hsgyhj.jpg)


关于Jeff Dean的真相出现在2007年的愚人节

  Google内部有一个私有的网站提供了一系列的关于Jeff Dean --- 这位Google最早的雇员之一以及这个网络巨人为什么比网上其他的机构能够承载更多流量的原因---的一些事情。这个网站只向Google员工开放，但是所有人都被鼓励向这个网站添加他们知道的关于Jeff Dean的事迹。很多人都提交了他们所知道的。

“有一次在1秒钟之内验证第203个斐波那契数的测试中，Jeff Dean没有通过图灵测试”， 有一条写到

“Jeff Dean在提交代码之前编译并运行了他的代码”， 另一条写到， “但是只是检查编译器和CPU的bug“

“光在真空中的传播速度曾经是35英里/时”， 另一条写到， “于是Jeff Dean花了一个周末来优化物理学“

  不， 这些“真相”比不是事实。 但是他们听上去很真实。 愚人节在Google是一个神圣的节日， 像所有的愚人节笑话一样，插科打诨总是围绕着事实周围。 一个叫Kenton Varda的Google员工建立了这个网站，playing off the satirical Chuck Norris facts that so often bounce around the net（ 这句话实在不知道咋翻译）。 当他把链接通过邮件发给公司其他人的时候，他隐藏了自己的踪迹。但是一会他就收到了Jeff Dean的提示消息，Jeff已经从Google的服务器日志里找到了他留下的电子足迹，从而追踪到他。

  在Google内部，Jeff Dean被认为是威严的。 在公司外部， 很少有人听过他的名字。 但是他们应该知道。 Dean是Google构建起基础软硬件架构一个工程师小组一员， 这些架构使Google为web上有统治力的公司， 他们的发明创造酷似网上其他如雷贯耳的名字。


  时光荏苒，我们再一次的[回忆](http://www.techspot.com/guides/477-xerox-parc-tech-contributions/)起施乐公司帕克研究中心（以下直接用 Xerox PARC， 不再翻译）硅谷实验室在PC的进化史上发明了几乎每一件的主要技术的往事，从用户图形界面、激光打印机到以太网和面向对象编程。但是由于google如此谨慎小心， 其最新的数据中心技术一直和[竞争者分开](http://www.wired.com/wiredenterprise/2012/03/google-miner-helmet/)---同时也因为像Jeff Dean这样工程师并不是自我夸耀的人 ----  google在现代计算领域的基础工作的影响并不为人所知。Google就是云计算时代的 Xerox PARC

  ”某段时间，当贝尔实验室和Xerox PARC这样的地方将要灭亡的时候，google很大一部分的工作就是招揽这些世界上最聪明的研究人员“。 Mike Miller，一位华盛顿大学粒子物理学合聘教授及 [Cloudant](https://cloudant.com/)（一家致力于开展研究google提出的技术的公司）的首席科学家。”它不仅抢夺他们的科学家， 而且抢夺赖以生存的生命之血“

  这些google的技术不是你用双手你能hand住的---甚至无法装进书桌。 他们并不是运行在一部手机或者PC上。 他们运行在数据中心连接起来的互联网中。


  这些技术包括席卷软件平台的比如 [Google文件系统](http://research.google.com/archive/gfs.html)，[MapReduce](http://research.google.com/archive/mapreduce.html)， [Bigtable](http://research.google.com/archive/bigtable.html)， 这些发明通过将一个任务分解成小块，然后分配到上千台机器中运行（就像蚁群协同工作一样）来驱动大量的在线应用。 同时他们采用了Google最新设计的用来跟他们的软件一起工作的服务器，网络设备， 数据中心。他们的想法是建立一个看起来就像单台计算机的数据仓库式的计算设备。 就像蚁群工作起来像一个整体，google的数据中心也是这样


  当硅谷被社会网络和触屏设备的爆发所惊恐的时候，Google重造了这些现象背后的一些基础的东西。 很快，当其他互联网巨头碰到了在线数据的雪崩式增长的时候，他们开始跟随Google的脚步。 当重构的Google的搜索引擎，GFS和MapReduce，产生出了[Hadoop](http://www.wired.com/wiredenterprise/2011/10/how-yahoo-spawned-hadoop/)---一个海量计算能力的数字处理平台，几乎是现在最成功的开源项目。 Bigtable则引发了NoSQL运动， 产生了服务于Web规模大小的数据库的家族编队。 当然在很多方面，Google在数据中心硬件的发展的很多方法借鉴了来自Facebook, Amazon, Microsoft以及其他公司的经验。


  实话说，Google的优势建立在一些来自很多公司和研究机构（比如PARC和贝尔实验室）被埋没的计算机科学家的数十年来的贡献。 跟Google一样，Amazon也对互联网的基础有很大的影响---最显著的是通过一篇研究论文发表了一个叫做Dynamo的文件系统。 但是Google的影响更加深远。

  Google和Xerox PARC的区别在于，Google的优势大部分来源于它的发明往往领先于世界上其他公司能赶得上的程度。像GFS，MapReduce这样的工具使得公司在竞争中处于领先。 现在，它大部分已经抛弃了这些工作，发展到新的软硬件领域。 又一次，世界上其他公司开始追赶google

---------------------------------------
### Google的孪生神人 ###

  Kenton Varda 在愚人节那天同时对其他几名google的工程师恶作剧。 Jeff Dean看起来像“最有趣的选择” Varda回忆。“他的风度可能是这些神里面最大的”

  另一个候选人是Sanjay Ghemawat， Dean长期的合作者。 2004年，Google发表了一个关于MapReduce的研究论文， 这个海量计算能力的处理系统可能是这个公司最具有影响力的数据中心方面的发明。 这篇论文只列了两个作者： Dean and Ghemawat。 这两个工程师同时在设计Bigtables数据库的过程中扮演了主要角色。 同时Ghemawat也是描述Google文件系统---一种在公司跨数据中心的网络上存储数据的办法------的三名作者之一


  即使是工作那个团队的Varda里来看Google的基础设施架构部门， 这两个工程师也是难以区分开。 ”Jeff和Sanjay一起工作开发了google的大部分基础架构， 很多时候他们就好像连髋关节都在一起“， Varda说， ”往往很难分辨谁做了什么“

  "在google所有的代码修改在提交之前都需要peer review(另一个人帮忙评审）， 但是Jeff和Sanjay的例子，总是一个人发一大段要评审的代码对方，对方直接 'LGTM'。 因为他们俩总是一起修改代码：”  LGTM在Google的意思是“looks  good to  me"

  Varda说的这些都是真实。 这些年来， Dean和Ghemawat养成了坐在同一台机器前面一起coding的习惯。 一般是Ghemawat编码. "他是他的地盘上的收割者“， Dean说

  他们俩在来google之前就认识了。 90年代，他们都在DEC公司----前互联网时代的计算领域巨人----的硅谷研究所工作。Dean在加州Palo Alto的DEC西部研究所工作， Ghemawat则在两栋楼之外的姊妹实验室-----DEC系统研究中心----工作。他们俩经常合作项目，并不仅仅因为Dean对处于两个实验室之间的冰激凌店感兴趣，而是他们俩一起合作的很好。 在DEC， 他们一起为java语言写了一个新的编译器， 以及一个改变了我们追踪服务器行为方式的系统分析器


  他们作为DEC研究力量“移民”的一部分， 一起来到Google。 在90年代后期， 此时的Google刚刚起步，但是DEC却已是日薄西山。 Google用很多RISC架构的微处理器制造出大型（计算能力）的服务器， 并且当时世界的潮流迅速的涌向装备了X86芯片的低功耗机器。1998年， DEC被PC巨人康普收购。 四年后，康普被HP合并。 DEC研究院的顶级研究员逐渐的向其他地方发展。

  ” DEC实验室在被康普收购后经历了一段动荡的时期“， Dean说，“而且它并不清楚在研院在这家刚合并了的公司里所处的角色“ 。一些工程师去了刚刚在硅谷开了分支研究机构的微软， 一些去了Palo Alto的一家叫做Vmware、从事试图将数据中心颠覆的虚拟机行业的创业公司。 很多则去了Google这家成立于DEC被康普收购同年成立的公司。

  这是一段好几家世界上最有影响力的研究所--包括Xerox PARC， 贝尔实验室（产生出[Unix操作系统和C语言](http://www.wired.com/wiredenterprise/2011/10/thedennisritchieeffect/)）----失去动力的时期。 但是即使这些研究所的好日子到头了，很多他们的研究员却迎来了曙光。

  “2001年， 互联网泡沫崩溃。 包括DEC在内的巨头们的时候缩减规模的时候， Google和Vmware两个高科技公司在招人”。 Eric Brewer，现在和Dean和Ghemawat一起工作的加州大学伯克利分校计算机系教授说道。 “由于需求和供给的严重不对称， 公司（指Google和Vmware）招聘了很多真正的伟大的人并且都因为这个因素发展的很好。”


  和Dean和Ghemawat一样，好几位从DEC来到Google的工程师帮忙设计了一系列的导致了整个Web可以被当做一个整体的技术， 其中包括Mike Burrows, Shun-Tak Leung, 和Luiz André Barroso. 同时，这些工程师在寻找有意思的工作----Google也在找寻足够聪明的人来使得搜索引擎跑起来。 但是作为事后诸葛亮来分析，这些来自DEC的“移民”提供了理想的象征使得Google能后改变形象屹立在世界上其他公司之前。

  DEC是第一个构建起成功的web搜索引擎的公司--出自DEC西部研究所的AltaVista--- 至少在刚开始时，整个AltaVista运行在单台的DEC机器上。Google让ALtaVista黯然失色很大一部分是因为它完全改变了这种模式。 与其用大型的机器来运行搜索引擎，Google将整个软件系统分解成小块， 并且把任务发散到小型廉价的机器上去。这就是隐匿在GFS， MapReduce， BigTable背后的思想。

  事后诸葛亮来分析的话， 这是自然进化的过程。 “构建一个像Google这样用上千台计算机搭建起来的系统的架构上的挑战和构建一个复杂完整的系统的挑战并不是完全不相同”， Armando Fox， 加州大学伯克利分校计算机系大规模计算方面的教授说道，“这些问题本质上很相近， 这也是为什么需要有在DEC工作过的人来参这些工作”



