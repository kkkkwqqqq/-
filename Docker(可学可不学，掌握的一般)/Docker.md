# Docker

## Docker为什么出现？

> Docker的诞生场景：

一般来说，我们开发一款产品：会有开发和上线两套环境！

开发 ----运维。 问题：我在我的电脑上可以运行！版本更新，导致服务不可用！对于运维来说，考验就十分大？

但是现在对程序员的要求就是，开发既运维！

环境配置是十分麻烦的2，每一个机器都要部署环境（集群Redis、ES、Hadoop）！费时费力

发布一个项目（jar+环境（`Redis Mysql jdk ES`）），项目能不能都带上环境安装打包!

之前在服务器配置一个应用环境Redis Mysql jdk es hadoop，配置超麻烦，不能跨平台。

我们都是在Windows开发项目，最后发布到Liunx

传统：开发jar，运维来做！

现在：开发打包部署上线，一套流程过完！



> 一个类比：

`比如安卓开发：`

java  ---apk(安卓开发的东西) --发布（应用商店）---张三使用apk ---安装即可用！

java  ---jar（环境） ---打包项目带上环境（`镜像`） ---(Docker仓库：商店) ---下载我们发布的镜像 --直接运行即可！

==**Docker给以上的问题，提出了解决方案！**==

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204101925994.png" alt="image-20220410192546736" style="zoom:150%;" />

Docker的思想就来自于集装箱！

JRE ---多个应用（端口冲突）--原来都是交叉的

隔离： Docker核心思想！打包装箱！每个箱子都是互相隔离的。

Docker 通过隔离机制，可以将服务器利用到极致！

==**本质：所有的技术都是因为出现了一些问题，我们需要去解决，才去学习！**==



## Docker的历史

2010年，几个的年轻人，就在美国成立了一家公司 `dotcloud`

做一些pass的云计算服务！LXC（Linux Container容器）有关的容器技术！

Linux Container容器是一种==内核虚拟化技术==，可以提供==轻量级(启动消耗的资源小)==的虚拟化，以便隔离进程和资源。

他们将自己的技术（容器化技术）命名就是 Docker
Docker刚刚延生的时候，没有引起行业的注意！dotCloud，就活不下去！

`开源`

2013年，Docker开源！

越来越多的人发现docker的优点！火了。Docker每个月都会更新一个版本！

2014年4月9日，Docker1.0发布！

docker为什么这么火？十分的轻巧！

在容器技术出来之前，我们都是使用==虚拟机技术（Vmware）==

虚拟机：在window中装一个VMware，通过这个软件我们可以虚拟出来一台或者多台电脑！笨重（`特别占用电脑内存,可以说是重量级的了,多虚拟出几台就寄了`）！

虚拟机也属于==虚拟化技术==，Docker容器技术，也是一种==虚拟化技术==！

```
vm : linux centos 原生镜像（==相当于一个电脑！==） 隔离、需要开启多个虚拟机！ 几个G 几分钟
docker: 隔离，镜像（最核心的环境 4m + jdk + mysql）十分的小巧，运行镜像就可以了！小巧！ 几个M 秒级启动！
```

>
> 聊聊Docker

Docker基于==Go语言==开发的！开源项目！

docker官网：https://www.docker.com/

文档：https://docs.docker.com/ Docker的文档是超级详细的！

![image-20220410195702580](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204101957708.png)

仓库：https://hub.docker.com/



## Docker能干嘛

> 之前的虚拟机技术！  从低到顶依次是内核，库（也就是运行环境），应用，若lib冲突，则运维需要去调（比如两个app共用一个端口等）

![image-20220410200443792](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102004890.png)

虚拟机技术缺点：

1、 资源占用十分多

2、 冗余步骤多（开机等）

3、 启动很慢！

> 容器化技术
>

==容器化技术不是模拟一个完整的操作系统（虚拟机是）==

![image-20220410200921910](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102009991.png)

> 比较Docker和虚拟机技术的不同：
>

- 传统虚拟机，虚拟出一条硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件
- 容器内的应用直接运行在宿主机的内容，容器是没有自己的内核的，也没有虚拟我们的硬件，所以就轻便了
  每个容器间是互相隔离
- 每个容器内都有一个属于自己的文件系统，互不影响

> DevOps（开发、运维）

==应用更快速的交付和部署：==

传统：一堆帮助文档，安装程序。

Docker：打包镜像发布测试一键运行。

==更便捷的升级和扩缩容：==

使用了 Docker之后，我们部署应用就和搭积木一样
项目打包为一个镜像，扩展服务器A！服务器B

**更简单的系统运维
在容器化之后，我们的开发，测试环境都是高度一致的**

==更高效的计算资源利用：==

Docker是内核级别的虚拟化，可以在一个物理机上可以运行很多的容器实例！服务器的性能可以被压榨到极致。



## Docker的基本组成

![image-20220410203628179](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102036277.png)

==**镜像（image)：**==

docker镜像就好比是一个模板，可以通过这个模板来创建容器服务，tomcat镜像=>run=>tomcat01容器（提供服务器），通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中的）。

==**容器(container)：**==

Docker利用容器技术，独立运行一个或者一组应用，通过镜像来创建的

启动，停止，删除，基本命令

目前就可以把这个容器理解为就是一个简易的==Linux系统==

==**仓库(repository)：**==

仓库就是存放镜像的地方！

仓库分为公有仓库和私有仓库。(很类似git)

Docker Hub是国外的。

阿里云…都有容器服务器(配置镜像加速!)



## 安装Docker

==**这里我们用centos7**==

> 云服务器环境查看：

```shell
# Linux要求内核3.0以上
[root@id ~]# uname -r
3.10.0-862.14.4.el7.x86_64
```

```shell
#系统版本
[root@id ~]# cat /etc/os-release
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```

> docker官网找到安装教程(==帮助文档==)，然后一步步跟着执行：

```shell
#1、卸载旧的版本
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
                  
#2、需要的安装包
yum install -y yum-utils

#3、设置镜像的仓库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo  #默认是从国外的！

yum-config-manager \
    --add-repo \
	http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo  #推荐使用阿里云的，十分快

#更新yum软件包索引
yum makecache fast

# 4、安装docker docker-ce 社区 ee 企业版
yum install docker-ce docker-ce-cli containerd.io

#5、启动docker
systemctl start docker
#6、使用docker version 查看是否安装
```

![image-20220410214205241](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102142324.png)

> 启动镜像：因为刚开始我们的docker没有hello-world这个服务 所以出现了下面第一个所圈 然后去远程仓库中下载

![image-20220410214505709](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102145782.png)

> 查看docker镜像  `docker images`

![image-20220410214543335](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102145386.png)

了解：卸载docker

```shell
#1、卸载依赖
yum remove docker-ce docker-ce-cli containerd.io

#2、删除资源
rm -rf /var/lib/docker

# /var/lib/docker     docker的默认工作路径
```



## 配置阿里云镜像加速

> 登录阿里云，找到容器镜像服务

![image-20220410220206272](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102202370.png)

> 在镜像工具中找到镜像加速器

![image-20220410220247131](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102202220.png)

> 配置使用指令指令：

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://r3uc3nlm.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```



## 回顾HelloWorld流程

> run的时候发生了什么？

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102219843.png)

![image-20220410221907965](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102219262.png)

## 底层原理

==**Docker是怎么工作的？**==

Docker是一个Client-Server（客户端-服务器）结构的系统，Docker的守护进程运行在主机上，通过Socker从客户端访问！Docker Server(服务器)接收到Docker-Client(客户端)的指令，就会执行这个指令！

![image-20220410222655927](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102226070.png)

**为什么Docker比Vm快？**

1、Docker有着比虚拟机更少的抽象层。

2、docker利用的是宿主机的内核,而不需要Guest OS。

![image-20220410222451689](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102224047.png)

> GuestOS： VM（虚拟机）里的的系统（OS）;
>
> HostOS：物理机里的系统（OS）；

因此,当新建一个 容器时,docker不需要和虚拟机一样重新加载一个操作系统内核。仍而避免引导。虚拟机是加载 Guest OS，分钟级别的，而docker 是利用宿主机的操作系统，省略了这个复杂的过程，秒级！



