---
layout: post
title: [翻译] Pinterest扩容--两年内从0开始到每月数十亿PV实战
categories: [翻译, HighScalability]
tags: [翻译, HighScalability, Scale, Pinterest]
---
--------------------------------------------------------

注：网上找到个同名的ppt，下载地址在[这里](http://pan.baidu.com/share/link?shareid=422881&uk=304492695)


----------------------------------------------------------


Pinterest经历了每一个半月翻一番的指数级增长。在过去的两年里，他们经历了从0开始到每个月数十亿的PV（page view），员工数从开始时的两个创始人和一个工程师到超过40个工程师，从仅仅一台MySQL服务器到多达180台Web服务器、240台API引擎、88个MySQL数据库（EC2的cc2.8xlarge实例）+ 每台DB一个从库（slave）， 110台Redis实例服务，以及200台的Memcache实例。

令人叹为观止的增长。想一探Pinterest的传奇吗？Pinterest的[Yashwanth Nelapati]](http://www.linkedin.com/in/yashh) 和 [Marty Weiner](http://pinterest.com/martaaay/)将以 Scaling Pinterest为题讲述关于Pinterest架构的传奇故事。他们说更希望在一年半前的那段飞速发展的时候能看到有人做类似题材演讲，那时的他们有太多（技术上需要）选择的东西。这段过程中他们做了很多错误的选择。

这是一个充满了令人惊讶的细节的伟大演讲。同时也是个很实用的演讲，归根结底，它带来了可让大家适用的方案。高度推荐！


这个演讲中我最喜欢的两个经验：<br />
1.架构就是做正确的事情，使得当业务发展需要时能通过增加相同的部件来（承受增长量）。通过在一个难题上花钱来解决扩容，意味着你可以按需在难题上投入相同的硬件资源。如果你是这样的架构师， 那你做得很好。

2.当你把某些东西逼到极限的时候，所有的技术都会以不同的方式失败。这导致我们在做工具选择的时候更倾向于带有如下特性的：成熟；简单好用；知名；良好的技术支持；一贯优异的表现；尽可能少的出故障；免费。他们用这些标准选择了：MySQL， Solr， Memcache， Redis。 Cassandra和Mongo被抛弃了。<br />

这两个方案是相关的。符合准则2中的工具可以通过增加相同硬件资源的方法来扩容。并且随着负载的增高，使用成熟稳定的工具可以更少的出问题。当你的工具出的问题太tricky、太finicky的时候，（技术难度太大导致）你不能解决他们的时候，至少有社区来协助帮忙解决。


关于为什么分片比（数据库）集群更好的讨论，我认为这个演讲中最有价值的部分。主题是可以通过增加（硬件）资源来抗住业务增长量，更少的出错场景，简单稳定并且有着良好技术支持，有了这些因素才取得了圆满的成功。注意他们是通过增加shard来抗住业务量的增长，而不是采用集群的思路。
关于为什么他们喜欢分片以及如何分片这部分很有趣， 并且很可能有你以前没有考虑过的东西。

现在让我们来一探究竟如何扩容：

## 基本信息：

* Pin就是是一幅带着一些其他信息、一段对用户来说有用的文字描述、一个指向哪里可以找到它的链接的图片

* Pinterest是一个社交网络。那你可以follow任何人和board

* 数据库：存放的数据有：用户有哪些board，board上有哪些pin;  follow和repin的关系数据； 授权信息。


## 2010年三月上线 --- 发现自我的时期

那时候连在做什么产品都没弄明白。他们有点子，所以不断的快速迭代和尝试。最终数据库里多了很多现实生活中根本不会有的奇怪的MySQL短查询。

早期的一些数字：

* 2 founders

* 1 engineer

* Rackspace（译注：服务器提供商）

* 1 small web engine

* 1 small MySQL DB

--------------------------------------------------------------------

## 2011年1月

仍然处于“隐身”模式，产品还是从用户的反馈中不断演进。 一些数字：

* Amazon EC2 + S3 + CloudFront（译注：CDN提供商）

* 1 NGinX, 4 Web Engines (为了冗余，而不是负载均衡)

* 1 MySQL DB + 1 Read Slave (放置master挂掉)

* 1 Task Queue + 2 Task Processors

* 1 MongoDB (计数器功能)

* 2 Engineers

--------------------------------------------------------------------

## 直到2011年九月 - 实验期

每一个半月翻倍的疯狂发展期。非常疯狂。

* 当你以如此快的速度增长的时候，所有的东西可能在每个星期的午夜里宕掉。

* 这时候你会读很多那些宣称只要增加相同的组件（就能解决问题）的论文。所以你照做了，使用很多的技术。很不幸都失败了。

*因此你会看到一个非常复杂的架构：

** Amazon EC2 + S3 + CloudFront

** 2NGinX, 16 Web Engines + 2 API Engines

** 5 Functionally Sharged MySQL DB + 9 read slaves

** 4 Cassandra Nodes

** 15 Membase Nodes (3个独立集群)

** 8 Memcache Nodes

** 10 Redis Nodes

** 3 Task Routers + 4 Task Processors

** 4 Elastic Search Nodes

** 3 Mongo Clusters

** 3 Engineers


* 5个主数据库只是为了他们的数据分离。

* 如此快速的增长导致MySQL负载很大。其他所有的技术都被逼到了极限。

* 当你把一些东西逼到极限的时候，所有的技术都会以他们自己的方式失败。

* 扔掉技术来问问自己究竟想要什么。为所有的东西做重新设计架构。

--------------------------------------------------------------------

## 2012年1月----发展期

* 所有的一切都重新架构之后，系统看起来像这样：

** Amazon EC2 + S3 + Akamai（译注：CDN提供商）, ELB（译注：Amazon Elastic Load Balance）

** 90 Web Engines + 50 API Engines

** 66 MySQL DBs (EC2的m1.xlarge虚拟机) + 1 slave each

** 59 Redis Instances

** 51 Memcache Instances

** 1 Redis Task Manager + 25 Task Processors

** Sharded Solr

** 6 Engineers

* Memcache和Solr。（使用这些的）优点是这些都是简单成熟的技术。

* Web访问的流量保持同样的速度增长，与此同时iphone的流量上升。

--------------------------------------------------------------------

2012年10月12 --- 回归期

（流量）相当于2012年1月的四倍

* 数字看起来是这样：

** Amazon EC2 + S3 + Edge Cast,Akamai, Level 3

** 180 Web Engines + 240 API Engines

** 88 MySQL DBs (cc2.8xlarge) + 1 slave each

** 110 Redis Instances

** 200 Memcache Instances

** 4 Redis Task Manager + 80 Task Processors

** Sharded Solr

** 40 Engineers (持续增长)


* 架构就是做正确的事情，使得当业务发展需要时能通过增加相同的部件来（承受增长量）。通过在一个难题上花钱来解决扩容，意味着你可以按需在难题上投入相同的硬件资源。

* 这时候开始慢慢转向SSD。

--------------------------------------------------------------------

## 为什使用Amazon的EC2/S3 ？

* 很好的可靠性。数据中心也会宕掉。多租户虽然增加了一些风险，但不见得是个坏方案。

* （AWS有)很好的汇报和支持系统。 他们有很好的架构师，并且知道问题所在。

