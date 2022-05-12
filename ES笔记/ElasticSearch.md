ElasticSearch

版本：`ElasticSearch 7.6.1`

> 如何查数据？

SQL: like %红米%,   如果是大数据情况下，就十分慢。用了索引依旧达不到我们的标准

ElasticSearch:搜索！（百度、github、电商（淘宝，京东等......））

==以后你只要，需要用到搜索，就可以使用ES！！！（大数据的情况下使用）==



## 聊聊Doug Cutting

1998年9月4日,Google公司在美国硅谷成立,正如大家所知,它是一家做==搜索==引擎起家的公司

无独有偶，一位名叫**Doug Cutting**的美国工程师，也迷上了搜索引擎。他做了一个用于文本搜索的函数库（姑且理解为软件的功能组件），命名为**Lucene**。

Lucene是用JAVA写成的，目标是为各种中小型应用软件加入==全文检索功能==。因为好用而且==开源(代码公开）==，非常受程序员们的欢迎。

早期的时候，这个项目被发布在Doug Cutting的个人网站和SourceForge (一个开源软件网站)。后来，2001年底，Lucene成为**Apache软件基金会**jakarta项目的一个子项目。

2004年，Doug Cutting再接再励，在Lucene的基础上，和Apache开源伙伴Mike Cafarella合作，开发了一款可以代替当时的主流搜索的开源搜索引擎，命名为**Nutch**。

**Nutch** 是一个建立在Lucene核心之上的网页==搜索应用程序==，它在Lucene的基础上加了==爬虫==和==一些网页相关==的功能，目的就是从一个简单的站内检索推广到全球网络上的搜索上。

随着时间的推移，作为互联网搜索引擎，都**面临对象“体积”不断增大的问题**。**需要存储大量的网页**，并不断优化自己的搜索算法，提升搜索效率。

在2004年，Doug Cutting实现了**分布式文件存储系统**，并将它命名为**NDFS(Nutch Distributed File System)**。后来他加入了雅虎，将NDFS和MapReduce进行了改造，并重新命名为**Hadoop**(NDFS也改名为**HDFS,Hadoop Distributed File System)**. 这就是大名鼎鼎的**大数据框架系统--Hadoop**的由来，而Doug Cutting则被人称为Hadoop之父。



## ElasticSearch概述

Elaticsearch，简称为ES，ES是一个开源的==高扩展==的==分布式全文检索引擎==，它可以近乎==实时的存储、检索数据==；本身扩展性很好，可以扩展到上百台服务器，处理 PB 级别（大数据时代）的数据。ES由 Java 语言开发并使用 Lucene 作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的 ==RESTFULL API== 来隐藏 Lucene 的复杂性，从而让全文搜索变得简单。

据国际权威的数据库产品评测机构 DB Engines 的统计，在2016 年1月，ElasticSearch 已超过 Solr 等，==成为排名第一的搜索引擎类应用。==



> 历史：

多年前，一个叫做==Shay Banon==的刚结婚不久的失业开发者，由于妻子要去伦敦学习厨师，他便跟着也去了。在他找工作的过程中，为了给妻子构建一个==食谱的搜索引擎==，他开始构建一个早期版本的Lucene。

直接基于Lucene工作会比较困难，所以Shay==开始抽象Lucene代码以便lava程序员可以在应用中添加搜索功能==。他发布了他的第一个开源项目，叫做“Compass”。

后来Shay找到一份工作，这份工作处在高性能和内存数据网格的分布式环境中，因此高性能的、实时的、分布式的搜索引擎也是理所当然需要的。然后他决定重写Compass库使其成为一个独立的服务叫做==Elasticsearch。==

第一个公开版本出现在2010年2月，在那之后Elasticsearch已经成为Github上最受欢迎的项目之一，代码贡献者超过300人。一家主营Elasticsearch的公司就此成立，他们一边提供商业支持一边开发新功能，不过Elasticsearch将永远开源且对所有人可用。

Shay的妻子依旧等待着她的食谱搜索…..



> 谁在使用？

**谁在使用：**

1、维基百科,类似百度百科，全文检索,高亮

2、The Guardian (国外新闻网站) ,类似搜狐新闻,用户行为日志(点击,浏览,收藏,评论) +社交网络数据(对某某新闻的相关看法) ,数据分析,给到每篇新闻文章的作者,让他知道他的文章的公众反馈(好,坏,热门，垃圾,鄙视，崇拜)

3、Stack Overflow (国外的程序异常讨论论坛) , IT问题,程序的报错,提交上去,有人会跟你讨论和回答,全文检索,搜索相关问题和答案,程序报错了,就会将报错信息粘贴到里面去,搜索有没有对应的答案

4、GitHub (开源代码管理),搜索 上千亿行代码

5、电商网站,检索商品

6、日志数据分析, logstash采集日志, ES进行复杂的数据分析, ==**ELK技术, elasticsearch+logstash+kibana**==

7、商品价格监控网站,用户设定某商品的价格阈值,当低于该阈值的时候,发送通知消息给用户,比如说订阅牙膏的监控,如果高露洁牙膏的家庭套装低于50块钱,就通知我,我就去买

8、BI系统,商业智能, Business Intelligence。比如说有个大型商场集团，BI ,分析一下某某区域最近3年的用户消费 金额的趋势以及用户群体的组成构成,产出相关的数张报表, **区,最近3年,每年消费金额呈现100%的增长,而且用户群体85%是高级白领，开-个新商场。ES执行数据分析和挖掘, Kibana进行数据可视化

9、国内:站内搜索(电商,招聘,门户,等等),IT系统搜索(OA,CRM,ERP,等等),数据分析(ES热门
的一一个使用场景)



## ES和solr的差别

> ElasticSearch简介：

- Elasticsearch是一个**实时分布式搜索和分析引擎**。 它让你以前所未有的速度处理大数据成为可能。
- 它用于<mark>**全文搜索、结构化搜索、分析**</mark>以及将这三者混合使用:
- `维基百科`使用Elasticsearch提供**全文搜索**并**高亮关键字**,以及输入**实时搜索**(search-asyou-type)和**搜索纠错**(did-you-mean)等搜索建议功能。
- `英国卫报`使用Elasticsearch结合用户日志和社交网络数据提供给他们的编辑以实时的反馈,以便及时了解公众对新发表的文章的回应。
- `StackOverflow`结合全文搜索与地理位置查询,以及more-like-this功能来找到相关的问题和答案。
- `Github`使用Elasticsearch检索1300亿行的代码。
- 但是Elasticsearch不仅用于大型企业，它还让像`DataDog`以及`Klout`这样的创业公司将最初的想法变成可扩展的解决方案。
- Elasticsearch可以在你的笔记本上运行,也可以在数以百计的服务器上处理PB级别的数据。
- Elasticsearch是一个基于Apache Lucene(TM)的开源搜索引擎。无论在开源还是专有领域, Lucene可被认为是迄今为止最先进、性能最好的、功能最全的搜索引擎库。
  - 但是, **Lucene只是一个库**。 想要使用它,你必须使用Java来作为开发语言并将其直接集成到你的应用中,更糟糕的是, Lucene非常复杂,你需要深入了解检索的相关知识来理解它是如何工作的。
- Elasticsearch也使用Java开发并使用Lucene作为其核心来实现所有索引和搜索的功能,但是它的**目的**是<mark>通过简单的**RESTful API**来隐藏Lucene的复杂性,从而让全文搜索变得简单。</mark>

> Solr简介

- Solr是Apache下的一个顶级开源项目,采用Java开发,它是**基于Lucene的全文搜索服务器**。Solr提供了比Lucene更为**丰富的查询语言**,同时实现了**可配置**、**可扩展**，并**对索引、搜索性能进行了优化**
- Solr可以**独立运行**,运行在letty. Tomcat等这些Selrvlet容器中 , Solr 索引的实现方法很简单,<mark>用POST方法向Solr服务器发送一个描述Field及其内容的XML文档, Solr根据xml文档**添加、删除、更新**索引</mark>。Solr 搜索只需要发送HTTP GET请求,然后对Solr返回xml、json等格式的查询结果进行解析,组织页面布局。
- Solr不提供构建UI的功能, **Solr提供了一个管理界面,通过管理界面可以查询Solr的配置和运行情况。**
- Solr是基于lucene开发企业级搜索服务器,实际上就是封装了lucene.
- Solr是一个独立的企业级搜索应用服务器,它**对外提供类似于Web-service的API接口**。用户可以通过http请求,向搜索引擎服务器提交-定格式的文件,生成索引;也可以通过提出查找请求,并得到返回结果。

> ElasticSearch与Solr比较

```
当单纯的对已有数据进行搜索时，Solr更快
```

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204042228207.png)