## Docker的常用命令

### 帮助命令

```shell
docker version          #查看docker的版本信息
docker info             #查看docker的系统信息,包括镜像和容器的数量
docker --help           #列出所有命令  
```

帮助文档的地址：https://docs.docker.com/engine/reference/commandline/

![image-20220410223730611](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102237730.png)

### 镜像命令

**docker images**   查看所有本地主机上的镜像 

```shell
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    feb5d9fea6a5   6 months ago   13.3kB

# 解释
#REPOSITORY			# 镜像的仓库源
#TAG				# 镜像的标签
#IMAGE ID			# 镜像的id
#CREATED			# 镜像的创建时间
#SIZE				# 镜像的大小
# 可选项  可以通过docker images --help查看可选项  也可以去官网看
Options:
  -a, --all             Show all images (default hides intermediate images) #列出所有镜像
  -q, --quiet           Only show numeric IDs # 只显示镜像的id

[root@iZwz97sm51bf4kooxyiz71Z ~]# docker images -q
feb5d9fea6a5

```

**docker search mysql** 查看mysql相关字眼的镜像

```
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker search mysql
```

![image-20220410231202335](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102312415.png)

==也可以去这个网址搜索==

![image-20220410224941638](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102249812.png)

**docker search mysql --filter=STARS=3000:**  搜索大于3000stars的mysql相关字眼的镜像  `--filter=STARS=是可选项`

![image-20220410231607029](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102316103.png)

**docker pull **: 下载镜像

```shell
# 下载镜像 docker pull 镜像名[:tag]
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker pull mysql
Using default tag: latest      #如果不写 tag 默认就是latest
latest: Pulling from library/mysql
72a69066d2fe: Pull complete   #分层下载，docker image的核心 联合文件系统
93619dbc5b36: Pull complete 
99da31dd6142: Pull complete 
626033c43d70: Pull complete 
37d5d7efb64e: Pull complete 
ac563158d721: Pull complete 
d2ba16033dad: Pull complete 
688ba7d5c01a: Pull complete 
00e060b6d11d: Pull complete 
1c04857f594f: Pull complete 
4d7cfa90e6ea: Pull complete 
e0431212d27d: Pull complete 
Digest: sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709  #签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest   #真实地址

docker pull mysql等价于 docker pull docker.io/library/mysql:latest

#指定版本下载   版本一定要在官网的mysql镜像中有这个版本 如下图
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker pull mysql:5.7    
5.7: Pulling from library/mysql
72a69066d2fe: Already exists 
93619dbc5b36: Already exists 
99da31dd6142: Already exists 
626033c43d70: Already exists 
37d5d7efb64e: Already exists 
ac563158d721: Already exists 
d2ba16033dad: Already exists 
0ceb82207cd7: Pull complete 
37f2405cae96: Pull complete 
e2482e017e53: Pull complete 
70deed891d42: Pull complete 
Digest: sha256:f2ad209efe9c67104167fc609cca6973c8422939491c9345270175a300419f94
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7

```

![image-20220410233324064](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102333152.png)

![image-20220410233507267](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204102335355.png)

**docker rmi** 删除镜像！

```shell
docker rmi -f  容器id  #根据id从本地删除镜像
docker rmi -f  容器id 容器id 容器id      #删除多个镜像
docker rmi -f  $(docker images -aq)  #删除全部镜像
```



```shell
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker rmi -f  c20987f18b13
Untagged: mysql:5.7
Untagged: mysql@sha256:f2ad209efe9c67104167fc609cca6973c8422939491c9345270175a300419f94
Deleted: sha256:c20987f18b130f9d144c9828df630417e2a9523148930dc3963e9d0dab302a76
Deleted: sha256:6567396b065ee734fb2dbb80c8923324a778426dfd01969f091f1ab2d52c7989
Deleted: sha256:0910f12649d514b471f1583a16f672ab67e3d29d9833a15dc2df50dd5536e40f
Deleted: sha256:6682af2fb40555c448b84711c7302d0f86fc716bbe9c7dc7dbd739ef9d757150
Deleted: sha256:5c062c3ac20f576d24454e74781511a5f96739f289edaadf2de934d06e910b92
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
mysql         latest    3218b38490ce   3 months ago   516MB
hello-world   latest    feb5d9fea6a5   6 months ago   13.3kB
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker rmi -f  $(docker images -aq)
Untagged: mysql:latest
Untagged: mysql@sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709
Deleted: sha256:3218b38490cec8d31976a40b92e09d61377359eab878db49f025e5d464367f3b
Deleted: sha256:aa81ca46575069829fe1b3c654d9e8feb43b4373932159fe2cad1ac13524a2f5
Deleted: sha256:0558823b9fbe967ea6d7174999be3cc9250b3423036370dc1a6888168cbd224d
Deleted: sha256:a46013db1d31231a0e1bac7eeda5ad4786dea0b1773927b45f92ea352a6d7ff9
Deleted: sha256:af161a47bb22852e9e3caf39f1dcd590b64bb8fae54315f9c2e7dc35b025e4e3
Deleted: sha256:feff1495e6982a7e91edc59b96ea74fd80e03674d92c7ec8a502b417268822ff
Deleted: sha256:8805862fcb6ef9deb32d4218e9e6377f35fb351a8be7abafdf1da358b2b287ba
Deleted: sha256:872d2f24c4c64a6795e86958fde075a273c35c82815f0a5025cce41edfef50c7
Deleted: sha256:6fdb3143b79e1be7181d32748dd9d4a845056dfe16ee4c827410e0edef5ad3da
Deleted: sha256:b0527c827c82a8f8f37f706fcb86c420819bb7d707a8de7b664b9ca491c96838
Deleted: sha256:75147f61f29796d6528486d8b1f9fb5d122709ea35620f8ffcea0e0ad2ab0cd0
Deleted: sha256:2938c71ddf01643685879bf182b626f0a53b1356138ef73c40496182e84548aa
Deleted: sha256:ad6b69b549193f81b039a1d478bc896f6e460c77c1849a4374ab95f9a3d2cea2
Untagged: hello-world:latest
Untagged: hello-world@sha256:bfea6278a0a267fad2634554f4f0c6f31981eea41c553fdf5a83e95a41d40c38
Deleted: sha256:feb5d9fea6a5e9606aa995e879d862b825965ba48de054caab5ef356dc6b3412
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
[root@iZwz97sm51bf4kooxyiz71Z ~]# 

```



### 容器命令

**说明：我们有了镜像才可以创建容器，linux，下载一个centos镜像来测试学习**

```shell
docker pull centos     #下载镜像
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker images     #查看镜像
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
centos       latest    5d0da3dc9764   6 months ago   231MB
```

**新建容器并启动**

```shell
docker run [可选参数] image
 
# 参数说明
--name=“Name”   容器名字    tomcat01    tomcat02    用来区分容器
-d      后台方式运行
-it     使用交互方式运行，进入容器查看内容
-p      指定容器的端口     -p 8080:8080
    -p  ip:主机端口：容器端口
    -p  主机端口：容器端口（常用）
    -p  容器端口
    容器端口
-p      随机指定端口

#测试，启动并且进入容器  进入centos镜像的容器，相当于进入了一个小的VM ware
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker run -it centos /bin/bash
[root@33e09a6762a8 /]# ls     #查看容器内的centos，基础版本，很多命令都是不完善的！
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

#从容器中退回主机
[root@332d0a5b354d /]# exit
exit
[root@iZwz97sm51bf4kooxyiz71Z /]# ls
bin  boot  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var


```

**列出所有的运行的容器**

```shell

# docker ps 命令
        # 列出当前正在运行的容器
-a      # 列出正在运行的容器包括历史容器
-n=?    # 显示最近创建的容器
-q      # 只显示当前容器的编号
 
[root@iZwz97sm51bf4kooxyiz71Z /]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@iZwz97sm51bf4kooxyiz71Z /]# docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED              STATUS                          PORTS     NAMES
332d0a5b354d   centos         "/bin/bash"   About a minute ago   Exited (0) About a minute ago             festive_villani
33e09a6762a8   centos         "/bin/bash"   4 minutes ago        Exited (0) 2 minutes ago                  nervous_shaw
4b5a7230beec   feb5d9fea6a5   "/hello"      11 hours ago         Exited (0) 11 hours ago                   cool_bartik
[root@iZwz97sm51bf4kooxyiz71Z /]# docker ps -a -n=1
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS                     PORTS     NAMES
332d0a5b354d   centos    "/bin/bash"   2 minutes ago   Exited (0) 2 minutes ago             festive_villani
[root@iZwz97sm51bf4kooxyiz71Z /]# docker ps -aq
332d0a5b354d
33e09a6762a8
4b5a7230beec

```