* 齐全的周边设备（译注：指的是围绕在EC2周围的比如存储，负载均衡，数据库等AWS其他服务）。特别是当你快速增长的时候，你可以专注于你的APP的引擎，而不用重写比如Maged Cache（咋翻译？） 负载均衡，Map/Reduce， 可管理的DB以及所有其他AWS可用服务。你可以在Amazon的服务基础上快速启动。当你有余力的时候再找工程师自己实现他们。

* 新的实例在数秒内就可用。这就是云的力量。特别是仅仅有两个工程师的时候，你根本不用操心容量规划或者等上两周去等你的memcache设置好。 10台Memcache实例可以在数分钟内设置好。

* 缺点：选择有限。直到最近才有SSD的选择，并且不能获取更大的内存。

* 优点：选择有限。你不会得到一大堆不同配置的硬件。

--------------------------------------------------------------------

## 为什么使用MySQL？

* 非常成熟。

* 坚实可靠。从不挂掉并且不丢失数据。

* 便于招聘。很多MySQL方面的工程师。

* 响应时间随着请求量线性增长。有些技术在请求量猛增时没有表现的像MySQL这么好。

* 优秀的软件技术支持--- XtraBackup, Innotop, Maatkit

* 良好的社区。 有问题很容易得到解决。

* 很容易从像Percona这样的公司得到技术支持。