```
当实时建立索引时，Solr会产生io阻塞，查询性能较差，ElasticSearch具有明显的优势
```

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204042228284.png)

```
随着数据量的增加，Solr的搜索效率会变得更低，而ElasticSearch却没有明显的变化
```

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204042228041.png)

```
转变我们的搜索基础设施后从Solr ElasticSearch，我们看见一个即时~ 50x提高搜索性能！
```

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204042228150.png)

> 总结

1、**es**基本是**开箱即用**(解压就可以用!) ,非常简单。Solr安装略微复杂一丢丢!

2、**Solr 利用Zookeeper进行分布式管理**,而**Elasticsearch<mark>自身带有分布式协调管理功能</mark>。**

3、Solr 支持更多格式的数据,比如JSON、XML、 CSV ,而**Elasticsearch仅支持json文件格式**。

4、Solr 官方提供的功能更多,而Elasticsearch本身更注重于核心功能，`高级功能多有第三方插件提供，例如图形化界面需要kibana友好支撑`

<mark>5、</mark>**Solr 查询快,但更新索引时慢(即插入删除慢)** ，用于电商等查询多的应用;

- **ES建立索引快(即查询慢)** ，即**实时性查询快**，用于facebook新浪等搜索。
- Solr是传统搜索应用的有力解决方案，但Elasticsearch更适用于新兴的`实时`搜索应用。(==因为建立索引快==)

6、Solr比较成熟，有一个更大，更成熟的用户、开发和贡献者社区，而Elasticsearch相对开发维护者较少,更新太快,学习使用成本较高。



## ElasticSearch安装

```
JDK8，最低要求
```

使用Java开发，必须保证`ElasticSearch`的版本与Java的核心jar包版本（`maven依赖`）对应！（Java环境保证没错）

下载地址：https://www.elastic.co/cn/downloads/

历史版本下载：https://www.elastic.co/cn/downloads/past-releases/

==官网下载巨慢，翻墙，网盘中下载即可！==

==我们学习的话 Window 和 Linux 都可以学习！==

==我们这里在Window学习！==

==ELK三剑客，解压即用！==



> window下安装！

1.安装包解压就能使用了

2.熟悉目录：

![image-20220404234114186](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204042341271.png)

```
bin 启动文件目录
config 配置文件目录
    1og4j2 日志配置文件
    jvm.options java 虚拟机相关的配置(默认启动占1g内存，内容不够需要自己调整)
    elasticsearch.ym1 elasticsearch 的配置文件! 默认9200端口!存在跨域!
1ib 
    相关jar包
modules 功能模块目录
plugins 插件目录
    ik分词器
```

> 调整虚拟机启动的大小

![image-20220404231532226](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204042315293.png)

3.启动，进入ES目录的bin目录，双击elasticsearch.bat

![image-20220404234204401](../AppData/Roaming/Typora/typora-user-images/image-20220404234204401.png)

4.访问9200

![image-20220404235139986](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204042351048.png)



> head插件安装

==需要依赖环境：npm、node.js==

地址:[mobz/elasticsearch-head: A web front end for an elastic search cluster (github.com)](https://github.com/mobz/elasticsearch-head/)

下载解压后：

![image-20220405085424015](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050854130.png)

下载依赖：

![image-20220405085708743](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050857807.png)

启动：

![image-20220405085748979](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050857031.png)

访问9100端口（要先启动es），发现会存在==跨域问题==

解决方案：

在`es安`装目录的`conf`目录下的`elasticsearch.yml`末尾添加`这两句`

![image-20220405090240533](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050902587.png)

再次访问9100：

![image-20220405090529966](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050905044.png)

> 新建索引：

![image-20220405090952332](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050909423.png)

![image-20220405093548955](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050935012.png)

我们初学，就把es当成一个数据库就好了（可以建立索引（库），文档（库中的数据！））

==这个head我们就把它当做数据展示工具!我们后面所有的查询，去Kibana操作,因为head无法将json格式化==



## Kibana的安装

> 了解ELK

ELK是Elasticsearch、Logstash、 Kibana三大开源框架首字母大写简称,市面上也被成为Elastic Stack。其中Elasticsearch是一个基于Lucene、分布式、通过Restful方式进行交互的近实时搜索平台框架。像类似百度、谷歌这种大数据全文搜索引擎的场景都可以使用Elasticsearch作为底层支持框架，可见Elasticsearch提供的搜索能力确实强大,市面上很多时候我们简称Elasticsearch为es。

Logstash是ELK的中央数据流引擎,用于从不同目标(文件/数据存储/MQ )收集的不同格式数据,经过过滤后支持输出到不同目的地(文件/MQ/redis/elasticsearch/kafka等)。

Kibana可以将elasticsearch的数据通过友好的页面展示出来 ,提供实时分析的功能。

市面上很多开发只要提到ELK能够一致说出它是一个日志分析架构技术栈总称 ,但实际上ELK不仅仅适用于日志分析,它还可以支持其它任何数据分析和收集的场景,日志分析和收集只是更具有代表性。并非唯一性。

```
收集清洗数据(Logstash) ==> 搜索、存储(ElasticSearch) ==> 展示(Kibana)
```

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050939779.png)

> Kibana概述

Kibana是一个针对ElasticSearch的开源分析及可视化平台,用来搜索、查看交互存储在Elasticsearch索引中的数据。使用Kibana ,可以通过各种图表进行高级数据分析及展示。Kibana让海量数据更容易理解。它操作简单,基于浏览器的用户界面可以快速创建仪表板( dashboard )实时显示Elasticsearch查询动态。设置Kibana非常简单。无需编码或者额外的基础架构,几分钟内就可以完成Kibana安装并启动Elasticsearch索引监测。

### 1、下载地址:

> 下载的版本需要与ElasticSearch版本对应

下载地址:[Get Started with Elasticsearch, Kibana, and the Elastic Stack | Elastic](https://www.elastic.co/cn/start)



### 2、安装

解压即可（尽量将ElasticSearch相关工具放在统一目录下）

> 查看目录：

![image-20220405092947890](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050929972.png)

> 启动：

![image-20220405093019548](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050930612.png)

> 默认端口：

![image-20220405093127861](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050931928.png)



> 访问：localhost:5601

![image-20220405093256200](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050932307.png)



#### 1.开发工具

（Postman、curl、head、谷歌浏览器插件）

> 可以使用 `Kibana`进行测试

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050939081.png)

> 如果说，你在英文方面不太擅长，kibana是支持汉化的

#### 2.kibana汉化

编辑器打开`kibana解压目录/config/kibana.yml`，添加

```
i18n.locale: "zh-CN"
```

> 重启kibana

**汉化成功**

![汉化成功](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050939689.png)



## ElasticSearch核心概念

> 概述

在前面的学习中，我们已经掌握了es是什么，同时也把es的服务已经安装启动，那么es是如何去存储数据的，数据结构又是什么，又是如何实现搜索的呢？我们先来聊聊ElasticSearch的相关概念吧！



> ElasticSearch是面向文档，关系行数据库和ElasticSearch客观对比！一切都是JSON！

| Relational DB      | ElasticSearch          |
| ------------------ | ---------------------- |
| 数据库（database） | 索引（indices）        |
| 表（tables）       | types \<慢慢会被弃用!> |
| 行（rows）         | documents              |
| 字段（columns）    | fields                 |

**elasticsearch**中可以包含多个**索引（数据库）**,每个索引中可以包含多个**类型（表）** ,每个类型下又包含多个**文档（行）** ,每个文档中又包含多个**字段（列）**。

> 物理设计:

elasticsearch在后台把**每个索引划分成多个分片**，每分分片可以在集群中的不同服务器间迁移

一个人就是一个集群! ，即**启动的ElasticSearch服务，默认就是一个集群，且默认集群名为elasticsearch**

