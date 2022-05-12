# Mysql运维篇

## 日志

### 错误日志

==错误日志==是 MySQL 中`最重要的日志之一`，它记录了当 `mysqld 启动和停止时`，以及`服务器在运行过程中发生任何严重错误时的相关信息`。当数据库==出现任何故障导致无法正常使用时，建议首先查看此日志==。

该日志是默认开启的，默认存放目录 /var/log/，默认的日志文件名为 mysqld.log 。(`仅仅是黑马老师的和yum安装的mysql`,宝塔和docker装的mysql这个都不一样)

查看日志位置

```sql
show variables like '%log_error%'; 
```

![image-20220506150041701](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205061500799.png)

宝塔安装的mysql的话。直接在这配就好了

![image-20220506151608279](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205061516395.png)

==我用的是yun的方式安装的mysql。和黑马老师官网下载压缩包上传是一样的（至少每个系统变量的值是一样的）。==



### 二进制日志

**介绍**

二进制日志（BINLOG）记录了所有的 `DDL`（数据定义语言）语句和 `DML`（数据操纵语言）语句，但不包括数据查询（SELECT、SHOW）语句。

作用：

①. 灾难时的数据恢复；

②. MySQL的主从复制。` 主从复制的原理就是基于二进制日志的`

在MySQL8版本中，默认二进制日志是开启着的，涉及到的参数如下：

```sql
show variables like '%log_bin%';
```

![image-20220506200556162](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062005653.png)

![image-20220507133859115](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071338298.png)

![image-20220507133924131](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071339322.png)

参数说明：

- log_bin_basename：当前数据库服务器的binlog日志的基础名称(前缀)，具体的binlog文件名需要再该basename的基础上加上编号(编号从000001开始)。
- log_bin_index：binlog的索引文件，里面记录了当前服务器关联的binlog文件有哪些。

**格式**

MySQL服务器中提供了多种格式来记录二进制日志，具体格式及特点如下：

![image-20220506203656484](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062036728.png)

```sql
show variables like '%binlog_format%';
```

![image-20220506203720007](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062037316.png)

如果我们需要配置二进制日志的格式，只需要在 /etc/my.cnf 中配置 binlog_format 参数即可。

**查看**

由于日志是以二进制方式存储的，不能直接读取，需要通过二进制日志查询工具 mysqlbinlog 来查看，具体语法

![image-20220506203756908](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062037108.png)

>  案例：ROW条件下

![image-20220507135528179](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071355339.png)

查看：mysqlbinlog -v binlog.000008（`这个选编号最大的二进制日志文件即可，因为代表最新`）

![image-20220507135731526](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071357590.png)



![image-20220507135955289](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071359339.png)

改了配置文件后，重启mysql服务  `systemctl restart mysqld`

![image-20220507140843789](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071408832.png)

![image-20220507140316102](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071403153.png)

==查询不会记录。建表语句也会记录==。  

==**PS：修改日志格式，重启主库，并不会影响主从复制**==

**删除**

对于比较繁忙的业务系统，每天生成的binlog数据巨大，如果长时间不清除，将会占用大量磁盘空间。可以通过以下几种方式清理日志：

![image-20220506203826870](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062038029.png)

也可以在mysql的配置文件中配置二进制日志的过期时间，设置了之后，二进制日志过期会自动删除。

```sql
show variables like '%binlog_expire_logs_seconds%';
```



### 查询日志

查询日志中记录了客户端的`所有操作语句`，而`二进制日志不包含查询数据的SQL语句`。默认情况下，查询日志是`未开启的`。

![image-20220506205033908](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062050032.png)

如果需要开启查询日志，可以修改MySQL的配置文件 /etc/my.cnf 文件，添加如下内容：

```sql
#该选项用来开启查询日志 ， 可选值 ： 0 或者 1 ； 0 代表关闭， 1 代表开启 
general_log=1 
#设置日志的文件名 ， 如果没有指定， 则会使用默认的 上图具体位置的.log即是 
general_log_file=xxx.log
```

改了配置文件，需要重启mysql服务

```sql
systemctl restart mysqld
```

开启了查询日志之后，在MySQL的数据存放目录，也就是 /var/lib/mysql/ 目录下就会出现xxx.log 文件。之后所有的客户端的增删改查操作都会记录在该日志文件之中，长时间运行后，该日志文件将会非常大

![image-20220506205738796](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062057897.png)

> 演示：

通过tail尾巴追加指令，看看我们对数据库进行操作的时候，查询日志文件追加了什么

![image-20220506205851633](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062058770.png)

对数据库进行操作

![image-20220506210021122](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062100235.png)

![image-20220506210230138](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062102212.png)

查看追加

![image-20220506210324402](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062103623.png)

### 慢查询日志

慢查询日志记录了所有执行时间超过参数 `long_query_time` 设置值并且扫描记录数不小于`min_examined_row_limit` 的所有的SQL语句的日志，默认未开启。long_query_time 默认为10 秒，最小为 0， 精度可以到微秒。

如果需要开启慢查询日志，需要在MySQL的配置文件 /etc/my.cnf 中配置如下参数：

```sql
#慢查询日志 
slow_query_log=1 
#执行时间参数 
long_query_time=2
```

> 案例：查出那些sql查询慢 进行优化

在/etc/my.cnf下添加如下两行。老规矩改了配置文件，就要重启mysql服务。

![image-20220506211951898](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062119306.png)

紧接着进入/var/lib/msql,使用tail查看追加日志

![image-20220506212042504](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062120243.png)

查询：

![image-20220506212156618](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062121771.png)

查看追加：

![image-20220506212239352](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062122674.png)

默认情况下，不会记录管理语句(`show databases这些`)，==也不会记录不使用索引进行查找的查询==。可以在/etc/my.cnf使用log_slow_admin_statements和 更改此行为 log_queries_not_using_indexes的两条语句，如下所述

```sql
#记录执行较慢的管理语句 
log_slow_admin_statements =1 
#记录执行较慢的未使用索引的语句 
log_queries_not_using_indexes = 1
```

上述所有的参数配置完成之后，都需要重新启动MySQL服务器才可以生效。



## 主从复制

### 概述

主从复制是指将`主数据库的 DDL 和 DML 操作通过二进制日志传到从库服务器中，然后在从库上对这些日志重新执行（也叫重做），从而使得从库和主库的数据保持同步。`

==MySQL支持一台主库同时向多台从库进行复制， 从库同时也可以作为其他从服务器的主库，实现链状复制==

![image-20220506213229507](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062132666.png)

MySQL 复制的优点主要包含以下三个方面：

- 主库出现问题，可以快速切换到从库提供服务。
- 实现读写分离，降低单台数据库的访问压力。
- 可以在从库中执行备份，以避免备份期间影响主库服务。(`数据库备份是需要上全局锁，因而只能查询。但是增删改就阻塞了，十分影响用户体验，我们备份从库（数据和主库一样），那么就不会影响主库服务`)



### 原理

MySQL主从复制的核心就是 二进制日志，具体的过程如下：

![image-20220506214127282](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062141435.png)

从上图来看，复制分成三步：

1. Master 主库在事务提交时，会把数据变更记录在二进制日志文件 Binlog 中。

2. 从库读取主库的二进制日志文件 Binlog ，写入到从库的中继日志 Relay Log 。 

3. slave重做中继日志中的事件，将改变反映它自己的数据。



### 搭建环境

> 准备

![image-20220506215556888](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205062155043.png)

准备好两台服务器之后，在上述的两台服务器中分别安装好MySQL，并完成基础的初始化准备(`密码配置等操作`)工作。 其中：

- 192.168.200.200 作为主服务器master
- 192.168.200.201 作为从服务器slave

ps：外网能访问mysql的条件，开放指定端口（`接着重启防火墙`）或者直接关闭防火墙

测试阶段建议直接关掉防火墙

具体的生产环境中建议开放指定端口

```sql
systemctl disable firewall   关闭开机自动启动
```

#### 主库配置

> master

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071002304.png)

1. 修改配置文件 /etc/my.cnf

```sql
#mysql 服务ID，保证整个集群环境中唯一，取值范围：1 – 232-1，默认为1 
server-id=1 
#是否只读,1 代表只读, 0 代表读写 
read-only=0 
#忽略的数据, 指不需要同步的数据库 
#binlog-ignore-db=mysql
#指定同步的数据库 
#binlog-do-db=db01 12345678
```

2. 重启MySQL服务器

```sql
systemctl restart mysqld
```

3. 登录mysql，创建远程连接的账号，并授予主从复制权限

```sql
#创建itcast用户，并设置密码，该用户可在任意主机连接该MySQL服务 
CREATE USER 'kwq'@'%' IDENTIFIED WITH mysql_native_password BY 'Root@123456' ;
#为 'itcast'@'%' 用户分配主从复制权限 
GRANT REPLICATION SLAVE ON *.* TO 'kwq'@'%';
```

==%代表可以在任意主机上通过kwq用户实现与主库的主从复制，不可用来实现远程登录==

> 区别下面这个

默认的root用户只能当前节点localhost访问，是无法远程访问的，我们还需要创建一个root账户，用户远程访问

```
create user 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123123';
```

并给root用户分配所有权限

```
grant all on *.* to 'root'@'%';
```

这样root这个用户就拥有一切权限包含==远程登录==

4. 通过指令，查看二进制日志坐标

```sql
show master status ;
```

![image-20220507101436957](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071014185.png)

字段含义说明：

-  file : 从哪个日志文件开始推送日志文件
- position ： 从哪个位置开始推送日志
- binlog_ignore_db : 指定不需要同步的数据库



#### 从库配置

![image-20220507112051999](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071120234.png)

1. 修改配置文件 /etc/my.cnf

```sql
#mysql 服务ID，保证整个集群环境中唯一，取值范围：1 – 2^32-1，和主库不一样即可 
server-id=2 
#是否只读,1 代表只读, 0 代表读写 
read-only=1
```

超级管理员可以写

`super-read-only=1 超级管理员也变成仅可读`



2. 重新启动MySQL服务