* 免费 -- 当你初始时没有多少启动资金的时候，这点非常重要

--------------------------------------------------------------------

## 为什么使用Memcache ？

* 非常成熟。

* 真的很简单。就是哈希表加上socket网络套接字。

* 一贯表现优异的性能

* 非常知名

* 从不crash

* 免费

--------------------------------------------------------------------

## 为什么使用Redis ？

* 不成熟，但是的确是一个简单优美的解决方案

* 提供了一系列的数据结构。

* 提供选项，通过你想要的方式实现持久化和复制功能。如果你希望有MySQL式的持久化，可以有。如果你不需要持久性，可以去掉。或者你只想要3小时的持久性，也可以有。

** 你的Pinterest主页的feed就是在Redis上，每三小时保存一份。Redis没有每3三小时的复制策略，他们仅仅是每三小时备份一次。

** 如果box上的数据是存在die（如何翻译？）上，他们只是几小时备份一次。这并不是很可靠。但是足够简单。你不需要复杂的持久化和复制机制。目前的这种架构足够简单并且廉价。

* 非常知名而且很受欢迎

* 一贯表现优异的性能

* 很少会失败。由一些微妙的错误mode需要你去学习。这就是成熟稳定性的来源。学习他们并且解决（他们）


* 免费

--------------------------------------------------------------------


## Solr

* 非常好的刚起步时应该采用的技术。安装好后几分钟就可以构建索引。

* 并不能扩容到超过一台box（最近版本还是如此）

* 尝试了弹性搜索，但是根据他们现在拥有很多小文件以及大量query的规模，还有些困难

* 现在用了WebSolr。她们也有一个搜索团队，将来会用他们自己的搜索。

--------------------------------------------------------------------

## Clustering Vs Sharding

* 随着快速的增长，他们意识到需要把数据均匀的分散（到多台机器上）， 以便于能够hold住不断增长的负载。


* 给问题定义了范围后， 他们从各种不同的角度来分析Clustering还是sharding的问题

--------------------------------------------------------------------

## Clustering --- 一切都是自动化的

* 代表：Cassandra, MemBase, HBase

* 裁决意见：十分谨慎，或许在将来（会采用clustering），但是现在对于他们来说方案太过复杂，并且有太多失败的案例。

* 特性：

** 数据自动分布式存放

** 数据可以移动

** 可以根据容量做重新的均衡存储

** 节点间相互通信。A lot of crosstalk, gossiping and negotiation.

* 优点：

** 你的数据库自动扩容，至少这是白皮书里面说的

** 很容易上手设置

** 空间上分布式，colocate your data。 你可以在不同地区有数据中心，数据库能够处理这种（跨数据中心）的情况

** 高可用性

** 负载均衡

** 没有单点故障

--------------------------------------------------------------------

Cons (from first hand experience):

Still fairly young.

Fundamentally complicated. A whole bunch nodes have to symmetrical agreement, which is a hard problem to solve in production.

Less community support. There’s a split in the community along different product lines so there’s less support in each camp.

Fewer engineers with working knowledge. Probably most engineers have not used Cassandra.

Difficult and scary upgrade mechanisms. Could be related to they all use an API and talk amongst themselves so you don’t want them to be confused. This leads to complicated upgrade paths.

Cluster Management Algorithm is a SPOF. If there’s a bug it impacts every node. This took them down 4 times.

Cluster Managers are complex code replicated over all nodes that have the following failure modes:

Data rebalance breaks. Bring a new box and data starts replicating and then it gets stuck. What do you do? There aren’t tools to figure out what’s going on. There’s no community to help, so they were stuck. They reverted back to MySQL.

Data corruption across all nodes. What if there’s a bug that sprays badness into the write log across all of them and compaction or some other mechanism stops? Your read latencies increase. All your data is screwed and the data is gone.

Improper balancing that cannot be easily fixed. Very common. You have 10 nodes and you notice all the node is on one node. There’s a manual process, but it redistributes the load back to one node.