![image-20220405095330517](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050953760.png)

> 逻辑设计:

一个索引类型中，包含多个文档，比如说文档1，文档2。当我们索引一篇文档时，可以通过这样的顺序找到它：索引 => 类型 => 文档ID ，通过这个组合我们就能索引到某个具体的文档。 注意：ID不必是整数，实际上它是个字符串。

> 文档（”行“）

之前说elasticsearch是面向文档的，那么就意味着**索引和搜索数据的最小单位是文档**，elasticsearch中，文档有几个重要属性:

- 自我包含，一篇文档同时包含字段和对应的值，也就是同时包含key:value !
- ==可以是层次型的，一个文档中包含自文档，复杂的逻辑实体就是这么来的! {就是一个json对象 ! fastjson进行自动转换 !}==
- 灵活的结构，文档不依赖预先定义的模式，我们知道关系型数据库中，要提前定义字段才能使用，在elasticsearch中，对于字段是非常灵活的，有时候,我们可以忽略该字段，或者动态的添加一个新的字段。

尽管我们可以随意的新增或者忽略某个字段，但是，每个字段的类型非常重要，比如一个年龄字段类型，可以是字符串也可以是整形。因为elasticsearch会保存字段和类型之间的映射及其他的设置。这种映射具体到每个映射的每种类型，这也是为什么在elasticsearch中，类型有时候也称为映射类型。

> 类型（“表”）

类型是文档的逻辑容器，就像关系型数据库一样，表格是行的容器。类型中对于字段的定义称为映射，比如name映射为字符串类型。我们说文档是无模式的，它们不需要拥有映射中所定义的所有字段，比如新增一个字段，那么elasticsearch是怎么做的呢?

- ==elasticsearch会自动的将新字段加入映射，但是这个字段的不确定它是什么类型，elasticsearch就开始猜，如果这个值是18，那么elasticsearch会认为它是整形。但是elasticsearch也可能猜不对，所以最安全的方式就是提前定义好所需要的映射，这点跟关系型数据库殊途同归了，先定义好字段，然后再使用，别整什么幺蛾子。==

> 索引（“库”）

索引是映射类型的容器， elasticsearch中的索引是一个非常大的文档集合。 索引存储了映射类型的字段和其他设置。然后它们被存储到了各个分片上了。我们来研究下分片是如何工作的。

**物理设计：节点和分片 如何工作**

创建新索引

![创建新索引](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050948455.png)

一个集群至少有一个节点，而一个节点就是一个elasricsearch进程，节点可以有多个索引默认的，如果你创建索引，那么索引将会有个5个分片(primary shard ,又称主分片)构成的，每一个主分片会有一个副本(replica shard，又称复制分片)

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204050948572.png)

上图是一个有3个节点的集群，可以看到主分片和对应的复制分片都不会在同一个节点内，这样有利于某个节点挂掉了，数据也不至于失。实际上，**一个分片是一个Lucene索引（<mark>一个ElasticSearch索引包含多个Lucene索引</mark>）** ，**一个包含倒排索引的文件目录，倒排索引的结构使得elasticsearch在不扫描全部文档的情况下，就能告诉你哪些文档包含特定的关键字**。不过，等等，倒排索引是什么鬼?

**倒排索引（Lucene索引底层）**

> 简单说就是 按（文章关键字，对应的文档\<0个或多个\>）形式建立索引，根据关键字就可直接查询对应的文档（含关键字的），无需查询每一个文档，如下图

![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201125003531.png)



## IK分词器(elasticsearch插件)

> 什么是IK分词器?

分词：即把一段中文或者别的划分成一个个的关键字，我们在搜索时候会把自己的信息进行分词，==会把数据库中或者索引库中的数据进行分词，然后进行一一个匹配操作==，**默认的中文分词是将每个字看成一个词**（<mark>不使用用IK分词器的情况下</mark>），比如“我爱狂神”会被分为”我”，”爱”，”狂”，”神” ，这显然是不符合要求的，所以我们需要安装中文分词器ik来解决这个问题。

**IK提供了两个分词算法**: ik_smart和ik_max_wordm其中ik_smart为**最少切分**, k_max_word为**最细粒度划分**!

==1、下载==

> 版本要与ElasticSearch版本对应

下载地址：https://github.com/medcl/elasticsearch-analysis-ik/releases

==2、安装==

解压压即可（但是我们需要解压到ElasticSearch的plugins目录下）

![image-20220405102403519](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051024715.png)

==3、重启ElasticSearch，观察ES==

> 加载了IK分词器

![image-20220405102636239](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051026363.png)

==4、使用 `ElasticSearch安装目录/bin/elasticsearch-plugin` 可以查看插件==

![image-20220405102835482](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051028580.png)

==5、使用kibana测试==

> 查看不同的分词效果：

`ik_smart`：最少切分

![image-20220405103752603](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051037812.png)

`ik_max_word`：最细粒度划分（穷尽词库的可能）

![image-20220405103827578](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051038863.png)

> 从上面看，感觉分词都比较正常，但是大多数，分词都满足不了我们的想法，如下例  

![image-20220405104256315](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051042493.png)

> 我们不想让狂神说分开，那么，我们需要手动将该词添加到分词器的词典当中



==6、添加自定义的词添加到扩展字典中==

在`elasticsearch-analysis-ik-7.6.1（ik）`目录下的conf目录下自定义一个`.dic文件`

![image-20220405105110430](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051051679.png)

```
在elasticsearch目录/plugins/elasticsearch-analysis-ik-7.6.1/config/IKAnalyzer.cfg.xml中添加刚刚写的.dic文件
```

![image-20220405105244443](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051052736.png)

> 重启es

![image-20220405105750756](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051057907.png)



## Rest风格说明以及增删查改

**Rest风格是一种软件架构风格**,而不是标准,只是提供了一组设计原则和约束条件。它主要用于客户端和服务器交互类的软件。基于这个风格设计的软件可以**更简洁**，**更有层次**，**更易于实现缓存**等机制。



> 基本Rest命令说明：

|      method      |                     url地址                     |          描述          |
| :--------------: | :---------------------------------------------: | :--------------------: |
| PUT（创建,修改） |     localhost:9200/索引名称/类型名称/文档id     | 创建文档（指定文档id） |
|   POST（创建）   |        localhost:9200/索引名称/类型名称         | 创建文档（随机文档id） |
|   POST（修改）   | localhost:9200/索引名称/类型名称/文档id/_update |        修改文档        |
|  DELETE（删除）  |     localhost:9200/索引名称/类型名称/文档id     |        删除文档        |
|   GET（查询）    |     localhost:9200/索引名称/类型名称/文档id     |   查询文档通过文档ID   |
|   POST（查询）   | localhost:9200/索引名称/类型名称/文档id/_search |      查询所有数据      |

> 测试

1、创建一个索引，添加

```
PUT /索引名/类型/文档id
{请求体}
```

![image-20220405120330283](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051203513.png)

用head查看

![image-20220405120522221](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051205351.png)

![image-20220405120609721](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051206928.png)==完成了自动增加索引！数据也成功的添加了，这就是我说大家在初期可以把它当做数据库学习的原因！==

> 查询文档记录：根据id来查

![image-20220405143715013](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051437086.png)



> 字段数据类型

- 字符串类型

  text、keyword

- 数值型

  long、Integer、short、byte、double、float、**half float**、**scaled float**

- 日期类型

  date

- te布尔类型

  boolean

- 二进制类型

  binary

- 等等…

> 指定字段的类型（使用PUT）

==类似于建库（建立索引和字段对应类型），也可看做规则的建立==

![image-20220405121315235](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051213531.png)

去head中查看

![image-20220405121401504](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051214806.png)

> 获取建立的规则

![image-20220405121621828](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051216194.png)



> 查看默认的信息

![image-20220405121925389](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051219587.png)

![image-20220405122000535](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051220683.png)

![image-20220405122114427](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051221556.png)

==如果自己的文档字段没有指定，那么es就会给我们默认配置字段类型==

扩展：通过`get _cat/` 可以获取ElasticSearch的当前的很多信息！

![image-20220405122922321](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051229497.png)



> 修改 

es中的案例

![image-20220405141201603](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051412679.png)

曾经!

![image-20220405141329781](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051413848.png)