**退出容器**

```shell
exit            # 直接退出容器并关闭
Ctrl + P + Q    # 容器不关闭退出

[root@iZwz97sm51bf4kooxyiz71Z /]# docker run -it centos /bin/bash
[root@cf43023264ea /]# [root@iZwz97sm51bf4kooxyiz71Z /]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
cf43023264ea   centos    "/bin/bash"   21 seconds ago   Up 21 seconds             exciting_williams
[root@iZwz97sm51bf4kooxyiz71Z /]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
cf43023264ea   centos    "/bin/bash"   39 seconds ago   Up 38 seconds             exciting_williams

```

**删除容器**

```shell
docker rm -f 容器id                       # 删除指定容器
docker rm -f $(docker ps -aq)       # 删除所有容器
docker ps -a -q|xargs docker rm -f  # 删除所有的容器

[root@iZwz97sm51bf4kooxyiz71Z /]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
cf43023264ea   centos    "/bin/bash"   2 minutes ago   Up 2 minutes             exciting_williams
[root@iZwz97sm51bf4kooxyiz71Z /]# docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS                     PORTS     NAMES
cf43023264ea   centos         "/bin/bash"   2 minutes ago   Up 2 minutes                         exciting_williams
332d0a5b354d   centos         "/bin/bash"   6 minutes ago   Exited (0) 6 minutes ago             festive_villani
33e09a6762a8   centos         "/bin/bash"   9 minutes ago   Exited (0) 7 minutes ago             nervous_shaw
4b5a7230beec   feb5d9fea6a5   "/hello"      11 hours ago    Exited (0) 11 hours ago              cool_bartik
[root@iZwz97sm51bf4kooxyiz71Z /]# docker rm 332d0a5b354d
332d0a5b354d
[root@iZwz97sm51bf4kooxyiz71Z /]# docker rm cf43023264ea   无法删除正在运行的容器。但是可以用强制删除指令
Error response from daemon: You cannot remove a running container cf43023264eac185cd6e69b96f9d049dd19f4e797d2e1a057ebfdfce5d6e260b. Stop the container before attempting removal or force remove
[root@iZwz97sm51bf4kooxyiz71Z /]# docker rm -f $(docker ps -aq)
cf43023264ea
33e09a6762a8
4b5a7230beec
[root@iZwz97sm51bf4kooxyiz71Z /]# docker ps -aq
[root@iZwz97sm51bf4kooxyiz71Z /]# 

```

**启动和停止容器的操作**

```shell
docker start 容器id           # 启动容器
docker restart 容器id         # 重启容器
docker stop 容器id            # 停止当前正在运行的容器
docker kill 容器id            # 强制停止当前的容器

[root@iZwz97sm51bf4kooxyiz71Z /]# docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@iZwz97sm51bf4kooxyiz71Z /]# docker run  centos
[root@iZwz97sm51bf4kooxyiz71Z /]# docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS                     PORTS     NAMES
313eed07e1a1   centos    "/bin/bash"   3 seconds ago   Exited (0) 2 seconds ago             condescending_wiles
[root@iZwz97sm51bf4kooxyiz71Z /]# docker start 313eed07e1a1
313eed07e1a1
[root@iZwz97sm51bf4kooxyiz71Z /]# docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED              STATUS                     PORTS     NAMES
313eed07e1a1   centos    "/bin/bash"   About a minute ago   Exited (0) 5 seconds ago             condescending_wiles
[root@iZwz97sm51bf4kooxyiz71Z /]# docker stop 313eed07e1a1
313eed07e1a1
[root@iZwz97sm51bf4kooxyiz71Z /]# docker ps 
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@iZwz97sm51bf4kooxyiz71Z /]# 
```



### 其他常用的命令

**后台启动容器**

```shell
# 命令 docker run -d 镜像名
[root@iZwz97sm51bf4kooxyiz71Z /]#  docker run -d centos
130179d27a01ef79c426e5045fd4d22e1d530cc329373a98d92831c081ac834e
[root@iZwz97sm51bf4kooxyiz71Z /]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

# 问题 docker ps， 发现centos停止了
 
# 常见的坑， docker 容器使用后台运行， 就必须要有一个前台进程，docker发现没有应用，就会自动停止
# nginx， 容器启动后，发现自己没有提供服务，就会立即停止，就是没有程序了

```

**查看日志**

```shell
docker logs -tf --tail number 容器id  #显示日志

 
# 自己编写一段shell脚本
[root@iZwz97sm51bf4kooxyiz71Z /]# docker run -d centos /bin/sh -c "while true;do echo xiaofan;sleep 1;done"
f7dc520b2edea5f11bac6897ce36f79ba202f75342e287735f7a38400fcfefb3
[root@iZwz97sm51bf4kooxyiz71Z /]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
f7dc520b2ede   centos    "/bin/sh -c 'while t…"   4 seconds ago   Up 3 seconds             exciting_jones
[root@iZwz97sm51bf4kooxyiz71Z /]# docker logs -tf --tail number f7dc520b2ede  #显示日志
2022-04-11T01:02:02.257351185Z xiaofan
2022-04-11T01:02:03.259606023Z xiaofan
2022-04-11T01:02:04.261975096Z xiaofan
2022-04-11T01:02:05.264340423Z xiaofan
2022-04-11T01:02:06.266593317Z xiaofan
2022-04-11T01:02:07.268955313Z xiaofan
2022-04-11T01:02:08.271343802Z xiaofan
2022-04-11T01:02:09.273691601Z xiaofan
2022-04-11T01:02:10.276136611Z xiaofan
2022-04-11T01:02:11.278417346Z xiaofan


 
# 显示日志
-tf                 # 显示日志
--tail number       # 显示日志条数
[root@iZwz97sm51bf4kooxyiz71Z /]# # docker logs -tf --tail 10 f7dc520b2ede

```

**查看容器中的进程信息**

```shell
# 命令 docker top 容器id
[root@iZwz97sm51bf4kooxyiz71Z /]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
f7dc520b2ede   centos    "/bin/sh -c 'while t…"   6 minutes ago   Up 6 minutes             exciting_jones
[root@iZwz97sm51bf4kooxyiz71Z /]# docker top f7dc520b2ede
UID                 PID                 PPID                C                   STIME               TTY                
root                16666               16621        	    0                   09:02               ?                 
root                17205               16666               0                   09:08               ?    
```

**查看镜像的元数据**

