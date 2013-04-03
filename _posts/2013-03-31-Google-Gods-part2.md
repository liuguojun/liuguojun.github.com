---
layout: post
title: (译) Google众神 -- If Xerox PARC Invented the PC, Google Invented the Internet Part 2.
categories: [IT History]
tags: [Google, Jeff Dean]
---

- 原文[地址](http://www.wired.com/wiredenterprise/2012/08/google-as-xerox-parc/all/)
  , 第一部分在[这里](http://www.liugj.com/2013/03/Google-Gods-part1/)
 
- 花了好大力气翻译了这篇文章， 水平有限很多英语习语不知道该如何翻译，有错误的地方请留言指出，谢谢
-  题目 Google众神是我自己取得(以前有本书叫做美国众神...)


---------------------------------------

### Jeff Dean追随他“叔叔”来到Google ###


![Uncle John](http://ww1.sinaimg.cn/large/a74ecc4cjw1e3bkubsy01j.jpg)

Jeff Dean 是第一个从DEC来到Google的。他追随他“学术上的叔叔(学术上的领路人）”---Urs Hölzle的脚步。

Hölzle是Google最开始的10名员工之一，也是公司第一个工程副总，他[见证了Google基础架构](http://www.wired.com/wiredenterprise/2012/04/going-with-the-flow-google/)----根据外界猜测，Google有分布在全球的多达35个数据中心----从无到有诞生的过程。他以加州大学圣芭芭拉分校教授的身份加入了Google。 在这之前，他在斯坦福大学跟随曾经开发过现今Java语言编译器的一些核心技术的[David Ungar](http://en.wikipedia.org/wiki/David_Ungar)教授学习。


Dean学术上的导师（译注：指Urs Hölzle）也跟随过Ungar一起学习过，所以我们称呼Hölzle为“学术上的叔叔”。 1999年，当DEC垂死挣扎的时候， Dean离开了DEC，去了一家叫MySimon的创业公司。但是当他看到Hölzle在Google的时候，他发了封邮件询问Google是否能提供他想要的工作。很快他就被当初招聘Hölzle的那个人给招进了公司。 那个人正是Google的合伙创始人， 拉里佩奇。


起先，Dean负责为Google的雏形搜索引擎构建广告系统。几个月后，他开始接手由于全球互联网迅猛发展苦苦支撑的Google公司的最核心的搜索技术。很大程度上因为Dean和DEC的其他同事比如Krishna Bharat 和 Monika Henzinger都加入了Google，很快Ghemawat也加入了Dean的工作行列。

“如果不是Jeff在Google， 我很可能不会去Google面试”， Ghemawat说到。他们很快捡起在DEC所丢下的工作内容。接下来的3、4年里，与一个一直处在不断变动的工程师小组一起， 他们俩设计并实现了好几个版本的公司的核心系统， 包括抓取web页面、建好索引、为全球的用户提供搜索服务。


是的， 他们经常喝着咖啡在同一台机器上写代码。卡布奇诺是他们的最爱。 Dean对于他们一起合作完成的工作评价是，因为Ghemawat更加冷静沉着，“我有时候会变得没有耐心，思考我们做某件事情的所有可能方案，我的想法变得很快， 手上能很迅速敲出来代码（这一句原文是 my mind and hands spinning at a very fast rate）。Sanjay也会很兴奋，但是他能控制住自己。他不断修改我的前进路线，所以我们最终会朝着正确的方向前进”。


Ghemawat认为Dean的做事的方法同样重要。他（译注：指Dean）保持他俩不断的前进。 “我经常会沮丧，思考做事情的其他不同的方法，担心是不是错过了正确的方案”， Ghemawat说， “为了达到目的，需要有这么一个充满能量和激情的人”。

大的突破伴随着GFS和MapReduce这两个在最近十年的头几年里Google的数据中心的不断推广应用的发明的到来。这些平台为Google的搜索引擎海量数据的索引提供了可靠的解决方案。Google抓取互联网上的网页，它可以通过GFS将数据存储在数千台的服务器上。 有了MapReduce， 它就有了将这些数据处理成单个的、可检索的索引的计算能力。

这些平台的巧妙在于，即使有机器失败或者网络变慢，它们也不会崩溃掉。当你像Google一样需要处理数千台普通务器的时候，机器挂掉是常有的事。有了GFS和MapReduce， Google可以在不同机器上放置多份数据的备份。即使有一台机器挂掉， 其他的仍然正常工作。

“Google的索引的规模，导致了处理机器挂掉和延迟是一项复杂的任务。为了高性能和可扩展性， 我们试图对一堆机器的自动并行化做抽象（译注：抽象是计算机领域很重要的概念，屏蔽底层的细节）， 同时也需要分布在数千台服务器上的长期运行的系统提供鲁棒性和可靠性”， Jeff Dean在描述MapReduce背后设计的思考的时候说到。他解释道，当这些工具运用在Google的搜索引擎里的时候，Google意识到他们可以用（同样的解决方案）来跑其他的Web业务。


BigTable出现在类似的场景。和MapReduce一样，它运行了GFS之上，但是它不处理数据， 更像一个海量的数据库。 “它管理行数据”， Dean说到，“并且按照你的需要将这些数据分布到更多的机器上去”。 它并不像传统的关系型数据库那样，给了你很多对数据的操作的功能，但是它可以管理远远超过单台机器的软件平台能够管理的数据量。


同样的故事一遍遍的发生。随着公司的成长，Google遇到了空前规模的数据增长，它需要重新构建软件系统。

“当你的工作是接管整个英特网，建立索引，做一份拷贝---当然并不是拷贝一份和Internet同样大小的那样直接的方式来做----你会怎么做？ 这是个很有趣的技术挑战”， Jason Hoffman（Joyent云计算部门的首席技术官）说到， “挥舞锤子的人知道如何造一个锤子。 大多数创新来自于（之前有过的）做事的经验中， 来自你面临失败的那些地方”

---------------------------------------

### Crème Brûlée打造的数据中心帝国 ###

![](http://ww4.sinaimg.cn/large/a74eed94jw1e3bl0n0w1hj.jpg)

Luiz André Barroso 追随Jeff Dean和Sanjay Ghemawat离开DEC来到Google。 但是他差一点就没能来。 

Barroso在DEC的西部研究所和Dean一起工作。2001年，他在Google和Vmware的工作offer之间权衡。在访问及面试了两家公司之后， 他列了一个加入各家公司的理由的表格。 但是这个表格上两家公司最终打成了平手：加入Google有122个理由， 同样有122个理由加入Vmware。

然后他跟Dean作了交流。Dean提醒他是否把Google的行政主厨Charlie Ayers提供的焦糖布丁考虑在内。 “焦糖布丁是他的最爱”， Dean还记得。 “我问他是否考虑（焦糖布丁）在这122条理由之内， 他说， '没有！我忘记了！'”。 第二天Varroso就接受了Google的工作Offer。

Barroso不同寻常之处在于，他不是一个纯粹的软件工程师。在DEC工作时， 他帮忙开发多核处理器---真正的在1颗CPU上的有多个处理器。 但是当Barroso在Google的软件部门工作的时候， Hölzle让他去负责Google的硬件部门的大整修，不仅仅包括服务器和其他计算设备，还有和数据中心相关所有硬件设备。“我是这伙人里技能最接近硬件工程师的人”， Barroso回忆道（此处原文是：I was the closest thing we had to a hardware person）。

Hölzle, Barroso 以及他们的“平台团队” 开始重新思考公司的服务器。2003年， 不再跟往常一样从DELL、HP这些公司采购常规服务器，这个团队开始降低成本， 设计自己的服务器，并且联系同样为DELL和HP这些公司提供产能的亚洲的的工厂来生产。简而言之，Google减少了中间供应商。

Google的服务器独特之处在于， 每台服务器都包含一个12V的电池。
当机器没有主电源供应的时候，这块电池可以接手继续撑一会。据Google的说法，这种做法比在数据中心里装备大量的UPS（不间断电源， 为服务器提供后备电力）要更明显的有效率。


之后这个团队继续工作在托管服务器的数据中心领域。当时Google都是从其他的公司租用数据中心托管服务。但是Barroso和Crew从头开始， 为了节能和省钱，同时也是为了提供Google的Web服务的性能，设计和构建了Google自己的数据中心。


Google的第一个新数据中心在Oregon的Dalles， 一个能提供廉价电力以及税费优惠的偏僻乡村。但是他们的主要目标是，将整个数据中心建设成看起来就像一台完整的大计算机。Barroso和Hölzle称呼它为“数据仓库式的计算（中心）”

“为了提供更高水平的英特网服务性能， 这些数据中心里的大部分软硬件资源必须紧密联系在一起协同工作。这个目标只能通过从全局整体来设计并部署才能实现。” Barroso和Hölzle 在他们2009年出版的一本很有影响力的、标题叫做 "[The Data Center as a Computer](http://www.morganclaypool.com/doi/pdf/10.2200/s00193ed1v01y200905cac006)"一书中写到，“换句话说，我们必须把数据中心整体当做一个数据仓库式的电脑（来使用）”。


他们用新型的组件来设计了这种数据中心。紧密放置的服务器，网络设备，以及其他硬件设备被装进标准化的方便运输的装置内 ---- 和陆运及海运运输货物的同样的装置。  这些数据中心模块可以被拼接成一个大的多的计算设备。 这么做的目的是最大化每个模块的工作效率。很明显，这个概念来自于拉里佩奇在2003年看到的Internet Achieve做的一个关于他们的关于类似模块的设想的[汇报](http://www.baumgart.org/petabytebox.pdf)---即使Barroso不记得这个想法从何而来。“反正不是我”， 他说到。

公司在Delles的数据中心2005年投入使用。这几年来，关于Google的数据中心模块和定制服务器的传言满天飞， 但是细节直到2009年Google在硅谷总部举行的一次小型会议上才[揭开](http://news.cnet.com/8301-1001_3-10209580-92.html)。 在数据中心里， Google不满足于仅仅创新。它保持极度的创新欲望直到已经足够好并且能够和世界上其他的（公司）分享。

---------------------------------------

### 特斯拉现象  ###

拉里佩奇特别崇拜Nikola Tesla。 根据Steven Levy的那本讲述Google背后故事的书---[In The Plex](http://books.google.com/books?id=V1u1f8sv3k8C&pg=PA197&lpg=PA197&dq=barroso+%22western+research+lab%22&source=bl&ots=BRqOaugkhF&sig=BLNj8Nxie2rWOUwqwgpxfDmyDdY&hl=en&sa=X&ei=ysAWUP_jPMr26gH79IHADA&ved=0CE8Q6AEwBQ#v=onepage&q=barroso%20%22western%20research%20lab%22&f=false)介绍，  佩奇认为Tesla作为一个发明家和爱迪生不分上下，但是令人惋惜的是他缺乏将发明创造转化为盈利的能力以及得到长期认可的能力。


Nicola Tesla的境遇警示Google如何对待它的核心技术。 它把它们视为商业机密。 跟苹果公司一样，Google在保密方面很有一套。但是另一方面，当一项技术在Google服务好几年后， Google就会将它披露出来。“在不放弃我们的竞争优势的前提下，我们会尽量做到开放”， Hölzle说。“我们会（跟别人）沟通想法， 但是不会公开实现方式的细节”。


2003和2004年，Google发表了[GFS](http://research.google.com/archive/gfs.html)和[MapReduce](http://research.google.com/archive/mapreduce.html)的论文。 Google的论文对这些系统本身做了很好地说明。不久之后，一个叫做Doug Cutting的开发者就利用（这些论文的思想）， 为他称做Nutch的一款开源搜索引擎构建了索引系统。Cutting加盟当时的Google搜索的竞争对手雅虎之后，这个项目[演变为Hadoop](http://www.wired.com/wiredenterprise/2011/10/how-yahoo-spawned-hadoop/)。


作为能够驱动上千台服务器来处理史诗般壮丽、罕见数量的数据的Hadoop， 被包括Facebook、Twitter、 微软等很多其他互联网巨头所采用。现在Hadoop已经逐渐发展到其他的商业领域。根据[IDC的研究](http://gigaom.com/cloud/all-aboard-the-hadoop-money-train/)，到2016年，Hadoop项目将会创造813个million美元价值的软件市场。

历史又在BigTable身上重演。Google在2006年发表了关于它的sweeping database（如何翻译sweeping database ？）的论文，以及Amazon的一篇描述名为Dynamo的数据库存储的论文，拉开了轰轰烈烈的在成千上万台机器上扩展运行的NoSQL运动的序幕。

“如果你自己观察各种NoSQL的方案，所有的都可以归结到Amazon的Dynamo或者Google的BigTable的论文思想里”， Joyent的Jason Hoffman说，“如果Google或者Amazon没有发表出这两篇学术论文，这个世界将会是怎么样的另一番景象”。


Google的硬件部门是完完全全不同的另一番故事。我们对于Google的数据中心内部依然所知甚少，但是公司在设计及构建这方面东西的努力同时也影响了了互联网上的其他公司（这一句翻译的不好， 原文是：but the company’s efforts to design and build its own gear has undoubtedly inspired similar efforts across the web and beyond）。 Facebook在亚洲生产厂商的协助下正在[设计它自己的服务器](http://www.wired.com/wiredenterprise/2012/01/facebook-server-lab/)、服务器机架以及存储设备。根据外部消息，Amazon和微软也在做类似的事情。在开源计算基金会的支持下， Facebook开源了它在服务器方面的设计， 很多其他公司也在探索类似的硬件。


此外，模块化数据中心是当今Web的支柱。微软用这些， eBay以及其他数不清的公司也在用。Mike Manos， 前微软数据中心领域专家，否认Google是模块化数据中心浪潮的开创者。他指出类似的模块最早可以追溯到1960年左右。但是Google最先借用了这种思路。 Cloudant的Mike Miller指出， GFS和MapReduce的思想也来自于以前（译注：指的是MapReduce的思想来自于函数式编程）。但是Google能够很好地将这些旧的思想应用到解决新的问题中来。 

![The building that once housed DEC’s Systems Research Center. It’s now home to an Amazon.com research operation called A9. Photo: Ariel Zambelich/Wired](http://ww3.sinaimg.cn/large/a74e55b4jw1e3bmc5gum6j.jpg)

---------------------------------------

### Google的过去仅仅是拉开序幕 ###

具有讽刺意味的是， Google差不多已经在内部替代了这些技术。过去几年，Google用一个叫做[“Colossus”的](http://www.wired.com/wiredenterprise/2012/07/google-colossus/)新平台替代了GFS， 并且用了一个叫做[Caffeine](http://www.wired.com/wiredenterprise/2012/03/microsoft-bing-v-google/)的新系统来构建索引。Caffeine系统包含部分MapReduce，但是使用实时构建索引而不是（隔段时间）从头开始重新构建这样一种完全不同的方式来运作。


Google或许会一直用其在Dalles的数据中心，但是在新的数据中心里，它不再扮演重要角色了。我们对Google现在在它高度机密的数据中心里使用的技术所知甚少， 但是你可以从它过往的所作所为来推测现在。


最近几年，Google发表了关于Caffeine以及两个其他的软件平台：Pregal这个从数据分片中映射（图结构）关系的图数据库； 以及能在超大量的数据中快速分析数据的Dremel。好几个开源项目正在试图实现Pregel。至少有一个正在克隆Dremel. Cloudant的Miller如此评价Caffeine：它改变了Hadoo和NoSQL的市场。

这些仅仅是一些在Google内部得到应用的最新发明。毫无疑问，还有更多我们不知道的东西。但是不管现在Google在用什么，它一定会很快的迭代前进。去年五月，加州大学伯克利分校的教授Eric Brewer[声明](https://twitter.com/eric_brewer/status/68051541063503872)他加入了Google的构建Google下一代基础设施架构的团队。“云还很年轻”， 他说，“有很多东西可做，很多目标可以实现”。


作为分布式计算研究领域的巨人，Brewer是Google作为Xerox PARC现在继任者的又一个标志。但是Google将PARC的精神又发扬了一大步。

你可以对比DEC来追溯Google的研究机构（的发展轨迹）， 所有的都很像PARC早期的时候。DEC的系统研究中心是Robert Taylor建立的，同时Taylor也是PARC的计算机科学实验室的成立者。


Taylor感觉到DEC在80年代失去了它的发展方向，所以建立了SRC（Systems Research Center）。 “我在PARC工作的很多同事都跟我一样， 感觉（工作上）理想在幻灭，在失去希望”， 他说， “所以他们加入了我（创办的实验室）”。 他用旧的PARC计算机科学实验室的影子作为蓝图来建设SRC---至少在组成架构上和它一样。 某种程度上，他成功了。


但是他也遇到了同其他很多合作研究机构一样的限制。它（指SRC）花了很长时间将研究成果孵化成型，投入到市场上来。这也是Jeff Dean工作所在 的DEC西部研究所的困境。这促成了他去Google工作。“最终，我所做的工作脱离了实际用户这个挫折导致了我想去一个创业公司”， Dean说。


但是Google不是传统的创业公司。它把科学研究上的挑战以及迅速将研究成果展示（给用户，让其满意）结合起来。 Google是一个研究机构--同时你也可以说它不是。“Google的基础设施方面的工作并不能真正的被当做是研究”，Ghemawat说， “它是如何解决我们在实际生产环境中遇到的问题”。


对于一些人来说，在Google的核心基础部门工作的一个缺点就是你不能告诉任何人你的工作内容是什么。这就是为什么Amir Michael离开了Google， 去Facebook构建服务器。但是，工程师们也有将他们的工作发表成论文或者公开的讨论的机会。


对于Google来说，这就是需要平衡的艺术。甚至一些平衡还很苛刻（原文是：Though some are critical of the particular balance）， 但这对于Google来说很适用。没有反对它这种做事方式的其他方法使得这个世界进步。 即使是PARC也没有（像Google一样）表现的更好。