Data authority failure. Clustering schemes are very smart. In one case they bring in a new secondary. At about 80% the secondary says it’s primary and the primary goes to secondary and you’ve lost 20% of the data. Losing 20% of the data is worse than losing all of it because you don’t know what you’ve lost.


* 缺点（第一手的经验）：

** 还是处于年轻（不成熟）的阶段

** 功能复杂。集群所有节点都要一致，在生产环境是一个难题（这句话没翻译好）。

缺乏社区支持。在社区和数条产品线之间有间隙，缺乏良好的支持

很少有工程师有这方面的经验。很可能大部分的工程师没用过Cassandra

困难吓人的升级机制。可能是和他们（机器）至今都用api并且相互通信，所以你不想要他们困惑（指的是？？）。这导致了复杂的升级路线。

集群管理算法是SPOF。如果有bug的话，所有的节点都会受影响。这导致了Pinterest宕机了4回

集群管理器代码如此复杂，在所有节点上都有一份，所以会出现如下的场景：

数据重分布失败。加入一台新节点，数据开始复制，然后这个节点挂了。这时你怎么办？没有工具能找出究竟发生了什么。也没有社区帮助，所以我们会发疯。然后他们回退到用MySQL

数据在所有节点上发生了冲突。如果有个bug将缺陷扩散到所有节点的写日志里，然后compacting或者其他机制停了怎么办？（这一句翻译不好） 你的读延迟会增加。你所有的数据会拧在一起，数据会丢失

数据均衡分布不当不会被轻易修复。xxxxxxxxxxxxxxxxxxxxx

数据授权失败。Clustering的scheme是很智能的。在某种场景下，他们引进了一个xxxxxxxxxxxxxxxxxxxxxxx














Sharding - Everything Is Manual:

Verdict: It’s the winner. Note, I think their sharding architecture has a lot in common with Flickr Architecture.

Properties:

Get rid of all the properties of clustering that you don’t like and you get sharding.

Data distributed manually

Data does not move. They don’t ever move data, though some people do, which puts them higher on the spectrum.

Split data to distribute load.

Nodes are not aware of each other. Some master node controls everything.

Pros:

Can split your database to add more capacity.

Spatially distribute and collocate your data

High availability

Load balancing

Algorithm for placing data is very simple. The main reason. Has a SPOF, but it’s half a page of code rather than a very complicated Cluster Manager. After the first day it will work or won’t work.

Sharding  - 所有的东西都是手工的

裁决意见：Sharding胜出。我猜他们的Sharding架构和Flickr的架构有很大程度上的相似。

特性：

去掉Clustering的那些特性里你不喜欢的，就是sharding

数据手工分布式

数据不移动。xxxxxxxxxxxxxxxxxxxxxxxxxxxxx

把数据分片，分布化负载

节点之间互相不知道对方存在。一些master节点控制一切。

优点：

将你的数据库分片，增加容量

空间上分布式，collocate your data

高可用性

负载均衡

数据放置的算法很简单。主要原因是SPOF是仅仅半页纸的代码，而不是一个复杂的集群管理器。After the first day it will work or won’t work.





ID generation is simplistic.

Cons:

Can’t perform most joins.

Lost all transaction capabilities. A write to one database may fail when a write to another succeeds.

Many constraints must be moved to the application layer.

Schema changes require more planning.

Reports require running queries on all shards and then perform all the aggregation yourself.

Joins are performed in the application layer.

Your application must be tolerant to all these issues.


ID 生成过程很简单

缺点：

无法做大多数的join操作

失去了事务兼容性。往一个数据库写可能失败的同时，往另一个库写可能成功

很多限制被挪到了应用层

Schema的改变需要更多的规划xxxxxxxxxxxxxx

数据汇报需要在所有shard上执行query， 然后手工做整合

只能在应用层做join

你的应用必须对所有的这些问题做容错处理













When To Shard?

If your project will have a few TBs of data then you should shard as soon as possible.

When the Pin table went to a billion rows the indexes ran out of memory and they were swapping to disk.

They picked the largest table and put it in its own database.

Then they ran out of space on the single database.

Then they had to shard.

什么时候该做Shared？
如果你的项目将会有上TB的数据，那么尽可能早的做shard