```shell
# 命令
docker inspect 容器id
 
[root@iZwz97sm51bf4kooxyiz71Z /]#  docker inspect f7dc520b2ede
[
    {
        "Id": "f7dc520b2edea5f11bac6897ce36f79ba202f75342e287735f7a38400fcfefb3",
        "Created": "2022-04-11T01:02:01.906721658Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while true;do echo xiaofan;sleep 1;done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 16666,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2022-04-11T01:02:02.259397886Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6",
        "ResolvConfPath": "/var/lib/docker/containers/f7dc520b2edea5f11bac6897ce36f79ba202f75342e287735f7a38400fcfefb3/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/f7dc520b2edea5f11bac6897ce36f79ba202f75342e287735f7a38400fcfefb3/hostname",
        "HostsPath": "/var/lib/docker/containers/f7dc520b2edea5f11bac6897ce36f79ba202f75342e287735f7a38400fcfefb3/hosts",
        "LogPath": "/var/lib/docker/containers/f7dc520b2edea5f11bac6897ce36f79ba202f75342e287735f7a38400fcfefb3/f7dc520b2edea5f11bac6897ce36f79ba202f75342e287735f7a38400fcfefb3-json.log",
        "Name": "/exciting_jones",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/937224cbcdd2e5abebae6c0c2ca07aad9f08ac0e101da1831046dc79cc2944aa-init/diff:/var/lib/docker/overlay2/ba39b0022cd31bfe2fa4b42cfc7c848458cfec14af48273183fa221abb98c6e6/diff",
                "MergedDir": "/var/lib/docker/overlay2/937224cbcdd2e5abebae6c0c2ca07aad9f08ac0e101da1831046dc79cc2944aa/merged",
                "UpperDir": "/var/lib/docker/overlay2/937224cbcdd2e5abebae6c0c2ca07aad9f08ac0e101da1831046dc79cc2944aa/diff",
                "WorkDir": "/var/lib/docker/overlay2/937224cbcdd2e5abebae6c0c2ca07aad9f08ac0e101da1831046dc79cc2944aa/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "f7dc520b2ede",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "while true;do echo xiaofan;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20210915",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "896433fa472ce8d65312a93d8c39d1223395bc985b677a7fe6e41b003e886841",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/896433fa472c",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "769dd56a7442bed16258894bfa847b67efe7f64bfeab1d8c71064106cca9628e",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "684bee37059ac89e13556c0b5d1150985f24cf7f69916dbefa78260f7c0bb988",
                    "EndpointID": "769dd56a7442bed16258894bfa847b67efe7f64bfeab1d8c71064106cca9628e",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
[root@iZwz97sm51bf4kooxyiz71Z /]# 

```

**进入当前正在运行的容器**

```shell
# 我们通常容器使用后台方式运行的， 需要进入容器，修改一些配置
 
# 命令
docker exec -it 容器id /bin/bash
 
# 测试
[root@iZwz97sm51bf4kooxyiz71Z /]# docker run -it centos /bin/bash
[root@d618867c15fa /]# [root@iZwz97sm51bf4kooxyiz71Z /]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
d618867c15fa   centos    "/bin/bash"   7 seconds ago   Up 6 seconds             fervent_babbage
[root@iZwz97sm51bf4kooxyiz71Z /]# docker exec -it d618867c15fa /bin/bash
[root@d618867c15fa /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var


# 方式二
docker attach 容器id

#测试
[root@iZwz97sm51bf4kooxyiz71Z /]# docker attach d618867c15fa
[root@d618867c15fa /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

 
# docker exec       # 进入容器后开启一个新的终端，可以在里面操作
# docker attach     # 进入容器正在执行的终端，不会启动新的进程,如若这个容器写了个死循环输出，则会看到死循环一直在输出，退不出去
```

**从容器中拷贝文件到主机**

```shell
docker cp 容器id：容器内路径    目的地主机路径
 
[root@iZwz97sm51bf4kooxyiz71Z home]#  docker run -it centos /bin/bash
[root@2966a3375c75 /]# cd /home      
[root@2966a3375c75 home]# touch test.java    #在容器内新建一个文件
[root@2966a3375c75 home]# ls
test.java
[root@2966a3375c75 home]# exit
exit
[root@iZwz97sm51bf4kooxyiz71Z home]# docker ps 
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@iZwz97sm51bf4kooxyiz71Z home]# docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS                     PORTS     NAMES
2966a3375c75   centos    "/bin/bash"   35 seconds ago   Exited (0) 7 seconds ago             hardcore_jennings

#将这文件拷贝出来到主机上
[root@iZwz97sm51bf4kooxyiz71Z home]# docker cp 2966a3375c75:/home/test.java /home
[root@iZwz97sm51bf4kooxyiz71Z home]# ls
test.java

#拷贝是一个手动的过程，未来我们使用 -v 卷的技术，可以实现
```

### 小结：

![image-20220411095225022](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204110952210.png)

```shell
  attach      Attach local standard input, output, and error streams to a running container
  #当前shell下 attach连接指定运行的镜像
  build       Build an image from a Dockerfile # 通过Dockerfile定制镜像
  commit      Create a new image from a container's changes #提交当前容器为新的镜像
  cp          Copy files/folders between a container and the local filesystem #拷贝文件
  create      Create a new container #创建一个新的容器
  diff        Inspect changes to files or directories on a container's filesystem #查看docker容器的变化
  events      Get real time events from the server # 从服务获取容器实时时间
  exec        Run a command in a running container # 在运行中的容器上运行命令
  export      Export a container's filesystem as a tar archive #导出容器文件系统作为一个tar归档文件[对应import]
  history     Show the history of an image # 展示一个镜像形成历史
  images      List images #列出系统当前的镜像
  import      Import the contents from a tarball to create a filesystem image #从tar包中导入内容创建一个文件系统镜像
  info        Display system-wide information # 显示全系统信息
  inspect     Return low-level information on Docker objects #查看容器详细信息
  kill        Kill one or more running containers # kill指定docker容器
  load        Load an image from a tar archive or STDIN #从一个tar包或标准输入中加载一个镜像[对应save]
  login       Log in to a Docker registry #
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes
```

==**docker的命令十分多的，上面我们学习的那些都是最常用的容器和镜像的命令，之后我们还会学习很多命令**==



## 作业练习

### Docker部署Nginx

```shell
# 1、搜索镜像  建议大家去docker搜索，可以看到帮助文档以及都有哪些版本
# 2、下载镜像  docker pull nginx
# 3、查看镜像
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    605c77e624dd   3 months ago   141MB
centos       latest    5d0da3dc9764   6 months ago   231MB


# -d 后台运行
# --name 给容器命名
# -p 宿主机端口：容器内部端口

[root@iZwz97sm51bf4kooxyiz71Z ~]# docker run -d --name nginx01 -p 3344:80 nginx
290ceb434143e1c55b6ef1061f4b3ab6f8433ee468b3033ee8ec69b90def3796
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                  NAMES
290ceb434143   nginx     "/docker-entrypoint.…"   8 seconds ago   Up 7 seconds   0.0.0.0:3344->80/tcp   nginx01

[root@iZwz97sm51bf4kooxyiz71Z ~]# curl localhost:3344
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

#进入容器
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker exec -it nginx01 /bin/bash
root@290ceb434143:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
root@290ceb434143:/# cd /etc/nginx
root@290ceb434143:/etc/nginx# ls
conf.d	fastcgi_params	mime.types  modules  nginx.conf  scgi_params  uwsgi_params
root@290ceb434143:/etc/nginx# 

```

与此同时，我们可以访问下是否docker成功部署了nginx

![image-20220411113327744](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111133820.png)

> 解释说明：

![image-20220411113151115](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111131197.png)

```shell
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker rm -f 290ceb434143     删掉服务或者停止服务就访问不到了
290ceb434143

```

访问不到了

![image-20220411113910555](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111139644.png)

### Docker部署Tomcat