```sql
systemctl restart mysqld
```

3. 登录mysql，设置主库配置

```sql
CHANGE REPLICATION SOURCE TO SOURCE_HOST='120.24.210.204', SOURCE_USER='kwq', SOURCE_PASSWORD='Root@123456', SOURCE_LOG_FILE='binlog.000008', SOURCE_LOG_POS=650;
```

上述是8.0.23中的语法。如果mysql是 8.0.23 之前的版本，执行如下SQL：

```sql
CHANGE MASTER TO MASTER_HOST='120.24.210.204', MASTER_USER='kwq', 

MASTER_PASSWORD='Root@123456', MASTER_LOG_FILE='binlog.000008', 

MASTER_LOG_POS=650; 
```

高版本两种写法都兼容兼容

![image-20220507112621877](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071126211.png)

4. 开启同步操作

```sql
start replica ; #8.0.22之后 

start slave ; #8.0.22之前 
```

![image-20220507113205626](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071132826.png)

5. 查看主从同步状态

```sql
show replica status ; #8.0.22之后 

show slave status ; #8.0.22之前 
```

![image-20220507113411068](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071134356.png)

 Replica_IO_Running  表示io线程运行是否正常       io线程作用：表示读取二进制并且写入继日志 

 Replica_SQL_Running作用：表示sql线程运行是否正常  sql线程作用：读取继志日志 再从库执行sql实现与主库的同步

只要这两个为yes 说明主从复制是正常的，

第四步和第五步高版本都可以用（`兼容`）



### 测试：

主库刚开始的情况

![image-20220507131101439](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071311620.png)

从库刚开始的情况：

![image-20220507131125484](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071311547.png)

主库执行下面操作

![image-20220507131611386](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071316481.png)

从库执行下面操作看看是否同步了

![image-20220507131301614](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071313879.png)==**到这里就成功实现了主从复制啦**==



## 分库分表

### 问题分析

![image-20220507132218779](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071322849.png)

随着互联网及移动互联网的发展，`应用系统的数据量也是成指数式增长`，若采用`单数据库`进行数据存储，存在以下性能瓶颈：

1. IO瓶颈：热点数据太多，数据库缓存不足(`缓冲池`)，产生大量磁盘IO，效率较低。 请求数据太多，带宽不够，网络IO瓶颈。

2. CPU瓶颈：排序、分组、连接查询、聚合统计等SQL会耗费大量的CPU资源，请求数太多，CPU出现瓶颈。

仅仅加磁盘和内存容量治标不治本。

为了解决上述问题，我们需要对数据库进行`分库分表`处理。

![image-20220507132300183](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071323287.png)

分库分表的中心思想都是将数据分散存储，==使得单一数据库/表的数据量变小==来缓解单一数据库的性能问题，==从而达到提升数据库性能的目的。==



### 拆分策略

分库分表的形式，主要是两种：==垂直拆分和水平拆分==。而拆分的`粒度`，一般又分为==分库和分表==，所以组成的拆分策略最终如下：

![image-20220507142238883](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071422954.png)

#### 垂直拆分

> 垂直分库

![image-20220507142331064](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071423136.png)

垂直分库：==以表为依据，根据业务将不同表拆分到不同库中。==

特点：

- 每个库的表结构都不一样。
- 每个库的数据也不一样。
- 所有库的并集是全量数据。



> 垂直分表

![image-20220507142435334](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071424403.png)

垂直分表：==以字段为依据，根据字段属性将不同字段拆分到不同表中。==

特点：

- 每个表的结构都不一样。
- 每个表的数据也不一样，一般通过一列（主键/外键）关联。
- 所有表的并集是全量数据。



#### 水平拆分

> 水平分库

![image-20220507142540444](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071425516.png)

水平分库：==按照一定策略，将一个库的数据拆分到多个库中。==

特点：

- 每个库的表结构都一样。
- 每个库的数据都不一样。
- 所有库的并集是全量数据



> 水平分表

![image-20220507142622790](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071426861.png)

水平分表：以字段为依据，按照一定策略，将一个表的数据拆分到多个表中。

特点：

- 每个表的表结构都一样。
- 每个表的数据都不一样。
- 所有表的并集是全量数据

==**在业务系统中，为了缓解磁盘IO及CPU的性能瓶颈，到底是垂直拆分，还是水平拆分；具体是分库，还是分表，都需要根据具体的业务需求具体分析。**==



#### 实现技术

==**分库分表有他的好处，但是我们的应用程序的编写就会变得复杂。因为数据库多了。我们可以通过下面技术。即使使用了分库分表，应用程序也可以做到无感知。无需改动太多代码。**==

shardingJDBC：基于AOP原理，在应用程序中对本地执行的SQL进行拦截，解析、改写、路由处理。需要自行编码配置实现，只支持java语言，性能较高。

MyCat：数据库分库分表中间件，不用调整代码即可实现分库分表，支持多种语言，性能不及前者

![image-20220507142727778](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071427841.png)

本次课程，我们选择的是MyCat数据库中间件，通过MyCat中间件来完成分库分表操作。

### MyCat

#### 介绍

Mycat是开源的、活跃的、`基于Java语言编写的MySQL数据库中间件`。`可以像使用mysql一样来使用mycat`，对于开发人员来说根本==感觉不到mycat的存在。==

==开发人员只需要连接MyCat即可，而具体底层用到几台数据库，每一台数据库服务器里面存储了什么数据，都无需关心。== 具体的`分库分表`的策略，只需要在==MyCat中配置即可==。

![image-20220507154552229](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071545299.png)

#### 优势：

- 性能可靠稳定
- 强大的技术团队
- 体系完善
- 社区活跃

#### 下载

![image-20220507154640674](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071546772.png)

#### 安装

Mycat是采用java语言开发的开源的数据库中间件，支持Windows和Linux运行环境，下面介绍MyCat的Linux中的环境搭建。我们需要在准备好的服务器中安装如下软件。

- MySQL
- JDK
- Mycat

![image-20220507154725033](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071547094.png)

> 主服务器上安装JDK8

1. 上传安装包

使用`XFTP`将jdk的二进制发布包上传到Linux 

由于上述在进行文件上传时，选择的上传目录为根目录 /，上传完毕后，我们执行指令 cd / 切换到根目录下，查看上传的安装包。

![image-20220511111329233](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205111113408.png) 



2. 解压安装包

执行如下指令，将上传上来的压缩包进行解压，并通过-C参数指定解压文件存放目录为 /usr/local。

```
tar -zxvf jdk-8u171-linux-x64.tar.gz -C /usr/local
```

![image-20220511111338144](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205111113361.png) 





3. 配置环境变量

使用vim命令修改/etc/profile文件，在文件末尾加入如下配置

```
JAVA_HOME=/usr/local/jdk1.8.0_171
PATH=$JAVA_HOME/bin:$PATH
```

具体操作指令如下: 

```
1). 编辑/etc/profile文件，进入命令模式
	vim /etc/profile

2). 在命令模式中，输入指令 G ， 切换到文件最后
	G

3). 在命令模式中输入 i/a/o 进入插入模式，然后切换到文件最后一行
	i

4). 将上述的配置拷贝到文件中
	export JAVA_HOME=/usr/local/jdk1.8.0_171
	export PATH=$JAVA_HOME/bin:$PATH
	
5). 从插入模式，切换到指令模式
	ESC
	
6). 按:进入底行模式，然后输入wq，回车保存
	:wq
```

4. 重新加载profile文件

为了使更改的配置立即生效，需要重新加载profile文件，执行命令:

```
source /etc/profile
```

5. 检查安装是否成功

```
java -version
```

![image-20210814182327675](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205111112718.png) 





> MyCat安装

1. 上传Mycat压缩包到服务器的`/ `目录下

Mycat-server-1.6.7.3-release-20210913163959-linux.tar.gz     



2. 解压MyCat的压缩包 解压到/usr/local下

```sql
tar -zxvf Mycat-server-1.6.7.3-release-20210913163959-linux.tar.gz -C /usr/local/
```

`mycat默认支持的是5版本的数据库。我们用的是8，因此修改一下mycat目录下的lib目录，换掉驱动。`

![image-20220507153513567](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071535667.png)

![image-20220507153728172](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071537246.png)

```sql
chmod 777 mysql-connector-java-8.0.22.jar  #执行此指令
```

再次`ll`发现变绿了。至此环境搭建完成。

#### mycat目录介绍

![image-20220507155131958](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071551054.png)

- bin : 存放可执行文件，用于启动停止mycat
- conf：存放mycat的配置文件
- lib：存放mycat的项目依赖包（jar）
- logs：存放mycat的日志文件



#### mycat整体结构

在MyCat的整体结构中，分为==两个部分==：上面的逻辑结构、下面的物理结构。

![image-20220507155434631](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071554709.png)

在MyCat的逻辑结构主要负责逻辑库、逻辑表、分片规则、分片节点等逻辑结构的处理，而具体的数据存储还是在物理结构，也就是数据库服务器中存储的。在后面讲解MyCat入门以及MyCat分片时，还会讲到上面所提到的概念。



#### MyCat入门

> 需求

由于 tb_order 表中数据量很大，磁盘IO及容量都到达了瓶颈，现在需要对 tb_order 表进行数据分片，分为三个数据节点，每一个节点主机位于不同的服务器上, 具体的结构，参考下图：

![image-20220507193531056](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205071935288.png)

**环境准备**

准备3台服务器：

- 192.168.200.210：MyCat中间件服务器，同时也是第一个分片服务器。
- 192.168.200.213：第二个分片服务器。
- 192.168.200.214：第三个分片服务器。

并且在上述3台数据库中创建数据库 db01 。 

**mycat的相关配置 **   `均在/usr/local/mycat/conf目录下`

1). schema.xml

在schema.xml中配置逻辑库、逻辑表、数据节点、节点主机等相关信息。具体的配置如下：