当Pin表达到10亿行的时候，表的索引用尽了内存，被swap到磁盘上

然后他们将最大的表放到了单独的数据库上（译注:应该是独立的物理机上）

即使这样单台数据库最终也用尽了空间

最终他们还是做了shared












Transition To Sharding

Started the transition process with a feature freeze.

Then they decided how they wanted to shard. Want to perform the least amount of queries and go to least number of databases to render a single page.

Removed all MySQL joins. Since the tables could be loaded into separate partitions joins would not work.

Added a ton of cache. Basically every query has to be cached.

The steps looked like:

1 DB + Foreign Keys + Joins

1 DB + Denormalized + Cache

1 DB + Read Slaves + Cache

Several functionally sharded DBs + Read slaves + Cache

ID sharded DBs + Backup slaves + cache

Earlier read slaves became a problem because of slave lag. A read would go to the slave and the master hadn’t replicated the record yet, so it looked like a record was missing. Getting around that require cache.

They have background scripts that duplicate what the database used to do. Check for integrity constraints, references.

User table is unsharded. They just use a big database and have a uniqueness constraint on user name and email. Inserting a User will fail if it isn’t unique. Then they do a lot of writes in their sharded database.


过渡到sharding

过渡到sharding的过程伴随着一些特性被冻结xxxxxxx


他们得决定如何做shard。希望（sharding之后能）执行最少的查询，并且渲染一个页面时访问尽量少的数据库

（所以）移除了所有的MySQL join。 因为数据库的表可能只被load进特殊的部分，所以join不能执行成功。

加了大量的cache。基本上每个查询都被缓存了。

步骤看起来是这样：

1 DB + Foreign Keys + Joins

1 DB + Denormalized + Cache

1 DB + Read Slaves + Cache

Several functionally sharded DBs + Read slaves + Cache

ID sharded DBs + Backup slaves + cache


因为从库的延迟，早起的MySQL只读从库成了麻烦。一个读操作在从库上执行时，可能主库并没有将数据复制过来。这样导致看起来就像一条记录丢失了。开始考虑require cache

他们用后台脚本复制数据库过去常常做的（xxxxxx）。检查完整性约束，reference等


用户表没做Shard。仅仅用了一张大表来存放，并且在用户名和email两个字段做了唯一性约束 。 插入一个不唯一的用户名将会失败。他们在shard db上做了很多写操作











How To Shard?

Looked at Cassandra’s ring model. Looked at Membase. And looked at Twitter’s Gizzard.

Determined: the least data you move across your nodes the more stable your architecture.

Cassandra has a data balancing and authority problems because the nodes weren’t sure of who owned which part of the data. They decided the application should decide where the data should go so there is never an issue.

Projected their growth out for the next five years and presharded with that capacity plan in mind.

Initially created a lot of virtual shards. 8 physical servers, each with 512 DBs. All the databases have all the tables.

For high availability they always run in multi-master replication mode. Each master is assigned to a different availability zone. On failure the switch to the other master and bring in a new replacement node.

When load increasing on a database:

Look at code commits to see if a new feature, caching issue, or other problem occurred.

If it’s just a load increase they split the database and tell the applications to find the data on a new host.

Before splitting the database they start slaves for those masters. Then they swap application code with the new database assignments. In the few minutes during the transition writes are still write to old nodes and be replicated to the new nodes. Then the pipe is cut between the nodes.




如何做shard？


审视Cassandra的环模型。审视Membase， 审视Twitter的Gizzard。

坚定的一点：节点间数据移动越少，你的架构越稳定。

Canssdra由于节点间不知道谁拥有那部分数据，所以存在数据平衡性和授权的问题。他们觉得应用应该决定数据存在哪，所以这不是个问题。


为未来五年的发展做规划，提前对系统的容量做shard（in mind没翻译）


开始时创建了很多虚拟的shard。8台物理机，每个有512个DB。所有的DB有所有的表（？？？？）

他们为了高可用性而采用了multi-master的复制模型。每个master被赋予了不同的 availability zone。万一失败的话会选择一个新的替代节点。

当数据库上负载增加的时候：

看看最近的代码提交是否有增加新功能，影响cache命中，或者其他的问题。

如果仅仅是负载的增加，他们会将数据库split， 让应用去新的host上找数据