官网[Tomcat - Official Image | Docker Hub](https://hub.docker.com/_/tomcat)查看对应的版本

![image-20220411114850578](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111148697.png)

```shell
# 官方的使用
docker run -it --rm tomcat:9.0
 
# 我们之前的启动都是后台的，停止了容器之后， 容器还是可以查到，docker run -it --rm 一般用来测试，用完就删
 
# 下载再启动
docker pull tomcat:9.0
 
# 启动运行
docker run -d -p 3344:8080 --name tomcat01 tomcat:9.0
 
# 测试访问没有问题

```

![image-20220411120616242](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111206305.png)

```shell
# 进入容器
docker exec -it tomcat01 /bin/bash
 
root@b18657d586e7:/usr/local/tomcat# ls
BUILDING.txt  CONTRIBUTING.md  LICENSE	NOTICE	README.md  RELEASE-NOTES  RUNNING.txt  bin  conf  lib  logs  native-jni-lib  temp  webapps  webapps.dist  work
root@b18657d586e7:/usr/local/tomcat# ll
bash: ll: command not found
root@b18657d586e7:/usr/local/tomcat# ls -al
total 172
drwxr-xr-x 1 root root  4096 Dec 22 17:16 .
drwxr-xr-x 1 root root  4096 Dec 22 17:00 ..
-rw-r--r-- 1 root root 18970 Dec  2 14:30 BUILDING.txt
-rw-r--r-- 1 root root  6210 Dec  2 14:30 CONTRIBUTING.md
-rw-r--r-- 1 root root 57092 Dec  2 14:30 LICENSE
-rw-r--r-- 1 root root  2333 Dec  2 14:30 NOTICE
-rw-r--r-- 1 root root  3378 Dec  2 14:30 README.md
-rw-r--r-- 1 root root  6898 Dec  2 14:30 RELEASE-NOTES
-rw-r--r-- 1 root root 16507 Dec  2 14:30 RUNNING.txt
drwxr-xr-x 2 root root  4096 Dec 22 17:16 bin
drwxr-xr-x 1 root root  4096 Apr 11 04:03 conf
drwxr-xr-x 2 root root  4096 Dec 22 17:16 lib
drwxrwxrwx 1 root root  4096 Apr 11 04:03 logs
drwxr-xr-x 2 root root  4096 Dec 22 17:16 native-jni-lib
drwxrwxrwx 2 root root  4096 Dec 22 17:16 temp
drwxr-xr-x 2 root root  4096 Dec 22 17:16 webapps
drwxr-xr-x 7 root root  4096 Dec  2 14:30 webapps.dist
drwxrwxrwx 2 root root  4096 Dec  2 14:30 work
root@b18657d586e7:/usr/local/tomcat# cd webapps
root@b18657d586e7:/usr/local/tomcat/webapps# ls -al   发现目录是空的
total 12
drwxr-xr-x 2 root root 4096 Dec 22 17:16 .
drwxr-xr-x 1 root root 4096 Dec 22 17:16 ..
root@b18657d586e7:/usr/local/tomcat/webapps# cd ..
root@b18657d586e7:/usr/local/tomcat# ls
BUILDING.txt  CONTRIBUTING.md  LICENSE	NOTICE	README.md  RELEASE-NOTES  RUNNING.txt  bin  conf  lib  logs  native-jni-lib  temp  webapps  webapps.dist  work
root@b18657d586e7:/usr/local/tomcat# cd webapps.dist/
root@b18657d586e7:/usr/local/tomcat/webapps.dist# ls
ROOT  docs  examples  host-manager  manager
root@b18657d586e7:/usr/local/tomcat/webapps.dist# cd ..
root@b18657d586e7:/usr/local/tomcat# cp -r webapps.dist/* webapps
root@b18657d586e7:/usr/local/tomcat# cd webapps
root@b18657d586e7:/usr/local/tomcat/webapps# ls
ROOT  docs  examples  host-manager  manager
root@b18657d586e7:/usr/local/tomcat/webapps# 

 
# 发现问题：
1.linux命令少了，  ll命令用不了
2. webapps下内容为空，阿里云净吸纳过默认是最小的镜像，所有不必要的都剔除了，保证最小可运行环境即可

```

再次访问：

![image-20220411121104710](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111211813.png)

思考问题：我们以后要部署项目，如果每次都要进入容器是不是十分麻烦？我要是可以在容器外部提供一个映射路径，webapps，我们在外部放置项目，就自动同步到内部就好了

### Docker部署ES+Kibana

```shell
# es 暴露的端口很多
# es 十分的耗内存
# es 的数据一般需要放置到安全目录！ 挂载
# --net somenetwork 网络配置
 
# 启动elasticsearch
docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2   这里我们先把--net somenetwork去掉
 
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2

 
# 启动了linux就卡主了，es光是启动就需要1.xg的内存，服务器是1核2g的话，根本带不动。
 
# 测试一下es成功了
[root@iZwz97sm51bf4kooxyiz71Z ~]# curl localhost:9200
{
  "name" : "8d347f48c537",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "ExBCkmuVQoGraZshZLUBcw",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}

 
```

`docker stats` 查看cpu状态   直接占了1.23g 如果是1核两g的 就容易被卡死

![image-20220411150445272](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111504346.png)

```shell
# 增加内存限制，修改配置文件 -e 环境配置修改
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2


[root@iZwz97sm51bf4kooxyiz71Z ~]# docker rm -f  8d347f48c537
8d347f48c537
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2
0722931d4166abb61f196c585e508e0b47a3126aa088e7d1b3fd76b04efb153b
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker stats

CONTAINER ID   NAME            CPU %     MEM USAGE / LIMIT     MEM %     NET I/O   BLOCK I/O    PIDS
0722931d4166   elasticsearch   0.06%     348.8MiB / 3.701GiB   9.20%     0B / 0B   0B / 696kB   43

CONTAINER ID   NAME            CPU %     MEM USAGE / LIMIT     MEM %     NET I/O   BLOCK I/O    PIDS
0722931d4166   elasticsearch   0.06%     348.8MiB / 3.701GiB   9.20%     0B / 0B   0B / 696kB   43

```

==这样es就部署好了，并且启动es占用的内存就小了==，

问题：我们还没安装Kibana，并且kibana如何连接es呢？大致原理是下面这样的

![image-20220411151121048](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111511127.png)



## 可视化

- portainer（先用这个）

**什么是portainner？**

Docker图形化界面管理工具！提供一个后台面板供我们操作！

```
docker run -d -p 8088:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
 
# 访问测试
[root@iZwz97sm51bf4kooxyiz71Z ~]# curl localhost:8088

 
# 外网访问 http://ip:8088  需要在阿里云的安全组先开放8088端口
 
```

![image-20220411152608701](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111526795.png)

设置密码

![image-20220411153110344](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111531435.png)

![image-20220411153213015](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111532126.png)

==点开images看看，可以看到我们当前所有的镜像==

![image-20220411153256279](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111532370.png)

==可视化面板我们平时不会使用，大家自己测试玩玩即可==



## Docker镜像讲解

### 镜像是什么

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，他包含运行某个软件所需的所有内容，包括代码、运行时库、环境变量和配置文件

所有的应用，直接打包docker镜像，就可以直接跑起来！

**如何得到镜像：**

- 从远程仓库下载
- 朋友拷贝给你
- 自己制作一个镜像 DockerFile

### Docker镜像加载原理

> UnionFs （联合文件系统）

UnionFs（联合文件系统）：Union文件系统（UnionFs）是一种分层、轻量级并且高性能的文件系统，他支持对文件系统的修改作为一次提交来一层层的叠加（==所以我们拉取镜像的时候也是层层下载下来的==），同时可以将不同目录挂载到同一个虚拟文件系统下（ unite several directories into a single virtual filesystem)。==Union文件系统是 Docker镜像的基础==。==镜像可以通过分层来进行继承，基于基础镜像（比如我们拉群两个tomcat镜像，版本不一样，拉取第一个的时候，会分层加载所以，拉取第二个，基于第一个的情况下，再去拉取的分层），可以制作各种具体的应用镜像==
特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录

> Docker镜像加载原理
>

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。

boots(boot file system）主要包含 bootloader和 Kernel, bootloader主要是引导加 kernel, Linux刚启动时会加bootfs文件系统，在 Docker镜像的最底层是 boots。这一层与我们典型的Linux/Unix系统是一样的，包含boot加載器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由 bootfs转交给内核，此时系统也会卸载bootfs。

rootfs（root file system),在 bootfs之上。包含的就是典型 Linux系统中的/dev,/proc,/bin,/etc等标准目录和文件。 rootfs就是各种不同的操作系统发行版，比如 Ubuntu, Centos等等。

![image-20220411154202049](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111542128.png)



平时我们安装进虚拟机的CentOS都是好几个G，为什么Docker这里才200M？

对于一个精简的OS,rootfs可以很小，只需要包合最基本的命令，工具和程序库就可以了，因为底层直接用Host的kernel，自己只需要提供rootfs就可以了。由此可见对于不同的Linux发行版， boots基本是一致的， rootfs会有差別，因此不同的发行版可以公用bootfs.

虚拟机是分钟级别，容器是秒级！



### 分层理解

> 分层的镜像

我们可以去下载一个镜像，注意观察下载的日志输出，可以看到是一层层的在下载

![image-20220411163227305](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111632389.png)

**理解：**

所有的 Docker镜像都起始于一个==基础镜像层==，当进行修改或培加新的内容时，就会在当前镜像层之上，创建新的镜像层。

举一个简单的例子，假如基于 Ubuntu Linux16.04创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加 Python包，
就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就会创健第三个镜像层该像当前已经包含3个镜像层，如下图所示（这只是一个用于演示的很简单的例子）。