```sql
<?xml version="1.0"?>
<!DOCTYPE mycat:schema SYSTEM "schema.dtd">
<mycat:schema xmlns:mycat="http://io.mycat/">

	<schema name="DB01" checkSQLschema="true" sqlMaxLimit="100">

		<table name="TB_ORDER" dataNode="dn1,dn2" rule="auto-sharding-long" />

		
	</schema>
	
	<dataNode name="dn1" dataHost="dhost1" database="db01" />
	<dataNode name="dn2" dataHost="dhost2" database="db01" />
	
	
	<dataHost name="dhost1" maxCon="1000" minCon="10" balance="0"
			  writeType="0" dbType="mysql" dbDriver="jdbc" switchType="1"  slaveThreshold="100">
		<heartbeat>select user()</heartbeat>
		
		<writeHost host="hostM1" url="jdbc:mysql://120.24.210.204:3306?useSSL=false&amp;serverTimezone=Asia/Shanghai&amp;characterEncoding=utf8" user="root"
				   password="123123" />
			
	
	</dataHost>

<dataHost name="dhost2" maxCon="1000" minCon="10" balance="0"
			  writeType="0" dbType="mysql" dbDriver="jdbc" switchType="1"  slaveThreshold="100">
		<heartbeat>select user()</heartbeat>
		
		<writeHost host="hostM2" url="jdbc:mysql://120.79.14.203:3306?useSSL=false&amp;serverTimezone=Asia/Shanghai&amp;characterEncoding=utf8" user="root"
				   password="123123" />
			
	
	</dataHost>
	
</mycat:schema>
```

2). server.xml

需要在server.xml中配置用户名、密码，以及用户的访问权限信息，具体的配置如下：

```sql
<user name="root" defaultAccount="true">
		<property name="password">123123</property>
		<property name="schemas">DB01</property>
		
		<!-- 表级 DML 权限设置 -->
		<!-- 		
		<privileges check="false">
			<schema name="TESTDB" dml="0110" >
				<table name="tb01" dml="0000"></table>
				<table name="tb02" dml="1111"></table>
			</schema>
		</privileges>		
		 -->
	</user>

	<user name="user">
		<property name="password">123123</property>
		<property name="schemas">DB01</property>
		<property name="readOnly">true</property>
	</user>

```

还需要修改一个地方，因为我们只有两台服务器，而mycat默认是三个，约定大于配置，因此我们需要修改这个文件

![image-20220507210603432](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205072106587.png)

==注释最下面一行即可==

![image-20220508000200477](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205080002589.png)

修改为配置文件重启mycat

![image-20220507232430766](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205072324856.png)

另外一个终端，查看日志，判断是否启动成功（==配置文件修改无误，方可启动成功==）

![image-20220507210954684](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205072109815.png)



连上mycat客户端

![image-20220511115009271](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205111150534.png)

下图是我们定义的逻辑表

![image-20220507220418956](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205072204012.png)

库表对应下面的配置。也就是逻辑结构

![image-20220507220331646](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205072203751.png)

刚开始我们两个数据库db01都是没表的

![image-20220507220848686](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205072208743.png)

在mycat服务端执行建表语句

![image-20220507220952689](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205072209757.png)

发现两个数据库db01都有表了

![image-20220511115220773](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205111152938.png)

插入数据测试：

![image-20220511115429915](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205111154043.png)

第一个分片结点

![image-20220511115309739](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205111153870.png)

再插入id为1000w 发现出现在第二个分片结点

![image-20220511131815783](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205111318341.png)

经过多次测试，我们发现，在往 TB_ORDER 表中插入数据时：

如果id的值在1-500w之间，数据将会存储在第一个分片数据库中。

如果id的值在500w-1000w之间，数据将会存储在第二个分片数据库中。

如果id的值超出1000w，在插入数据时，将会报错。

![image-20220507233925900](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205072339981.png)

为什么会出现这种现象，数据到底落在哪一个分片服务器到底是如何决定的呢？ 这是由逻辑表配置时的一个`参数 rule` 决定的，而`这个参数配置的就是分片规则`，关于分片规则的配置，在后面的课程中会详细讲解。



#### MyCat配置

##### schema.xml

schema.xml 作为MyCat中最重要的配置文件之一 , 涵盖了MyCat的逻辑库 、 逻辑表 、 分片规则、分片节点及数据源的配置。

![image-20220511132003574](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205111320795.png)

主要包含以下三组标签：

- schema标签
- datanode标签
- datahost标签



> schema标签

1). schema 定义逻辑库

![image-20220507234608833](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205072346925.png)

schema 标签用于定义 MyCat实例中的逻辑库 , 一个MyCat实例中, 可以有多个逻辑库 , 可以通过 schema 标签来划分不同的逻辑库。MyCat中的逻辑库的概念，等同于MySQL中的database概念, 需要操作某个逻辑库下的表时, 也需要切换逻辑库(use xxx)。

核心属性：

- name：指定自定义的逻辑库库名
- checkSQLschema：==在SQL语句操作时指定了数据库名称，执行时是否自动去除；true：自动去除，false：不自动去除==

==大白话就是我们没有选择数据库的时候，也可以通过库.表的形式crud 如果是false的话，一定要先ues 库。才能增删查改表==

- sqlMaxLimit：如果未指定limit进行查询，列表查询模式查询多少条记录



2). schema 中的table定义逻辑表

![image-20220507234821858](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205072348958.png)

table 标签定义了MyCat中逻辑库schema下的逻辑表 , 所有`需要拆分的表`都需要在table标签中定义 。

核心属性：

- name：定义逻辑表表名，在该逻辑库下唯一
- dataNode：定义逻辑表所属的dataNode，该属性需要与dataNode标签中name对应；多个dataNode逗号分隔.==代表需要将TB_ORDR这个表的数据，应该分布在哪个dataNode中==
- rule：分片规则的名字，分片规则名字是在rule.xml中定义的
- primaryKey：逻辑表对应真实表的主键
- type：逻辑表的类型，目前逻辑表只有全局表和普通表（`需要去分片的表`），如果未配置，就是普通表；全局表(`所有分片结点都会包含这个全局表`后面会讲)，配置为 global



> datanode标签

![image-20220507235017586](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205072350765.png)

核心属性：

- name：定义数据节点名称
- dataHost：数据库实例主机名称，引用自 dataHost 标签中name属性
- database：定义`所属数据库` (==也就是我们真实mysql的数据库名字==)

> datahost标签

![image-20220511132056165](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205111320308.png)

该标签在MyCat逻辑库中作为底层标签存在, 直接定义了具体的`数据库实例`、`读写分离`、`心跳语句`。

核心属性：

- name：唯一标识，供上层标签使用
- maxCon/minCon：最大连接数/最小连接数
- balance：负载均衡策略，取值 0,1,2,3（`读写分离的时候会讲`）
- writeType：写操作分发方式（0：写操作转发到第一个writeHost，第一个挂了，切换到第二个；1：写操作随机分发到配置的writeHost）（`后面会详细讲解`）
- dbDriver：数据库驱动，支持 native、jdbc。 ==native主要是支持mysql5.6/5.7这些版本的 8版本的要配置成jdbc==



##### rule.xml

==rule.xml中定义所有拆分表的规则, 在使用过程中可以灵活的使用分片算法, 或者对同一个分片算法使用不同的参数, 它让分片过程可配置化。主要包含两类标签：tableRule、Function。==

![image-20220507235703664](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205072357099.png)

- tableRule定义表的分片规则
- columns 表示根据什么数据库字段进行分片
- algorithm(引用function) 表示关联的分片规则算法是什么
- property表分片规则所关联的属性配置（也有可能将这些属性封装在一个外部文件中）
- class 表示实现分配规则对应的java类

##### server.xml

server.xml配置文件包含了MyCat的系统配置信息，主要有两个重要的标签：system、user。

1). system标签

![image-20220508102558508](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081025667.png)

主要配置MyCat中的系统配置信息，对应的系统配置项及其含义，如下：

![image-20220508102636276](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081026396.png)

![image-20220508102650576](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081026680.png)

![image-20220508102711000](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081027118.png)

![image-20220508102721714](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081027828.png)

![image-20220508102730339](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081027472.png)

2). user标签

配置`MyCat中的用户、访问密码，以及用户针对于逻辑库、逻辑表的权限信息`，具体的权限描述方式及

配置说明如下

![image-20220508103226216](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081032354.png)

在测试权限操作时，我们只需要将 privileges 标签的注释放开，`然后将check改为true`。 在 privileges 下的schema标签中配置的dml属性配置的是逻辑库的权限。 在privileges的schema下的table标签的dml属性中配置逻辑表的权限。 ==操作的时候权限看表。表没有声明权限就用库的权限==



#### Mycat分片-垂直分库

**场景**

在业务系统中, 涉及以下表结构 ,但是由于用户与订单每天都会产生大量的数据, 单台服务器的数据存储及处理能力是有限的, 可以对数据库表进行拆分, 原有的数据库表如下。

![image-20220508113028562](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081130718.png)

现在考虑将其进行垂直分库操作，将商品相关的表拆分到一个数据库服务器，订单表拆分的一个数据库服务器，用户及省市区表拆分到一个服务器。最终结构如下：

![image-20220508113050378](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081130491.png)

**准备**

准备三台服务器，IP地址如图所示：

![image-20220508113115372](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081131535.png)

**配置**

1). schema.xml

![image-20220508150932482](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081509660.png)

![image-20220508150953398](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081509520.png)

![image-20220508151012926](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081510058.png)

server.xml

![image-20220508151033637](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081510753.png)

刚开始我们的数据库shopping是没有表的，而mycat中是有表的（`逻辑表`）

![image-20220508114736243](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081147353.png)

> 测试

 1). 上传测试SQL脚本到服务器的/root目录

![image-20220508151237758](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081512883.png)

2). 执行指令导入测试数据

重新启动MyCat后，在mycat的命令行中，通过source指令导入表结构，以及对应的数据，查看数据分布情况

![image-20220508151312149](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081513257.png)

将表结构及对应的测试数据导入之后，可以检查一下各个数据库服务器中的表结构分布情况。 检查是否和我们准备工作中规划的服务器一致。

![image-20220508151333080](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081513336.png)