![image-20220405141357925](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051413008.png)

`弊端:`

==修改的时候需要将字段写全，否则其他字段就会消失==

现在！

先恢复修改的记录（版本号变3），然后执行下面的请求

![image-20220405141742507](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051417593.png)

![image-20220405124317506](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051243680.png)

==现在这种方法，并不用写全字段即可完成我们想要的修改==

> 删除索引！

通过DELETE命令删除，根据你的请求来判断删除索引还是删除文档记录！

使用RESTFUL风格是我们es推荐大家使用的

==删除文档记录！==

![image-20220405140821285](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051408386.png)

![image-20220405140844784](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051408854.png)

==删除索引==

![image-20220405140935377](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051409433.png)

可以发现test2不见了

![image-20220405141015090](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051410157.png)



## 花式查询详解

> 以下这些查询mysql也可以做，只是mysql的效率比较低！

- 匹配
- 按照条件匹配
- 精确匹配
- 区间范围匹配
- 匹配字段过滤
- 多条件查询
- 高亮查询

> 搭建环境：

![image-20220405145634471](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051456542.png)

![image-20220405145717509](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051457573.png)

> 模糊查询

![image-20220405150233733](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051502852.png)

![image-20220405150447719](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051504826.png)

> 为了演示需要多加一个用户

![image-20220405150632689](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051506741.png)

> 查询：

![image-20220405151128695](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051511796.png)

![image-20220405151044537](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051510629.png)

==查询匹配记录的指定字段（结果过滤）==

![image-20220405151429516](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051514609.png)`我们之后使用java操作es，所有的方法和对象就是这里面的key！`



> 通过某个字段进行排序    升序改成asc即可

![image-20220405151922289](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051519389.png)



> 分页查询

![image-20220405152218839](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051522926.png)

==数据下标还是从0开始的，和我们的mysql分页一样==



> 布尔值查询   

`must（相当于and），所有的条件都要符合`

![image-20220405154131316](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051541408.png)

`should(or),满足其中一个条件即可被查询出来`

![image-20220405154322561](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051543657.png)

`must_not`  ==查询年龄不是23的记录==

![image-20220405154528985](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051545083.png)

`过滤器filter`

- gt    大于
- lt    小于
- gte   大于等于
- lte   小于等于

![image-20220405154907864](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051549972.png)![image-20220405155114315](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051551409.png)



> 匹配多个条件

![image-20220405155708191](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051557286.png)

> 精确查询

`term`查询是通过倒排索引指定的此条进行精确查询的！ 若字段不是keyword则正常匹配查询

**两个类型 text keyword**

![image-20220405160831386](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051608456.png)

==插入两条数据==

![image-20220405160956036](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051609107.png)

==查看索引信息：==

![image-20220405161039025](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051610099.png)



![image-20220405161142200](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051611265.png)

可以看到上面 狂神说java name 没有被分词器解析 `原因是keyword字段类型不会被分词器解析`

![image-20220405161223891](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051612995.png)

可以看到被拆分了

![image-20220405161644181](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051616272.png)

![image-20220405161755242](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051617329.png)



> 多值查询

插入两条文档

![image-20220405194845795](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051948883.png)

去head中查看所有记录

![image-20220405194915683](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051949777.png)

![image-20220405195251584](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051952832.png)



> 高亮查询

![image-20220405195555371](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051955553.png)

![image-20220405195903288](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204051959675.png)



## SpringBoot中集成ES

> 官网：

