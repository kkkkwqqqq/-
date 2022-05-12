# Redis

内存和持久化疑惑,参照博客  https://blog.csdn.net/tr1912/article/details/70197085

补充：查询数据时先从缓存查询，若没有，去数据库中查询缓存到内存中，并返回查询数据！！！

## Nosql概述

### 为什么要用Nosql

> 1.单机MySql年代       DAL（放sql）

![image-20220117102701606](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220117102701606.png)

90年代，一个基本的网站访问量一般不会太大，单个数据库完全足够！！！

那个时代，更多的是去使用HTML这些静态网页~服务器没什么压力！！！==（直接跳转，没有什么业务走到java程序中）==

思考一下，这种情况下：整个网站的瓶颈是什么？

1. 如果数据量太大，一个机器放不下！
2. 当数据达到一定程度，查询变慢。需要建立数据的索引(B+ Tree)，一个机器内存也放不下(索引占用内存)
3. 访问量（读写混合），一个服务器承受不了！！

`只要出现以上三种情况之一，那么你就必须晋级。`



> 2.Memcached(缓存)+MySQL+==垂直拆分(读写分离)== 年代

![image-20220117105517547](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220117105517547.png)

`网站80%的情况下都是在读。每次都要去查询数据库就十分麻烦！所以说我们希望减轻数据库的压力，可以使用缓存来保证效率`

![image-20220117110017773](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220117110017773.png)

优化过程经历了以下几个过程：

1. 优化数据库的数据结构和索引(难度大)
2. 文件缓存，通过IO流获取比每次都访问数据库效率略高，但是流量爆炸式增长时候，IO流也承受不了
3. `MemCache,当时最热门的技术，通过在数据库和数据库访问层之间加上一层缓存，第一次访问时查询数据库，将结果保存到缓存，后续的查询先检查缓存，若有直接拿去使用，效率显著提升。`

> 3.分库分表+水平拆分+MySQL集群

本质：==解决数据库（读写）的问题==

早些年Mysql的==MyISAM:表锁==（假设100w条数据，要查某条记录 就会将表锁起来，高并发场景下别的访问就被拒之门外）

转战Mysql的==Innodb：行锁==（之锁当前操作的那条记录） 更适合高并发场景

==微服务开发下：==

`订单、用户、支付....分别建库。如下图类似操作。` 解决使用分库分表来解决写的压力！！==Mysql的集群很好的满足了那个年代的所有需求！！！==

![image-20220117114206082](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220117114206082.png)

==两个集群存放不同的数据，用到的时候根据一定的机制去其中一个集群中找。一个集群三个mysql，单个mysql压力就降低了！！！==

> 4.如今的时代

如今信息量井喷式增长，各种各样的数据出现（用户定位数据，图片数据,热榜等）

==redis等非关系型数据库（NoSql）为什么会出现？==

`MySQL等关系型数据库就不够用了！数据量很多，变化很快~  比如热榜，不可能随时去修改MySql，这样的话压力太大了。我们可以先把数据放入非关系型数据库的缓存中，定时刷新Mysql的数据，这样就完美减少压力。` 

MySQL有的使用它来存储一些较大的文件，博客，图片！`数据库表很大，效率就低了！`如果有一种数据库来专门处理这种数据，MySQL的压力就变得十分小(研究如何处理这些问题！)

> 目前一个基本的互联网项目!!!

![image-20220117115150851](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220117115150851.png)

> 为什么要用NoSQL！

用户的`个人信息`，`社交网络`，`地理位置`。用户自己产生的数据，用户日志等等爆发式增长！

这时候我们就需要使用`NoSQL数据库`的，Nosql可以很好的处理以上的情况！

### 什么是NoSQL

NoSQL = Not Only SQL（不仅仅是SQL）

`关系型数据库：列+行，同一个表下数据的结构是一样的`。

`NoSQL泛指非关系型数据库`，随着web2.0互联网的诞生，`传统的关系型数据库`很难对付web2.0时代！`尤其是超大规模的高并发的社区`，暴露出来很多难以克服的问题(比如前面说的热榜)，NoSQL在当今大数据环境下发展的十分迅速，`Redis是发展最快的`(当然可以解决热榜的问题)，而且是我们当下必须要掌握的一个技术！

==很多数据类型用户的`个人信息`，`社交网络`，`地理位置`。这些数据类型的存储不需要一个固定的格式(`也就是数据库中表的字段`)！不需要多余的操作就可以横向扩展的！！`Map<String,Object>`使用键值对来控制==



> NoSQL特点

1.方便扩展（数据之间没有关系，很好扩展！）`类似Map<String,Object>，Object里面可以放集合实现扩展`

2.大数据量高性能（Redis一秒可以写8万次，读11万次，NoSQL的缓存记录级，是一种细粒度的缓存，性能会比较高！）

3.`数据类型是多样型的`！（`不需要事先设计数据库，随取随用!`）

4.传统的 RDBMS 和 NoSQL

```
传统的 RDBMS(关系型数据库)
- 结构化组织       通俗来说就是行表格式 数据要满足存储结构要求
- SQL
- 数据和关系都存在单独的表中 row col
- 操作，数据定义语言
- 严格的一致性         针对事务而言    
- 基础的事务
- ...
```

```
Nosql                              回头
- **不仅仅是数据**
- 没有固定的查询语言
- 键值对存储（redis），列存储，文档存储，图形数据库（社交关系）
- 最终一致性 
- **CAP定理和BASE**
- 高性能(响应快)，高可用(并发)，高扩展(类似Map集合的数据存储)
- ...
```

最终一致性详解：

> 最终一致性是弱一致性的一种特例。假如A首先write了一个值到存储系统，存储系统保证如果在A,B,C后续读取之前没有其它写操作更新同样的值的话，最终所有的读取操作都会读取到最A写入的最新值

> 了解：3V+3高

大数据时代的3V：主要是描述问题的

1. 海量Velume
2. 多样Variety
3. 实时Velocity

大数据时代的3高：主要是对程序的要求

1. 高并发

2. 高可扩

3. 高性能（保证用户体验和性能!!!）

   

`真正在公司中的实践:NoSQL+RDBM一起使用才是最强的`



### 思考问题：

下面这么多的的信息都是在一个数据库中的吗（图片，视频，用户评价等等）？

==当然不是，图片用一个种类的数据库存，视频用其他种类的数据库存，评价用其他种类的数据库存==

![image-20220117154903575](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220117154903575.png)

![image-20220117154917937](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220117154917937.png)

![image-20220117154935604](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220117154935604.png)

`存储商品信息的一般用到多种数据库`

```1
# 1、商品的基本信息
	名称、价格、商家信息；
	关系型数据库就可以解决！MySQL/Oracle 
	淘宝内部的MySQL不是大家用的MySQL 是改过的
	
# 2、商品的描述，评论（文字较多）
	文档型数据库，MongoDB
	
# 3、图片
	分布式文件系统 FastDFS
	- 淘宝自己的 TFS
	- Gooale的  GFS
	- Hadoop的  HDFS
	- 阿里云的   oss
	
# 4、商品的关键字(搜索)       具体可以参考这篇博客https://blog.csdn.net/m0_45946298/article/details/108211097
	-搜索引擎 sola elasticsearch
	-ISerach       
	
# 5、商品热门波段信息（热搜等）
	-内存数据库
	-Redis Tair、Memache...
	
# 6、商品的交易，外部的支付接口
	-第三方应用
```

`要知道，一个简单的网页背后的技术一定不是大家所想的那么简单！`

### 阿里巴巴演进分析

![image-20220117204751370](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220117204751370.png)

![image-20220117204818895](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220117204818895.png)

==加一层类似jdbc这样，屏蔽底层差异。==

### NoSQL的四大分类

> KV键值对

- 新浪：==Redis==
- 美团：Redis + Tair
- 阿里、百度：Redis + Memcache

> 文档型数据库（`bson`数据格式）：

- ==MongoDB==(掌握)
  - 基于分布式文件存储的数据库。C++编写，用于`处理大量文档`。
  - MongoDB是RDBMS和NoSQL的`中间产品`。`MongoDB是非关系型数据库中功能最丰富的，NoSQL中最像关系型数据库的数据库`。
- ConthDB

> 列存储数据库

- ==HBase(大数据必学)==
- 分布式文件系统

> 图关系数据库

用于`广告推荐`，`社交网络(推荐好友)`

- ==Neo4j==、InfoGrid

它不是存图形，放的是关系，比如：朋友圈社交网络，广告推荐！

> 不同种类的NoSQL对比

| 分类                | Examples举例                                       | 典型应用场景                                                 | 数据模型                                            | 优点                                                         | 缺点                                                         |
| ------------------- | -------------------------------------------------- | ------------------------------------------------------------ | --------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 键值对（key-value） | Tokyo Cabinet/Tyrant, Redis, Voldemort, Oracle BDB | ==内容缓存，主要用于处理大量数据的高访问负载，====也用于一些日志系统等等。== | ==Key 指向 Value 的键值对，通常用hash table来实现== | ==查找速度快==                                               | ==数据无结构化，通常只被当作字符串或者二进制数据==           |
| 列存储数据库        | Cassandra, HBase, Riak                             | 分布式的文件系统                                             | 以列簇式存储，==将同一列数据存在一起==              | ==查找速度快，可扩展性强，更容易进行分布式扩展==             | ==功能相对局限==                                             |
| 文档型数据库        | CouchDB, MongoDb                                   | ==Web应用（与Key-Value类似，Value是结构化的，不同的是数据库能够了解Value的内容）== | Key-Value对应的键值对，==Value为结构化数据==        | 数据结构要求不严格，表结构可变，不需要像关系型数据库一样需要预先定义表结构 | 查询性能不高，而且缺乏统一的查询语法。                       |
| 图形(Graph)数据库   | Neo4J, InfoGrid, Infinite Graph                    | ==社交网络，推荐系统等。专注于构建关系图谱==                 | 图结构                                              | 利用图结构相关算法。比如最短路径寻址，N度关系查找等          | 很多时候需要对整个图做计算才能得出需要的信息，而且这种结构不太好做分布式的集群 |

## Redis入门

### 概述

> Redis是什么

Redis（==R==emote ==D==ictionary ==S==erver )，即远程字典服务。

是一个开源的使用ANSI C语言编写、==支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。==

==与memcached一样，为了保证效率，数据都是缓存在内存中==。==区别的是redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave(主从)同步。==

免费和开源！是当下最热门的NoSQL技术之一！也被人们称为`结构化数据库`！

> Redis能该干什么？

1. 内存存储、持久化，内存是断电即失的，所以需要持久化（RDB、AOF）
2. 高效率、用于高速缓冲
3. 发布订阅系统
4. 地图信息分析
5. 计时器、计数器(eg：浏览量)
6. .......

> 特性

1. 多样的数据类型

2. 持久化

3. 集群

4. 事务

   …

> ### 学习中需要用到的东西

1. 官网：https://redis.io/
2. redis中文官方网站: http://www.redis.cn
3. Window版本在GitHub上下载

==推荐使用Linux服务器学习。windows版本的Redis已经停更很久==



### Window安装Redis

csdn查win下安装redis即可！！！

在redis目录下cmd输入： **redis-server.exe redis.windows.conf**启动服务

![image-20220117214149256](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220117214149256.png)

然后在redis目录下双击redis -cli，打开客户端连接redis

![image-20220117214207565](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220117214207565.png)

![image-20220117214222129](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220117214222129.png)



> 说明：redis不需要建表即可插入数据，底层存储是Map集合

![image-20220117214510227](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220117214510227.png)

`记住一句话，Win下使用确实简单，但是Redis推荐我们使用Linux环境下去开发使用`官网说的

官网对Redis的介绍：

> ```
>  Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件. 它支持多种类型的数据结构，如 字符串（strings）， 散列（hashes）， 列表（lists）， 集合（sets）， 有序集合（sorted sets） 与范围查询， bitmaps， hyperloglogs 和 地理空间（geospatial） 索引半径查询. Redis 内置了 复制（replication）， LUA脚本（Lua scripting）， LRU驱动事件（LRU eviction）， 事务（transactions） 和不同级别的 磁盘持久化（persistence）， 并通过 Redis哨兵（Sentinel） 和自动 分区（Cluster）提供高可用性（high availability）.
> 
>     你可以对这些类型执行 原子操作 ， 列如： 字符串（strings）的append 命令; 散列（hashes）的hincrby命令; 列表（lists）的lpush命令; 集合（sets）计算交集sinter命令， 计算并集union命令 和 计算差集sdiff命令; 或者 在有序集合（sorted sets）里面获取成员的最高排名zrangebyscore命令.
> 
>   为了实现其卓越的性能， Redis 采用运行在 内存中的数据集工作方式. 根据您的使用情况， 您可以每隔一定时间将 数据集导出到磁盘， 或者 追加到命令日志中. 您也可以关闭持久化功能，将Redis作为一个高效的网络的缓存数据功能使用.
> 
>     Redis 同样支持 主从复制（能自动重连和网络断开时自动重新同步），并且第一次同步是快速的非阻塞式的同步.
> 
> 其他功能包括:
> 
> 事务（Transactions）
> 订阅分发（Pub/Sub）
> LUA脚本（Lua scripting）
> 过期自动删除key
> 内存回收
> 自动故障转移
> 您可以使用 大多数的编程语言 来使用Redis.
> 
>     Redis 使用 ANSI C 编写并且能在绝大Linux系统上运行，基于BSD协议，对OS X没有外部依赖. 我们支持Linux 和 OS X两种系统的开发和测试，我们推荐使用Linux部署. Redis 可以像SmartOS一样运行在Solaris系统中， 但是我们会最大力度的支持它. 官方不支持Windos版本的Redis,但微软开发和维护着支持win-64 的Redis版本.
> ```

![image-20220117220025933](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220117220025933.png)

![image-20220117215625438](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220117215625438.png)

### Linu安装Redis

> 安装步骤

1.redis官网下载安装包：`redis-6.2.6.tar.gz`

> 统一规范，所有环境我都装在`/home/kwq`目录下，`比如jdk那些....`

![image-20220119192236208](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119192236208.png)

2.将redis安装包移动到`/opt`目录下(与此同时/kwq/home下的安装包就消失了)，解压Redis安装包！

> 程序都建议放在`/opt`目录下

操作：先切到kwq目录 cd /home/kwq

然后执行命令：mv redis-6.2.6.tar.gz /opt

进入opt目录 ： cd /opt

解压：tar -zxvf redis-6.2.6.tar.gz

ls查看目录

![image-20220119193055766](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119193055766.png)