![image-20220411163255388](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111632495.png)

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点非常重要。下图中举了一个简单的例子，每个镜像层包含3个文件，而镜像包含了来自两个镜像层的6个文件。

![image-20220411161500039](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111615105.png)

上图中的镜像层跟之前图中的略有区別，主要目的是便于展示文件
下图中展示了一个稍微复杂的三层镜像，在外部看来整个镜像只有6个文件，这是因为最上层中的文件7是文件5的一个更新版

![image-20220411161709714](../AppData/Roaming/Typora/typora-user-images/image-20220411161709714.png)

这种情況下，上层镜像层中的文件覆盖了底层镜像层中的文件。这样就使得文件的更新版本作为一个新镜像层添加到镜像当中

Docker通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多镜像层对外展示为统一的文件系统

Linux上可用的存储引撃有AUFS、 Overlay2、 Device Mapper、Btrfs以及ZFS。顾名思义，每种存储引擎都基于 Linux中对应的
件系统或者块设备技术，井且每种存储引擎都有其独有的性能特点。

Docker在 Windows上仅支持 windowsfilter 一种存储引擎，该引擎基于NTFS文件系统之上实现了分层和CoW [1]。

下图展示了与系统显示相同的三层镜像。所有镜像层堆并合井，对外提供统一的视图

![image-20220411161729272](../AppData/Roaming/Typora/typora-user-images/image-20220411161729272.png)



> 特点

Docker 镜像都是只读的，当容器启动时，一个新的可写层加载到镜像的顶部！

这一层就是我们通常说的容器层，容器之下的都叫镜像层（==不能改==）！

![image-20220411162021464](../AppData/Roaming/Typora/typora-user-images/image-20220411162021464.png)

那如何提交一个自己的镜像？

### commit镜像

```shell
docker commit 提交容器成为一个新的副本
 
# 命令和git原理类似
docker commit -m="描述信息" -a="作者" 容器id 目标镜像名:[TAG]
```

> 实战测试

```shell
# 1、启动一个默认的tomcat
docker run -d -p 8080:8080 tomcat   需要打开阿里云安全组的8080端口
# 2、发现这个默认的tomcat 是没有webapps应用，官方的镜像默认webapps下面是没有文件的！
docker exec -it 容器id /bin/bash
# 3、拷贝文件进去
cp -r webapps.dist/* webapps  进入tomcat目录执行  相当于加了一层镜像

# 4、将操作过的容器通过commit调教为一个镜像！我们以后就使用我们修改过的镜像即可，这就是我们自己的一个修改的镜像。
docker commit -m="描述信息" -a="作者" 容器id 目标镜像名:[TAG]
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker commit -a="kwq" -m="add webapps app" 8d2c26163d7a tomcat02:1.0
sha256:d15e4fd4080d270b0adae0e264a20917ef08ecf81079257058e48e6b8b871c72
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker images
REPOSITORY            TAG       IMAGE ID       CREATED         SIZE
tomcat02              1.0       d15e4fd4080d   8 seconds ago   684MB
nginx                 latest    605c77e624dd   3 months ago    141MB
tomcat                9.0       b8e65a4d736d   3 months ago    680MB
tomcat                latest    fb5657adc892   3 months ago    680MB
redis                 latest    7614ae9453d1   3 months ago    113MB
centos                latest    5d0da3dc9764   6 months ago    231MB
portainer/portainer   latest    580c0e4e98b0   12 months ago   79.1MB
elasticsearch         7.6.2     f29a1ee41030   2 years ago     791MB

#这样子我们的镜像就存在我们本地了 上面可以看到
```

![image-20220411180637169](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111806241.png)

学习方式说明：理解概念，但是一定要实践，最后实践和理论相结合，一次性搞定这个知识点

```shell
如果你想要报错当前容器的状态，就可以通过commit来提交，获得一个镜像，
就好比我们以前学习VM时候，快照
```

==**到了这里才能算是Docker入门！**==



## 容器数据卷

### 什么是容器数据卷

将应用和环境打包成一个镜像！ 运行起来就是一个容器

数据？如果数据都在容器中，那么我们容器删除，数据就会丢失！==需求：数据可以持久化==

MySQL，容器删了，删库跑路！需求：MySQL数据可以存储在本地！

容器之间可以有一个==数据共享技术==！==Docker容器中产生的数据，同步到本地！==

==这就是卷技术，本质就是目录的挂载，将我们容器内的目录挂载到linux目录上面！==

`图示理解:`

![image-20220411181820977](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111818069.png)

**总结： 容器的持久化和同步操作！容器间数据也是可以共享的！**



### 使用数据卷

> 方式一: 直接使用命令 -v 进行数据地址挂载

```
docker run -it -v 宿主机目录:容器目录 
```

测试一:

```bash
# 测试
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker run -it -v /home/ceshi:/home centos /bin/bash
[root@0a0fee2b12f9 /]# cd /home
[root@0a0fee2b12f9 home]# ls
[root@0a0fee2b12f9 home]# touch test.java
[root@0a0fee2b12f9 home]# ls
test.java

# 查看卷是否挂载成功

[root@iZwz97sm51bf4kooxyiz71Z home]# docker inspect 0a0fee2b12f9
下图说明

```

![image-20220411182829768](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111828830.png)

> 测试文件的同步： 双向绑定

![image-20220411184125733](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111841811.png)

==**好处：我们以后修改只需要在本地修改即可，容器会自动同步**==



### 实战：安装MySQL

思考：MySQL的数据持久化问题！

```shell
#获取镜像
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker pull mysql:5.7   
 
# 运行容器， 需要做数据挂载！ # 安装启动mysql，需要配置密码（注意）
# 官方测试， docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
 
# 启动我们的
-d      # 后台运行
-p      # 端口映射
-v      # 数据卷挂载
-e      # 环境配置
--name  # 容器的名字
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker run -d -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123123 --name mysql01 mysql:5.7
0afd9baaecec3126b755cd91c7eb967d44631a8e795898222ed1e1f7b2869a81

 
# 启动成功之后，我们在本地使用sqlyog链接测试一下
# sqlyog链接到服务器的3310   3310 和 容器的3306映射，这个时候我们就可以连接上mysql喽！
 
# 在本地测试创建一个数据库，查看下我们的路径是否ok！
```

![image-20220411192029550](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111920635.png)

```shell
[root@iZwz97sm51bf4kooxyiz71Z ~]# cd /home/mysql
[root@iZwz97sm51bf4kooxyiz71Z mysql]# ls
conf  data
[root@iZwz97sm51bf4kooxyiz71Z mysql]# cd data
[root@iZwz97sm51bf4kooxyiz71Z data]# ls
auto.cnf    ca.pem           client-key.pem  ibdata1      ib_logfile1  mysql               private_key.pem  server-cert.pem  sys
ca-key.pem  client-cert.pem  ib_buffer_pool  ib_logfile0  ibtmp1       performance_schema  public_key.pem   server-key.pem
```



`创建数据库`

![image-20220411193033584](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204111930650.png)

```shell
[root@iZwz97sm51bf4kooxyiz71Z data]# ls
auto.cnf    ca.pem           client-key.pem  ibdata1      ib_logfile1  mysql               private_key.pem  server-cert.pem  sys
ca-key.pem  client-cert.pem  ib_buffer_pool  ib_logfile0  ibtmp1       performance_schema  public_key.pem   server-key.pem   test
#这样test就映射过来了 数据就不会丢失了

#删掉mysql01容器
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker rm -f mysql01
mysql01
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@iZwz97sm51bf4kooxyiz71Z ~]# cd /home/mysql/data
[root@iZwz97sm51bf4kooxyiz71Z data]# ls
auto.cnf    ca.pem           client-key.pem  ibdata1      ib_logfile1  mysql               private_key.pem  server-cert.pem  sys
ca-key.pem  client-cert.pem  ib_buffer_pool  ib_logfile0  ibtmp1       performance_schema  public_key.pem   server-key.pem   test
发现，我们挂载到本地的数据卷依旧没有丢失，这就实现了容器数据持久化
```





### 具名挂载和匿名挂载