[Elastic Stack and Product Documentation | Elastic](https://www.elastic.co/guide/index.html)

![image-20220408141323690](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081413893.png)

![image-20220408141415015](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081414192.png)

![image-20220408141557389](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081415557.png)

> 继续点进去，就有文档教你如何用

![image-20220408141652814](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081416964.png)

1、找到原生的依赖

```xml
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
    <version>7.6.2</version>
</dependency>
```

2、找对象

![image-20220408141909996](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081419144.png)



> 配置基本的项目环境  新建一个springboot项目

![image-20220408142139875](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081421143.png)

> 分析依赖   点开es的依赖

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>
```

![image-20220408144114321](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081441553.png)

再点进去发现封装了es高级客户端

![image-20220408144214703](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081442952.png)

![image-20220408144304416](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081443659.png)

> 因为封装了es高级客户端因此我们可以在maven中看到es客户端的版本，这个版本一定要和我们使用的es的版本一致

![image-20220408150252870](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081502023.png)

我们需要切es版本，保持和我们本地的一致！如何操作？

点开我们的2.2.5的boot 找到es的信息

![image-20220408150550714](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081505918.png)

在properties中添加下面这行

![image-20220408150730809](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081507974.png)

==再次查看==

![image-20220408153446796](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081534033.png)

> 配置类   和官网一样操作就对了

```
package com.kwq.config;

import org.apache.http.HttpHost;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestHighLevelClient;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
//spring两步骤：
//1. 找对象
//2. 放到spring中待用
//3.如果是springboot 就先分析源码
// xxxAutoConfiguration xxxProperties
@Configuration   //将@Bean注入spring中
public class ElasticSearchClientConfig {
    @Bean
    public RestHighLevelClient restHighLevelClient(){
        RestHighLevelClient client=new RestHighLevelClient(
                RestClient.builder(
                        new HttpHost("127.0.0.1",9200,"http")));
                return client;
    }
}

```

> 外部库中找到这个：

![image-20220408151639117](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081516423.png)

点开es

![image-20220408151754848](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081517977.png)

> restClient：

![image-20220408152347908](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081523022.png)

![image-20220408152913326](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081529488.png)

> es的自动装配类

![image-20220408152558784](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081525144.png)

![image-20220408152629630](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081526908.png)

### 具体的API测试---索引

==索引的创建==

```
@Autowired
@Qualifier("restHighLevelClient")   //指定类名去找spring中的bean然后地址给client
private RestHighLevelClient client;

//测试索引的创建
@Test
void contextLoads() throws IOException {
    //1.创建索引请求
    CreateIndexRequest request = new CreateIndexRequest("kuang_index");
    //2.客户端执行请求 IndicesClient，请求后得到响应
    CreateIndexResponse createIndexResponse = client.indices().create(request, RequestOptions.DEFAULT);

    System.out.println(createIndexResponse);

}
```

![image-20220408155711650](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081557861.png)

`Kibana中查看`

![image-20220408155933279](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081559469.png)

==测试获取索引==

```java
//测试获取索引
@Test
void testExistIndex() throws IOException {
    GetIndexRequest request = new GetIndexRequest("kuang_index");
    boolean exists = client.indices().exists(request, RequestOptions.DEFAULT);
    System.out.println(exists); //输出true

}
```

==测试索引的删除==

```
@Test
void testDeleteIndex() throws IOException {
    DeleteIndexRequest request = new DeleteIndexRequest("kuang_index");
    AcknowledgedResponse delete = client.indices().delete(request, RequestOptions.DEFAULT);
    System.out.println(delete.isAcknowledged()); //输出是否删除成功 返回true

}
```

`利用kibana查看发现确实删除了`



### 具体的API测试---文档

> 新建实体类

```java
package com.kwq.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@AllArgsConstructor
@NoArgsConstructor
@Data
public class User {
    private String name;
    private int age;
}
```

> 引入json依赖：

```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.79</version>
</dependency>
```

> 添加文档：

```java
//测试添加文档
@Test
void testAddDocument() throws IOException {
  //创建对象
    User user = new User("狂神说", 3);
//创建请求
    IndexRequest request = new IndexRequest("kuang_index");
    //规则 put /kuang_index/_doc/1
    request.id("1");
    //设置超时时间  //等价于 request.timeout("1s")
    request.timeout(TimeValue.timeValueSeconds(1));
    //将我们的数据放入请求 json
    request.source(JSON.toJSONString(user), XContentType.JSON); //转换成json传入进去
    System.out.println(JSON.toJSONString(user));
    //客户端发送请求 获取响应的结果
    IndexResponse indexResponse = client.index(request, RequestOptions.DEFAULT);
    System.out.println(indexResponse.toString());  
    System.out.println(indexResponse.status());  //对应我们命令返回的状态  CREATED
}
```

> 输出：

![image-20220408163017992](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081630279.png)

==Kibana查看==

![image-20220408163427082](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081634262.png)

> 获取文档，判断是否存在

```java
//获取文档，判断是否存在
@Test
void testIsExists() throws IOException {
    GetRequest getRequest = new GetRequest("kuang_index", "1");
    //不获取返回的_source的上下文了
    getRequest.fetchSourceContext(new FetchSourceContext(false));
    getRequest.storedFields("_none_");
    boolean exists = client.exists(getRequest, RequestOptions.DEFAULT);
    System.out.println(exists);//返回true
}
```

这样也ok

![image-20220408165720667](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081657808.png)



> 获取文档的信息

```java
//获取文档信息
@Test
void testGetDocument() throws IOException {
    GetRequest getRequest = new GetRequest("kuang_index", "1");
    GetResponse getResponse = client.get(getRequest, RequestOptions.DEFAULT);
    System.out.println(getResponse.getSourceAsString());//打印文档的内容
    System.out.println(getResponse); //返回的全部内容和命令式一样的
}
```

![image-20220408170129812](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081701006.png)

![image-20220408170211316](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081702409.png)



> 更新文档信息

```java
//更新文档信息
@Test
void testUpdateRequest() throws IOException {
    UpdateRequest updateRequest = new UpdateRequest("kuang_index", "1");
    updateRequest.timeout("1s");

    User user = new User("狂神说", 18);
    updateRequest.doc(JSON.toJSONString(user),XContentType.JSON);

    UpdateResponse updateResponse = client.update(updateRequest, RequestOptions.DEFAULT);
    System.out.println(updateResponse.status()); //输出ok 查看kibana发现确实更新了
}
```

上面的`.doc`和下面的命令对应

![image-20220408171002141](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081710379.png)



> 删除文档记录：

```java
//删除文档记录
@Test
void testDeleteRequest() throws IOException {
    DeleteRequest request = new DeleteRequest("kuang_index", "1");
    request.timeout("1s");

    DeleteResponse deleteResponse = client.delete(request, RequestOptions.DEFAULT);

    System.out.println(deleteResponse.status());//输出ok
}
```

> 批量操作

```java
  //特殊的，真实项目一般都会批量插入数据
    @Test
    void testBulkRequest() throws IOException {
        BulkRequest bulkRequest = new BulkRequest();
        bulkRequest.timeout("10s");

        ArrayList<User> userList = new ArrayList<>();
        //批处理请求
        userList.add(new User("kuangshen1",3));
        userList.add(new User("kuangshen2",3));
        userList.add(new User("kuangshen3",3));
        userList.add(new User("qinqiang1",3));
        userList.add(new User("qinqiang1",3));
        userList.add(new User("qinqiang1",3));
        for (int i=0;i<userList.size();i++){
            // 批量更新和批量删除，就在这里修改对应的请求就可以了
            bulkRequest.add(
                    new IndexRequest("kuang_index")
                    .id(""+(i+1))   //设置文档的id，如果不设置就是随机值
                    .source(JSON.toJSONString(userList.get(i)),XContentType.JSON));
        }
        BulkResponse bulkResponse = client.bulk(bulkRequest, RequestOptions.DEFAULT);
        System.out.println(bulkResponse.hasFailures());//这里返回了false， 是否失败，返回false代表成功
    }
}
```

![image-20220408192733999](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081927225.png)

==把上面的.id()去掉,再进行一次批量操作==

![image-20220408193231439](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081932697.png)

==批量删除==（修改类似）

```
//特殊的，真实项目一般都会批量插入数据
@Test
void testBulkRequest() throws IOException {
    BulkRequest bulkRequest = new BulkRequest();
    bulkRequest.timeout("10s");

    ArrayList<User> userList = new ArrayList<>();
    //批处理请求
    userList.add(new User("kuangshen1",3));
    userList.add(new User("kuangshen2",3));
    userList.add(new User("kuangshen3",3));
    userList.add(new User("qinqiang1",3));
    userList.add(new User("qinqiang1",3));
    userList.add(new User("qinqiang1",3));
    for (int i=0;i<userList.size();i++){
        bulkRequest.add(new DeleteRequest("kuang_index").id(i+""));
    }
    BulkResponse bulkResponse = client.bulk(bulkRequest, RequestOptions.DEFAULT);
    System.out.println(bulkResponse.hasFailures());//这里返回了false， 是否失败，返回false代表成功
}
```

![image-20220408193747372](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081937486.png)



> 查询

```java
//查询
//searchRequest 搜索请求
//SearchSourceBuilder 条件构造
//HighLightBuilder 构造高亮
//TermQueryBuilder 精确查询
//MatchAllQueryBuilder 
@Test
void testSearch() throws IOException {
    SearchRequest searchRequest = new SearchRequest("kuang_index");
    //构建搜索条件
    SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
    //查询条件，我们可以使用QueryBuilders 工具来实现
    //QueryBuilders.termQuery() 精确匹配
    //QueryBuilders.matchAllQuery() 匹配所有
    
    TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("name", "qinqiang1");
   // MatchAllQueryBuilder matchAllQueryBuilder = QueryBuilders.matchAllQuery();
    sourceBuilder.query(termQueryBuilder);
    sourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS));

    searchRequest.source(sourceBuilder);

    SearchResponse searchResponse = client.search(searchRequest, RequestOptions.DEFAULT);

    System.out.println(JSON.toJSONString(searchResponse.getHits()));
    System.out.println("============================");
    for (SearchHit documentFields : searchResponse.getHits().getHits()) {
        System.out.println(documentFields.getSourceAsMap());
    }
}
```

> 输出：

![image-20220408195813851](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204081958015.png)



## 实战之京东搜索（高亮）

> 最终实现大致效果：

![image-20220408200518115](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204082005386.png)

### 项目搭建

> 新建项目：

![image-20220408201125498](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204082011697.png)

> 接着切boot版本和es版本：  pom如下

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.kwq</groupId>
    <artifactId>kwq-es-jd</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>kwq-es-jd</name>
    <description>Demo project for Spring Boot</description>
    <parent>
        <artifactId>spring-boot-starter-parent</artifactId>
        <groupId>org.springframework.boot</groupId>
        <version>2.2.5.RELEASE</version>
        <relativePath/>
    </parent>
    <properties>
        <java.version>1.8</java.version>
        <!--自定义es版本依赖 保证和本地一致-->
        <elasticsearch.version>7.6.1</elasticsearch.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <spring-boot.version>2.2.5.RELEASE</spring-boot.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.79</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>2.3.7.RELEASE</version>
                <configuration>
                    <mainClass>com.kwq.KwqEsJdApplication</mainClass>
                </configuration>
                <executions>
                    <execution>
                        <id>repackage</id>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

</project>
```

> 修改配置文件

```
# 应用名称
spring.application.name=kwq-es-jd
# 应用服务 WEB 访问端口
server.port=9090
# 关闭模板缓存（默认值： true ）
spring.thymeleaf.cache=false
```

> 导入狂神给的静态资源

![image-20220408202114192](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204082021294.png)

> index.html具体源码

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">

<head>
    <meta charset="utf-8"/>
    <title>狂神说Java-ES仿京东实战</title>
    <link rel="stylesheet" th:href="@{/css/style.css}"/>
    <script th:src="@{/js/jquery.min.js}"></script>
</head>