进入解压后的文件: cd redis-6.2.6 可以看到我们redis的配置文件  `下图红框圈起来的即为redis的配置文件`

![image-20220119193225904](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119193225904.png)

疑问？为什么没有启动服务redis-server.exe在这解压文件中？ 



3.执行命令：yum -y install gcc gcc-c++ autoconf pcre pcre-devel make automake `用来进行基本的环境安装，安装gcc环境` 

查看gcc环境是否安装好 ： gcc -v  出现下列情况说明安装好了

![image-20220119193709742](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119193709742.png)

紧接着执行make命令 执行完后，`程序就多了redis`

最后执行make install来判断是否make命令 安装完毕 `出现下图三个圈起来的东西就说明安装完毕`

![image-20220119194103951](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119194103951.png)

4.Redis的默认安装路径`usr/local/bin`

​	`我们自己安装的本地程序都默认安装在usr/local/bin中`

![image-20220119194912903](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119194912903.png)

5.将redis配置文件拷贝一份在`usr/local/bin`目录下

先在`usr/local/bin`新建一个文件夹   mkdir kwqconfig 然后执行拷贝命令 cp /opt/redis-6.26/redis.conf kwqconfig

![image-20220119195250273](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119195250273.png)

6.redis默认不是后台启动的，==默认是前台启动的（命令行窗口不能关闭，否则服务器停止==）`修改配置文件daemonize的默认值为yes`,

![image-20220123195440306](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220123195440306.png)

![image-20220119195803206](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119195803206.png)

==任何安装在远程服务器的应用程序都应该设置密码  不然很容易被攻击以致于要重装系统==

redis设置密码方式可参照这篇博客：