在splitdb之前，他们先启动这些master对应的slave。然后将应用的代码更新到使用新数据库。在迁移的几分钟内，写操作仍然写到旧的节点上去，然后才会被复制到新的节点。（过了一会之后）这个pipe会断掉。（这一段意思没翻译出来）






ID Structure

64 bits:

shard ID: 16 bits

type : 10 bits - Pin, Board, User, or any other object type

local ID - rest of the bits for the ID within the table. Uses MySQL auto increment.

Twitter uses a mapping table to map IDs to a physical host. Which requires a lookup. Since Pinterest is on AWS and MySQL queries took about 3ms, they decided this extra level of indirection would not work. They build the location into the ID.

New users are randomly distributed across shards.

All data (pins, boards, etc) for a user is collocated on the same shard. Huge advantage. Rendering a user profile, for example, does not take multiple cross shard queries. It’s fast.

Every board is collocated with the user so boards can be rendered from one database.

Enough shards IDs for 65536 shards, but they only opened 4096 at first, they’ll expand horizontally. When the user database gets full they’ll open up more shards and allow new users to go to the new shards.


ID的结构

64位：
shard ID：16位
类型：10位，--- Pin, Board, User,还是其他类型
local ID --- 剩下的38位， 作为一个表中的ID。 使用了MySQL的自动增加ID值

Twitter适应了映射表将ID映射到物理机器。这个过程需要一次查找。既然Pinterest是在AWS上，MySQL一次查询需要3ms， 他们觉得这一个额外的重定向层不是很有效。所以他们将location信息写在了ID里。


新用户随机的分布到shard上。

一个用户的所有的数据（pins, boards, etc） 被收集到同一个shared上。这是巨大的优点。举个例子，当渲染一个用户profile页面的时候，你不需要多次的跨shared查询。 这样做很快。

一个用户的所有的board被收集到一起，所以boards可以从一个数据库里取出来渲染。

shard ID支持65536个shard， 但是他们开始时仅仅用了4096个 ，他们将会（根据需要）水平扩展。当他们的用户数据库满了的时候，他们将会开更多的shard，将新的用户数据存在新的shard上。











Lookups

If they have 50 lookups, for example, they split the IDs and run all the queries in parallel so the latency is the longest wait.

Every application has a configuration file that maps a shard range to a physical host.

“sharddb001a”: : (1, 512)

“sharddb001b”: : (513, 1024) - backup hot master

If you want to look up a User whose ID falls into sharddb003a:

Decompose the ID into its parts

Perform the lookup in the shard map

Connect to the shard, go to the database for the type, and use the local ID to find the right user and return the serialized data.



查找

如果有50个查找，举个例子来说，将ID分解成小块部分， 并行的执行所有的查询， 那么延迟将会是那个最长的query（的等待时间）。

每个应用都是有一个配置文件，表明了shared range映射到哪些物理机器上去

“sharddb001a”: : (1, 512)

“sharddb001b”: : (513, 1024) - backup hot master

如果你想查找一个ID在sharddb003a机器上的用户：
将ID分解成部分

在shared map里执行查找

向shared发起连接，连接到那个类型的数据库，用local ID（译注：应该是ID分解后的一部分）找到对应的用户，然后返回序列化的数据


















Objects And Mappings

All data is either an object (pin, board, user, comment) or a mapping (user has boards, pins has likes).

For objects a Local ID maps to a MySQL blob. The blob format started with JSON but is moving to serialized thrift.

For mappings there’s a mapping table.  You can ask for all the boards for a user. The IDs contain a timestamp so you can see the order of events.

There’s a reverse mapping, many to many table, to answer queries of the type give me all the users who like this pin.

Schema naming scheme is noun_verb_noun: user_likes_pins, pins_like_user.

Queries are primary key or index lookups (no joins).

Data doesn’t move across database as it does with clustering. Once a user lands on shard 20, for example, and all the user data is collocated, it will never move. The 64 bit ID has contains the shard ID so it can’t be moved. You can move the physical data to another database, but it’s still associated with the same shard.

All tables exist on all shards. No special shards, not counting the huge user table that is used to detect user name conflicts.

No schema changes required and a new index requires a new table.