<body class="pg">
<div class="page">
    <div id="mallPage" class=" mallist tmall- page-not-market ">

        <!-- 头部搜索 -->
        <div id="header" class=" header-list-app">
            <div class="headerLayout">
                <div class="headerCon ">
                    <!-- Logo-->
                    <h1 id="mallLogo">
                        <img th:src="@{/images/jdlogo.png}" alt="">
                    </h1>

                    <div class="header-extra">

                        <!--搜索-->
                        <div id="mallSearch" class="mall-search">
                            <form name="searchTop" class="mallSearch-form clearfix">
                                <fieldset>
                                    <legend>天猫搜索</legend>
                                    <div class="mallSearch-input clearfix">
                                        <div class="s-combobox" id="s-combobox-685">
                                            <div class="s-combobox-input-wrap">
                                                <input type="text" autocomplete="off" value="dd" id="mq"
                                                       class="s-combobox-input" aria-haspopup="true">
                                            </div>
                                        </div>
                                        <button type="submit" id="searchbtn">搜索</button>
                                    </div>
                                </fieldset>
                            </form>
                            <ul class="relKeyTop">
                                <li><a>狂神说Java</a></li>
                                <li><a>狂神说前端</a></li>
                                <li><a>狂神说Linux</a></li>
                                <li><a>狂神说大数据</a></li>
                                <li><a>狂神聊理财</a></li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- 商品详情页面 -->
        <div id="content">
            <div class="main">
                <!-- 品牌分类 -->
                <form class="navAttrsForm">
                    <div class="attrs j_NavAttrs" style="display:block">
                        <div class="brandAttr j_nav_brand">
                            <div class="j_Brand attr">
                                <div class="attrKey">
                                    品牌
                                </div>
                                <div class="attrValues">
                                    <ul class="av-collapse row-2">
                                        <li><a href="#"> 狂神说 </a></li>
                                        <li><a href="#"> Java </a></li>
                                    </ul>
                                </div>
                            </div>
                        </div>
                    </div>
                </form>

                <!-- 排序规则 -->
                <div class="filter clearfix">
                    <a class="fSort fSort-cur">综合<i class="f-ico-arrow-d"></i></a>
                    <a class="fSort">人气<i class="f-ico-arrow-d"></i></a>
                    <a class="fSort">新品<i class="f-ico-arrow-d"></i></a>
                    <a class="fSort">销量<i class="f-ico-arrow-d"></i></a>
                    <a class="fSort">价格<i class="f-ico-triangle-mt"></i><i class="f-ico-triangle-mb"></i></a>
                </div>

                <!-- 商品详情 -->
                <div class="view grid-nosku">

                    <div class="product">
                        <div class="product-iWrap">
                            <!--商品封面-->
                            <div class="productImg-wrap">
                                <a class="productImg">
                                    <img src="https://img.alicdn.com/bao/uploaded/i1/3899981502/O1CN01q1uVx21MxxSZs8TVn_!!0-item_pic.jpg">
                                </a>
                            </div>
                            <!--价格-->
                            <p class="productPrice">
                                <em><b>¥</b>2590.00</em>
                            </p>
                            <!--标题-->
                            <p class="productTitle">
                                <a> dkny秋季纯色a字蕾丝dd商场同款连衣裙 </a>
                            </p>
                            <!-- 店铺名 -->
                            <div class="productShop">
                                <span>店铺： 狂神说Java </span>
                            </div>
                            <!-- 成交信息 -->
                            <p class="productStatus">
                                <span>月成交<em>999笔</em></span>
                                <span>评价 <a>3</a></span>
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<script>

</script>

</body>
</html>
```

> 访问9090端口

![image-20220408202155879](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204082021048.png)

> controller

```java
package com.kwq.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class IndexController {
  @GetMapping({"/","/index"})
    public String index(){
      return  "index";
  }
}
```



### 爬虫

> 数据问题？

数据库获取，消息队列中获取，都可以成为数据源，爬虫也是ok的！

> 导入jSoup依赖

```
<!--jsoup解析网页-->
<dependency>
    <groupId>org.jsoup</groupId>
    <artifactId>jsoup</artifactId>
    <version>1.10.2</version>
</dependency>
```

我们在京东搜索java，uri中显示[java - 商品搜索 - 京东 (jd.com)](https://search.jd.com/Search?keyword=java) 

![image-20220408204103995](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204082041289.png)

> 新建utils包，建个工具类在里面

```
package com.kwq.utils;

import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;

import java.io.IOException;
import java.net.MalformedURLException;
import java.net.URL;

public class HtmlParseUtil {
    public static void main(String[] args) throws IOException {
        //前提 需要联网

        String url="https://search.jd.com/Search?keyword=java";
        //解析网页（Jsoup返回Document就是浏览器的Document对象）
        Document document = Jsoup.parse(new URL(url), 30000);
        //所有你在js中可以使用的方法，这里都能用
        Element element = document.getElementById("J_goodsList");
        System.out.println(element);
    }
}
```

> 输出

![image-20220408204452219](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204082044337.png)



> 修改utils

==li标签里面的内容==

![image-20220408205651835](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204082056012.png)

```java
package com.kwq.utils;

import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

import java.io.IOException;
import java.net.MalformedURLException;
import java.net.URL;

public class HtmlParseUtil {
    public static void main(String[] args) throws IOException {
        //前提 需要联网

        String url="https://search.jd.com/Search?keyword=java";
        //解析网页（Jsoup返回Document就是浏览器的Document对象）
        Document document = Jsoup.parse(new URL(url), 30000);
        //所有你在js中可以使用的方法，这里都能用
        Element element = document.getElementById("J_goodsList");
        //System.out.println(element);
        //获取所有的li标签
        Elements elements = element.getElementsByTag("li");
        //获取li元素中的内容  这里的e1就是li
        for (Element e1 : elements) {
            String img = e1.getElementsByTag("img").eq(0).attr("data-lazy-img");
            String price = e1.getElementsByClass("p-price").eq(0).text();
            String title = e1.getElementsByClass("p-name").eq(0).text();
            System.out.println("======================");
            System.out.println(img);
            System.out.println(price);
            System.out.println(title);
        }
    }
}

```

> 输出：

![image-20220408211136874](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204082111103.png)



> 上面那个只是用来测试的，接下来修改

==封装个实体类==

```java
package com.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@AllArgsConstructor
@NoArgsConstructor
@Data
public class Content {
    private String title;
    private String img;
    private String price;
}
```

> 修改工具类

```java
package com.kwq.utils;

import com.pojo.Content;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;
import org.springframework.stereotype.Component;

import java.io.IOException;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;
@Component
public class HtmlParseUtil {
    //测试下面的方法是否可用
    public static void main(String[] args) throws IOException {
        new HtmlParseUtil().parseJD("java").forEach(System.out::println);
    }
    public List<Content> parseJD(String keywords) throws IOException {
        //前提 需要联网

        String url = "https://search.jd.com/Search?keyword="+keywords;
        //解析网页（Jsoup返回Document就是浏览器的Document对象）
        Document document = Jsoup.parse(new URL(url), 30000);
        //所有你在js中可以使用的方法，这里都能用
        Element element = document.getElementById("J_goodsList");
        //System.out.println(element);
        //获取所有的li标签
        Elements elements = element.getElementsByTag("li");
        ArrayList<Content> goodsList = new ArrayList<>();
        //获取li元素中的内容  这里的e1就是li
        for (Element e1 : elements) {
            String img = e1.getElementsByTag("img").eq(0).attr("data-lazy-img");
            String price = e1.getElementsByClass("p-price").eq(0).text();
            String title = e1.getElementsByClass("p-name").eq(0).text();

            Content content = new Content();
            content.setImg(img);
            content.setPrice(price);
            content.setTitle(title);
            goodsList.add(content);
        }
        return goodsList;
    }
}

```

> 输出：

![image-20220408212109956](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204082121161.png)



### 业务编写

> 建配置类：来操作es

```java
package com.kwq.config;

import org.apache.http.HttpHost;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestHighLevelClient;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration   
public class ElasticSearchClientConfig {
    @Bean
    public RestHighLevelClient restHighLevelClient(){
        RestHighLevelClient client=new RestHighLevelClient(
                RestClient.builder(
                        new HttpHost("127.0.0.1",9200,"http")));
                return client;
    }
}
```



```
package com.kwq.Service;

import com.alibaba.fastjson.JSON;
import com.kwq.utils.HtmlParseUtil;
import com.pojo.Content;
import org.elasticsearch.action.bulk.BulkRequest;
import org.elasticsearch.action.bulk.BulkResponse;
import org.elasticsearch.action.delete.DeleteRequest;
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestHighLevelClient;
import org.elasticsearch.common.unit.TimeValue;
import org.elasticsearch.common.xcontent.XContentType;
import org.elasticsearch.index.query.FuzzyQueryBuilder;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.index.query.TermQueryBuilder;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.concurrent.TimeUnit;

//业务编写
@Service
public class ContentService {

    @Autowired
    private RestHighLevelClient restHighLevelClient;