3). 查询用户的收件人及收件人地址信息(包含省、市、区)。 

在MyCat的命令行中，当我们执行以下多表联查的SQL语句时，可以正常查询出数据。

```sql
select ua.user_id, ua.contact, p.province, c.city, r.area , ua.address from tb_user_address ua ,tb_areas_city c , tb_areas_provinces p ,tb_areas_region r where ua.province_id = p.provinceid and ua.city_id = c.cityid and ua.town_id = r.areaid ;
```

![image-20220508151455398](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081514702.png)

4). 查询每一笔订单及订单的收件地址信息(包含省、市、区)。

实现该需求对应的SQL语句如下：

```sql
SELECT order_id , payment ,receiver, province , city , area FROM tb_order_master o , tb_areas_provinces p , tb_areas_city c , tb_areas_region r WHERE o.receiver_province = p.provinceid AND o.receiver_city = c.cityid AND o.receiver_region = r.areaid ;
```

但是现在存在一个问题，订单相关的表结构是在 192.168.200.213 数据库服务器中，而省市区的数据库表是在 192.168.200.214 数据库服务器中。那么在MyCat中执行是否可以成功呢？

![image-20220508151536147](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081515392.png)

经过测试，我们看到，SQL语句执行报错。==原因就是因为MyCat在执行该SQL语句时，需要往具体的数据库服务器中路由，而当前没有一个数据库服务器完全包含了订单以及省市区的表结构，造成SQL语句失败，报错。==

对于上述的这种现象，我们如何来解决呢？ 下面我们介绍的全局表，就可以轻松解决这个问题。

**全局表**

对于省、市、区/县表tb_areas_provinces , tb_areas_city , tb_areas_region，是属于

`数据字典表`，在`多个业务模块中都可能会遇到`，可以将其设置为全局表，利于业务操作。

==修改schema.xml中的逻辑表的配置，修改 tb_areas_provinces、tb_areas_city、tb_areas_region 三个逻辑表，增加 type 属性，配置为global，就代表该表是全局表，就会在所涉及到的dataNode中创建给表。对于当前配置来说，也就意味着所有的节点中都有该表了==

```sql
<table name="tb_areas_provinces" dataNode="dn1,dn2,dn3" primaryKey="id" type="global"/>
<table name="tb_areas_city" dataNode="dn1,dn2,dn3" primaryKey="id" type="global"/>
<table name="tb_areas_region" dataNode="dn1,dn2,dn3" primaryKey="id" type="global"/>
```

![image-20220508151654988](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081516161.png)

配置完毕后，重新启动MyCat。

1). 删除原来每一个数据库服务器中的所有表结构

2). 通过source指令，导入表及数据

![image-20220508151715774](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081517871.png)

3). 检查每一个数据库服务器中的表及数据分布，看到三个节点中都有这三张全局表

4). 然后再次执行上面的多表联查的SQL语句

```sql
SELECT order_id , payment ,receiver, province , city , area FROM tb_order_master o , tb_areas_provinces p , tb_areas_city c , tb_areas_region r WHERE o.receiver_province = p.provinceid AND o.receiver_city = c.cityid AND o.receiver_region = r.areaid ;
```

![image-20220508151755312](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081517807.png)

是可以正常执行成功的。

5). 当在MyCat中更新全局表的数据时候，我们可以看到，所有分片节点中的数据都发生了变化，每个节点的全局表数据时刻保持一致。



#### Mycat分片-水平分表

**场景**

在业务系统中, 有一张表(日志表), 业务系统每天都会产生大量的日志数据 , 单台服务器的数据存储及处理能力是有限的, 可以对数据库表进行拆分。

![image-20220508153833070](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081538226.png)



 **准备**

准备两台服务器，具体的结构如下：

![image-20220508153923945](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081539083.png)

并且，在两台数据库服务器中分表创建一个数据库itcast。

**配置**

1). schema.xml

```sql
<?xml version="1.0"?>
<!DOCTYPE mycat:schema SYSTEM "schema.dtd">
<mycat:schema xmlns:mycat="http://io.mycat/">

	<schema name="SHOPPING" checkSQLschema="true" sqlMaxLimit="100">
		<table name="tb_goods_base" dataNode="dn1" primaryKey="id" /> 
		<table name="tb_goods_brand" dataNode="dn1" primaryKey="id" /> 
		<table name="tb_goods_cat" dataNode="dn1" primaryKey="id" />
		 <table name="tb_goods_desc" dataNode="dn1" primaryKey="goods_id" /> 
		<table name="tb_goods_item" dataNode="dn1" primaryKey="id" />
		 <table name="tb_order_item" dataNode="dn2" primaryKey="id" /> 
		<table name="tb_order_master" dataNode="dn2" primaryKey="order_id" /> 
		<table name="tb_order_pay_log" dataNode="dn2" primaryKey="out_trade_no" />
		 <table name="tb_user" dataNode="dn2" primaryKey="id" /> 
		<table name="tb_user_address" dataNode="dn2" primaryKey="id" /> 
		<table name="tb_areas_provinces" dataNode="dn2" primaryKey="id"/>
		<table name="tb_areas_city" dataNode="dn2" primaryKey="id"/>
		 <table name="tb_areas_region" dataNode="dn2" primaryKey="id"/>		
	</schema>
<schema name="ITCAST" checkSQLschema="true" sqlMaxLimit="100">
		<table name="tb_log" dataNode="dn3,dn4" primaryKey="id" rule="mod-long" /> 
	</schema>

	<dataNode name="dn1" dataHost="dhost1" database="shopping" />
	<dataNode name="dn2" dataHost="dhost2" database="shopping" />

	<dataNode name="dn3" dataHost="dhost1" database="itcast" />
	<dataNode name="dn4" dataHost="dhost2" database="itcast" />

	<dataHost name="dhost1" maxCon="1000" minCon="10" balance="0"
			  writeType="0" dbType="mysql" dbDriver="jdbc" switchType="1"  slaveThreshold="100">
		<heartbeat>select user()</heartbeat>
		<writeHost host="master" url="jdbc:mysql://120.24.210.204:3306?useSSL=false&amp;serverTimezone=Hongkong&amp;characterEncoding=utf-8&amp;autoReconnect=true"
 user="root" password="123123" />
		
		 </dataHost>

<dataHost name="dhost2" maxCon="1000" minCon="10" balance="0"
			  writeType="0" dbType="mysql" dbDriver="jdbc" switchType="1"  slaveThreshold="100">
		<heartbeat>select user()</heartbeat>
		<writeHost host="master" url="jdbc:mysql://120.79.14.203:3306?useSSL=false&amp;serverTimezone=Hongkong&amp;characterEncoding=utf-8&amp;autoReconnect=true"
 user="root" password="123123" />
		
		 </dataHost>
</mycat:schema>
```

主要看下面这段，上面的schema是之前配的不用去管。

![image-20220508154626496](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081546617.png)

tb_log表最终落在2个节点中，分别是 dn3、dn4，而具体的数据分别存储在 dhost1、dhost2的itcast数据库中。

`rule=mod-long`:表示数据分配规则是取模

对应rule.xml文件下面两个。 2表示记录`id%2==0 `数据放第一个分片结点`id%2 ==1 `数据放在第二个分片结点

![image-20220508155456059](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081554192.png)

![image-20220508155524344](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081555451.png)

2). server.xml

配置root用户既可以访问 SHOPPING 逻辑库，又可以访问ITCAST逻辑库。

```SQL
<user name="root" defaultAccount="true">
		<property name="password">123123</property>
		<property name="schemas">SHOPPING,ITCAST</property>
		
		<!-- 表级 DML 权限设置 -->
		<!-- 		
		<privileges check="false">
			<schema name="TESTDB" dml="0110" >
				<table name="tb01" dml="0000"></table>
				<table name="tb02" dml="1111"></table>
			</schema>
		</privileges>		
		 -->
	</user>
```

**测试**

配置完毕后，重新启动MyCat，然后在mycat的命令行中查看逻辑库：

![image-20220508160955934](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081609052.png)

执行如下SQL创建表、并插入数据，查看数据分布情况

![image-20220508154059065](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081540215.png)

插入id为1-4的4条数据，查看数据分布

==**发现1和3记录在第一个分片结点数据库中，2，4在第二个分片结点数据库中**==



#### 分片规则

##### 范围分片

1). 介绍

根据指定的字段及其配置的范围与数据节点的对应情况， 来决定该数据属于哪一个分片。

![image-20220508162730130](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081627273.png)

2). 配置

schema.xml逻辑表配置：

```sql
<table name="TB_ORDER" dataNode="dn1,dn2,dn3" rule="auto-sharding-long" />
```

schema.xml数据节点配置：

```sql
<dataNode name="dn1" dataHost="dhost1" database="db01" />
<dataNode name="dn2" dataHost="dhost2" database="db01" /> 
<dataNode name="dn3" dataHost="dhost3" database="db01" />
```

rule.xml分片规则配置：

```sql
<tableRule name="auto-sharding-long">
	<rule>
		<columns>id</columns> 
		<algorithm>rang-long</algorithm> 
	</rule> 
</tableRule> 

	<function name="rang-long" class="io.mycat.route.function.AutoPartitionByLong">
   		 <property name="mapFile">autopartition-long.txt</property>
   		 <property name="defaultNode">0</property> 
   	</function>
```

分片规则配置属性含义：

补充下：columns也就是根据这个字段的值结合外部配置文件的规则，将数据存放到某个结点

![image-20220508163032152](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081630313.png)

在rule.xml中配置分片规则时，关联了一个映射配置文件 autopartition-long.txt，该配置文

件的配置如下：

```sql
# range start-end ,data node index # K=1000,M=10000. 
0-500M=0 
500M-1000M=1 
1000M-1500M=2
```

含义：0-500万之间的值，存储在0号数据节点(数据节点的索引从0开始) ； 500万-1000万之间的数据存储在1号数据节点 ； 1000万-1500万的数据节点存储在2号节点 ； 

该分片规则，主要是针对于`数字类型的字段`适用。 在MyCat的入门程序中，我们使用的就是该分片规则