Since the value for a key is a blob, you can add fields without destroying the schema. There’s versioning on each blob so applications will detect the version number and change the record to the new format and write it back. All the data doesn’t need to change at once, it will be upgraded on reads.

Huge win because altering a table takes a lock for hours or days.  If you want a new index you just create a new table and start populating it. When you don’t want it anymore just drop it. (no mention of how these updates are transaction safe).



对象和映射

所有的数据不是对象（pin, board, user, comment），就是映射（用户有board，pin有like）

对象有一个Local ID对应到MySQL的blob。开始时blob的格式是JSON的，但是后来迁移到序列化的Thrift上。

对于映射来说有映射表。你可以请求一个用户的所有board。ID带有时间戳，所有你能获得事件（译注：指pin， like这些动作）的（时间）顺序

还有一个反向映射，为了响应诸如给我找出喜欢这个pin的所有用户之类query的多对多的表。

Schema的命名格式是 名词_动词_名词： user_likes_pins, pins_like_user.

查询是有主键或者索引的查询。（没有join）

数据并不像clustering上那样在不同db间移动。举个例子，当一个用户在shard 20上出现，所有这个用户的数据都（会在shared 20上）不再移动。64位的ID包含了shared的ID，所以不能被移动。你可以移动物理数据到其他的数据库，但是他仍然和相同的shared关联（译的不好这一句）

所有的数据表在所有的shared上都有（一份？）。没有特殊的shared，不包括那张巨大来防止用户名冲突的用户表。

数据库的schema不需要被改动，新的索引只在新表上建立。

既然key对应的value是一个blob，你可以在不损坏schema的前提下随意增加字段。blob有版本号，所以应用程序需要检查版本号，将记录改成最新的格式再写回。所有的数据都不需要立马修改，它只在读的时候升级（这句话翻得不好）


巨大的成功来源于避免了在修改表的时候需要加锁长达数小时甚至几天。如果你想要一个新的索引，那么只需要创建一个新表并且填入数据。如果你再也不用这个表，将它drop掉就行，（不必关心这些update操作是否是事务安全）。
 













Rendering A User Profile Page

Get the user name from the URL. Go to the single huge database to find the user ID.

Take the user ID and split it into its component parts.

Select the shard and go to that shard.

SELECT body from users WHERE id = <local_user_id>

SELECT board_id FROM user_has_boards WHERE user_id=<user_id>

SELECT body FROM boards WHERE id IN (<boards_ids>)

SELECT pin_id FROM board_has_pins WHERE board_id=<board_id>

SELECT body FROM pins WHERE id IN (pin_ids)

Most of the calls are served from cache (memcache or redis), so it’s not a lot of database queries in practice.


渲染一个用户的Profile页面

从URL里面获取用户名。去打的数据库里面找用户的ID

将用户ID分解成好几个部分：


Select the shard and go to that shard.

SELECT body from users WHERE id = <local_user_id>

SELECT board_id FROM user_has_boards WHERE user_id=<user_id>

SELECT body FROM boards WHERE id IN (<boards_ids>)

SELECT pin_id FROM board_has_pins WHERE board_id=<board_id>

SELECT body FROM pins WHERE id IN (pin_ids)

大多数的调用是从cache（Memcache或者REdia）里面获取，所以实际环境里并没有太多的数据库查询












Scripting

When moving to a sharded architecture you have two different infrastructures, one old,


the unsharded system, and one new, the sharded system. Scripting is all the code to transfer the old stuff to the new stuff.

They moved 500 million pins and 1.6 billion follower rows, etc

Underestimated this portion of the project. They thought it would take 2 months to put data in the shards, it actually took 4-5 months. And remember, this was during a feature freeze.

Applications must always write to both old and new infrastructures.

Once confident that all your data is in the new infrastructure then point your reads to the new infrastructure and slowly increase the load and test your backends.

Built a scripting farm. Spin up more workers to complete the task faster. They would do tasks like move these tables over to the new infrastructure.

Hacked up a copy of Pyres, a Python interface to Github’s Resque queue, a queue on built on top of redis. Supports priorities and retries. It was so good they replaced Celery and RabbitMQ with Pyres.

A lot of mistakes in the process. Users found errors like missing boards. Had to run the process several times to make sure no transitory errors prevented data from being moved correctly.