    //1.解析数据放入es索引中
    public boolean parseContent(String keywords) throws IOException {
        List<Content> contents = new HtmlParseUtil().parseJD(keywords);
        //把查询到的数据放入es中
        BulkRequest bulkRequest = new BulkRequest();
        bulkRequest.timeout("2ms");
        for (int i=0;i<contents.size();i++){
            bulkRequest.add(new IndexRequest("jd_goods")
                    .source(JSON.toJSONString(contents.get(i)), XContentType.JSON));
        }
        BulkResponse bulk = restHighLevelClient.bulk(bulkRequest, RequestOptions.DEFAULT);
        return !bulk.hasFailures();
    }

    //2、获取这些数据实现搜索功能
    public List<Map<String,Object>> searchPage(String keyword,int pageNo,int pageSize) throws IOException {
        if (pageNo<=1){
            pageNo=1;
        }
        //条件搜索
        SearchRequest searchRequest = new SearchRequest("jd_goods");
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();

        //分页
        sourceBuilder.from(pageNo);
        sourceBuilder.size(pageSize);

        //精准匹配
       /* TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("title", keyword);*/
        FuzzyQueryBuilder fuzzyQueryBuilder = QueryBuilders.fuzzyQuery("title", keyword);
        sourceBuilder.query(fuzzyQueryBuilder);
        sourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS));

        //执行搜索
        searchRequest.source(sourceBuilder);
        SearchResponse searchResponse = restHighLevelClient.search(searchRequest, RequestOptions.DEFAULT);

        //解析结果
        ArrayList<Map<String,Object>> list = new ArrayList<>();
        for (SearchHit documentFields : searchResponse.getHits().getHits()) {
            list.add(documentFields.getSourceAsMap());
        }
    return  list;}

}

```

> controller

```java
package com.kwq.controller;

import com.kwq.Service.ContentService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import java.io.IOException;

@RestController
public class ContentController {
    @Autowired
    private ContentService contentService;

    @GetMapping("/parse/{keyword}")
    public Boolean parse(@PathVariable("keyword") String keyword) throws IOException {
        return contentService.parseContent(keyword);
    }
}
```

> 访问：

![image-20220409164812099](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204091648596.png)

> es中：

![image-20220409164831049](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204091648331.png)



> service中添加方法   searchPage

```java
package com.kwq.Service;

import com.alibaba.fastjson.JSON;
import com.kwq.utils.HtmlParseUtil;
import com.pojo.Content;
import org.elasticsearch.action.bulk.BulkRequest;
import org.elasticsearch.action.bulk.BulkResponse;
import org.elasticsearch.action.delete.DeleteRequest;
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestHighLevelClient;
import org.elasticsearch.common.unit.TimeValue;
import org.elasticsearch.common.xcontent.XContentType;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.index.query.TermQueryBuilder;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.concurrent.TimeUnit;

//业务编写
@Service
public class ContentService {

    @Autowired
    private RestHighLevelClient restHighLevelClient;

    //1.解析数据放入es索引中
    public boolean parseContent(String keywords) throws IOException {
        List<Content> contents = new HtmlParseUtil().parseJD(keywords);
        //把查询到的数据放入es中
        BulkRequest bulkRequest = new BulkRequest();
        bulkRequest.timeout("2ms");
        for (int i=0;i<contents.size();i++){
            bulkRequest.add(new IndexRequest("jd_goods")
                    .source(JSON.toJSONString(contents.get(i)), XContentType.JSON));
        }
        BulkResponse bulk = restHighLevelClient.bulk(bulkRequest, RequestOptions.DEFAULT);
        return !bulk.hasFailures();
    }

    //2、获取这些数据实现搜索功能
    public List<Map<String,Object>> searchPage(String keyword,int pageNo,int pageSize) throws IOException {
        if (pageNo<=1){
            pageNo=1;
        }
        //条件搜索
        SearchRequest searchRequest = new SearchRequest("jd_goods");
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();

        //分页
        sourceBuilder.from(pageNo);
        sourceBuilder.size(pageSize);

        //精准匹配
        TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("title", keyword);
        sourceBuilder.query(termQueryBuilder);
        sourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS));

        //执行搜索
        searchRequest.source(sourceBuilder);
        SearchResponse searchResponse = restHighLevelClient.search(searchRequest, RequestOptions.DEFAULT);

        //解析结果
        ArrayList<Map<String,Object>> list = new ArrayList<>();
        for (SearchHit documentFields : searchResponse.getHits().getHits()) {
            list.add(documentFields.getSourceAsMap());
        }
    return  list;}

}
```

> controller

```java
package com.kwq.controller;

import com.kwq.Service.ContentService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import java.io.IOException;
import java.util.List;
import java.util.Map;

@RestController
public class ContentController {
    @Autowired
    private ContentService contentService;

    @GetMapping("/parse/{keyword}")
    public Boolean parse(@PathVariable("keyword") String keyword) throws IOException {
        return contentService.parseContent(keyword);
    }

        @GetMapping("/search/{keyowrd}/{pageNo}/{pageSize}")
        public List<Map<String,Object>> search(@PathVariable("keyowrd") String keyword,
                                               @PathVariable("pageNo") int pageNo,
                                               @PathVariable("pageSize") int pageSize) throws IOException {
       return contentService.searchPage(keyword,pageNo,pageSize);
         }

}
```

![image-20220409164850056](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204091648381.png)

### 前后端交互

下载vue依赖（可以在任意一个文件夹下执行 `npm install vue`）

下载axios依赖（`npm install axios`） 

以上两个下载依赖命令均在a文件夹下操作

==将axios.min.js放在项目的静态资源的js目录里面==

![image-20220409213548371](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204092135786.png)

也可以使用在线.`js`

这里我们的vue.min.js就演示在线的

> index.html 页面修改之后

```java
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">

<head>
    <meta charset="utf-8"/>
    <title>狂神说Java-ES仿京东实战</title>
    <link rel="stylesheet" th:href="@{/css/style.css}"/>

</head>

<body class="pg">
<div class="page" id="app">
    <div id="mallPage" class=" mallist tmall- page-not-market ">

        <!-- 头部搜索 -->
        <div id="header" class=" header-list-app">
            <div class="headerLayout">
                <div class="headerCon ">
                    <!-- Logo-->
                    <h1 id="mallLogo">
                        <img th:src="@{/images/jdlogo.png}" alt="">
                    </h1>

                    <div class="header-extra">

                        <!--搜索-->
                        <div id="mallSearch" class="mall-search">
                            <form name="searchTop" class="mallSearch-form clearfix">
                                <fieldset>
                                    <legend>天猫搜索</legend>
                                    <div class="mallSearch-input clearfix">
                                        <div class="s-combobox" id="s-combobox-685">
                                            <div class="s-combobox-input-wrap">
                                                <input v-model="keyword" type="text" autocomplete="off" value="dd" id="mq"
                                                       class="s-combobox-input" aria-haspopup="true">
                                            </div>
                                        </div>
                                        <button type="submit" @click.prevent="searchKey" id="searchbtn">搜索</button>
                                    </div>
                                </fieldset>
                            </form>
                            <ul class="relKeyTop">
                                <li><a>狂神说Java</a></li>
                                <li><a>狂神说前端</a></li>
                                <li><a>狂神说Linux</a></li>
                                <li><a>狂神说大数据</a></li>
                                <li><a>狂神聊理财</a></li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- 商品详情页面 -->
        <div id="content">
            <div class="main">
                <!-- 品牌分类 -->
                <form class="navAttrsForm">
                    <div class="attrs j_NavAttrs" style="display:block">
                        <div class="brandAttr j_nav_brand">
                            <div class="j_Brand attr">
                                <div class="attrKey">
                                    品牌
                                </div>
                                <div class="attrValues">
                                    <ul class="av-collapse row-2">
                                        <li><a href="#"> 狂神说 </a></li>
                                        <li><a href="#"> Java </a></li>
                                    </ul>
                                </div>
                            </div>
                        </div>
                    </div>
                </form>

                <!-- 排序规则 -->
                <div class="filter clearfix">
                    <a class="fSort fSort-cur">综合<i class="f-ico-arrow-d"></i></a>
                    <a class="fSort">人气<i class="f-ico-arrow-d"></i></a>
                    <a class="fSort">新品<i class="f-ico-arrow-d"></i></a>
                    <a class="fSort">销量<i class="f-ico-arrow-d"></i></a>
                    <a class="fSort">价格<i class="f-ico-triangle-mt"></i><i class="f-ico-triangle-mb"></i></a>
                </div>

                <!-- 商品详情 -->
                <div class="view grid-nosku" v-for="result in results">

                    <div class="product">
                        <div class="product-iWrap">
                            <!--商品封面-->
                            <div class="productImg-wrap">
                                <a class="productImg">
                                    <img :src="result.img">
                                </a>
                            </div>
                            <!--价格-->
                            <p class="productPrice">
                                <em><b>{{result.price}}</b>2590.00</em>
                            </p>
                            <!--标题-->
                            <p class="productTitle">
                                <a> {{result.title}} </a>
                            </p>
                            <!-- 店铺名 -->
                            <div class="productShop">
                                <span>店铺： 狂神说Java </span>
                            </div>
                            <!-- 成交信息 -->
                            <p class="productStatus">
                                <span>月成交<em>999笔</em></span>
                                <span>评价 <a>3</a></span>
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
<!--前端使用Vue，实现前后端分离-->
<script th:src="@{/js/axios.min.js}"></script>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.min.js"></script>