```bash
# 匿名挂载
-P 大写的P, 映射随机端口
-v 容器内路径
示例：docker run -d -P --name nginx02 -v /etc/nginx nginx

# 具名挂载
# 通过 -v 卷名:容器内路径
示例：docker run -d -P --name nginx03 -v juming-nginx:/etc/nginx nginx

# 查看数据卷的相关帮助命令
docker volume --help


# 查看所有数据卷信息
docker volume ls
# VOLUME NAME 当前显示为匿名数据卷, 在 -v 挂载的时候只写了容器内的路径,没有写宿主机的路径
[root@iZwz97sm51bf4kooxyiz71Z data]# docker run -d -P --name nginx02 -v /etc/nginx nginx
[root@iZwz97sm51bf4kooxyiz71Z data]# docker volume ls
DRIVER    VOLUME NAME
local     cd3eae6ebcf246de953052bbaa426be50f4a2bb78a03b1600334f3d6238c36c7

#具名挂载
[root@iZwz97sm51bf4kooxyiz71Z data]# docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx
fc6c648fc3ab910176d9758d87e9686951986fc0fa3d823ea992c97b1a2c3843
[root@iZwz97sm51bf4kooxyiz71Z data]# docker volume ls
DRIVER    VOLUME NAME
local     cd3eae6ebcf246de953052bbaa426be50f4a2bb78a03b1600334f3d6238c36c7
local     juming-nginx
[root@iZwz97sm51bf4kooxyiz71Z data]# docker volume inspect  juming-nginx
[
    {
        "CreatedAt": "2022-04-11T19:58:43+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/juming-nginx/_data",  #映射路径
        "Name": "juming-nginx",
        "Options": null,
        "Scope": "local"
    }
]

[root@iZwz97sm51bf4kooxyiz71Z data]# cd /var/lib/docker
[root@iZwz97sm51bf4kooxyiz71Z docker]# ls
buildkit  containers  image  network  overlay2  plugins  runtimes  swarm  tmp  trust  volumes
[root@iZwz97sm51bf4kooxyiz71Z docker]# cd volumes/
[root@iZwz97sm51bf4kooxyiz71Z volumes]# ll
total 36
drwx-----x 3 root root   4096 Apr 11 15:21 5efb12b354d7f8d92e242e8804353e52e7f26e0fe309bfd9c5dec50579f1a5b5
brw------- 1 root root 253, 1 Apr 11 15:02 backingFsBlockDev
drwx-----x 3 root root   4096 Apr 11 19:56 cd3eae6ebcf246de953052bbaa426be50f4a2bb78a03b1600334f3d6238c36c7
drwx-----x 3 root root   4096 Apr 11 19:58 juming-nginx
-rw------- 1 root root  32768 Apr 11 19:58 metadata.db
[root@iZwz97sm51bf4kooxyiz71Z volumes]# cd juming-nginx/
[root@iZwz97sm51bf4kooxyiz71Z juming-nginx]# ll
total 4
drwxr-xr-x 3 root root 4096 Apr 11 19:58 _data
[root@iZwz97sm51bf4kooxyiz71Z juming-nginx]# cd _data
[root@iZwz97sm51bf4kooxyiz71Z _data]# ls
conf.d  fastcgi_params  mime.types  modules  nginx.conf  scgi_params  uwsgi_params


```



![image-20220411201322021](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112013109.png)

所有Docker容器内的数据卷, 在没有指定目录的情况下都是在 `/var/lib/docker/volumes/xxxx/_data` 路径下.

我们通过具名挂载可以方便地找到我们需要查询的数据卷. 大多数情况都是在使用 `具名挂载`

```bash
# 如何确定是具名挂载还是匿名挂载, 亦或是指定路径挂载?
-v 容器内路径                                        # 匿名挂载
-v 数据卷名:/容器内路径                   # 具名挂载
-v /宿主机路径:/容器内路径             # 指定路径挂载
```

**拓展:**

```bash
# 通过 -v 容器内路径: ro rw 改变读写权限
# ro readonly           只读
# rw readwrite        读写
# 如果设置了容器权限, 容器对挂载出来的数据就有权限限定了. 
docker run -d -P --name nginx04 -v juming-nginx:/etc/nginx:ro nginx
docker run -d -P --name nginx04 -v juming-nginx:/etc/nginx:rw nginx
# ro    只要看到ro就说明这个路径只能通过宿主机来操作, 容器内部是无法操作
```



### 初始Dockerfile

DockerFile就是用来构建docker镜像的构建文件！命令脚本！先体验一下！

通过这个脚本可以生成镜像，镜像是一层一层的，脚本一个个的命令，每个命令都是一层！

```shell
# 创建一个dockerfile文件， 名字可以随机
# 文件的内容 指定（大写） 参数
FROM centos

VOLUME ["volume01","volume02"]

CMD echo "---- end ---"

CMD /bin/bash

# 这里的每一个命令都是镜像的一层！

```

![image-20220411211121275](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112111391.png)

```shell
# 启动自己写的容器
```

![image-20220411211512421](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112115503.png)

这个卷和外部一定有一个同步的目录！

![image-20220411211804134](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112118193.png)

查看一下卷挂载的路径

```shell
#新建一个文件
[root@d2d27820fce5 /]# cd volume01
[root@d2d27820fce5 volume01]# touch container.txt
[root@d2d27820fce5 volume01]# ls
container.txt

#查看一下卷挂载的路径
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker ps -a
CONTAINER ID   IMAGE            COMMAND                  CREATED             STATUS                      PORTS                   NAMES
d2d27820fce5   kwq/centos:1.0   "/bin/bash"              6 minutes ago       Exited (0) 36 seconds ago                           quizzical_brown
fc6c648fc3ab   nginx            "/docker-entrypoint.…"   About an hour ago   Up About an hour            0.0.0.0:49154->80/tcp   nginx02
9d87304c97d9   nginx            "/docker-entrypoint.…"   About an hour ago   Up About an hour            0.0.0.0:49153->80/tcp   nginx01
0a0fee2b12f9   centos           "/bin/bash"              3 hours ago         Exited (0) 3 hours ago                              hungry_bohr
ce04d174eb34   centos           "/bin/bash"              3 hours ago         Exited (127) 3 hours ago                            focused_mclaren
4e0a3c463b3d   tomcat:9.0       "catalina.sh run"        6 hours ago         Created                                             tomcat01
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker inspect d2d27820fce5



```

![image-20220411212149157](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112121236.png)

测试一下刚才的文件是否同步出去了！

![image-20220411212338816](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112123875.png)

这种方式在我们未来使用的十分多，因为我们通常会构建自己的镜像！

假设构建镜像的时候没有挂载卷，==启动的时候要手动镜像挂载 -v 卷名:容器内路径！==



### 数据卷容器

多个mysql同步数据！

![image-20220411222423032](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112224172.png)

==启动3个容器，通过我们刚才自己写的镜像启动==

```shell
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker images
REPOSITORY            TAG       IMAGE ID       CREATED             SIZE
kwq/centos            1.0       bb027defb306   About an hour ago   231MB
tomcat02              1.0       d15e4fd4080d   4 hours ago         684MB
nginx                 latest    605c77e624dd   3 months ago        141MB
tomcat                9.0       b8e65a4d736d   3 months ago        680MB
tomcat                latest    fb5657adc892   3 months ago        680MB
redis                 latest    7614ae9453d1   3 months ago        113MB
mysql                 5.7       c20987f18b13   3 months ago        448MB
centos                latest    5d0da3dc9764   6 months ago        231MB
portainer/portainer   latest    580c0e4e98b0   12 months ago       79.1MB
elasticsearch         7.6.2     f29a1ee41030   2 years ago         791MB

```

```shell
#启动第一个容器
```

![image-20220411222741879](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112227020.png)

```shell
#启动第二个容器
```

![image-20220411222901357](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112229503.png)

```shell
#第一个容器添加一个文件
```

![image-20220411223002196](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112230331.png)

```shell
#查看是否同步
```

![image-20220411223138166](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112231266.png)

```shell
#说明
```

![image-20220411223335875](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112233985.png)

```shell
#启动第三个容器
```

![image-20220411224209796](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112242964.png)

![image-20220411224811285](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112248382.png)