脚本

当迁移到shared架构的时候，你会有两套系统：一个旧的没有shared系统，一个新的shared系统。脚本就是用来将旧机器（上的数据？）迁移到新机器的代码。

他们移动了500 million的pins和16亿行的follow数据库

他们低估了项目中这个部分（的复杂度）。他们本来计划2个月时间来把数据放到shard上，但实际上花了4-5个月。记住，这是在一个特征冻结的时候操作的（xxxxxxxxxxxxxxxxxxxx

应用数据必须同时写回新旧两套系统里。

确保所有数据写到新系统之后， 从新系统执行读操作， 慢慢的增大负载，测试你的后台（能否支撑）

（这一段不好翻译）

Pyres----Github基于Redis搭建的请求队列的一个python接口， 被他们使用并且hack了。支持优先级和出错重试。他如此好用以至于替代了Celery和RabbitMQ

这中间犯过很多错。勇士（访问网站过程中）能够发现诸如丢失board之类的错误。必须把前一流程运行好几次，保证没有迁移错误以防止直接移动过程中的数据丢失。


















Development

Initially tried to give developers a slice of the system. Each having their own MySQL server, etc, but things changed so fast this didn’t work.

Went to Facebook’s model where everyone has access to everything. So you have to be really careful.


部署

刚开始给了开发者每人一部分的系统权限。每个都有他们自己的MySQL数据库等。但是发展太快导致这种方法不奏效

学习了Facebook的模式，每个人都有整个系统的所有权限。所以你需要特别的小心。










Future Directions

Service based architecture.

As they are starting to see a lot of database load they start to spawn a lot of app servers and a bunch of other servers. All these servers connect to MySQL and memcache. This means there are 30K connections on memcache which takes up a couple of gig of RAM and causes the memcache daemons to swap.

As a fix these are moving to a service architecture. There’s a follower service, for example, that will only answer follower queries. This isolates the number of machines to 30 that access the database and cache, thus reducing the number of connections.

Helps isolate functionality. Helps organize teams around around services and support for those services. Helps with security as developer can’t access other services.


未来的方向

基于服务的架构


















Lessons Learned

It will fail. Keep it simple.

Keep it fun. There’s a lot of new people joining the team. If you just give them a huge complex system it won’t be fun. Keeping the architecture simple has been a big win. New engineers have been contributing code from week one.

When you push something to the limit all these technologies fail in their own special way.

Architecture is doing the right thing when growth can be handled by adding more of the same stuff. You want to be able to scale by throwing money at the problem by throwing more boxes at the problem as you need them. If you are architecture can do that, then you’re golden.

Cluster Management Algorithm is a SPOF. If there’s a bug it impacts every node. This took them down 4 times.

To handle rapid growth you need to spread data out evenly to handle the ever increasing load.

The least data you move across your nodes the more stable your architecture. This is why they went with sharding over clusters.

A service oriented architecture rules. It isolates functionality, helps reduce connections, organize teams, organize support, and  improves security.

Asked yourself what your really want to be. Drop technologies that match that vision, even if you have to rearchitecture everything.

Don’t freak out about losing a little data. They keep user data in memory and write it out periodically. A loss means only a few hours of data are lost, but the resulting system is much simpler and more robust, and that’s what matters.


学习到的经验

（任何部件）都会宕机。 保持架构简单。

保持做的事情有趣。很多新人会加入团队。如果你仅仅是给他们一个巨大的复杂的系统，那就不那么有趣了。保持架构的简单性就是一个巨大的成功。新来的工程师可以从第一周就贡献代码。

如果你对一些东西做出限制，所有的技术都会以不同的方式失败。

（下面这3段在以前出现过）

机器节点之间数据移动的越少，你的架构越稳定。这就是为什么他们总是在cluster上分区分片

面向服务的架构规则。做到功能上隔离，减少（不同机器之间）连接，搭建团队， organize support， 提高安全性。

思考一下你真正想要什么。（下一句有问题）


不要害怕丢失一点数据。Pinterest把数据放在内存，并且阶段性的（写到磁盘）。一次丢失数据意味着仅仅会丢失几个小时的数据，但是整个系统会更加简洁和鲁棒。这是最重要的