<script>
  new Vue({
      el: '#app' ,
      data: {
          keyword: '', //搜索的关键字
          results: [] //搜索的结果
      },
      methods: {
          searchKey(){
              var keyword= this.keyword;
              console.log(keyword);
              //对接后端的接口
              axios.get('search/'+keyword+"/1/10").then(response=>{
                  console.log(response);
                  this.results=response.data;//绑定数据！
              })
          }
      }
  })
</script>

</body>
</html>
```

> 访问：9090端口，输入java，即可拿到我们es中的数据了

![image-20220409224116405](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204092241810.png)



### 关键词高亮实现

> service新加高亮搜索方法   searchPageHighlightBuilder

```
package com.kwq.Service;

import com.alibaba.fastjson.JSON;
import com.kwq.utils.HtmlParseUtil;
import com.pojo.Content;
import org.elasticsearch.action.bulk.BulkRequest;
import org.elasticsearch.action.bulk.BulkResponse;
import org.elasticsearch.action.delete.DeleteRequest;
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestHighLevelClient;
import org.elasticsearch.common.text.Text;
import org.elasticsearch.common.unit.Fuzziness;
import org.elasticsearch.common.unit.TimeValue;
import org.elasticsearch.common.xcontent.XContentType;
import org.elasticsearch.index.query.*;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.elasticsearch.search.fetch.subphase.highlight.HighlightBuilder;
import org.elasticsearch.search.fetch.subphase.highlight.HighlightField;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import javax.swing.text.Highlighter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.concurrent.TimeUnit;

//业务编写
@Service
public class ContentService {

    @Autowired
    private RestHighLevelClient restHighLevelClient;

    //1.解析数据放入es索引中
    public boolean parseContent(String keywords) throws IOException {
        List<Content> contents = new HtmlParseUtil().parseJD(keywords);
        //把查询到的数据放入es中
        BulkRequest bulkRequest = new BulkRequest();
        bulkRequest.timeout("2ms");
        for (int i=0;i<contents.size();i++){
            bulkRequest.add(new IndexRequest("jd_goods")
                    .source(JSON.toJSONString(contents.get(i)), XContentType.JSON));
        }
        BulkResponse bulk = restHighLevelClient.bulk(bulkRequest, RequestOptions.DEFAULT);
        return !bulk.hasFailures();
    }

    //2、获取这些数据实现搜索功能
    public List<Map<String,Object>> searchPage(String keyword,int pageNo,int pageSize) throws IOException {
        if (pageNo<=1){
            pageNo=1;
        }
        //条件搜索
        SearchRequest searchRequest = new SearchRequest("jd_goods");
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();

        //分页
        sourceBuilder.from(pageNo);
        sourceBuilder.size(pageSize);

        //精准匹配
       /* TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("title", keyword);*/
        FuzzyQueryBuilder fuzzyQueryBuilder = QueryBuilders.fuzzyQuery("title", keyword);
        sourceBuilder.query(fuzzyQueryBuilder);
        sourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS));

        //执行搜索
        searchRequest.source(sourceBuilder);
        SearchResponse searchResponse = restHighLevelClient.search(searchRequest, RequestOptions.DEFAULT);

        //解析结果
        ArrayList<Map<String,Object>> list = new ArrayList<>();
        for (SearchHit documentFields : searchResponse.getHits().getHits()) {
            list.add(documentFields.getSourceAsMap());
        }
    return  list;}  //2、获取这些数据实现搜索功能

    //3、获取这些数据实现搜索高亮功能
    public List<Map<String,Object>> searchPageHighlightBuilder(String keyword,int pageNo,int pageSize) throws IOException {
        if (pageNo<=1){
            pageNo=1;
        }
        //条件搜索
        SearchRequest searchRequest = new SearchRequest("jd_goods");
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();

        //分页
        sourceBuilder.from(pageNo);
        sourceBuilder.size(pageSize);

        //精准匹配
        TermsQueryBuilder title1 = QueryBuilders.termsQuery("title", keyword);
/*
        FuzzyQueryBuilder fuzzyQueryBuilder = QueryBuilders.fuzzyQuery("title", keyword);
*/
     /*   FuzzyQueryBuilder fuzzyQueryBuilder = QueryBuilders.fuzzyQuery("title", keyword);
        QueryBuilders.termQuery()*/
        FuzzyQueryBuilder title2 = QueryBuilders.fuzzyQuery("title", keyword).fuzziness(Fuzziness.TWO);
        /*MatchQueryBuilder title2 = QueryBuilders.matchQuery("title", keyword);*/
        sourceBuilder.query(title2);
        sourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS));

        //高亮
        HighlightBuilder highlightBuilder = new HighlightBuilder();
        highlightBuilder.field("title");
        highlightBuilder.requireFieldMatch(false); //关闭多个高亮显示！只显示第一个
        highlightBuilder.preTags("<span style='color:red'>"); //高亮字段的前缀
        highlightBuilder.postTags("</span>"); //高亮字段的后缀
        sourceBuilder.highlighter(highlightBuilder);


        //执行搜索
        searchRequest.source(sourceBuilder);
        SearchResponse searchResponse = restHighLevelClient.search(searchRequest, RequestOptions.DEFAULT);

        //解析结果
        ArrayList<Map<String,Object>> list = new ArrayList<>();
        for (SearchHit documentFields : searchResponse.getHits().getHits()) {
            //解析高亮的字段
            Map<String, HighlightField> highlightFields = documentFields.getHighlightFields();
            //把所有的标题获取出来
            HighlightField title = highlightFields.get("title");
            Map<String, Object> sourceAsMap = documentFields.getSourceAsMap();//原来的结果
            //解析高亮的字段，将原来的字段换为我们高亮的字段即可！
            if (title!=null){
                Text[] fragments = title.fragments();
                String n_title="";
                for (Text text : fragments) {
                    n_title+=text;
                    sourceAsMap.put("title",n_title);  //高亮字段替换掉原来的内容即可！
                }
            } list.add(sourceAsMap);

        }
    return  list;}

}

```

> controller  修改搜索方法改为高亮的searchPageHighlightBuilder

```
package com.kwq.controller;

import com.kwq.Service.ContentService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import java.io.IOException;
import java.util.List;
import java.util.Map;

@RestController
public class ContentController {
    @Autowired
    private ContentService contentService;

    @GetMapping("/parse/{keyword}")
    public Boolean parse(@PathVariable("keyword") String keyword) throws IOException {
        return contentService.parseContent(keyword);
    }

        @GetMapping("/search/{keyowrd}/{pageNo}/{pageSize}")
        public List<Map<String,Object>> search(@PathVariable("keyowrd") String keyword,
                                               @PathVariable("pageNo") int pageNo,
                                               @PathVariable("pageSize") int pageSize) throws IOException {
       return contentService.searchPageHighlightBuilder(keyword,pageNo,pageSize);
         }

}
```

> 访问：效果演示

![image-20220409231453899](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204092314212.png)

==貌似没有达到我们预期的目的==

> 只需修改前端即可  用下面那个替换掉上面那个

![image-20220409232309429](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204092323570.png)

![image-20220409232235410](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204092322715.png)==好了，到这里搜索之高亮显示就完成啦==