##### 取模分片

1). 介绍

根据`指定的字段值与节点数量`进行`求模`运算，根据运算结果， 来决定该数据属于哪一个分片(`分片就是数据库`)。

![image-20220508175141452](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081751613.png)

2). 配置

schema.xml逻辑表配置：

```sql
<table name="tb_log" dataNode="dn4,dn5,dn6" primaryKey="id" rule="mod-long" />
```

schema.xml数据节点配置：

```sql
<dataNode name="dn4" dataHost="dhost1" database="itcast" /> 
<dataNode name="dn5" dataHost="dhost2" database="itcast" /> 
<dataNode name="dn6" dataHost="dhost3" database="itcast" />
```

rule.xml分片规则配置：

```sql
<tableRule name="mod-long"> 
	<rule>
		<columns>id</columns> 
		<algorithm>mod-long</algorithm> 
	</rule> 
</tableRule>

<function name="mod-long" class="io.mycat.route.function.PartitionByMod">
	<property name="count">3</property>
</function>
```

分片规则属性说明如下：

![image-20220508175439517](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081754646.png)

该分片规则，主要是`针对于数字类型的字段`适用。 `在前面水平拆分的演示`中，我们选择的就是`取模分片。`





##### 一致性hash分片

1). 介绍

所谓一致性哈希，==相同的哈希因子计算值总是被划分到相同的分区表中，不会因为分区节点的增加而改变原来数据的分区位置==

![image-20220508180011375](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081800510.png)

2). 配置

schema.xml中逻辑表配置：

```sql
<!-- 一致性hash --> 
<table name="tb_order" dataNode="dn3,dn4" rule="sharding-by-murmur" />
```

schema.xml中数据节点配置：

```sql
<schema name="ITCAST" checkSQLschema="true" sqlMaxLimit="100">
		<table name="tb_log" dataNode="dn3,dn4" primaryKey="id" rule="mod-long" /> 
		<table name="tb_order" dataNode="dn3,dn4" rule="sharding-by-murmur" />
	</schema>
```

rule.xml中分片规则配置：

```sql
<tableRule name="sharding-by-murmur"> 
	<rule>
		<columns>id</columns> 
		<algorithm>murmur</algorithm> 
	</rule> 
</tableRule> 

<function name="murmur" class="io.mycat.route.function.PartitionByMurmurHash"> 
	<property name="seed">0</property> <!-- 默认是0 --> 
	<property name="count">3</property>
    <property name="virtualBucketTimes">160</property> 
</function>
```

`重启mycat  发现ITCAST虚拟库多了一张tb_order 虚拟表`

![image-20220508182404621](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081824766.png)

分片规则属性含义：

![image-20220508180318924](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081803073.png)

3). 测试

配置完毕后，重新启动MyCat，然后在mycat的命令行中，执行如下SQL创建表、并插入数据，查看数据分布情况

```sql
create table tb_order(
    id  varchar(100) not null primary key,
    money   int null,
    content varchar(200) null
);
```

```sql
INSERT INTO tb_order (id, money, content) VALUES ('b92fdaaf-6fc4-11ec-b831-482ae33c4a2d', 10, 'b92fdaf8-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b93482b6-6fc4-11ec-b831-482ae33c4a2d', 20, 'b93482d5-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b937e246-6fc4-11ec-b831-482ae33c4a2d', 50, 'b937e25d-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b93be2dd-6fc4-11ec-b831-482ae33c4a2d', 100, 'b93be2f9-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b93f2d68-6fc4-11ec-b831-482ae33c4a2d', 130, 'b93f2d7d-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b9451b98-6fc4-11ec-b831-482ae33c4a2d', 30, 'b9451bcc-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b9488ec1-6fc4-11ec-b831-482ae33c4a2d', 560, 'b9488edb-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b94be6e6-6fc4-11ec-b831-482ae33c4a2d', 10, 'b94be6ff-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b94ee10d-6fc4-11ec-b831-482ae33c4a2d', 123, 'b94ee12c-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b952492a-6fc4-11ec-b831-482ae33c4a2d', 145, 'b9524945-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b95553ac-6fc4-11ec-b831-482ae33c4a2d', 543, 'b95553c8-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b9581cdd-6fc4-11ec-b831-482ae33c4a2d', 17, 'b9581cfa-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b95afc0f-6fc4-11ec-b831-482ae33c4a2d', 18, 'b95afc2a-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b95daa99-6fc4-11ec-b831-482ae33c4a2d', 134, 'b95daab2-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b9667e3c-6fc4-11ec-b831-482ae33c4a2d', 156, 'b9667e60-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b96ab489-6fc4-11ec-b831-482ae33c4a2d', 175, 'b96ab4a5-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b96e2942-6fc4-11ec-b831-482ae33c4a2d', 180, 'b96e295b-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b97092ec-6fc4-11ec-b831-482ae33c4a2d', 123, 'b9709306-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b973727a-6fc4-11ec-b831-482ae33c4a2d', 230, 'b9737293-6fc4-11ec-b831-482ae33c4a2d');
INSERT INTO tb_order (id, money, content) VALUES ('b978840f-6fc4-11ec-b831-482ae33c4a2d', 560, 'b978843c-6fc4-11ec-b831-482ae33c4a2d');
```



##### 枚举分片

1). 介绍

通过在==配置文件中配置可能的枚举值==, 指定数据分布到不同数据节点上, 本规则适用于==按照省份、性别、状态拆分数据==等业务 。

![image-20220508184846556](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081848693.png)

2). 配置

schema.xml中逻辑表配置：

```xml
<!-- 枚举 --> 

		<table name="tb_user" dataNode="dn3,dn4" rule="sharding-by-intfile-enumstatus" />
```

schema.xml中数据节点配置：

```xml
<dataNode name="dn3" dataHost="dhost1" database="itcast" /> 
<dataNode name="dn4" dataHost="dhost2" database="itcast" /> 
```

rule.xml中分片规则配置：

```sql
<tableRule name="sharding-by-intfile"> 
	<rule>
		<columns>sharding_id</columns> 
		<algorithm>hash-int</algorithm> 
	</rule> 
</tableRule> 

<!-- 自己增加(不是mycat自带的) tableRule 自己增加的好处就是可以不使用原来自带的。因为可能我们有多个表都要用到hash-int算法 --> 
<tableRule name="sharding-by-intfile-enumstatus">
		<rule>
			<columns>status</columns>
			<algorithm>hash-int</algorithm>
		</rule>
	</tableRule>

<function name="hash-int" class="io.mycat.route.function.PartitionByFileMap"> 
	<property name="defaultNode">1</property> 
	<property name="mapFile">partition-hash-int.txt</property> 
</function>
```

partition-hash-int.txt ，内容如下 : 

```xml
1=0 
2=1 
3=1
```

分片规则属性含义：

![image-20220508185217703](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205081852838.png)

3). 测试

配置完毕后，重新启动MyCat，然后在mycat的命令行中，执行如下SQL创建表、并插入数据，查看数据分布情况。

```sql
CREATE TABLE tb_user (
  id bigint(20) NOT NULL COMMENT 'ID',
  username varchar(200) DEFAULT NULL COMMENT '姓名',
  status int(2) DEFAULT '1' COMMENT '1: 未启用, 2: 已启用, 3: 已关闭',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;


insert into tb_user (id,username ,status) values(1,'Tom',1);
insert into tb_user (id,username ,status) values(2,'Cat',2);
insert into tb_user (id,username ,status) values(3,'Rose',3);
insert into tb_user (id,username ,status) values(4,'Coco',2);
insert into tb_user (id,username ,status) values(5,'Lily',1);
insert into tb_user (id,username ,status) values(6,'Tom',1);
insert into tb_user (id,username ,status) values(7,'Cat',2);
insert into tb_user (id,username ,status) values(8,'Rose',3);
insert into tb_user (id,username ,status) values(9,'Coco',2);
insert into tb_user (id,username ,status) values(10,'Lily',1);
```

> 查看：

==第一个库数据 status全是1==



==第二个分片数据库数据 status全是2或者3==



##### 应用指定算法

1). 介绍

运行阶段由应用自主决定路由到那个分片 , 直接根据字符子串（`必须是数字`）计算分片号。

![image-20220508213835446](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205082138534.png)

2). 配置

schema.xml中逻辑表配置：

```sql
<!-- 应用指定算法 -->
<table name="tb_app" dataNode="dn3,dn4" rule="sharding-by-substring" />
```

schema.xml中数据节点配置：

```sql
<dataNode name="dn3" dataHost="dhost1" database="itcast" />
	<dataNode name="dn4" dataHost="dhost2" database="itcast" />
```

rule.xml中分片规则配置：

```sql
<tableRule name="sharding-by-substring"> 
	<rule>
		<columns>id</columns> 
		<algorithm>sharding-by-substring</algorithm> 
	</rule> 
</tableRule> 

<function name="sharding-by-substring" 
class="io.mycat.route.function.PartitionDirectBySubString"> 
	<property name="startIndex">0</property> <!-- zero-based -->
    <property name="size">2</property> 
    <property name="partitionCount">2</property> 
    <property name="defaultPartition">0</property> 
 </function>
```

分片规则属性含义：

![image-20220509002825065](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205090028407.png)

3). 测试

配置完毕后，重新启动MyCat，然后在mycat的命令行中，执行如下SQL创建表、并插入数据，查看数据分布情况。

```sql

CREATE TABLE tb_app (
  id varchar(10) NOT NULL COMMENT 'ID',
  name varchar(200) DEFAULT NULL COMMENT '名称',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;



insert into tb_app (id,name) values('0000001','Testx00001');
insert into tb_app (id,name) values('0100001','Test100001');
insert into tb_app (id,name) values('0100002','Test200001');
insert into tb_app (id,name) values('0200001','Test300001');
insert into tb_app (id,name) values('0200002','TesT400001');
```

> 运行结果：

==第一个分片结点数据是00或者02开头==

==第二个分片结点数据全是01开头==



##### 固定分片hash算法

1). 介绍