[redis设置密码_jattxgt的博客-CSDN博客_redis加密码](https://blog.csdn.net/qq_38555490/article/details/94336297)

想要连接云服务器上的redis，需要参考此博客：

(https://blog.csdn.net/qq_44624536/article/details/120213607)

8.使用以下redis-cli进行连接测试！！！

> 说明一下，`redis-cli命令等价于redis-cli -h 主机ip -p redis端口号 主机ip和redis端口号默认就是配置文件中的 `类似mysql一样可以连上任意的redis数据库，

![image-20220123172835227](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220123172835227.png)

9.查看redis进程是否开启! 

`指令ps -ef | grep redis 查看进程 ,出现下面两个圈就表示开启了`

![image-20220119200751061](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119200751061.png)





10.如何关闭redis服务呢?

`shutdown`命令就行了

![image-20220119201126325](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119201126325.png)

==再次查看进程==

发现服务关闭了

![image-20220119200953456](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119200953456.png)

11.`后面我们会使用单机多Redis启动集群测试！！！`



### 测试性能

**redis-benchmark** 是一个压力测试工具

![image-20220119201539160](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119201539160.png)

Redis官方提供的性能测试工具！

redis-benchmark命令参数：

图片来自菜鸟教程：

![img](https://img-blog.csdnimg.cn/20200513214125892.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

```
# 测试：100个并发连接 100000 请求
redis-benchmark -h localhost -p 6379 -c 100 -n 100000
```

![image-20220119222053528](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119222053528.png)

如何查看这些分析呢？

![image-20220119221320906](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119221320906.png)

### 基础的知识

==redis默认有16个数据库，打开redis的配置文件：redis.conf可以看到==

![image-20220119222950265](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119222950265.png)

==默认使用的是第0个==

可以使用`select`切换当前数据库

```
127.0.0.1:6379> select 3       #切换到3号数据库
OK
127.0.0.1:6379[3]> dbsize      #当前数据库的大小
(integer) 0 
127.0.0.1:6379[3]> set name qinjiang     #设置一个k-v对
OK 
127.0.0.1:6379[3]> dbsize             #当前数据库的大小
(integer) 1
127.0.0.1:6379[3]> select 7        #切换到7号数据库
OK
127.0.0.1:6379[7]> dbsize      
(integer) 0
127.0.0.1:6379[7]> get name         #获取key为name对应的value
(nil)
127.0.0.1:6379[7]> select 3
OK
127.0.0.1:6379[3]> get name
"qinjiang"
127.0.0.1:6379[3]> keys *        #查看当前数据库所有的key
1) "name"
127.0.0.1:6379[3]> flushdb        #清除当前数据库
OK
127.0.0.1:6379[3]> keys *
(empty array)
```

清除当前数据库：`flushdb`

清除全部数据库的内容：`flushdball`

> Redis是单线程的！

==官方表示：==Redis是`基于内存操作`，`CPU不是Redis性能瓶颈，而是机器内存和网络带宽`。

既然可以使用单线程来实现，就使用单线程了！所以就使用了单线程了！

`Redis是C语言写的，官方提供的数据为100000+的QPS，完全不必同样是使用K-V的Memecache差!`

> **Reids**为什么是单线程的还那么快？

- 误区1：高性能的服务器一定是多线程的
- 误区2：多线程（CPU上下文会切换！,切换占用一定的时间）一定比单线程效率高！

要对CPU>内存>硬盘的速度要有所了解 ==狂神说juc有讲，学完redis去看看==

==核心：Redis是将所有的数据放在内存中的，所以说使用单线程去操作效率就是最高的，多线程（CPU上下文会切换：耗时的操作！），对于内存系统来说，如果没有上下文切换效率就是最高的，多次读写都是在一个CPU上的，在内存情况下，单线程就是最佳的方案。==



## 五大数据类型

> 官网文档：

![image-20220119232139537](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119232139537.png)

> 笔记的所有命令一定要记住！！！！，后面我们使用SpringBoot，Jedis，所有的方法就是这些命令

### Redis-Key

```java
127.0.0.1:6379> flushall 
OK
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> set name kwq
OK
127.0.0.1:6379> keys *
1) "name"
127.0.0.1:6379> set age 1
OK
127.0.0.1:6379> keys *
1) "age"
2) "name"
127.0.0.1:6379> exists name       #判断当前的key是否存在
(integer) 1
127.0.0.1:6379> exists name1
(integer) 0
127.0.0.1:6379> move name 1          #将当前的key移动到其他1号数据库
(integer) 1
127.0.0.1:6379> keys *
1) "age"
127.0.0.1:6379> set name kwq
OK
127.0.0.1:6379> keys *
1) "age"
2) "name"
127.0.0.1:6379> get name
"kwq"
127.0.0.1:6379> expire name 10       #设置key的过期时间
(integer) 1
127.0.0.1:6379> ttl name               #查看当前key的剩余时间 实际开发用来做热点数据过期
(integer) 5
127.0.0.1:6379> ttl name
(integer) 4
127.0.0.1:6379> ttl name
(integer) 3
127.0.0.1:6379> ttl name
(integer) 2
127.0.0.1:6379> get name
(nil)
127.0.0.1:6379> set name kwq
OK
127.0.0.1:6379> keys *
1) "age"
2) "name"
127.0.0.1:6379> type name           #查看当前key的一个类型!
string
127.0.0.1:6379> type age
string
```

==后面如果遇到不会的命令，可以在官网查看帮助文档==

![image-20220119235053763](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119235053763.png)

随便点开一看：

![image-20220119235107292](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119235107292.png)

![image-20220119235116279](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220119235116279.png)

`看一眼例子也就会了！！！`

### String字符串

> 90%的 java程序员使用 redis 只会使用一个String类型！

```
*********************************************************************
127.0.0.1:6379> set key1 v1         #设置值
OK
127.0.0.1:6379> get key1         	#获取值
"v1"
127.0.0.1:6379> append key1 hello          #追加字符串，如果当前key不存在，相当于set key
(integer) 7
127.0.0.1:6379> get key1
"v1hello"
127.0.0.1:6379> strlen key1
(integer) 7
127.0.0.1:6379> append key1 ",keweiqin"
(integer) 16
127.0.0.1:6379> strlen key1       #获取字符串的长度
(integer) 16
127.0.0.1:6379> get key1       
"v1hello,keweiqin"
127.0.0.1:6379> append name "zhangsan"        # 追加字符串，如果当前key不存在，相当于set key
(integer) 8
127.0.0.1:6379> keys *
1) "key1"
2) "name"
127.0.0.1:6379> 
*********************************************************************

# i++
# 步长 i+=

127.0.0.1:6379> set views 0    #初始化浏览器为0
OK
127.0.0.1:6379> get views
"0"
127.0.0.1:6379> incr views     #自增1 浏览量+1
(integer) 1
127.0.0.1:6379> incr views
(integer) 2
127.0.0.1:6379> get views
"2"
127.0.0.1:6379> decr views    #自减1 浏览量-1
(integer) 1
127.0.0.1:6379> decr views    
(integer) 0
127.0.0.1:6379> decr views
(integer) -1
127.0.0.1:6379> get views
"-1"
127.0.0.1:6379> incrby views 10  #可以设置步长，指定增量！
(integer) 9
127.0.0.1:6379> incrby views 10
(integer) 19
127.0.0.1:6379> decrby views 5  #可以设置步长，指定减量！
(integer) 14
*********************************************************************
截取字符串 getrange
127.0.0.1:6379> set key1 "keweiqinhaoshuai"   #设置key1的值
OK
127.0.0.1:6379> get key1
"keweiqinhaoshuai"
127.0.0.1:6379> GETRANGE key1 0 3            #截取字符串 [0,3]
"kewe"
127.0.0.1:6379> getrange key1 0 -1           #获取全部字符串 和 get key是一样的
"keweiqinhaoshuai"

************************************************************************
# 替换 setrange
127.0.0.1:6379> set key2 abcdefg
OK
127.0.0.1:6379> get keys
(nil)
127.0.0.1:6379> get key2
"abcdefg"
127.0.0.1:6379> setrange key2 1 xx       #替换指定位置开始的字符串
(integer) 7
127.0.0.1:6379> get key2
"axxdefg"
************************************************************************
# setex(set with expire)  #设置过期时间   和set key value + expire key second两者一起用一致
# setnx(set if not exist)  #不存在再设置 （分布式所中会常常使用!）

127.0.0.1:6379> setex key3 30 "hello"      #设置key3 的值为hello，30s后过期
OK
127.0.0.1:6379> ttl key3
(integer) 26
127.0.0.1:6379> ttl key3
(integer) 24
127.0.0.1:6379> get key3
"hello"
127.0.0.1:6379> setnx mykey "redis"         #如果mykey 不存在，创建mykey
(integer) 1
127.0.0.1:6379> keys *
1) "key2"
2) "mykey"
3) "key1"
4) "key3"
127.0.0.1:6379> ttl key3
(integer) 1
127.0.0.1:6379> keys *
1) "key2"
2) "mykey"
3) "key1"
127.0.0.1:6379> ttl key3
(integer) -2
127.0.0.1:6379> setnx mykey "MongoDB" 		 #如果mykey 存在，创建失败
(integer) 0
127.0.0.1:6379> get mykey
"redis"


************************************************************************
# mset
# mget
# msetnx
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3   #同时设置多个值
OK
127.0.0.1:6379> mget k1 k2 k3           #同时获取多个值
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> msetnx k1 v1 k4 v4        # msetnex 是一个原子性操作，要么一起成功，要么一起失败！
(integer) 0
127.0.0.1:6379> get k4
(nil)
************************************************************************
#对象
set user:1 {name:zhangsan,age:3} #设置一个user:1 对象 值为json字符来保存一个对象！

#这里的key是一个巧妙的设计：user:{id}:{filed},如此的设计在redis中完全ok！
应用场景： set article:1000:views 0 这样表示第1000篇文章的浏览为0 那第10001,10002篇只需要改key中的1000即可，实现复用

127.0.0.1:6379> mset user:1:name zhangsan user:1:age 2
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "zhangsan"
2) "2"

************************************************************************
getset #先get然后set
127.0.0.1:6379> getset db redis   #如果不存在值，则返回nil
(nil)
127.0.0.1:6379> get db
"redis"
127.0.0.1:6379> getset db monogodb     #如果存在值，获取原来的值，并且设置新的值
"redis"
127.0.0.1:6379> get db
"monogodb"

```

String的类似使用场景：==value除了是我们的字符串还可以是我们的数字！==

- 计数器       ==decr和incr==
- 统计多单位的数量   ==get uid:347858300:follow==
- 粉丝数
- 对象缓存存储      ==用expire设置过期时间==

> incr和decr的使用场景，就是下图的阅读量。redis使用incr即可，这样就可以避免频繁的操作mysql，提高了效率

![image-20220120133550186](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220120133550186.png)

### List（可重复集合）

基本数据类型，列表

![image-20220120144846843](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220120144846843.png)

==在redis里面，我们可以把list玩成栈和队列==，`阻塞队列`

所以的list命令都是l开头的

```
###########################################################
127.0.0.1:6379> lpush list one       # 将一个值或者多个值，插入到列表头部（左）
(integer) 1
127.0.0.1:6379> lpush list two
(integer) 2
127.0.0.1:6379> lpush list three
(integer) 3
127.0.0.1:6379> lrange 0 -1
(error) ERR wrong number of arguments for 'lrange' command
127.0.0.1:6379> lrange list 0 -1         #从头部（左边）取出所有元素
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> lrange list 0 1          #通过区间获取特定的值
1) "three"
2) "two"
127.0.0.1:6379> rpush list right         # 将一个值或者多个值，插入到列表尾部（右）
(integer) 4
127.0.0.1:6379> rrange list 0 -1
(error) ERR unknown command `rrange`, with args beginning with: `list`, `0`, `-1`, 
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "two"
3) "one"
4) "right"
##########################################################
LPOP
RPOP

127.0.0.1:6379> lrange list 0 -1 
1) "three"
2) "two"
3) "one"
4) "right"
127.0.0.1:6379> lpop list       #移除list的第一个元素
"three"
127.0.0.1:6379> rpop list		#移除list的最后一个元素
"right"
127.0.0.1:6379> lrange list 0 -1 
1) "two"
2) "one"

###########################################################
Lindex

127.0.0.1:6379> lrange list 0 -1
1) "two"
2) "one"
127.0.0.1:6379> lindex list 1     # 通过下标获得list中的某一值
"one"
127.0.0.1:6379> lindex list 0
"two"
127.0.0.1:6379> rindex list 0
(error) ERR unknown command `rindex`, with args beginning with: `list`, `0`, 

###########################################################
Llen 返回列表的的长度

127.0.0.1:6379> lpush list one
(integer) 3
127.0.0.1:6379> lpush list two
(integer) 4
127.0.0.1:6379> lpush list three
(integer) 5
127.0.0.1:6379> llen list             #Llen 返回列表的的长度
(integer) 5
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "two"
3) "one"
4) "two"
5) "one"

###########################################################

移除指定的值！
csdn取关的实现 uid
lren

127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "two"
3) "one"
4) "two"
5) "one"
127.0.0.1:6379> lrem list 1 one       #移除list集合中指定个数的value，精确匹配
(integer) 1
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "two"
3) "two"
4) "one"
127.0.0.1:6379> lpush list three
(integer) 5
127.0.0.1:6379> lrem list 2 three     #移除list集合中指定个数的value，精确匹配
(integer) 2
127.0.0.1:6379> lrange list 0 -1
1) "two"
2) "two"
3) "one"

###########################################################
trim 修剪 list 截断！

127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> rpush mylist "hello"
(integer) 1
127.0.0.1:6379> rpush mylist "hello1"
(integer) 2
127.0.0.1:6379> rpush mylist "hello2"
(integer) 3
127.0.0.1:6379> rpush mylist "hello3"
(integer) 4
127.0.0.1:6379> ltrim mylist 1 2   #通过下标截取指定的长度，这个list已经被改变了，截断了只剩下截取的元素！
OK
127.0.0.1:6379> lrange mylis 0 -1
(empty array)
127.0.0.1:6379> lrange mylist 0 -1
1) "hello1"
2) "hello2"

###########################################################

rpoplpush #移除列表的最后一个元素，将他移动到新的列表中！

127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> rpush mylist "hello"
(integer) 1
127.0.0.1:6379> rpush mylist "hello1"
(integer) 2
127.0.0.1:6379> rpush mylist "hello2"
(integer) 3
127.0.0.1:6379> rpoplpush mylist myotherlist   #移除列表的最后一个元素，将他移动到新的列表中！
"hello2"
127.0.0.1:6379> lrange mylist 0 -1       #查看原来的列表
1) "hello"
2) "hello1"
127.0.0.1:6379> lrange myotherlist 0 -1      #查看目标列表 确实存在该值
1) "hello2"

###########################################################
lset 将列表中指定下标的值替换成另外一个值 更新操作

127.0.0.1:6379> exists list      #判断这个列表是否存在
(integer) 0
127.0.0.1:6379> lset list 0 item    #如果列表不存在更新就会报错
(error) ERR no such key
127.0.0.1:6379> lpush list value1   
(integer) 1
127.0.0.1:6379> lrange list 0 0
1) "value1"
127.0.0.1:6379> lset list 0 item   #如果列表存在，更新当前下标的值
OK
127.0.0.1:6379> lrange list 0 0
1) "item"
127.0.0.1:6379> lset list 1 other   #list不存在下标为1的值 所以更新失败
(error) ERR index out of range

###########################################################
linsert #将某个具体的value插入到列表中某个元素的前面或后面

127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> clear
127.0.0.1:6379> rpush mylist "hello"
(integer) 1
127.0.0.1:6379> rpush mylist "world"
(integer) 2
127.0.0.1:6379> linsert mylist before "world" "other"  #插入到world前面
(integer) 3
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "other"
3) "world"
127.0.0.1:6379> linsert mylist after world new    #插入到world后面
(integer) 4
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "other"
3) "world"
4) "new"
```

> 小结：

- ==list实际上是一个链表，before Node after , left, right 都可以插入值==
- 如果key不存在，则创建新的链表
- 如果key存在，新增内容
- 如果移除了所有值，空链表，也代表不存在
- 在两边插入或者改动值，效率最高！修改中间元素，效率相对较低

应用：

`消息排队！消息队列（Lpush Rpop）,栈（Lpush Lpop）`



### Set(无序不重复集合)

> set中的值不能重复（无序不重复集合），list可以!!!

```
###########################################################
127.0.0.1:6379> sadd myset "hello"        #向set集合添加元素 sadd
(integer) 1
127.0.0.1:6379> sadd myset "hello"       #不能添加重复的元素
(integer) 0
127.0.0.1:6379> sadd myset "kwq"
(integer) 1
127.0.0.1:6379> sadd myset "love kwq" 
(integer) 1
127.0.0.1:6379> sismember myset hello      #判断set集合中是否存在hello这个元素 sismember
(integer) 1
127.0.0.1:6379> sismember myset hello1
(integer) 0
127.0.0.1:6379> smembers myset     #查看指定set的所有值 smembers
1) "love kwq"
2) "kwq"
3) "hello"
###########################################################
127.0.0.1:6379> scard myset       #获取set集合中的元素个数   scard
(integer) 3

###########################################################
# srem  移除指定元素  
127.0.0.1:6379> srem myset hello     #移除指定元素  srem
(integer) 1
127.0.0.1:6379> smembers myset     
1) "love kwq"
2) "kwq"
127.0.0.1:6379> scard myset
(integer) 2

###########################################################
set 无序不重复集合。可以用来实现随机抽奖

127.0.0.1:6379> smembers myset
1) "kxh"
2) "love kwq"
3) "kws"
4) "lmz"
5) "kc"
6) "kwq"
7) "kxn"
127.0.0.1:6379> srandmember myset          #随机抽取一个元素
"kc"
127.0.0.1:6379> srandmember myset
"kc"
127.0.0.1:6379> srandmember myset
"kxh"
127.0.0.1:6379> srandmember myset
"kc"
127.0.0.1:6379> srandmember myset
"kxn"
127.0.0.1:6379> srandmember myset
"lmz"
127.0.0.1:6379> srandmember myset
"love kwq"
127.0.0.1:6379> srandmember myset
"kxn"
127.0.0.1:6379> srandmember myset 2      #随机抽取出set集合的两个元素
1) "kws"
2) "lmz"
127.0.0.1:6379> srandmember myset 2
1) "lmz"
2) "kxh"
127.0.0.1:6379> srandmember myset 2
1) "kws"
2) "kxh"

###########################################################
#spop 随机删除key

127.0.0.1:6379> smembers myset
1) "love kwq"
2) "kws"
3) "lmz"
4) "kc"
5) "kwq"
6) "kxn"
7) "kxh"
127.0.0.1:6379> spop myset      #随机删除一些set集合中的元素！
"kc"
127.0.0.1:6379> spop myset
"kws"
127.0.0.1:6379> spop myset
"kxh"
127.0.0.1:6379> smembers myset
1) "love kwq"
2) "lmz"
3) "kwq"
4) "kxn"
127.0.0.1:6379> 

###########################################################
#smove  将一个指定的值，移动到另外一个set集合！

127.0.0.1:6379> sadd myset "hello"
(integer) 1
127.0.0.1:6379> sadd myset "world"
(integer) 1
127.0.0.1:6379> sadd myset "kwq"
(integer) 1
127.0.0.1:6379> sadd myset2 "set2"
(integer) 1
127.0.0.1:6379> smove myset myset2 "kwq"   #将一个指定的值，移动到另外一个set集合！
(integer) 1
127.0.0.1:6379> smembers myset
1) "world"
2) "hello"
127.0.0.1:6379> smembers myset2
1) "set2"
2) "kwq"

###########################################################
微博，B站，共同关注！（并集）
数字集合类：
 - 差集 sdiff
 - 交集 sinter
 - 并集 sunion
127.0.0.1:6379> sadd key1 a
(integer) 1
127.0.0.1:6379> sadd key1 b
(integer) 1
127.0.0.1:6379> sadd key1 c
(integer) 1
127.0.0.1:6379> sadd key2 c
(integer) 1
127.0.0.1:6379> sadd key2 d
(integer) 1
127.0.0.1:6379> sadd key2 e
(integer) 1
127.0.0.1:6379> sdiff key1 key2       #差集
1) "a"
2) "b"
127.0.0.1:6379> sinter key1 key2     #交集   共同好友就可以这样实现
1) "c"
127.0.0.1:6379> sunion key1 key2      #并集
1) "a"
2) "b"
3) "c"
4) "e"
5) "d"

```

==微博，A用户将所有关注的人放在一个set集合中！ 将他的粉丝也放在一个set集合中，并集就表示互相关注==

==共同关注，共同爱好，二度好友，推荐好友就是这样来的！==



### Hash(哈希)

Map集合，形式是==key-map== 也就是说value是一个map集合！==本质和String类型没有太大的区别，只是一个简单的key-value==

```
###########################################################
127.0.0.1:6379> hset myhash field1 kwq     #set一个具体的key-value
(integer) 1
127.0.0.1:6379> hget myhash field1      #获取多个字段值 
"kwq"
127.0.0.1:6379> hmset myhash field1 hello field2 world   #set多个 key-value
OK
127.0.0.1:6379> hmget myhash field1 field2       #获取多个字段值 
1) "hello"
2) "world"
127.0.0.1:6379> hgetall myhash      #获取全部的数据
1) "field1"
2) "hello"
3) "field2"
4) "world"
127.0.0.1:6379> hdel myhash field1      #删除hash指定的key字段！对应的value值也就消失了
(integer) 1
127.0.0.1:6379> hgetall myhash      
1) "field2"
2) "world"
###########################################################
hlen  #获取hash表的字段数量

127.0.0.1:6379> hgetall myhash
1) "field2"
2) "world"
127.0.0.1:6379> hlen myhash
(integer) 1
127.0.0.1:6379> hmset myhash field1 1 field2 world
OK
127.0.0.1:6379> hgetall myhash
1) "field2"
2) "world"
3) "field1"
4) "1"
127.0.0.1:6379> hlen myhash    #获取hash表的字段数量
(integer) 2
127.0.0.1:6379> 

###########################################################
hexists  #判断hash中指定字符是否存在！
 
127.0.0.1:6379> hexists myhash field1     #判断hash中指定字段是否存在！
(integer) 1
127.0.0.1:6379> hexists myhash field3
(integer) 0

###########################################################
hkeys  #只获得所有的field
hvals   #只获得所有的value
127.0.0.1:6379> hkeys myhash     #只获得所有的field
1) "field2"
2) "field1"
127.0.0.1:6379> hvals myhash 	#只获得所有的value
1) "world"
2) "1"


###########################################################
incr    decr
127.0.0.1:6379> hset myhash field3 5    
(integer) 1    
127.0.0.1:6379> hincrby myhash field3 1   #指定增量
(integer) 6
127.0.0.1:6379> hincrby myhash field3 -1
(integer) 5
127.0.0.1:6379> hdecrby myhash field3 1
(error) ERR unknown command `hdecrby`, with args beginning with: `myhash`, `field3`, `1`, 
127.0.0.1:6379> hsetnx myhash field4 hello     #如果不存在则可以设置
(integer) 1
127.0.0.1:6379> hsetnx myhash field4 world     #如果存在则不可以设置
(integer) 0
127.0.0.1:6379> 


```

`总结`：

==用户信息之类的，经常变动的信息，用hash！hash更适合于对象的存储，String更适合字符串存储==

比如：

```
用Sting
127.0.0.1:6379> mset user:1:name zhangsan user:1:age 2
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "zhangsan"
2) "2"
###########################################################
用hash
127.0.0.1:6379> hset user:1 name keweiqin
(integer) 1
127.0.0.1:6379> hget user:1 name
"keweiqin"
```

### Zset(有序集合)

在set的基础上，增加了一个值，set k1 v1  zset  k1 score v1

```
###########################################################
zadd  #添加值进zset集合
127.0.0.1:6379> zadd myset 1 one          #添加一个值进zset集合
(integer) 1
127.0.0.1:6379> zadd myset 2 on 3 three         #添加多个值进zset集合
(integer) 2 
127.0.0.1:6379> zrange myset 0 -1     #将zset集合所有元素列出来==
1) "one"
2) "on"
3) "three"

###########################################################
排序如何实现
zrevrange： 从大到小排序
zrangebyscore: 从小到大排序
127.0.0.1:6379> zadd salary 2500 xiaohong         
(integer) 1
127.0.0.1:6379> zadd salary 5000 zhangsan
(integer) 1
127.0.0.1:6379> zadd salary 5000 keweiqin
(integer) 1
127.0.0.1:6379> zrangebyscore salary -inf +inf     #显示全部的用户，按工资从小到大
1) "xiaohong"
2) "keweiqin"
3) "zhangsan"
127.0.0.1:6379[2]> zrevrange salary 0 -1 
1) "zhangsan"
2) "keweiqin"
3) "xiaohong"

#语法： zrangebyscore key min max     实际写命令写成max min就会表示空集
127.0.0.1:6379> zrangebyscore salary 0 -1
(empty array)
127.0.0.1:6379> zrangebyscore salary +inf -inf
(empty array)
127.0.0.1:6379> zrangebyscore salary -inf 2500          #显示工资工资小于等于2500的用户，按工资从小到大
1) "xiaohong"
127.0.0.1:6379> zrangebyscore salary -inf 2500 withscores  #显示工资工资小于等于2500的用户，按工资从小到大，并且附带成绩
1) "xiaohong"
2) "2500"
127.0.0.1:6379> zrangebyscore salary (0 2500      #返回所有符合条件0 < score <= 2500的成员；
1) "xiaohong"
127.0.0.1:6379> zrangebyscore salary (0 (2500     #返回所有符合条件0 < score < 2500的成员；
(empty array)

###########################################################
# zrem 移除元素

127.0.0.1:6379> zrange salary 0 -1
1) "xiaohong"
2) "keweiqin"
3) "zhangsan"
127.0.0.1:6379> zrem salary xiaohong   #移除指定元素
(integer) 1
127.0.0.1:6379> zrange salary 0 -1     
1) "keweiqin"
2) "zhangsan"
127.0.0.1:6379> 

###########################################################
# zcary 获取有序集合中的个数

127.0.0.1:6379> zcard salary    # zcary 获取有序集合中的个数
(integer) 2
127.0.0.1:6379> 


###########################################################
zcount：获取指定区间的成员数量 

127.0.0.1:6379> zadd myset 1 hello
(integer) 1
127.0.0.1:6379> zadd myset 2 world 3 keweiqin
(integer) 2
127.0.0.1:6379> zcount myset 1 3   #zcount：获取指定区间的成员数量 
(integer) 3
127.0.0.1:6379> zcount myset 1 2
(integer) 2
127.0.0.1:6379> 

```

==其余的一些API，如果工作有需要，这个时候可以去看看官方文档，官方文档YYDS！==

案例思路： 

- set 排序 存储班级成绩表，工资表排序
- 带权重进行判断 普通消息为1  重要消息为2
- 排行榜应用实现，取TOP N；
- ![image-20220121115545693](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220121115545693.png)

> 命令不会用就去官网看说明和示例！再不行就百度。csdn...



## 三种特殊数据类型

### geospatial地理位置

==朋友的定位，附件的人，打车距离计算？==

Redis 的Geo 在Redis 3.2 版本就推出了！==这个功能可以推出地理位置的信息，两地之间的距离，方圆几里的人！！！==

可以查询一些测试数据：http://www.jsons.cn/lngcode

只有六个命令

![image-20220121133556117](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220121133556117.png)

官方文档：https://www.redis.net.cn/order/3685.html

> getadd 

![image-20220121135305857](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220121135305857.png)

```
# getadd  添加地理位置
# 规则：两级无法直接添加（超出范围了），我们一般会下载城市数据，直接通过java程序一次性导入！
127.0.0.1:6379> geoadd china:city 39.90 116.40 beijing
(error) ERR invalid longitude,latitude pair 39.900000,116.400000



127.0.0.1:6379> geoadd china:city 113.54 22.26 zhuhai
(integer) 1
127.0.0.1:6379> geoadd china:city 121.47 31.23 shanghai
(integer) 1
127.0.0.1:6379> geoadd china:city 106.50 29.53 chongqin
(integer) 1
127.0.0.1:6379> geoadd china:city 114.05 22.52 shenzhen
(integer) 1
127.0.0.1:6379> geoadd china:city 120.16 30.24 hangzhou 108.96 34.26 xian
(integer) 2


```

> getpos   #获取指定的城市的经度和纬度

获取当前定位：一定是一个坐标值！

```
127.0.0.1:6379> geopos china:city zhuhai            #获取指定的城市的经度和纬度
1) 1) "113.54000240564346313"
   2) "22.26000106503118303"
127.0.0.1:6379> geopos china:city shanghai chongqin
1) 1) "121.47000163793563843"
   2) "31.22999903975783553"
2) 1) "106.49999767541885376"
   2) "29.52999957900659211"

```



> geodist

两人之间的距离！

单位：

- **m** 表示单位为米。
- **km** 表示单位为千米。
- **mi** 表示单位为英里。
- **ft** 表示单位为英尺。

```
127.0.0.1:6379> geodist china:city zhuhai shanghai   #查看珠海到北京的直线距离 单位是m
"1270189.8933"
127.0.0.1:6379> geodist china:city zhuhai chongqin     
"1071821.0739"
127.0.0.1:6379> geodist china:city zhuhai chongqin km    #查看珠海到重庆的直线距离 单位是km
"1071.8211"
127.0.0.1:6379> 

```



> georadius  以给定的经纬度为中心，找出某一半径内的元素

附件的人？（获得所有附件的人的地址，定位！）通过半径来查询

获得指定数量的人，200

所有的数据都应该录入：china:city,才会让结果更加清晰

```
127.0.0.1:6379> georadius china:city 110 30 1000 km  #获取 110，30 这个经纬度为中心，寻找方圆1000km内的城市
1) "chongqin"
2) "xian"
3) "zhuhai"
4) "shenzhen"
5) "hangzhou"
127.0.0.1:6379> georadius china:city 110 30 500 km     
1) "chongqin"
2) "xian"
127.0.0.1:6379> georadius china:city 110 30 500 km withdist  #显示到中间距离的位置
1) 1) "chongqin"
   2) "341.9374"
2) 1) "xian"
   2) "483.8340"
127.0.0.1:6379> georadius china:city 110 30 500 km withcoord   #显示出城市的经纬度（他人的定位信息）
1) 1) "chongqin"
   2) 1) "106.49999767541885376"
      2) "29.52999957900659211"
2) 1) "xian"
   2) 1) "108.96000176668167114"
      2) "34.25999964418929977"
127.0.0.1:6379> georadius china:city 110 30 500 km withdist withcoord 
1) 1) "chongqin"
   2) "341.9374"
   3) 1) "106.49999767541885376"
      2) "29.52999957900659211"
2) 1) "xian"
   2) "483.8340"
   3) 1) "108.96000176668167114"
      2) "34.25999964418929977"
127.0.0.1:6379> georadius china:city 110 30 500 km withdist withcoord count 1 #筛选出指定的结果！
1) 1) "chongqin"
   2) "341.9374"
   3) 1) "106.49999767541885376"
      2) "29.52999957900659211"
127.0.0.1:6379> georadius china:city 110 30 500 km withdist withcoord count 2
1) 1) "chongqin"
   2) "341.9374"
   3) 1) "106.49999767541885376"
      2) "29.52999957900659211"
2) 1) "xian"
   2) "483.8340"
   3) 1) "108.96000176668167114"
      2) "34.25999964418929977"
127.0.0.1:6379> georadius china:city 110 30 500 km withdist withcoord count 3
1) 1) "chongqin"
   2) "341.9374"
   3) 1) "106.49999767541885376"
      2) "29.52999957900659211"
2) 1) "xian"
   2) "483.8340"
   3) 1) "108.96000176668167114"
      2) "34.25999964418929977"

```

> georadiusbymember       #找出位于指定元素周围的其他元素！

```
127.0.0.1:6379> georadiusbymember china:city zhuhai 1000 km 
1) "zhuhai"
2) "shenzhen"
127.0.0.1:6379> georadiusbymember china:city shanghai 400 km
1) "hangzhou"
2) "shanghai"
127.0.0.1:6379> 

```

> geohash  #返回一个或多个位置元素的Geohash表示

==该命令将返回11个字符的Geohash字符串！！！==

```
# 将二维的经纬度转化为一维的字符串 如果两个字符串越接近，则两地距离越接近
127.0.0.1:6379> geohash china:city zhuhai chongqin
1) "webwxsm3cu0"
2) "wm5xzrybty0"
127.0.0.1:6379> 

```

==有个问题，关于geo，官方只给了六条指令来操作geo，而其中没有删除，那我们要删除指定元素，怎么办？==

==这就体现底层的重要性了==

> GEO 底层的实现原理其实就是Zset！ 我们可以使用Zset命令来操作geo！！！ 自然也就解决了删除指定元素的问题！！！

```
127.0.0.1:6379> zrange china:city 0 -1   #查看地图中全部的元素
1) "chongqin"
2) "xian"
3) "zhuhai"
4) "shenzhen"
5) "hangzhou"
6) "shanghai"
127.0.0.1:6379> zrem china:city zhuhai      #移除指定元素
(integer) 1
127.0.0.1:6379> zrange china:city 0 -1
1) "chongqin"
2) "xian"
3) "shenzhen"
4) "hangzhou"
5) "shanghai"

```

### Hyperloglog

> 什么是基数？

A{1,3,5,7,8,7}

B{1,3,5,7,8}

基数（不重复的元素 A=4  B=5，可以接受误差！！！

> 简介

应用场景：

**网页的访问量（UV）：一个用户多次访问，也只能算作一个人。**

传统的方式，set保存用户的id，然后就可以统计出set中元素数量作为标准判断！

这个方式如果保存大量的用户id，就会比较麻烦！（原因：占内存）

我们的目的是为了计数，而不是保存用户id；

**Hyperloglog的优点：**

Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定的、并且是很小的。

花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基数。如果从内存角度来比较的话，肯定比用set集合来实现好！

因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。

**缺点：**

0.81的容错率！统计UV任务，可以忽略不记！！！

> 测试使用：

```
127.0.0.1:6379> pfadd mykey a b c d e f g h i j  #创建第一组元素 mykey
(integer) 1
127.0.0.1:6379> pfcount mykey   #统计 mykey元素的基数数量
(integer) 10
127.0.0.1:6379> pfadd mykey2 i j z x c v b n m   #创建第二组元素 mykey2
(integer) 1
127.0.0.1:6379> pfcount mykey2
(integer) 9
127.0.0.1:6379> pfmerge mykey3 mykey mykey2  #合并两组mykey mykey2 => mykey3 并集
OK
127.0.0.1:6379> pfcount mykey3  # 看并集的数量
(integer) 15
```

==如果允许容错，那么一定可以使用Hyperloglog！==

==如果不允许容错，那么可以使用set或者自己的数据类型即可！==



### Bitmap

> 位存储

统计用户信息：活跃/不活跃    登录/未登录！ 打卡/未打卡   很多人的思维： 一张表： userid status day 非常麻烦 用Bitmaps的话十分方便

`只有两个状态的 都可以使用bitmaps`

`bitmaps：位图 是一种数据结构！都是操作二进制来进行记录，就只有0和1 两个状态`

365 days=365bit  1字节=8bit 46个字节左右 （非常省内存）

> 测试

![image-20220121205307210](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220121205307210.png)

使用bitmap来记录 周一到周日的打卡！

周一：1 周二：0 周三：0 ....

```
127.0.0.1:6379> setbit sign 0 1
(integer) 0
127.0.0.1:6379> setbit sign 1 0
(integer) 0
127.0.0.1:6379> setbit sign 2 0
(integer) 0
127.0.0.1:6379> setbit sign 3 1
(integer) 0
127.0.0.1:6379> setbit sign 4 1
127.0.0.1:6379> setbit sign 5 0
(integer) 0
127.0.0.1:6379> setbit sign 6 0
(integer) 0
```

查看某一天打卡情况：

```
127.0.0.1:6379> getbit sign 3
(integer) 1
127.0.0.1:6379> getbit sign 6
(integer) 0
```

统计操作，统计 打卡的天数！

```
127.0.0.1:6379> bitcount sign  #统计这周的打卡记录，就可以看到是否全勤！！！！
(integer) 3

```



## 事务

==回顾mysql事务四大属性：==

- 原子性（Atomicity）原子性是指事务包括的所有操作要么成功，要么全部失败回滚
- 一致性（Consistency）
  事务执行之前和执行之后都必须处于一致性状态。举例来说，假设用户A和用户B两者的钱加起来一共是1000，那么不管A和B之间如何转账、转几次账，事务结束后两个用户的钱相加起来应该还得是1000，这就是事务的一致性。
- ==隔离性（Isolation）==隔离性是当多个用户并发访问数据库时，比如操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离。

- 持久性（Durability）事务一旦被提交了，对数据库中的数据是永久性的



**事务隔离级别**

- 读未提交(**Read uncommitted**)
  指一个事务读取到其他未提交事务的数据。可能导致数据**脏读**。
  转账案例：A正在给B转账，本来转的1000，A多输入了个0，变成10000，但此事务还未提交，但此时B查询到转入的是10000，但A取消事务回滚之后，B又查询不到转入的数据。这种情况就是**脏读**,也可能出现**不可重复读**和**幻读**的情况
- 读已提交(**Read committed**)
  指一个事务只能读取到其他事务已提交的数据，从而解决了**脏读**的问题。但可能导致数据**不可重复读**；
  转账案例：A要给B转账1000，A先查看了一下余额，有1000，然后开始给B转钱，但此时A家里电费通过开启的自动缴费功能，自动从A账户扣除200缴纳电费，并提交；当A转账准备提交，再次确认余额时，钱少了200。这样就导致同一个事务中多次查询的结果不一致，这种情况就是**不可重复读**；
- 可重复读(**Repeatable read**)
  指事务只要一开启，就不允许其他事务进行==修改操作==，从而解决了**不可重复读**问题。但可能导致数据**幻读**；
- 序列化(**Serializable** )
  指事务之间只能串行话执行，就像队列一样，排队进行，这样就解决了**幻读**的问题，但是这种级别的并发性能不高，非特殊需求，这种级别一般不用。



Redis 事务本质：一组命令（命令放进队列中）的集合！一个事务中的所有命令都会被序列化，在事务执行的过程中，会按照顺序执行！

一次性、顺序性、==排他性==（多条命令同时执行）！执行一个队列一系列的命令

==Redis事务没有隔离级别的概念 也就是不存在脏读这些== `区别mysql`

所有的命令在事务中，并没有直接被执行！只有发起执行命令的时候才执行！Exec

==Redis单命令是保证原子性的，但是事务是不保证原子性的！==

redis的事务：

- 开启事务（multi）
- 命令入队（.....）
- 执行事务（exec）

> 正常的事务！

```
127.0.0.1:6379> multi            #开启事务
OK
127.0.0.1:6379(TX)> set k1 v1    #命令入队
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> get k2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> exec          #执行事务
1) OK
2) OK
3) "v2"
4) OK
```

> 放弃事务！

```
127.0.0.1:6379> multi        #开启事务
OK
127.0.0.1:6379(TX)> set k1 v1   #命令入队
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> set k4 v4
QUEUED
127.0.0.1:6379(TX)> discard   #取消事务
OK
127.0.0.1:6379> get k4    #事务队列中的命令都不会被执行，所以拿不到值 因为压根没设置成功
(nil)
127.0.0.1:6379> 

```

> 编译型异常（代码有问题！命令有错！），事务中所有的命令都不会被执行！
>

```
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> getset k3     #错误的命令
(error) ERR wrong number of arguments for 'getset' command
127.0.0.1:6379(TX)> set k4 v4
QUEUED
127.0.0.1:6379(TX)> set k5 v5
QUEUED
127.0.0.1:6379(TX)> get k5
QUEUED
127.0.0.1:6379(TX)> exec      #执行事务报错
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get k5          #所有的命令都不会被执行
(nil)
```

> 运行时异常（1/0） 如果事务队列中“编译”不报错，那么执行命令的时候，其他命令是可以正常执行的，错误命令抛出异常！！

```java
127.0.0.1:6379> set k1 v1
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> incr k1  # 会执行的时候报错
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> get k3
QUEUED
127.0.0.1:6379(TX)> exec
1) (error) ERR value is not an integer or out of range  # 虽然第一条命令报错了，但是依旧正常执行成功了！！
2) OK
3) OK
4) "v3"
127.0.0.1:6379> get k2
"v2"
127.0.0.1:6379> get k3
"v3"
```

> 监控！Watch（面试常问） == 用到了乐观锁
>
> - 获取version
> - 更新（提交事务）的时候比较version 如果不一致 则放弃事务

悲观锁：

- 很悲观，认为什么时候都会出问题，无论做什么都会加锁！

乐观锁：

- 很乐观，认为什么时候都不会出问题，所以不会上锁！更新数据的时候去判断一下，在此期间是否有人修改过这个数据
- 获取version
- 更新的时候比较version



> Redis测监视测试

正常执行成功！

```
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> set out 0
OK
127.0.0.1:6379> watch money      #监视 money对象
OK
127.0.0.1:6379> multi            #事务正常结束，数据期间没有发生变动，这个时候就正常执行成功！
OK
127.0.0.1:6379(TX)> decrby money 20
QUEUED
127.0.0.1:6379(TX)> incrby out 20
QUEUED
127.0.0.1:6379(TX)> exec
1) (integer) 80
2) (integer) 20
```

==上面代码是单线程。现在模拟多线程==

测试多线程修改值，使用watch可以当做redis的乐观锁的操作

```
127.0.0.1:6379> watch money            #监控money
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> decrby money 10
QUEUED
127.0.0.1:6379(TX)> incrby out 10
QUEUED
127.0.0.1:6379(TX)> exec      # 执行之前，另外一个线程，修改了我们的值（money），这个时候，就会导致事务执行失败！！！
(nil)
127.0.0.1:6379> 

```

如果修改失败，获取最新的值就好

![image-20220122221023288](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220122221023288.png)

> 应用场景：

`乐观锁实现秒杀系统`



## Jedis

为什么要使用java来操作redis

> 什么是Jedis 是Redis 官方推荐的java连接开发工具！使用java操作Redis中间件！如果你要使用java操作redis，那么一定要对Jedis十分熟悉！！

==学习不能急躁 慢慢来会很快==

> 测试

需要提前打开阿里云服务器的6379端口

1.导入对应的依赖

```

   <dependencies>
    <!--导入jedis的包-->
    <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
        <version>4.0.1</version>
    </dependency>

       <!-- fastjson -->
       <dependency>
           <groupId>com.alibaba</groupId>
           <artifactId>fastjson</artifactId>
           <version>1.2.79</version>
       </dependency>

   </dependencies>
```

2.编码顺序：

- 连接数据库
- 操作命令
- 断开连接！结束测试

```
package com.kuang;

import redis.clients.jedis.Jedis;

public class TestPing {
    public static void main(String[] args) {
        //1. new jedis 对象即可
        Jedis jedis = new Jedis("120.79.174.190",6379);
        jedis.auth("347858300aB");
        //jedis所有的命令就是我们之前学习的所有指令 所以之前的指令学很重要
        System.out.println(jedis.ping());
    }
}

```

输出：

![image-20220123134543688](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220123134543688.png)

> 若报错：连接超时，解决方案请看此博客  https://blog.csdn.net/qq_44624536/article/details/120213607

小测几个：

```
package com.kuang;

import redis.clients.jedis.Jedis;

import java.util.Set;

public class TestKey {
    public static void main(String[] args) {
        //1. new jedis 对象即可
        Jedis jedis = new Jedis("120.79.174.190",6379);
         jedis.auth("347858300aB");
        System.out.println("清空数据 "+jedis.flushDB());
        System.out.println("判断某个键是否存在 "+jedis.exists("username"));
        System.out.println("新增<'username'，'kuangshen'> "+jedis.set("username","kuangshen"));
        System.out.println("新增<'password'，'password'> "+jedis.set("password","password"));
        System.out.println("系统中所有的键如下：");
        Set<String> keys = jedis.keys("*");
        System.out.println(keys);

        System.out.println("删除键password "+jedis.del("password"));
        System.out.println("判断键password是否存在 "+jedis.exists("password"));
        System.out.println("查看键username所存储的值的类型 "+jedis.type("username"));
        System.out.println("随机返回key空间的一个："+jedis.randomKey());
        System.out.println("重命名key："+jedis.rename("username","name"));
        System.out.println("取出改名后的name："+jedis.get("name"));
        System.out.println("按索引查询"+jedis.select(0));
        System.out.println("删除当前所选数据库的所有key "+jedis.flushDB());
        System.out.println("返回当前数据库中key的数目 "+jedis.dbSize());
        System.out.println("删除所有数据库中的所有key："+jedis.flushAll());
    }
}

```

输出：

![image-20220123141937058](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220123141937058.png)

> 所有的api命令，就是我们对应的上面学习的指令，一个都没有变化！！！ 上面只是测试了一小部分 其他的api也是如此！！！



### 通过Redis再次理解事务

> 事务

```
package com.kuang;

import com.alibaba.fastjson.JSONObject;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.Transaction;

public class TestTX {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("120.79.174.190",6379);

        JSONObject jsonObject = new JSONObject();
        jsonObject.put("hello","world");
        jsonObject.put("name","kuangshen");
        //开启事务
        Transaction multi = jedis.multi();

        String result = jsonObject.toJSONString();
        System.out.println(result);
        try {
            multi.set("user1",result);
            multi.set("user2",result);
            multi.exec();
        }catch (Exception e){
            multi.discard();//抛出异常 放弃事务
        }
        finally {
            System.out.println(jedis.get("user1"));
            System.out.println(jedis.get("user2"));
            //关闭连接
            jedis.close();
        }

    }
}

```

输出：

![image-20220123144211551](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220123144211551.png)



再次测试（实现取消事务操作）：

```
package com.kuang;



import com.alibaba.fastjson.JSONObject;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.Transaction;

public class TestTX {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("120.79.174.190",6379);
         jedis.flushDB();
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("hello","world");
        jsonObject.put("name","kuangshen");
        //开启事务
        Transaction multi = jedis.multi();

        String result = jsonObject.toJSONString();
        System.out.println(result);
        try {
            multi.set("user1",result);
            multi.set("user2",result);
            int i = 1 / 0;
            multi.exec();
        }catch (Exception e){
            multi.discard();//抛出异常 放弃事务
        }
        finally {
            System.out.println(jedis.get("user1"));
            System.out.println(jedis.get("user2"));
            //关闭连接
            jedis.close();
        }

    }
}

```

输出：

![image-20220123144635487](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220123144635487.png)

## SpringBoot整合

springboot操作数据：spring-data jdbc mongodb redis！

SpringData也是和springboot齐名的项目！

![image-20220125122511843](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125122511843.png)

> 整合测试

导入依赖

```
  <!--操作redis-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
```

点进去看看发现底层是

1.

```
<dependency>
  <groupId>org.springframework.data</groupId>
  <artifactId>spring-data-redis</artifactId>
  <version>2.3.6.RELEASE</version>
  <scope>compile</scope>
</dependency>
```

说明：在SpringBoot2.x之后，原来使用的jedis被替换为了lettuce,

==Jedis：采用直连的方式，多个线程操作的话，是不安全的，如果想要避免不安全，就要适用jedis pool连接池！更像BIO模式==

==lettuce：采用netty，实例可以被多个线程进行共享，不存在线程不安全的情况！可以减少线程数量，更像NIO模式==

2.

```
<dependency>
  <groupId>io.lettuce</groupId>
  <artifactId>lettuce-core</artifactId>
  <version>5.3.5.RELEASE</version>
  <scope>compile</scope>
</dependency>
```

点进去可以看到netty的包

![image-20220125124359286](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125124359286.png)

`回顾一下：`

SpringBoot 所有的配置(可以在配置文件application.yml写的配置)，都有一个自动配置类（类名格式：xxxAutoConfiguration）

自动配置类都会绑定一个对应properties文件类     ==RedisProperties==

找下redis的自动装配类吧，==RedisAutoConfiguration==

打开这个位置：

![image-20220125125130858](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125125130858.png)

![image-20220125125224243](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125125224243.png)

![image-20220125125428324](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125125428324.png)

点开RedisProperties

![image-20220125125931549](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125125931549.png)

`源码分析：`

==Redis自动配置类的两个Bean：==

```
@Configuration(proxyBeanMethods = false)
@ConditionalOnClass(RedisOperations.class)   
@EnableConfigurationProperties(RedisProperties.class)
@Import({ LettuceConnectionConfiguration.class, JedisConnectionConfiguration.class })
public class RedisAutoConfiguration {

   @Bean
   @ConditionalOnMissingBean(name = "redisTemplate")  
   #这个注解的意思是：当这个类不存在的时候，这个bean才生效，如果自己写了一个RedisTemplate类，就可以替换这个
   public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory)
         throws UnknownHostException {
         //默认的 RedisTemplate，没有过多的设置，redis对象（操作redis的api传入的value是一个对象）都是需要序列化的
         //两个泛型都是Object类型，我们后面使用需要强制转化<String,Object>
      RedisTemplate<Object, Object> template = new RedisTemplate<>();
      template.setConnectionFactory(redisConnectionFactory);
      return template;
   }

   @Bean
   @ConditionalOnMissingBean   //因为String类型是最常用的，所以抽取了一个String的模板bean出来
   public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory)
         throws UnknownHostException {
      StringRedisTemplate template = new StringRedisTemplate();
      template.setConnectionFactory(redisConnectionFactory);
      return template;
   }

}
```

点开`RedisConnectionFactory `发现有两个实现类

![image-20220125141213934](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125141213934.png)

打开`JedisConnectionFactory`发现很多的类不存在 所以这个类的很多属性没有注入成功也就是不生效

![image-20220125141430987](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125141430987.png)

打开`LettuceConnectionFactory ` 发现都ok

![image-20220125141611015](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125141611015.png)

`redis的配置类都是spring.redis打头`

![image-20220125140129636](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125140129636.png)

配置文件

```
#配置redis
spring.redis.host=120.79.174.190
spring.redis.port=6379  #因为默认是6379所以可以不写
spring.redis.password=347858300aB
spring.redis.database=1   # 默认是0号数据库
```

```
package com.kuang;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.redis.core.RedisTemplate;

@SpringBootTest
class Redis02SpringbootApplicationTests {
    @Autowired
    private RedisTemplate redisTemplate;
    @Test
    void contextLoads() {
       // redisTemplate 操作不同的数据类型 api和我们指令一样
       //opsForValue   操作字符串 类似string
        //opsForList   操作List 类似List   ...其他数据类型类比
        
        //除了基本的操作，我们常用的方法都可以直接通过redisTemplate操作 比如事务，和基本的CRUD
     //    redisTemplate.multi();
      //  redisTemplate.delete("1");
      ....
        
            //获取redis的连接对象
     //   RedisConnection connection = redisTemplate.getConnectionFactory().getConnection();
       // connection.flushDb();
       // connection.flushAll();
       
        redisTemplate.opsForValue().set("user","user");
        System.out.println(redisTemplate.opsForValue().get("user"));

    }

}

```

输出：==发现redis客户端的相应的key出现了转义的情况==

![image-20220126014924729](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220126014924729.png)

![image-20220126015019247](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220126015019247.png)

点开这个

![image-20220125145323535](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125145323535.png)

查看序列化配置

![image-20220125145430430](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125145430430.png)

==就是因为这个序列化没处理好，因此redis客户端会出现转义==

![image-20220125145657534](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125145657534.png)

我们新建一个pojo包，下面写个User类

```
@Component
@AllArgsConstructor
@NoArgsConstructor
@Data
public class User  {
   private String name;
   private int age;

}
```

测试：

```
    @Test
    public void test() throws JsonProcessingException {
        //真实的开发中 一般都使用json来传递对象
        User user = new User("狂神说",3);
        //返回json字符串
        String jsonUser = new ObjectMapper().writeValueAsString(user);
        redisTemplate.opsForValue().set("user",jsonUser);
        System.out.println(redisTemplate.opsForValue().get("user"));
    }

}
```

输出：redis客户端出现了转义

![image-20220125154722903](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125154722903.png)

![image-20220125154740171](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125154740171.png)

那我们直接传一个对象呢？

```
@Test
public void test() throws JsonProcessingException {
    //真实的开发中 一般都使用json来传递对象
    User user = new User("狂神说",3);
    //返回json字符串
    String jsonUser = new ObjectMapper().writeValueAsString(user);
    redisTemplate.opsForValue().set("user",user);
    System.out.println(redisTemplate.opsForValue().get("user"));
}
```

报错：

![image-20220125155401636](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125155401636.png)

![image-20220125155626213](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125155626213.png)

输出：

![image-20220125155649068](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125155649068.png)

我们模仿spring为我们自带的RedisTemplate，编写一个自己的RedisTemplate 里面包含实现序列化的操作

```
package com.kuang.config;

import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

import java.net.UnknownHostException;
//这是一个固定模板，在企业中，可以直接使用
@Configuration
public class RedisConfig {
    //编写自己的redsiTemplate
    @Bean
    @SuppressWarnings("all")
    public RedisTemplate<String, Object> redisTemplate1(RedisConnectionFactory factory) {
        //我们为了自己开发方便，一般直接使用<String,Object>
        RedisTemplate<String, Object> template = new RedisTemplate<String, Object>();
        template.setConnectionFactory(factory);

        // 序列化配置（配置json的序列化） 解析任意对象
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        // json序列化利用ObjectMapper进行转义
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        // 2.序列化String类型
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();

        // key采用String的序列化方式
        template.setKeySerializer(stringRedisSerializer);
        // hash的key也采用String的序列化方式
        template.setHashKeySerializer(stringRedisSerializer);
        // value序列化方式采用jackson
        template.setValueSerializer(jackson2JsonRedisSerializer);
        // hash的value序列化方式采用jackson
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        template.afterPropertiesSet();

        return template;
    }

}


```

![image-20220125155932749](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125155932749.png)

发现有以下实现类：默认使用的是JDK的

![image-20220125160002217](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125160002217.png)

用我们自定义的模板进行测试！

![image-20220125161140700](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125161140700.png)

再次测试！！！`完美解决转义问题！！！`

![image-20220125161335977](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125161335977.png)

![image-20220125161746542](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125161746542.png)

> 在我们实际的开发中，或者你们在公司一般都可以看到一个公司自己封装的redis的工具类

```
package com.kuang.util;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;
import org.springframework.util.CollectionUtils;

import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.concurrent.TimeUnit;

/**
 * Redis工具类
 * @author ZENG.XIAO.YAN
 * @date   2018年6月7日
 */
@Component
public final class RedisUtil {

   @Autowired
   private RedisTemplate<String, Object> redisTemplate;

   // =============================common============================
   /**
    * 指定缓存失效时间
    * @param key 键
    * @param time 时间(秒)
    * @return
    */
   public boolean expire(String key, long time) {
      try {
         if (time > 0) {
            redisTemplate.expire(key, time, TimeUnit.SECONDS);
         }
         return true;
      } catch (Exception e) {
         e.printStackTrace();
         return false;
      }
   }

   /**
    * 根据key 获取过期时间
    * @param key 键 不能为null
    * @return 时间(秒) 返回0代表为永久有效
    */
   public long getExpire(String key) {
      return redisTemplate.getExpire(key, TimeUnit.SECONDS);
   }

   /**
    * 判断key是否存在
    * @param key 键
    * @return true 存在 false不存在
    */
   public boolean hasKey(String key) {
      try {
         return redisTemplate.hasKey(key);
      } catch (Exception e) {
         e.printStackTrace();
         return false;
      }
   }

   /**
    * 删除缓存
    * @param key 可以传一个值 或多个
    */
   @SuppressWarnings("unchecked")
   public void del(String... key) {
      if (key != null && key.length > 0) {
         if (key.length == 1) {
            redisTemplate.delete(key[0]);
         } else {
            redisTemplate.delete(CollectionUtils.arrayToList(key));
         }
      }
   }

   // ============================String=============================
   /**
    * 普通缓存获取
    * @param key 键
    * @return 值
    */
   public Object get(String key) {
      return key == null ? null : redisTemplate.opsForValue().get(key);
   }

   /**
    * 普通缓存放入
    * @param key 键
    * @param value 值
    * @return true成功 false失败
    */
   public boolean set(String key, Object value) {
      try {
         redisTemplate.opsForValue().set(key, value);
         return true;
      } catch (Exception e) {
         e.printStackTrace();
         return false;
      }

   }

   /**
    * 普通缓存放入并设置时间
    * @param key 键
    * @param value 值
    * @param time 时间(秒) time要大于0 如果time小于等于0 将设置无限期
    * @return true成功 false 失败
    */
   public boolean set(String key, Object value, long time) {
      try {
         if (time > 0) {
            redisTemplate.opsForValue().set(key, value, time, TimeUnit.SECONDS);
         } else {
            set(key, value);
         }
         return true;
      } catch (Exception e) {
         e.printStackTrace();
         return false;
      }
   }

   /**
    * 递增
    * @param key 键
    * @param delta 要增加几(大于0)
    * @return
    */
   public long incr(String key, long delta) {
      if (delta < 0) {
         throw new RuntimeException("递增因子必须大于0");
      }
      return redisTemplate.opsForValue().increment(key, delta);
   }

   /**
    * 递减
    * @param key 键
    * @param delta 要减少几(小于0)
    * @return
    */
   public long decr(String key, long delta) {
      if (delta < 0) {
         throw new RuntimeException("递减因子必须大于0");
      }
      return redisTemplate.opsForValue().increment(key, -delta);
   }

   // ================================Map=================================
   /**
    * HashGet
    * @param key 键 不能为null
    * @param item 项 不能为null
    * @return 值
    */
   public Object hget(String key, String item) {
      return redisTemplate.opsForHash().get(key, item);
   }

   /**
    * 获取hashKey对应的所有键值
    * @param key 键
    * @return 对应的多个键值
    */
   public Map<Object, Object> hmget(String key) {
      return redisTemplate.opsForHash().entries(key);
   }

   /**
    * HashSet
    * @param key 键
    * @param map 对应多个键值
    * @return true 成功 false 失败
    */
   public boolean hmset(String key, Map<String, Object> map) {
      try {
         redisTemplate.opsForHash().putAll(key, map);
         return true;
      } catch (Exception e) {
         e.printStackTrace();
         return false;
      }
   }

   /**
    * HashSet 并设置时间
    * @param key 键
    * @param map 对应多个键值
    * @param time 时间(秒)
    * @return true成功 false失败
    */
   public boolean hmset(String key, Map<String, Object> map, long time) {
      try {
         redisTemplate.opsForHash().putAll(key, map);
         if (time > 0) {
            expire(key, time);
         }
         return true;
      } catch (Exception e) {
         e.printStackTrace();
         return false;
      }
   }

   /**
    * 向一张hash表中放入数据,如果不存在将创建
    * @param key 键
    * @param item 项
    * @param value 值
    * @return true 成功 false失败
    */
   public boolean hset(String key, String item, Object value) {
      try {
         redisTemplate.opsForHash().put(key, item, value);
         return true;
      } catch (Exception e) {
         e.printStackTrace();
         return false;
      }
   }

   /**
    * 向一张hash表中放入数据,如果不存在将创建
    * @param key 键
    * @param item 项
    * @param value 值
    * @param time 时间(秒) 注意:如果已存在的hash表有时间,这里将会替换原有的时间
    * @return true 成功 false失败
    */
   public boolean hset(String key, String item, Object value, long time) {
      try {
         redisTemplate.opsForHash().put(key, item, value);
         if (time > 0) {
            expire(key, time);
         }
         return true;
      } catch (Exception e) {
         e.printStackTrace();
         return false;
      }
   }

   /**
    * 删除hash表中的值
    * @param key 键 不能为null
    * @param item 项 可以使多个 不能为null
    */
   public void hdel(String key, Object... item) {
      redisTemplate.opsForHash().delete(key, item);
   }

   /**
    * 判断hash表中是否有该项的值
    * @param key 键 不能为null
    * @param item 项 不能为null
    * @return true 存在 false不存在
    */
   public boolean hHasKey(String key, String item) {
      return redisTemplate.opsForHash().hasKey(key, item);
   }

   /**
    * hash递增 如果不存在,就会创建一个 并把新增后的值返回
    * @param key 键
    * @param item 项
    * @param by 要增加几(大于0)
    * @return
    */
   public double hincr(String key, String item, double by) {
      return redisTemplate.opsForHash().increment(key, item, by);
   }

   /**
    * hash递减
    * @param key 键
    * @param item 项
    * @param by 要减少记(小于0)
    * @return
    */
   public double hdecr(String key, String item, double by) {
      return redisTemplate.opsForHash().increment(key, item, -by);
   }

   // ============================set=============================
   /**
    * 根据key获取Set中的所有值
    * @param key 键
    * @return
    */
   public Set<Object> sGet(String key) {
      try {
         return redisTemplate.opsForSet().members(key);
      } catch (Exception e) {
         e.printStackTrace();
         return null;
      }
   }

   /**
    * 根据value从一个set中查询,是否存在
    * @param key 键
    * @param value 值
    * @return true 存在 false不存在
    */
   public boolean sHasKey(String key, Object value) {
      try {
         return redisTemplate.opsForSet().isMember(key, value);
      } catch (Exception e) {
         e.printStackTrace();
         return false;
      }
   }

   /**
    * 将数据放入set缓存
    * @param key 键
    * @param values 值 可以是多个
    * @return 成功个数
    */
   public long sSet(String key, Object... values) {
      try {
         return redisTemplate.opsForSet().add(key, values);
      } catch (Exception e) {
         e.printStackTrace();
         return 0;
      }
   }

   /**
    * 将set数据放入缓存
    * @param key 键
    * @param time 时间(秒)
    * @param values 值 可以是多个
    * @return 成功个数
    */
   public long sSetAndTime(String key, long time, Object... values) {
      try {
         Long count = redisTemplate.opsForSet().add(key, values);
         if (time > 0)
            expire(key, time);
         return count;
      } catch (Exception e) {
         e.printStackTrace();
         return 0;
      }
   }

   /**
    * 获取set缓存的长度
    * @param key 键
    * @return
    */
   public long sGetSetSize(String key) {
      try {
         return redisTemplate.opsForSet().size(key);
      } catch (Exception e) {
         e.printStackTrace();
         return 0;
      }
   }

   /**
    * 移除值为value的
    * @param key 键
    * @param values 值 可以是多个
    * @return 移除的个数
    */
   public long setRemove(String key, Object... values) {
      try {
         Long count = redisTemplate.opsForSet().remove(key, values);
         return count;
      } catch (Exception e) {
         e.printStackTrace();
         return 0;
      }
   }
   // ===============================list=================================

   /**
    * 获取list缓存的内容
    * @param key 键
    * @param start 开始
    * @param end 结束 0 到 -1代表所有值
    * @return
    */
   public List<Object> lGet(String key, long start, long end) {
      try {
         return redisTemplate.opsForList().range(key, start, end);
      } catch (Exception e) {
         e.printStackTrace();
         return null;
      }
   }

   /**
    * 获取list缓存的长度
    * @param key 键
    * @return
    */
   public long lGetListSize(String key) {
      try {
         return redisTemplate.opsForList().size(key);
      } catch (Exception e) {
         e.printStackTrace();
         return 0;
      }
   }

   /**
    * 通过索引 获取list中的值
    * @param key 键
    * @param index 索引 index>=0时， 0 表头，1 第二个元素，依次类推；index<0时，-1，表尾，-2倒数第二个元素，依次类推
    * @return
    */
   public Object lGetIndex(String key, long index) {
      try {
         return redisTemplate.opsForList().index(key, index);
      } catch (Exception e) {
         e.printStackTrace();
         return null;
      }
   }

   /**
    * 将list放入缓存
    * @param key 键
    * @param value 值

    * @return
    */
   public boolean lSet(String key, Object value) {
      try {
         redisTemplate.opsForList().rightPush(key, value);
         return true;
      } catch (Exception e) {
         e.printStackTrace();
         return false;
      }
   }

   /**
    * 将list放入缓存
    * @param key 键
    * @param value 值
    * @param time 时间(秒)
    * @return
    */
   public boolean lSet(String key, Object value, long time) {
      try {
         redisTemplate.opsForList().rightPush(key, value);
         if (time > 0)
            expire(key, time);
         return true;
      } catch (Exception e) {
         e.printStackTrace();
         return false;
      }
   }

   /**
    * 将list放入缓存
    * @param key 键
    * @return
    */
   public boolean lSet(String key, List<Object> value) {
      try {
         redisTemplate.opsForList().rightPushAll(key, value);
         return true;
      } catch (Exception e) {
         e.printStackTrace();
         return false;
      }
   }

   /**
    * 将list放入缓存
    *
    * @param key 键
    * @param value 值
    * @param time 时间(秒)
    * @return
    */
   public boolean lSet(String key, List<Object> value, long time) {
      try {
         redisTemplate.opsForList().rightPushAll(key, value);
         if (time > 0)
            expire(key, time);
         return true;
      } catch (Exception e) {
         e.printStackTrace();
         return false;
      }
   }

   /**
    * 根据索引修改list中的某条数据
    * @param key 键
    * @param index 索引
    * @param value 值
    * @return
    */
   public boolean lUpdateIndex(String key, long index, Object value) {
      try {
         redisTemplate.opsForList().set(key, index, value);
         return true;
      } catch (Exception e) {
         e.printStackTrace();
         return false;
      }
   }

   /**
    * 移除N个值为value
    * @param key 键
    * @param count 移除多少个
    * @param value 值
    * @return 移除的个数
    */
   public long lRemove(String key, long count, Object value) {
      try {
         Long remove = redisTemplate.opsForList().remove(key, count, value);
         return remove;
      } catch (Exception e) {
         e.printStackTrace();
         return 0;
      }
   }
}
```

测试！

```
@Test
public void test1(){
    redisUtil.set("kwq","kwq");
    System.out.println(redisUtil.get("kwq"));
}
```

![image-20220125163639804](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125163639804.png)

所有的redis操作，其实对于java开发人员来讲，十分简单，更重要的是去理解redis的思想和每一种数据结构的用处和作用场景！



## Redis.conf详解

启动的时候，就是通过配置文件来启动的

==工作中，一些小小的配置 可以让你脱颖而出==

> 单位：

![image-20220124002633524](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124002633524.png)

1、配置文件 unit单位 对大小写不敏感！

> 包含一些其他的配置文件

![image-20220124002907649](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124002907649.png)

就好比我们学习spring的improt



> 网络

```
bind 127.0.0.1 -::1   # 绑定的ip   注释掉才能外网访问 不然只能本机访问
protected-mode no   # 保护模式      默认为yes 改为no才能外网访问
port 6379           # 端口设置
```

> 通用GENERAL

```
daemonize yes  # 以守护进程的方式运行，默认是no 我们需要自己开启为yes 
pidfile /var/run/redis_6379.pid  #如果以后台的方式运行，我们就需要指定一个pid文件

# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)  生产环境
# warning (only very important / critical messages are logged)
loglevel notice    

logfile "" #日志的文件位置名
databases 16 #数据库的数量，默认是16个数据库
always-show-logo yes # 是否显示logo

```

> 快照

持久化，在规定的时间内，执行了多少次操作，则会持久化到文件 .rdb .aof

redis是内存数据库，如果没有持久化，那么数据断电即失去

```kka
# 如果900秒内至少有一个key进行了修改，我们就进行持久化操作
save 900 1
# 如果300秒内至少有10key进行了修改，我们就进行持久化操作
save 300 10
# 如果60秒内至少有10000key进行了修改，我们就进行持久化操作
save 60 10000
# 我们之后学习持久化，会自己定义这个配置
stop-writes-on-bgsave-error yes # 持久化出错后是否继续工作

rdbcompression yes # 是否压缩rdb文件，需要消耗一些cpu的资源

rdbchecksum yes	# 报错rdb文件时候进行错误的检查校验

dir ./ # rdb文件保存的目录，默认为当前目录

```

> REPLICATION 复制，我们后面讲解主从复制的时候再进行讲解





> SECURITY 安全

可以设置redis的密码，默认是没有密码！设置密码看上面linux安装redis笔记中的参考博客。设置完后如下：

```
redis-cli -p 6379 
127.0.0.1:6379> ping            #发现所有的命令都没有权限了
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth 347858300aB  #使用密码进行登录
OK
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> config get requirepass  #获取redis的密码
1) "requirepass"
2) "347858300aB"

```

> 限制 CLIENTS

```java
# maxclients 10000	# 设置能连接上的最大客户端的数量

# maxmemory <bytes>	# redis最大的内存容量

# maxmemory-policy noeviction	# 内存达到上限的处理策略
	# 移除一些过期的key
	# 报错
	# 。。。
noeviction: 不删除策略, 达到最大内存限制时, 如果需要更多内存, 直接返回错误信息。（默认值）
allkeys-lru: 所有key通用; 优先删除最近最少使用(less recently used ,LRU) 的 key。
volatile-lru: 只限于设置了 expire 的部分; 优先删除最近最少使用(less recently used ,LRU) 的 key。
allkeys-random: 所有key通用; 随机删除一部分 key。
volatile-random: 只限于设置了 expire 的部分; 随机删除一部分 key。
volatile-ttl: 只限于设置了 expire 的部分; 优先删除剩余时间(time to live,TTL) 短的key。

redis中并不会准确的删除所有键中最近最少使用的键，而是随机抽取maxmeory-samples个键，删除这三个键中最近最少使用的键。

```

> APPEND ONLY 模式 aof配置

```
appendonly no	# 默认不开启aof模式，默认使用rdb方式持久化，几乎在所有情况下rdb够用

appendfilename "appendonly.aof" # 持久化文件的名字

# appendfsync always	# 每次修改都会同步，消耗性能
appendfsync everysec	# 每秒都同步一次 sync，可能会丢失这1s数据
# appendfsync no		# 不执行同步，这时候操作系统自己同步数据，速度最快
```

具体的配置，我们在Redis持久化的时候给大家详细讲！



## Redis持久化

==面试和工作，持久化都是重点！！！==

Redis是内存数据库，如果不将内存中的数据状态保存到磁盘，那么一旦==服务器进程退出==，服务器中的数据库状态也会消失。所以redis提供了持久化功能

### RDB(Redis DataBase)

> 什么是RDB

![在这里插入图片描述](https://img-blog.csdnimg.cn/f68b6cd75bfa4b529c5758ec1022a4ac.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbG92ZXdhbmd5aWh1aQ==,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

==在指定的时间 间隔内将内存中的数据集快照写入到磁盘中，用行话将就是Snapshot快照，恢复时将快照文件直接读到内存中==

Redis会单独创建一个子进程（fork）进行持久化，会将数据先写入到一个临时RDB(==二进制文件来的，目的是节省内存==)文件中， 再用临时文件替换上次持久化完成的文件

整个过程主进程==不进行任何io操作==，==保证了性能==，如果需要进行==大规模数据恢复，且对数据恢复的完整性不是非常敏感==，那么RDB更加高效，==RDB缺点时最后一次需要持久化后的数据可能丢失，我们默认使用的就是RDB，一般情况不需要修改这个配置==

==RDB保存的文件名是dump.rdb==

都是在我们的配置文件中的快照中进行配置的

------------------------------------------------
![image-20220124012603529](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124012603529.png)

> 自己测试一次，60s内修改5次，就会触发rdb操作

修改配置文件：

![image-20201104175830756](https://img-blog.csdnimg.cn/img_convert/1b9128e01b714fbfdf56c11a5bd29699.png)

先删掉目录下的dump.rdb 这样才能验证结论

![image-20220124014708594](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124014708594.png)

![image-20220124014753906](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124014753906.png)

1分钟内修改五次命令

![image-20220129122129353](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220129122129353.png)

发现：

![image-20220124014708594](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124014708594.png)

验证实现，结论正确！！！

==触发机制==

1. save规则满足的情况下，会自动触发rdb规则  意思是：要先执行save命令 然后修改五次 就能生成
2. 执行flushall命令也会触发rdb规则
3. 退出redis，也会产生rdb           `也就是shutdown然后exit关闭redis-server这个进程 `

备份就自动生成dump.rdb



![image-20220124015803101](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124015803101.png)

dump.rdb文件内容

![image-20220124015751969](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124015751969.png)

关掉redis，再次打开redis，仍旧能得到值，说明持久化了

```
127.0.0.1:6379> shutdown
not connected> exit
[root@iZwz9awxiy8x3ci8733ijbZ bin]#redis-server kwqconfig/redis.conf 
[root@iZwz9awxiy8x3ci8733ijbZ bin]#redis-cli -p 6379
127.0.0.1:6379> auth 347858300aB
OK
127.0.0.1:6379> get k1
"v3"
127.0.0.1:6379> 
```

==如何恢复rdb文件：==

1. 只需要将rdb文件放在redis的启动目录下就可以了，redis启动的时候会自动检查dump.rdb文件并且恢复到内存数据库中

2. 查看rdb文件需要存在的位置

   ```
   127.0.0.1:6379> config get dir
   1) "dir"
   2) "/usr/local/bin"   #如果在这个目录下存在dump.rdb文件，启动就会自动恢复其中的数据
   ```

   > 几乎他自己的默认配置就够用了，但是我们还是要学习

   `有时候在生产环境，我们会将这个文件进行备份，以便恢复到某种状态`

   **优点**：

   1. 适合大规模的数据恢复！dump.rdb
   2. 如果对数据完整性不高！

   **缺点**：

   1. 需要一定的时间间隔进行操作，如果redis意外宕机了，那么最后一次修改的数据就没有了
   2. fork进程的时候会占用一定的内存空间

### AOF（Append Only File）

将我们的所有命令都记录下来，history，恢复的时候把这个文件全部再执行一遍

> 是什么

![img](https://img-blog.csdnimg.cn/img_convert/3eb0e5a9501dfa3228e646cb18d34e44.png)

以日志的形式来记录每个写的操作，将Redis执行过的所有指令记录下来（读操作不记录），只许==追加文件，但是不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作==  

==AOF保存的是appendonly.aof文件==

![image-20220124121025578](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124121025578.png)

默认是不开启的，我们需要手动进行配置，我们只需要将appendonly改为yes即可

重启redis就可以生效了 ==shutdown+exit+重新打开客户端就能实现重启==

![image-20220124121405784](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124121405784.png)

设置几个k-v

![image-20220124121725657](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124121725657.png)

打开appendonly.aof文件

![image-20220124121735660](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124121735660.png)



如果这个aof文件有错误，这时候redis是启动不起来的，我们需要修复这个aof文件（==手动制造错误，如最后一行所示==）

![image-20220124122105187](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124122105187.png)

![image-20220124122123845](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124122123845.png)

redis给我们提供了`redis-check-aof-- fix appendonly.aof`来进行appendonly.aof的修复

![image-20220124131653760](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124131653760.png)

再次打开文件：

![image-20220124131759261](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124131759261.png)

如果文件正常，重启就可以直接恢复了

![image-20220124131916234](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124131916234.png)

> 重写规则说明：

aof就是文件无限追加，文件会越来越大！

![image-20220124133101173](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124133101173.png)

如果一个aof文件大于64m，太大了！fork一个新的进程来将我们的文件进行重写！ ==弹幕：这里的重写是为了将aof文件变的更小，降低文件占用空间==

> 优点和缺点

```java
appendonly no	# 默认不开启aof模式，默认使用rdb方式持久化，几乎在所有情况下rdb够用
appendfilename "appendonly.aof" # 持久化文件的名字

# appendfsync always	# 每次修改都会同步，消耗性能
appendfsync everysec	# 每秒都同步一次 sync，可能会丢失这1s数据
# appendfsync no		# 不执行同步，这时候操作系统自己同步数据，速度最快
```

优点：

- 每一次修改都同步，文件完整性更加好
- 每秒同步一次，可能会丢失一秒的数据
- 从不同步，效率是最高的！



缺点：

- 相对于数据文件大小来说，aof远远大于rdb，修复速度也比rdb慢
- AOF运行效率也要比rdb慢，redis默认的配置就是rdb持久化
- 重写规则说明
  如果aof文件大于64MB，太大了，！fork一个新的进程来将我们文件进行重写
- **aof默认就是文件的无限追加，文件会越来越大**



### 扩展：

Redis 持久化

Redis 提供了不同级别的持久化方式:

- RDB持久化方式能够在指定的时间间隔能对你的数据进行快照存储.
- `AOF持久化方式记录每次对服务器写的操作,当服务器重启的时候会重新执行这些命令来恢复原始的数据,AOF命令以redis协议追加保存每次写的操作到文件末尾.Redis还能对AOF文件进行后台重写,使得AOF文件的体积不至于过大.`
- 如果你只希望你的数据在服务器运行的时候存在,你也可以不使用任何持久化方式.
- `你也可以同时开启两种持久化方式, 在这种情况下, 当redis重启的时候会优先载入AOF文件来恢复原始的数据,因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整.`   ==rdb丢失的数据比aof多，因为aof是每秒执行一次==
- 最重要的事情是了解RDB和AOF持久化方式的不同,让我们以RDB持久化方式开始:
  RDB的优点
  RDB是一个非常紧凑的文件,它保存了某个时间点得数据集,非常适用于数据集的备份,比如你可以在每个小时报保存一下过去24小时内的数据,同时每天保存过去30天的数据,这样即使出了问题你也可以根据需求恢复到不同版本的数据集.

==性能建议：==

![image-20220124135610438](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124135610438.png)



## Redis发布订阅

redis发布订阅（pub/sub）是一种==消息通信模式==：发送者（pub）发送消息，订阅者（sub）接受消息。微信，微博，关注系统

redis客户端可以订阅任意数量的频道

订阅/发布消息图:

第一个：消息发送者

第二个：频道

第三个：消息订阅者

![image-20220124141019837](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124141019837.png)



下图展示了频道 channel1 ， 以及订阅这个频道的三个客户端 —— client2 、 client5 和 client1 之间的关系：

![image-20220124141711086](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124141711086.png)

当有新消息通过 PUBLISH 命令发送给频道 channel1 时， 这个消息就会被发送给订阅它的三个客户端：

![image-20220124141745090](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124141745090.png)

> 命令：

这些命令被广泛与构建即时通讯应用，比如网络聊天室和实时广播，实时提醒等。

|                  命令                  |                描述                |
| :------------------------------------: | :--------------------------------: |
|     PSUBSCRIBE pattern [pattern..]     | 订阅一个或多个符合给定模式的频道。 |
|    PUNSUBSCRIBE pattern [pattern..]    | 退订一个或多个符合给定模式的频道。 |
| PUBSUB subcommand [argument[argument]] |      查看订阅与发布系统状态。      |
|        PUBLISH channel message         |         向指定频道发布消息         |
|     SUBSCRIBE channel [channel..]      |     订阅给定的一个或多个频道。     |
|     SUBSCRIBE channel [channel..]      |         退订一个或多个频道         |

> 实例

订阅端：

```
127.0.0.1:6379> subscribe kuangshenshuo    # 订阅skuangshenshuo频道
Reading messages... (press Ctrl-C to quit)   等待接收消息
1) "subscribe"         # 订阅成功的消息
2) "kuangshenshuo"
3) (integer) 1
```

消息发送端：

```
127.0.0.1:6379> publish kuangshenshuo "hello"    # 发布消息到kuangshenshuo频道
(integer) 1
127.0.0.1:6379> publish kuangshenshuo "java"     # 发布消息
(integer) 1
127.0.0.1:6379> 

```

订阅端：

```
127.0.0.1:6379> subscribe kuangshenshuo
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "kuangshenshuo"
3) (integer) 1
1) "message"
2) "kuangshenshuo"
3) "hello"                 # 接收到来自kuangshenshuo频道的消息 "hello"
1) "message"
2) "kuangshenshuo"
3) "java"                  # 接收到来自kuangshenshuo频道的消息 "java"

```

`原理`  

![image-20220124143833822](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124143833822.png)

==来个图吧，更形象点==

![image-20220124144624880](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124144624880.png)

==使用场景：==

- 实时消息系统！
- 实时聊天（频道当做聊天室，将信息回显给所有人即可！）
- 订阅，关注系统都是可以的！
- 稍微复杂的场景我们就是用消息中间件MQ Kafka...   `专业的事情交给专业的人来做！！！`
- 群聊



## Redis主从复制：

`概念：`

主从复制，是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点(master/leader)，后者称为从节点(slave/follower)；==数据的复制是单向的，只能由主节点到从节点==。Master以写为主，Slave以读为主。

==默认情况下，每台Redis服务器都是主节点；==

且一个主节点可以有多个从节点(或没有从节点)，但一个从节点只能有一个主节点。

**主从复制的作用主要包括：**

1. 数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余的方式。

2. 故障恢复：当主节点故障时，从节点可以暂时替代主节点提供服务，是一种服务冗余的方式

3. 负载均衡：在主从复制的基础上，配合读写分离，由主节点进行写操作，从节点进行读操作，分担服务器的负载；尤其是在多读少写的场景下，通过多个从节点分担负载，提高并发量性能。

4. 高可用（集群）基石：主从复制还是哨兵和集群能够实施的基础。
   
5. 一般来说，要将Redis运用于工程项目中，只使用一台Redis是万万不能的（宕机），原因如下：

   1、从结构上，单个Redis服务器会发生单点故障，并且一台服务器需要处理所有的请求负载，压力较 大；

电商网站上的商品，一般都是一次上传，无数次浏览的，说专业点也就是"多读少写"。

对于这种场景，我们可以使如下这种架构：

![image-20220124145900158](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124145900158.png)

==主从复制，读写分离！百分之80的情况下都是在进行读操作！减缓服务器的压力！架构中经常使用==      最低配置，一主二从！

`只要在公司中，主从复制就是必须要使用的，因为在真实的项目中不可能单机使用redis！`



### 环境配置

只配置从库，不用配置主库

```
127.0.0.1:6379> info replication          #查看当前库的信息
# Replication
role:master                             #角色 master
connected_slaves:0           			 #没有主机
master_failover_state:no-failover
master_replid:a47d94c3e6244286323eef32bd2a7a699c39aa1e
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
```

复制3个配置文件，然后修改对应的信息

1.端口

2.log文件名字

3.dump.rdb名字

若主机的redis设置类密码，则配置文件中的 masterauth <master-password> 下面需要加masterauth 密码，不然搭建不了主从复制

![image-20220124151453721](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124151453721.png)



![image-20220124151403372](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124151403372.png)



![image-20220124151345870](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124151345870.png)

修改完毕后启动我们的三个redis服务器，可以通过进程信息查看

![image-20220124152235092](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124152235092.png)



### 一主二从

默认情况下，每台redis服务器都是主节点  我们一般情况只用配置从机就好了

配置模型：

![image-20220124194349857](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124194349857.png)

命令模式`SLAVEOF ip 老大端口` 设置主机    设置80,的主机 并且查看信息 81类似

```
127.0.0.1:6380> slaveof 127.0.0.1 6379    # slaveof 127.0.0.1 6379  找谁当老大
OK
127.0.0.1:6380> info replication    #
# Replication
role:slave
master_host:127.0.0.1
master_port:6379
master_link_status:up
master_last_io_seconds_ago:5
master_sync_in_progress:0
slave_read_repl_offset:0
slave_repl_offset:0
slave_priority:100
slave_read_only:1
replica_announced:1
connected_slaves:0
master_failover_state:no-failover
master_replid:0f72b8cdd87b92667f3a77dabf23f174bc994559
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:0

在主机从查看！
127.0.0.1:6379> info replication
# Replication
role:master
connected_slaves:2                         #多了从机的配置
slave0:ip=127.0.0.1,port=6380,state=online,offset=280,lag=0      #多了从机的配置
slave1:ip=127.0.0.1,port=6381,state=online,offset=280,lag=0      #多了从机的配置
master_failover_state:no-failover
master_replid:0f72b8cdd87b92667f3a77dabf23f174bc994559
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:280
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:280

```

真实的主从配置应该在文件配置，这样的化是永久的，我们这里使用的命令是暂时的

![image-20220124154223268](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124154223268.png)



主机可以设置值（可以写），从机不能（只能读）
主机中所有的数据都会被从机自动保存

主机写：

![image-20220124175515049](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124175515049.png)

从机只能读取内容：

![image-20220124175614196](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124175614196.png)

测试：主机断开连接，从机依旧能能连接到主机，但是没有写操作，这个时候，主机如果回来，从机依旧可以以获取主机信息

如果是使用命令行配置的主从，如果从机重启就会自动变为主机，只要变为从机，立马会从主机中获取值

> 复制原理

Slave机启动成功连接到一个master后会发送一个sync同步命令

Master接受到命令，启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕后，==master将传送整个数据文件到slave，并完成一次完全同步。==

==全量复制==：slave服务在接受到数据库文件数据后，将其存盘并加载到内存中。

==增量复制==：Master继续将所收集到的修改命令一次传给slave，完成同步
但是只要是重新连接一次master，全量复制会被自动执行，我们的数据可以在从机中看到





> 层层链路

上一个M连接下一个S

![image-20220124194610000](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124194610000.png)

这时候也可以完成我们的主从复制！！！

![image-20220124195524293](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124195524293.png)

![image-20220124195547830](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124195547830.png)

> 如果没有老大（79宕机）了，这个时候能不能选择一个老大出来呢？不然读功能无法实现

==谋权篡位==

如果主机断开了连接，我们可以使用`SLAVEOF no one`让自己变成主机！其他的节点就可以手动连接到这个最新的主节点（手动）

！如果这个时候老大修复了，那就得==重新连接==！

```
127.0.0.1:6380> SLAVEOF no one
OK
127.0.0.1:6380> info replication
# Replication
role:master
connected_slaves:1
slave0:ip=127.0.0.1,port=6381,state=online,offset=11214,lag=1
master_failover_state:no-failover
master_replid:a638f4e097a32f9b65841cf7d42531ecf282e2c3
master_replid2:0f72b8cdd87b92667f3a77dabf23f174bc994559
master_repl_offset:11214
second_repl_offset:11215
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:11214
```

修复老大并且查看：

```java
127.0.0.1:6379> info replication
# Replication
role:master
connected_slaves:0
master_failover_state:no-failover
master_replid:2cfa4acdbba40ef78ca6918df306469758d09b83
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
```



### 哨兵模式

自动选举老大的模式

> 概述

主从切换的方法是：当主服务器宕机后，需要手动把一台服务器切换为主服务器，需要人工干预，费时费力，还会造成一段时间内服务器不可用。更多的时候我们考虑的是哨兵模式，redis从2.8开始正式提供了sentinel架构来解决这个问题。

谋朝篡位的自动版，能够后台监控主机是否故障，如果故障根据投票数==自动将从机转换为主机==。

哨兵模式是一种特殊的模式，首先redis提供了哨兵命令，哨兵是一个独立的进程，作为进程，他会独立运行，其原理是**哨兵通过发送命令，等待redis服务器响应，从而监控运行的多个redis实例**

![image-20220124200318181](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124200318181.png)



哨兵的作用

1.通过发送命令，让redis返回其运行状态，包括主服务器和从服务器

2.当哨兵检测到master宕机，会自动将slave切换成master，然后通过发布订阅模式通知其他从服务器修改配置文件，让他们切换为主机

然而一个哨兵进程对redis服务器进行监控，可能会出现问题，为此，我们可以使用多个哨兵进行监控，各个哨兵相互监控，这样就形成了多哨兵模式。

![image-20220124200857198](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124200857198.png)

假设主服务器宕机，哨兵1先检测到这个结果，系统并不会立马进行failover过程，仅仅是哨兵1主观的认为主服务器不可用，这个现象称为==主观下线==。当后面的哨兵也检测到主服务器不可用，并且达到一定数量，那么哨兵之间会进行一次投票，投票的结果由一个哨兵发起，进行failover（故障转移操作）。切换成功后，通过发布订阅模式，让各个哨兵把自己的监控的从服务器实现切换主机，这个过称为==客观下线==



> 测试

我们目前的状态是一主二从（不是层层链路）！

1.配置哨兵配置文件在usr/local/binkwqconfig文件夹下 sentinel.conf 文件内容如下

```
# sentinel monitor 被监视的名称（随便取） 主机 端口 1
sentinel monitor myredis 127.0.0.1 6379 1
# sentinel auth-pass 监控主机名（和上面一样） redis密码（若redis没设置密码则不需要）
sentinel auth-pass myredis 347858300aB
```

==后面这个数字1，代表主机挂了，slave投票看让谁接替成为主机，票数最多的，就会成为主机！==

2.启动哨兵

```java
[root@iZwz9awxiy8x3ci8733ijbZ bin]# redis-sentinel kwqconfig/sentinel.conf
```

![image-20220124205557020](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124205557020.png)

如果Master节点断开了，这个时候就会从从机中随机选择一个服务器！！！（这里面有一个投票算法）

![image-20220124211158961](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124211158961.png)

![image-20220124211223174](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124211223174.png)

哨兵日志！

![image-20220124211448278](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124211448278.png)

==如果主机此时回来，只能归并到新的主机下，当作从机，这就是哨兵模式的规则。与此同时sentinel.conf监控的服务器变成了80！==

![image-20220124211655192](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124211655192.png)

> 哨兵模式

优点：

1.哨兵集群，基于主从复制模式，所有主从配置的优点他都有

2.主从可以切换，故障可以转移，系统可用性会更好

3.哨兵模式就是主从模式的升级，手动到自动，更加健壮

缺点：

1.redis不好在线扩容，集群容量一旦到达上限，在线扩容就十分麻烦  QWQ

2.实现哨兵模式的配置其实是很麻烦的，里面有很多选择

> 哨兵模式的全部配置（sentinel.conf）！  这些配置一般是由运维配置

```
# Example sentinel.conf 
# 哨兵sentinel实例运行的端口 默认26379      如果有哨兵集群，我们还需要配置每个 哨兵端口
port 26379
 
# 哨兵sentinel的工作目录
dir /tmp
 
# 哨兵sentinel监控的redis主节点的 ip port 
# master-name  可以自己命名的主节点名字 只能由字母A-z、数字0-9 、这三个字符".-_"组成。
# quorum 当这些quorum个数sentinel哨兵认为master主节点失联 那么这时 客观上认为主节点失联了
# sentinel monitor <master-name> <ip> <redis-port> <quorum>
sentinel monitor mymaster 127.0.0.1 6379 1
 
# 当在Redis实例中开启了requirepass foobared 授权密码 这样所有连接Redis实例的客户端都要提供密码
# 设置哨兵sentinel 连接主从的密码 注意必须为主从设置一样的验证密码
# sentinel auth-pass <master-name> <password>
sentinel auth-pass mymaster MySUPER--secret-0123passw0rd
 
 
# 指定多少毫秒之后 主节点没有应答哨兵sentinel 此时 哨兵主观上认为主节点下线 默认30秒
# sentinel down-after-milliseconds <master-name> <milliseconds>
sentinel down-after-milliseconds mymaster 30000
 
# 这个配置项指定了在发生failover主备切换时最多可以有多少个slave同时对新的master进行 同步，
这个数字越小，完成failover所需的时间就越长，
但是如果这个数字越大，就意味着越 多的slave因为replication而不可用。
可以通过将这个值设为 1 来保证每次只有一个slave 处于不能处理命令请求的状态。
# sentinel parallel-syncs <master-name> <numslaves>
sentinel parallel-syncs mymaster 1
 
 
 
# 故障转移的超时时间 failover-timeout 可以用在以下这些方面： 
#1. 同一个sentinel对同一个master两次failover之间的间隔时间。
#2. 当一个slave从一个错误的master那里同步数据开始计算时间。直到slave被纠正为向正确的master那里同步数据时。
#3.当想要取消一个正在进行的failover所需要的时间。  
#4.当进行failover时，配置所有slaves指向新的master所需的最大时间。不过，即使过了这个超时，slaves依然会被正确配置为指向master，但是就不按parallel-syncs所配置的规则来了
# 默认三分钟
# sentinel failover-timeout <master-name> <milliseconds>
sentinel failover-timeout mymaster 180000
 
# SCRIPTS EXECUTION
 
#配置当某一事件发生时所需要执行的脚本，可以通过脚本来通知管理员，例如当系统运行不正常时发邮件通知相关人员。
#对于脚本的运行结果有以下规则：
#若脚本执行后返回1，那么该脚本稍后将会被再次执行，重复次数目前默认为10
#若脚本执行后返回2，或者比2更高的一个返回值，脚本将不会重复执行。
#如果脚本在执行过程中由于收到系统中断信号被终止了，则同返回值为1时的行为相同。
#一个脚本的最大执行时间为60s，如果超过这个时间，脚本将会被一个SIGKILL信号终止，之后重新执行。
 
#通知型脚本:当sentinel有任何警告级别的事件发生时（比如说redis实例的主观失效和客观失效等等），将会去调用这个脚本，
#这时这个脚本应该通过邮件，SMS等方式去通知系统管理员关于系统不正常运行的信息。调用该脚本时，将传给脚本两个参数，
#一个是事件的类型，
#一个是事件的描述。
#如果sentinel.conf配置文件中配置了这个脚本路径，那么必须保证这个脚本存在于这个路径，并且是可执行的，否则sentinel无法正常启动成功。
#通知脚本
# sentinel notification-script <master-name> <script-path>
  sentinel notification-script mymaster /var/redis/notify.sh
 
# 客户端重新配置主节点参数脚本
# 当一个master由于failover而发生改变时，这个脚本将会被调用，通知相关的客户端关于master地址已经发生改变的信息。
# 以下参数将会在调用脚本时传给脚本:
# <master-name> <role> <state> <from-ip> <from-port> <to-ip> <to-port>
# 目前<state>总是“failover”,
# <role>是“leader”或者“observer”中的一个。 
# 参数 from-ip, from-port, to-ip, to-port是用来和旧的master和新的master(即旧的slave)通信的
# 这个脚本应该是通用的，能被多次调用，不是针对性的。
# sentinel client-reconfig-script <master-name> <script-path>
sentinel client-reconfig-script mymaster /var/redis/reconfig.sh

```

## Redis缓存穿透与雪崩（面试高频，工作常用！）

> 服务器的高可用问题

==在这里不会详细的分析解决方案的底层，因为初学者==

Redis缓存的使用，极大的提升了应用程序的性能和效率，特别是数据查询方面。但同时，也带来了一些问题。其中，最要害的问题，就是数据的一致性问题，从严格意义上讲，这个问题无解。==如果对数据的一致性要求很高，那么就不能使用redis==



### 缓存穿透(查不到)

> 概念

缓存穿透的概念很简单，用户想要查询一个数据，发现redis内存数据库没有，也就是缓存没有命中，于是向持久层数据库查询。发现也没有，于是本次查询失败，当用户很多的时候，缓存都没有命中（秒杀），于是都去请求了持久层数据库。这就会给持久层数据库造成大压力，这时候就相当于出现了缓存穿透。==可能出现恶意攻击的现象，来频繁访问数据库==

![image-20220124230607115](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124230607115.png)

> 解决方案

**布隆过滤器**

布隆过滤器是一种数据结构，对所有可能查询的参数以hash形式存储，先在控制层进行校验，不符合规则丢弃，从而避免了对底层存储系统的查询压力：

![image-20220124230840329](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124230840329.png)

**缓存空对象**

当存储层不命中后，即使返回空对象也将其缓存起来，同时会设置一个过期时间，之后在访问这个数据将会从缓存中获取，保护了后端的数据源

![image-20220124231047822](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124231047822.png)

但是这个方法存在一个问题

如果空值能被缓存下来，这就意味着缓存需要更多的空间存储更多的键，这个当中可能会有很多的空值和键





### 缓存击穿（量太大，缓存过期）

> 概述

缓存击穿是指一个key非常热点，不停的扛着大量并发，大并发集中对这个点进行访问，当key在这个瞬间失效，持续的大并发就穿破缓存，直接请求数据库，就像在一个屏障上凿开了一个洞。
当某个key在过期的瞬间，有大量请求并发访问，这类数据一般是热点数据，`由于缓存过期，会同时访问数据库来查询最新的数据，并且回写缓存，会导致数据库压力瞬间过大。`

`例子：`

微博服务器宕机（热点在某一条微博上，expire 60s过期  60.1s恢复 0.1秒大量请求直接访问mysql，扛不住  ）



> 解决方案

**设置热点数据永不过期**

这样就不会出现热点数据过期的情况，但是当Redis内存空间满的时候也会清理部分数据，而且此种方案会占用空间，一旦热点数据多了起来，就会占用部分空间。

**加互斥锁(分布式锁)**

==在访问key之前，采用SETNX（set if not exists）来设置另一个短期key来锁住当前key的访问，访问结束再删除该短期key。保证同时刻只有一个线程访问。这样对锁的要求就十分高。==

![image-20220124235612834](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220124235612834.png)

### 缓存雪崩

> 概念

缓存雪崩，是指在某一个时间段，缓存集中过期失效

缓存雪崩的原因之一，比如写文本时，马上就要到双十二零点，很快就迎来一波抢购，这波商品比较集中的放入了缓存假设缓存缓存一个小时。到凌晨一点钟，这批商品的缓存就都过期了。而对于这批商品的查询访问都落到数据库上，数据库产生周期性的压力波值。于是所有的请求都会达到存储层，存储层的调用量就会爆增，造成存储器也会挂掉的情况。
其实集中过期不是最致命的，比较致命的缓存雪崩是缓存服务器某个节点宕机或者断网。因为自然原因形成的缓存雪崩，一定是在某个时间段集中创建缓存，这个时候数据库也会顶住压力，无非是对数据库产生周期性的压力而已。而缓存服务节点的宕机，对数据库服务器造成的压力是不可预知的，很有可能瞬间就把数据库压垮。

![image-20220125000032515](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220125000032515.png)

其实集中过期，倒不是非常致命，比较致命的缓存雪崩，是缓存服务器某个节点宕机或者断网。因为自然形成的缓存雪崩，一定实在某个时间段缓存集中失效，这个时候，数据库也是可以顶住压力的。无非就是对数据库产生周期性的压力而已。而缓存服务节点的宕机，对数据库服务器造成的压力是不可预知的，很可能瞬间就把数据库压垮。

双十一：停掉一些服务。如退款==显示服务繁忙，请明天再试==（保证主要服务可用）

> 解决方案

**redis高可用**

这个思想的含义是，既然redis有可能挂掉，那我多增设几台redis，这样一台挂掉之后其他的还可以继续工作，其实就是搭建的集群

**限流降级（在springcloud讲过）**

这个解决方案的思想是，在缓存失效后，==通过`加锁`或者`队列`来控制读数据库写缓存的线程数量。比如对某个key只允许一个线程查询数据和写缓存，其他线程等待。==

**数据预热**

数据加热的含义就是在正式部署之前，我先把可能的数据先预先访问一遍，这样部分可能大量访问的数据就会加载到缓存中。在即将发生大并发访问前手动触发加载缓存不同的key，设置不同的过期时间，让缓存失效的时间点尽量均匀。