```shell
#测试，可以删除docker01，查看一下docker02和docker03是否还可以访问这个文件
#测试依旧可以访问 
```

![image-20220411225514162](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112255271.png)

![image-20220411225443661](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112254776.png)

> 原理

![image-20220411225714517](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112257728.png)===

==三个容器的数据卷的外部映射文件是同一个==

> 刚刚删除了父容器，现在继承其中子容器 也可以实现共享

![image-20220411232700110](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204112327249.png)



==多个mysql实现数据共享，如何做？==   有bug 待处理

```shell
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker run -d -p 3310:3306 -v /etc/mysql/conf.d -v /var/lib/mysql -e MYSQL_ROOT_PASSWORD=123123 --name mysql01 mysql:5.7
b4dc54efcb39c05d71a548be8b52122860716ec0d69b28b23b3e5df68a255c1e
[root@iZwz97sm51bf4kooxyiz71Z ~]# docker run -d -p 3311:3307   -e MYSQL_ROOT_PASSWORD=123123 --name mysql02 --volumes-from mysql01 mysql:5.7
bff45e6b8d35369f78891429438fccd76847ea3cfd06f5de67ee424747f4dfc8


#这个时候，可以实现两个容器数据同步
```

==结论：==

容器之间配置信息的传递，数据卷容器的生命周期一直持续到没有容器使用为止。否则一直可以继承，实现数据的共享

但是你一旦持久化到本地，这个时候，本地的数据是不会被删除的！



## DockerFile

### DockerFile介绍

dockerFile是用来构建docker镜像的文件！命令参数脚本！

构建步骤

- 编写一个dockerFile文件
- docker build 构建成为一个镜像
- docker run 运行镜像
- docker push 发布镜像（发布到DockerHub、阿里云镜像）

> 查看一下官方是怎么做的？

![image-20220412091431356](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204120914577.png)

![image-20220412091501498](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204120915726.png)

很多官方镜像都是基础包，很多功能都不具备，我们通常会自己搭建自己的镜像！（比如centos+jdk+mysql等）

官方既然可以制作镜像，那我们一样可以！



### DockerFile的构建过程

**基础知识：**

1. 每个保留关键字（指令）都是必须大写字母
2. 执行从上到下顺序执行
3. `#`表示注释
4. 每个指令都会创建提交一个新的镜像层，并提交！

![image-20220412091702807](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204120917942.png)

dockerFile是面向开发的， 我们以后要发布项目， 做镜像， 就需要编写dockefile文件， 这个文件十分简单！

Docker镜像逐渐成为企业的交互标准，必须要掌握！

步骤：开发，部署， 运维..... 缺一不可！

DockerFile： 构建文件， 定义了一切的步骤，源代码

DockerImages： 通过DockerFile构建生成的镜像， 最终发布和运行的产品！

Docker容器：容器就是镜像运行起来提供服务器



### DockerFile的指令

```shell
FROM                        # 基础镜像
MAINTAINER          # 镜像作者   姓名+邮箱, 例如:   Oskari < oskari@bengder.cc >
RUN                           # 镜像构建时需要运行的命令
ADD                           # 添加内容, 例如 tomcat 压缩包
WORKDIR               # 镜像的工作目录
VOLUME                  # 挂载数据卷(目录)
EXPOSE                   # 指定暴露端口
CMD                          # 指定容器启动的时候要运行的命令, 只有最后一个会生效, 可以被替代
ENTRYPOINT        # 指定容器启动的时候要运行的命令, 可以追加命令
ONBUILD                # 当构建一个被继承 Dockerfile , 这个时候就会运行 ONBUILD 指令, 触发指令
COPY                         # 类似 ADD 命令, 将文件拷贝到镜像中
ENV                            # 构建的时候设置环境变量   相当于-e 我们前面部署es的时候用过 改启动大小 mysql中设置密码也用过
```

![image-20220412102121688](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204121021135.png)



### 实战测试

Docker Hub 中大多数镜像都是 `FROM scratch` 开始的. 然后配置需要的软件和配置来进行构建.

![image-20220412132702811](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204121327961.png)



远程仓库的centos的vim和ifconfig命令不可用

![image-20220412143711640](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204121437739.png)

> 创建一个自己的centos 

![image-20220412134855319](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204121348383.png)

```shell
FROM centos:centos7    #一定要指定版本 ：后面的东西不可少
MAINTAINER xiaofan<594042358@qq.com>
 
ENV MYPATH /usr/local
WORKDIR $MYPATH     # 镜像的工作目录
 
RUN yum -y install vim    #安装vim
RUN yum -y install net-tools  #安装ifconfig
 
EXPOSE 80   #暴露端口
 
CMD echo $MYPATH   #输出
CMD echo "---end---"   #输出
CMD /bin/bash

```

```shell

通过这个文件构建镜像
# 命令 docker build -f dockerfile文件路径 -t 镜像名:[tag] .
[root@iZwz97sm51bf4kooxyiz71Z dockerfile]# docker build -f mydockerfile-centos1 -t mycentos:0.1

```

> 创建我们镜像的容器   默认的工作目录就是/usr/local了 指令也可以用了 这些都是我们在dockerfile文件中配置的

![image-20220412144017909](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204121440999.png)

`通过docker histroy 镜像id `我们可查看镜像是如何创建的

![image-20220412144548699](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204121445786.png)

![image-20220412144805123](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204121448182.png)



> CMD 和ENTRYPOINT区别

```shell
CMD         # 指定这个容器启动的时候要运行的命令，只有最后一个会生效可被替代
ENTRYPOINT      # 指定这个容器启动的时候要运行的命令， 可以追加命令
```

==测试CMD==

![image-20220412152005762](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204121520843.png)

```shell
#dockerfile-cmd-test 文件内容
FROM centos
CMD ["ls", "-al"]  #这个语句无效 只有最后一个cmd指令会生效  这点和ENTRYPOINT一样
CMD ["ls", "-a"]

```

`无法进行追加：`

![image-20220412152211455](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204121522535.png)

`解释`

```shell
# cmd的情况下 -l替换了CMD["ls", "-a"]命令， -l不是命令，所以报错了
```

> 测试ENTRYPOINT

![image-20220412152835281](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204121528341.png)l

==里面的指令和上面cmd的一样==

![image-20220412152925288](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204121529372.png)

![image-20220412152934423](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204121529499.png)

```shell
#我们的追加命令， 是直接拼接到ENTRYPOINT命令的后面的！
```



### 实战：Dockerfile制作tomcat镜像(30-33未看)

1、准备镜像文件 tomcat压缩包，而tomcat依赖于java环境。因此还需要准备一个jdk的压缩包

![image-20220412195543679](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204121955842.png)

2、编写Dockerfile文件，官方命名`Dockerfile`, build会自动寻找这个文件，就不需要-f指定了！

![image-20220412200215463](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204122002546.png)

```shell
FROM centos
MAINTAINER kwq<347858300@qq.com>

COPY readme.txt /usr/local/readme.txt

ADD jdk-8u321-linux-x64.tar.gz /usr/local/   #将jdk放到 /usr/local文件夹下，ADD会自动帮我们解压
ADD apache-tomcat-9.0.62.tar.gz /usr/local/

RUN yum -y install vim

ENV MYPATH /usr/local
WORKDIR $MYPATH        #设置工作目录

ENV JAVA_HOME /usr/local/jdk1.8.0_321
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.62
ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.62
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin

EXPOSE 8080


CMD /usr/local/apache-tomcat-9.0.62/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.62/bin/logs/catalina.out     #容器启动时候要执行命令
报错了，不会改
```

![image-20220412202513512](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204122025633.png)

3、构建镜像

```shell
docker build -t diytomcat .
```

4、启动镜像

```shell
#  docker run -d -p 3344:8080 --name xiaofantomcat1 -v /tomcat/test:/usr/local/apache-tomcat-9.0.62/webapps/test -v /tomcat/tomcatlogs/:/usr/local/apache-tomcat-9.0.62/logs diytomcat

```





### 小结：

![image-20220412203603822](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204122036023.png)

==save是将镜像打包给别人，然后别人执行load加载我们的镜像，不推荐这样做，一般都是放到远程，别人来拉==