该算法类似于十进制的求模运算，但是为二进制的操作，例如，取 id 的二进制低 10 位 与1111111111 进行位 & 运算，位与运算最小值为 0000000000，最大值为1111111111，转换为十进制，也就是位于0-1023之间。

![image-20220509005543302](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205090055460.png)

特点：

- 如果是求模，连续的值，分别分配到各个不同的分片；但是此算法会`将连续的值可能分配到相同的分片，降低事务处理的难度`。
- 可以均匀分配，也可以非均匀分配。
- 分片字段必须为数字类型。



2). 配置

schema.xml中逻辑表配置：

```sql
<!-- 固定分片hash算法 --> 
<table name="tb_longhash" dataNode="dn3,dn4" rule="sharding-by-long-hash" />
```

schema.xml中数据节点配置：

```sql
<dataNode name="dn3" dataHost="dhost1" database="itcast" />
<dataNode name="dn4" dataHost="dhost2" database="itcast" />
```

rule.xml中分片规则配置：

```sql
<tableRule name="sharding-by-long-hash"> 
	<rule>
		<columns>id</columns> 
		<algorithm>sharding-by-long-hash</algorithm> 
	</rule> 
</tableRule> 

<!-- 分片总长度为1024，count与length数组长度必须一致； --> 
<function name="sharding-by-long-hash" 
class="io.mycat.route.function.PartitionByLong"> 
	<property name="partitionCount">1,1</property> 
	<property name="partitionLength">512,512</property> 
</function>
```

分片规则属性含义：

![image-20220509005914338](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205090059475.png)

约束 :

1). 分片长度 : 默认最大2^10 , 为 1024 ;

2). count, length的数组长度必须是一致的 ;

以上分为三个分区:0-511,512-1023

> 原理分析图，底层维护了一个1024长度的数组，初始化时将数组赋值为分片索引值。运算后的值去找数组，数组对应的下标即为这条数据应该插入哪个分片

![image-20220509005938037](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205090059147.png)

3). 测试
配置完毕后，重新启动MyCat，然后在mycat的命令行中，执行如下SQL创建表、并插入数据，查看数据分布情况。

```sql

CREATE TABLE tb_longhash (
  id int(11) NOT NULL COMMENT 'ID',
  name varchar(200) DEFAULT NULL COMMENT '名称',
  firstChar char(1)  COMMENT '首字母',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;



insert into tb_longhash (id,name,firstChar) values(1,'七匹狼','Q');
insert into tb_longhash (id,name,firstChar) values(2,'八匹狼','B');
insert into tb_longhash (id,name,firstChar) values(3,'九匹狼','J');
insert into tb_longhash (id,name,firstChar) values(4,'十匹狼','S');
insert into tb_longhash (id,name,firstChar) values(5,'六匹狼','L');
insert into tb_longhash (id,name,firstChar) values(6,'五匹狼','W');
insert into tb_longhash (id,name,firstChar) values(7,'四匹狼','S');
insert into tb_longhash (id,name,firstChar) values(8,'三匹狼','S');
insert into tb_longhash (id,name,firstChar) values(9,'两匹狼','L');
```

==**运行发现，数据全在第一个分片结点**==

再插入：

INSERT INTO TB_LONGHASH (ID,NAME,FIRSTCHAR) VALUES(512,'三匹狼','S');

发现数据在第二个分片结点

再插入下面两条

![image-20220509011533052](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205090115126.png)

==**发现第一条在第二个分片结点，第二条在第一个分片结点。**==



##### 字符串hash解析算法

1). 介绍

截取字符串中的指定位置的子字符串, 进行hash算法， 算出分片  `1023的二进制表示就是10个1`

![image-20220509101201816](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091012049.png)2). 配置

schema.xml中逻辑表配置：

```xml
<!-- 字符串hash解析算法 --> 
<table name="tb_strhash" dataNode="dn3,dn4" rule="sharding-by-stringhash" />
```

schema.xml中数据节点配置：

```xml
<dataNode name="dn3" dataHost="dhost1" database="itcast" /> 
<dataNode name="dn4" dataHost="dhost2" database="itcast" />
```

rule.xml中分片规则配置：

```xml
<tableRule name="sharding-by-stringhash"> 
    <rule>
        <columns>name</columns> 
        <algorithm>sharding-by-stringhash</algorithm> 
    </rule> 
</tableRule> 

<function name="sharding-by-stringhash" 
class="io.mycat.route.function.PartitionByString"> 
    <property name="partitionLength">512</property> <!-- zero-based --> 
    <property name="partitionCount">2</property> 
    <property name="hashSlice">0:2</property>
</function>
```

分片规则属性含义：

![image-20220509101410972](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091014188.png)

示例说明：

![image-20220509101424813](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091014028.png)

3). 测试

配置完毕后，重新启动MyCat，然后在mycat的命令行中，执行如下SQL创建表、并插入数据，查看数据分布情况。

```sql
create table tb_strhash(
	name varchar(20) primary key,
	content varchar(100)
)engine=InnoDB DEFAULT CHARSET=utf8mb4;


INSERT INTO tb_strhash (name,content) VALUES('T1001', UUID());
INSERT INTO tb_strhash (name,content) VALUES('ROSE', UUID());
INSERT INTO tb_strhash (name,content) VALUES('JERRY', UUID());
INSERT INTO tb_strhash (name,content) VALUES('CRISTINA', UUID());
INSERT INTO tb_strhash (name,content) VALUES('TOMCAT', UUID());
```





##### 按天分片算法

1). 介绍

按照日期及对应的时间周期来分片。

![image-20220509103639104](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091036280.png)

2). 配置

schema.xml中逻辑表配置：

```xml
<!-- 按天分片 --> 
<table name="tb_datepart" dataNode="dn3,dn4" rule="sharding-by-date" />
```

schema.xml中数据节点配置：

```xml
<dataNode name="dn3" dataHost="dhost1" database="itcast" /> 
<dataNode name="dn4" dataHost="dhost2" database="itcast" /> 
```

rule.xml中分片规则配置：

```xml
<tableRule name="sharding-by-date"> 
    <rule>
        <columns>create_time</columns> 
        <algorithm>sharding-by-date</algorithm> 
    </rule> 
</tableRule>


<function name="sharding-by-date"
class="io.mycat.route.function.PartitionByDate"> 
    <property name="dateFormat">yyyy-MM-dd</property> 
    <property name="sBeginDate">2022-01-01</property> 
    <property name="sEndDate">2022-01-20</property> 
    <property name="sPartionDay">10</property> 
</function>

<!--从开始时间开始，每10天为一个分片，到达结束时间之后，会重复开始分片插入 配置表的 dataNode 的分片，必须和分片规则数量一致，否则报错！ -->
```

分片规则属性含义：

![image-20220509103927262](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091039450.png)

3). 测试

配置完毕后，重新启动MyCat，然后在mycat的命令行中，执行如下SQL创建表、并插入数据，查看数据分布情况。

```sql
create table tb_datepart(
    id   bigint  not null comment 'ID'  primary key,
    name varchar(100) null comment '姓名',
    create_time date  null
);

insert into tb_datepart(id,name ,create_time) values(1,'Tom','2022-01-01');
insert into tb_datepart(id,name ,create_time) values(2,'Cat','2022-01-10');
insert into tb_datepart(id,name ,create_time) values(3,'Rose','2022-01-11');
insert into tb_datepart(id,name ,create_time) values(4,'Coco','2022-01-20');
insert into tb_datepart(id,name ,create_time) values(5,'Rose2','2022-01-21');
insert into tb_datepart(id,name ,create_time) values(6,'Coco2','2022-01-30');
insert into tb_datepart(id,name ,create_time) values(7,'Coco3','2022-01-31');
```

>  运行结果：

==第一个切片含有的数据是数据1256==

==第二个切片347==



##### 自然月分片

1). 介绍

使用场景为`按照月份来分片`, `每个自然月为一个分片`。

![image-20220509104049545](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091040902.png)2). 配置

schema.xml中逻辑表配置：

```xml
<!-- 按自然月分片 -->
<table name="tb_monthpart" dataNode="dn3,dn4" rule="sharding-by-month" />
```

schema.xml中数据节点配置：

```xml
<dataNode name="dn3" dataHost="dhost1" database="itcast" /> 
<dataNode name="dn4" dataHost="dhost2" database="itcast" /> 
```

rule.xml中分片规则配置：

```xml
<tableRule name="sharding-by-month"> 
    <rule>
        <columns>create_time</columns> 
        <algorithm>partbymonth</algorithm> 
    </rule> 
</tableRule> 

<function name="partbymonth" class="io.mycat.route.function.PartitionByMonth"> 
    <property name="dateFormat">yyyy-MM-dd</property> 
    <property name="sBeginDate">2022-01-01</property> 
    <property name="sEndDate">2022-02-28</property> 
</function> 

<!--从开始时间开始，一个月为一个分片，到达结束时间之后，会重复开始分片插入 配置表的 dataNode 的分片，必须和分片规则数量一致，例如 2022-01-01 到 2022-12-31 ，一 共需要12个分片。 -->
```

分片规则属性含义：

![image-20220509105511753](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091055920.png)

==**ps:他这个是按月来。你结束时间写2月22 插入2月28 还是在第二个节点 3月份的又到第一个分片 4月份的又到第2个分片 依次类推**==

3). 测试

配置完毕后，重新启动MyCat，然后在mycat的命令行中，执行如下SQL创建表、并插入数据，查看数据分布情况。

```sql

create table tb_monthpart(
    id   bigint  not null comment 'ID'  primary key,
    name varchar(100) null comment '姓名',
    create_time date  null
);

insert into tb_monthpart(id,name ,create_time) values(1,'Tom','2022-01-01');
insert into tb_monthpart(id,name ,create_time) values(2,'Cat','2022-01-10');
insert into tb_monthpart(id,name ,create_time) values(3,'Rose','2022-01-31');
insert into tb_monthpart(id,name ,create_time) values(4,'Coco','2022-02-20');
insert into tb_monthpart(id,name ,create_time) values(5,'Rose2','2022-02-25');
insert into tb_monthpart(id,name ,create_time) values(6,'Coco2','2022-03-10');
insert into tb_monthpart(id,name ,create_time) values(7,'Coco3','2022-03-31');
insert into tb_monthpart(id,name ,create_time) values(8,'Coco4','2022-04-10');
insert into tb_monthpart(id,name ,create_time) values(9,'Coco5','2022-04-30');
```

> 运行结果

==第一个分片结果：12367==

==第二个分片结果：4589==



#### MyCat管理及监控

> MyCat原理

![image-20220509140953929](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091409062.png)

在MyCat中，当执行一条SQL语句时，MyCat需要进行SQL解析、分片分析、路由分析、读写分离分析等操作，最终经过一系列的分析决定将当前的SQL语句到底路由到那几个(或哪一个)节点数据库，数据库将数据执行完毕后，如果有返回的结果，则将结果返回给MyCat，最终还需要在MyCat中进行结果合并、聚合处理、排序处理、分页处理等操作，最终再将结果返回给客户端。而在MyCat的使用过程中，MyCat官方也提供了一个`管理监控平台MyCat-Web（MyCat-eye）`。Mycat-web 是 ==Mycat 可视化运维的管理和监控平台==，弥补了 Mycat 在监控上的空白。`帮 Mycat分担统计任务和配置管理任务`。Mycat-web 引入了 ==ZooKeeper== 作为配置中心，可以管理多个节点。==Mycat-web 主要管理和监控 Mycat 的流量、连接、活动线程和内存等，具备 IP 白名单、邮件告警等模块，还可以统计 SQL 并分析慢 SQL 和高频 SQL 等。为优化 SQL 提供依据。==



##### MyCat管理

Mycat默认开通2个端口，可以在server.xml中进行修改。

- 8066 数据访问端口，即进行 DML 和 DDL 操作。
- 9066 数据库管理端口，即 mycat 服务管理控制功能，用于管理mycat的整个集群状态

连接MyCat的管理控制台

```sql
mysql -h 127.0.0.1 -P 9066 -uroot -p

```

![image-20220511143830023](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205111438181.png)

`相关指令：`

![image-20220509141238037](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091412193.png)

随便测试几个：

![image-20220509142003863](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091420007.png)

![image-20220509142134236](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091421354.png)

刚开始我们server.xml里面的这玩意是0，因此无论mycat怎么执行sql，我们的show @@sql和show @@sql.sum都是空的

![image-20220509144244003](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091442164.png)

修改成1，修改了配置文件我们理应重启8066让其生效，但是我们只需要在9066的这个客户端输入reload @@config 即可不用退出8066客户端，8066相当于是重启以后的状态（`配置都是最新的了`）。



##### **MyCat-eye**

**介绍**

Mycat-web(Mycat-eye)是对mycat-server提供`监控服`务，功能不局限于对mycat-server使用。他通过`JDBC连接对Mycat、Mysql监控`，`监控远程服务器(目前仅限于linux系统)的cpu、内存、网络、磁盘。`

Mycat-eye运行过程中需要依赖`zookeeper`，因此需要先安装zookeeper。

##### 安装

1). zookeeper安装

A. 上传安装包到root目录 
	zookeeper-3.4.6.tar.gz
	
B. 解压
	tar -zxvf zookeeper-3.4.6.tar.gz -C /usr/local/

​	

C. 创建数据存放目录
	cd /usr/local/zookeeper-3.4.6/
	mkdir data

​	

D. 修改配置文件名称并配置  

cd /usr/local/zookeeper-3.4.6/conf

mv zoo_sample.cfg zoo.cfg  `重命名为zoo.cfg`

dataDir=/usr/local/zookeeper-3.4.6/data
	

![image-20220509150420377](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091504535.png)


F. 启动Zookeeper
进入`zookeeper`目录 执行指令：bin/zkServer.sh start

`bin/zkServer.sh status` 查看状态

![image-20220511150812471](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205111508569.png)

==出现上面红色所圈 说明启动成功==



2). Mycat-web安装

A. 上传安装包到root目录 
	Mycat-web.tar.gz
	

B. 解压
	tar -zxvf Mycat-web.tar.gz -C /usr/local/

C.根据需要修改ip和端口（`下图圈起来的地方`）

> 备注: 
>
> ​	如果Zookeeper与Mycat-web不在同一台服务器上 , 需要设置Zookeeper的地址 ; 在/usr/local/mycat-web/mycat-web/WEB-INF/classes/mycat.properties文件中配置 : 
>

![image-20220511151231456](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205111512558.png)

D. 进入解压后的文件夹启动
	sh  start.sh & 	

E. 访问
	http://192.168.200.210:8082/mycat

##### 案例

1). 开启MyCat的实时统计功能(server.xml)

```xml
<property name="useSqlStat">1</property> <!-- 1为开启实时统计、0为关闭 -->
```

2). 在Mycat监控界面配置服务地址



 **测试**

配置好了之后，我们可以通过MyCat执行一系列的增删改查的测试，然后过一段时间之后，打开mycat-eye的管理界面，查看mycat-eye监控到的数据信息。

A. 性能监控

![image-20220509194408519](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091944715.png)

B. 物理节点

![image-20220509194422178](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091944318.png)

C. SQL统计

![image-20220509194435960](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091944085.png)

D. SQL表分析

![image-20220509194454615](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091944758.png)

E. SQL监控

![image-20220509194513423](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091945554.png)

F. 高频SQL

![image-20220509194530115](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205091945276.png)



##  读写分离

### 介绍

读写分离,简单地说是把对数据库的读和写操作分开,以对应不同的数据库服务器。主数据库提供写操作，从数据库提供读操作，这样能有效地减轻单台数据库的压力。

通过MyCat即可轻易实现上述功能，不仅可以支持MySQL，也可以支持Oracle和SQL Server。 

![image-20220509204213499](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092042723.png)

 

**一主一从** 

**原理**

MySQL的主从复制，是基于二进制日志（binlog）实现的。

![image-20220509204311747](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092043925.png)

**4.2.2** **准备**

![image-20220509204333834](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092043968.png)

搭建好主从配置环境后，将下面sql在主库中执行

```sql
CREATE TABLE TB_USER(
	ID INT(11) NOT NULL,
	NAME VARCHAR(50) NOT NULL,
	SEX VARCHAR(1),
	PRIMARY KEY (ID)
)ENGINE=INNODB DEFAULT CHARSET=UTF8;

INSERT INTO TB_USER(ID,NAME,SEX) VALUES(1,'TOM','1');
INSERT INTO TB_USER(ID,NAME,SEX) VALUES(2,'TRIGGER','0');
INSERT INTO TB_USER(ID,NAME,SEX) VALUES(3,'DAWN','1');
```

备注：主从复制的搭建，可以参考前面课程中 **主从复制** 章节讲解的步骤操作

 

### 一主一从读写分离

MyCat控制后台数据库的读写分离和负载均衡由schema.xml文件datahost标签的balance属性控制。

> schema.xml配置

```xml
<!-- 配置逻辑库 --> 
<schema name="ITCAST_RW" checkSQLschema="true" sqlMaxLimit="100" dataNode="dn7"> 
</schema> 

<dataNode name="dn7" dataHost="dhost7" database="itcast" />

	<dataHost name="dhost7" maxCon="1000" minCon="10" balance="1" writeType="0" dbType="mysql" dbDriver="jdbc" switchType="1" slaveThreshold="100"> 
    <heartbeat>select user()</heartbeat> 
    
    <writeHost host="master1" url="jdbc:mysql://120.24.210.204:3306?useSSL=false&amp;serverTimezone=Hongkong&amp;characterEncoding=utf-8&amp;autoReconnect=true" user="root" password="123123" > 		<readHost host="slave1" url="jdbc:mysql://120.79.14.203:3306?useSSL=false&amp;serverTimezone=Hongkong&amp;characterEncoding=utf-8&amp;autoReconnect=true" user="root" password="123123" /> </writeHost>

</dataHost>
```

上述配置的具体关联对应情况如下：

![image-20220509212630288](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092126934.png)

writeHost代表的是写操作对应的数据库，readHost代表的是读操作对应的数据库。 所以我们要想实现读写分离，就得配置writeHost关联的是主库，readHost关联的是从库。

而仅仅配置好了writeHost以及readHost还不能完成读写分离，还需要配置一个非常重要的负责均衡的`参数 balance`，取值有`4种`，具体含义如下：

![image-20220509212726349](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092127528.png)

所以，在一主一从模式的读写分离中，balance配置1或3都是可以完成读写分离的。



> server.xml配置

配置root用户可以访问SHOPPING、ITCAST 以及 ITCAST_RW逻辑库。

```xml
<user name="root" defaultAccount="true"> 
    <property name="password">123456</property> 
    <property name="schemas">SHOPPING,ITCAST,ITCAST_RW</property> 
    
    <!-- 表级 DML 权限设置 --> 
    <!-- 
	<privileges check="true"> 
		<schema name="DB01" dml="0110" >
 			<table name="TB_ORDER" dml="1110"></table> 
		</schema> 
	</privileges> --> 
</user>
```

**测试**

配置完毕MyCat后，重新启动MyCat。

```sql
bin/mycat stop 
bin/mycat start
```

> 测试mycat

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092155541.png)

然后观察，在执行增删改操作时，对应的主库及从库的数据变化。 在执行查询操作时，检查主库及从库对应的数据变化。

> 测试一

==我们将从库的tom改成tom1==  然后查询  `发现查出来的是tom1，这也就意味着查的是从库，因为主库是tom`

![image-20220509215831371](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092158495.png)

> 测试二：

插入一条数据

![image-20220509220124219](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092201338.png)

查看主库发现有数据，说明走的是主库（如果走的是从库，不会被同步到主库的（`也就是主库不可能有数据`））

![image-20220509220206083](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092202188.png)

> 测试三：我们将balance改为3，发现和balance=1实现的效果是一样的

![image-20220509220337177](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092203342.png)

> 测试4，将balance改为2   从下面结果可以看出是随机分发

![image-20220509220803552](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092208708.png)



在测试中，我们可以发现当主节点Master宕机之后，业务系统就只能够读，而不能写入数据了。

![image-20220509213053760](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092130216.png)

那如何解决这个问题呢？这个时候我们就得通过另外一种主从复制结构来解决了，也就是我们接下来讲解的双主双从。



### 双主双从

**介绍**

一个主机 Master1 用于`处理所有写请求`，它的`从机 Slave1 和另一台主机 Master2 还有它的从机 Slave2 负责所有读请求`。当 `Master1 主机宕机后，Master2 主机负责写请求`，==Master1 、Master2 互为备机==。架构图如下: 

![image-20220509232243363](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092322530.png)

**准备**

我们需要准备5台服务器，具体的服务器及软件安装情况如下：

![image-20220509232329251](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092323391.png)

关闭以上所有服务器的防火墙：

```xml
systemctl stop firewalld

systemctl disable firewalld
```

 

**搭建**

**主库配置**

**1). Master1(192.168.200.211)**

![image-20220509232509857](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092325993.png)

A. 修改配置文件 /etc/my.cnf

```sql
#mysql 服务ID，保证整个集群环境中唯一，取值范围：1 – 2^32-1，默认为1

server-id=1 

#指定同步的数据库 
binlog-do-db=db01 
binlog-do-db=db02
binlog-do-db=db03 

# 在作为从数据库的时候，有写入操作也要更新二进制日志文件 
log-slave-updates
```

B. 重启MySQL服务器

```sql
systemctl restart mysqld
```

C. 创建账户并授权

```sql
#创建itcast用户，并设置密码，该用户可在任意主机连接该MySQL服务 
CREATE USER 'itcast'@'%' IDENTIFIED WITH mysql_native_password BY 'Root@123456' ;

#为 'itcast'@'%' 用户分配主从复制权限 
GRANT REPLICATION SLAVE ON *.* TO 'itcast'@'%';
```

通过指令，查看两台主库的二进制日志坐标

```sql
show master status ;
```

![image-20220509232750950](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092327292.png)

**2). Master2(192.168.200.213)**

![image-20220509232811444](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092328681.png)

A. 修改配置文件 /etc/my.cnf

```sql
#mysql 服务ID，保证整个集群环境中唯一，取值范围：1 – 2^32-1，默认为1 

server-id=3 

#指定同步的数据库 
binlog-do-db=db01 
binlog-do-db=db02 
binlog-do-db=db03 

# 在作为从数据库的时候，有写入操作也要更新二进制日志文件
log-slave-updates
```



B. 重启MySQL服务器

```sql
systemctl restart mysqld
```

C. 创建账户并授权

```sql
#创建itcast用户，并设置密码，该用户可在任意主机连接该MySQL服务 
CREATE USER 'itcast'@'%' IDENTIFIED WITH mysql_native_password BY 'Root@123456' ;

#为 'itcast'@'%' 用户分配主从复制权限 
GRANT REPLICATION SLAVE ON *.* TO 'itcast'@'%';
```

通过指令，查看两台主库的二进制日志坐标

```sql
show master status ;
```

![image-20220509233032559](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092330956.png)

**从库配置**

**1). Slave1(192.168.200.212)**

![image-20220509233142878](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092331060.png)

A. 修改配置文件 /etc/my.cnf

```sql
#mysql 服务ID，保证整个集群环境中唯一，取值范围：1 – 232-1，默认为1

server-id=2
```

B. 重新启动MySQL服务器

```sql
systemctl restart mysqld
```

**2). Slave2(192.168.200.214)**

![image-20220509233306707](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092333918.png)

A. 修改配置文件 /etc/my.cnf

```sql
#mysql 服务ID，保证整个集群环境中唯一，取值范围：1 – 232-1，默认为1 
server-id=4 12
```

B. 重新启动MySQL服务器

```sql
systemctl restart mysqld
```

**从库关联主库**

**1).** **两台从库配置关联的主库**

![image-20220509233451559](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092334697.png)

`需要注意slave1对应的是master1，slave2对应的是master2。`

A. 在 slave1(192.168.200.212)上执行

```sql
CHANGE MASTER TO MASTER_HOST='192.168.200.211', MASTER_USER='itcast', MASTER_PASSWORD='Root@123456', MASTER_LOG_FILE='binlog.000002', MASTER_LOG_POS=663;
```

B. 在 slave2(192.168.200.214)上执行

```sql
CHANGE MASTER TO MASTER_HOST='192.168.200.213', MASTER_USER='itcast', MASTER_PASSWORD='Root@123456', MASTER_LOG_FILE='binlog.000002', MASTER_LOG_POS=663;
```

C. 启动两台从库主从复制，查看从库状态

```sql
start slave; show slave status \G;
```

![image-20220509233636647](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092336925.png)

**2).** **两台主库相互复制**

![image-20220509233736717](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092337972.png)

```sql
Master2 复制 Master1，Master1 复制 Master2。
```

A. 在 Master1(192.168.200.211)上执行 

```sql
CHANGE MASTER TO MASTER_HOST='192.168.200.213', MASTER_USER='itcast', MASTER_PASSWORD='Root@123456', MASTER_LOG_FILE='binlog.000002', MASTER_LOG_POS=663;
```

B. 在 Master2(192.168.200.213)上执行

```sql
CHANGE MASTER TO MASTER_HOST='192.168.200.211', MASTER_USER='itcast', MASTER_PASSWORD='Root@123456', MASTER_LOG_FILE='binlog.000002', MASTER_LOG_POS=663;
```

C. 启动两台从库主从复制，查看从库状态

```sql
start slave; show slave status \G;
```

![image-20220509234254876](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092342059.png)

经过上述的三步配置之后，双主双从的复制结构就已经搭建完成了。 接下来，我们可以来测试验证一下。

**测试**

分别在两台主库Master1、Master2上执行DDL、DML语句，查看涉及到的数据库服务器的数据同步情况。

```sql
create database db01; 
use db01; 
create table tb_user(
    id int(11) not null primary key ,
    name varchar(50) not null, 
    sex varchar(1) 
)engine=innodb default charset=utf8mb4; 

insert into tb_user(id,name,sex) values(1,'Tom','1');
insert into tb_user(id,name,sex) values(2,'Trigger','0'); 
insert into tb_user(id,name,sex) values(3,'Dawn','1'); 
insert into tb_user(id,name,sex) values(4,'Jack Ma','1'); 
insert into tb_user(id,name,sex) values(5,'Coco','0');
insert into tb_user(id,name,sex) values(6,'Jerry','1');
```

- 在Master1中执行DML、DDL操作，看看数据是否可以同步到另外的三台数据库中。

- 在Master2中执行DML、DDL操作，看看数据是否可以同步到另外的三台数据库中。

完成了上述双主双从的结构搭建之后，接下来，我们再来看看如何完成这种双主双从的读写分离。



### 双主双从读写分离

**配置**

MyCat控制后台数据库的==读写分离==和==负载均衡==由`schema.xml`文件`datahost`标签的`balance`属性控制，通过`writeType及switchType`来==完成失败自动切换==的。

1). schema.xml

配置逻辑库：

```sql
<schema name="ITCAST_RW2" checkSQLschema="true" sqlMaxLimit="100" dataNode="dn7"> </schema>
```

配置数据节点：

```sql
<dataNode name="dn7" dataHost="dhost7" database="db01" />
```

配置节点主机：

```sql
<dataHost name="dhost7" maxCon="1000" minCon="10" balance="1" writeType="0" 
dbType="mysql" dbDriver="jdbc" switchType="1" slaveThreshold="100"> 
	<heartbeat>select user()</heartbeat> 
	
	
	<writeHost host="master1" url="jdbc:mysql://192.168.200.211:3306? useSSL=false&amp;serverTimezone=Asia/Shanghai&amp;characterEncoding=utf8" 
user="root" password="1234" > 

		<readHost host="slave1" url="jdbc:mysql://192.168.200.212:3306? useSSL=false&amp;serverTimezone=Asia/Shanghai&amp;characterEncoding=utf8" 
		user="root" password="1234" /> 
	</writeHost>
		
	<writeHost host="master2" url="jdbc:mysql://192.168.200.213:3306? useSSL=false&amp;serverTimezone=Asia/Shanghai&amp;characterEncoding=utf8" user=
"root" password="1234" > 
		<readHost host="slave2" url="jdbc:mysql://192.168.200.214:3306? useSSL=false&amp;serverTimezone=Asia/Shanghai&amp;characterEncoding=utf8" 
		user="root" password="1234" /> 
	</writeHost> 
</dataHost>
```

具体的对应情况如下：

![image-20220509235429849](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205092354978.png)

属性说明：

```xml
balance="1"
代表全部的 readHost 与 stand by writeHost 参与 select 语句的负载均衡，简
单的说，当双主双从模式(M1->S1，M2->S2，并且 M1 与 M2 互为主备)，正常情况下，
M2,S1,S2 都参与 select 语句的负载均衡 ;
```



```xml
writeType
0 : 写操作都转发到第1台writeHost, writeHost1挂了, 会切换到writeHost2上;  但是这时查询还是writeHost2和全部的 readHost
1 : 所有的写操作都随机地发送到配置的writeHost上 ;
switchType
-1 : 不自动切换
1 : 自动切换
```



2). user.xml

配置root用户也可以访问到逻辑库 ITCAST_RW2。 

```xml
<user name="root" defaultAccount="true"> 
    <property name="password">123456</property> 
    <property name="schemas">SHOPPING,ITCAST,ITCAST_RW2</property>
    
    <!-- 表级 DML 权限设置 --> 
    <!-- 
	<privileges check="true"> 
		<schema name="DB01" dml="0110" > 
			<table name="TB_ORDER" dml="1110"></table> 
		</schema> 
	</privileges> 
	-->
</user>
```



**测试**

登录MyCat，测试查询及更新操作，判定是否能够进行读写分离，以及读写分离的策略是否正确。

当主库挂掉一个之后，是否能够自动切换。
