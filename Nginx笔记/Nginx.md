# Nginx

## 公司产品出现瓶颈？

我们公司项目刚刚上线的时候，并发量小，用户使用的少，所以在低并发的情况下，`一个jar包启动应用就够了，然后内部tomcat返回内容给用户。`

![image-20220128222808703](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220128222808703.png)

但是慢慢的，使用我们平台的用户越来越多了，`并发量慢慢增大`了，这时候`一台服务器满足不了我们的需求`了。

![image-20220128222914452](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220128222914452.png)

于是我们横向扩展，又增加了服务器。这个时候==几个项目启动在不同的服务器上，用户要访问，就需要增加一个代理服务器了，通过代理服务器来帮我们转发和处理请求==。

![image-20220128223010605](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220128223010605.png)

我们希望这个代理服务器可以帮助我们接收用户的请求，然后将用户的请求==按照规则帮我们转发到不同的服务器节点之上==。这个过程==用户是无感知的，用户并不知道是哪个服务器返回的结果，我们还希望他可以按照服务器的性能（比如一台服务器是4g，另一台是8g）提供不同的权重选择。保证最佳体验==！所以我们使用了`Nginx`。`没有什么是加一层解决不了的，有的话就加两层，使用Nginx不过是加了一层来改变架构实现更好的功能和用户体验`

## 什么是Nginx？

Nginx (engine x) 是一个==高性能（响应快，高并发下处理能力好）的HTTP和反向代理web服务器==，同时也提供了==IMAP/POP3/SMTP（发送邮件的）==服务。Nginx是由伊戈尔·赛索耶夫为俄罗斯访问量第二的Rambler.ru站点（俄文：Рамблер）开发的，第一个公开版本0.1.0发布于2004年10月4日。2011年6月1日，nginx 1.0.4发布。

其特点是==占有内存少（下载下来就1M）==，==并发能力强==，事实上nginx的并发能力在同类型的网页服务器中表现较好，中国大陆使用nginx网站用户有：==百度、京东、新浪、网易、腾讯、淘宝等==。在全球活跃的网站中有12.18%的使用比率，大约为2220万个网站。

Nginx 是一个安装非常的简单、==配置文件非常简洁（还能够支持perl语法）、Bug非常少的服务。Nginx 启动特别容易，并且几乎可以做到`7*24`==不间断运行，即使运行数个月也不需要重新启动。==你还能够不间断服务的情况下进行软件版本的升级。==

Nginx代码完全用C语言从头写成。官方数据测试表明能够支持高达 ==50,000个并发连接数==的响应。

## Nginx作用？

> Http代理（正向代理），反向代理：作为Nginx最常用的功能之一，尤其是反向代理。

### 正向代理(==配置vpn==)

HTTP代理：HTTP代理就是介于浏览器和web服务器之间的一台服务器，连接代理后，浏览器不再直接向web服务器取回网页，而是向代理服务器发出request信号，代理服务器再向web服务器发出请求，收到web服务器返回的数据后再反馈给浏览器。

Nginx不仅可以做反向代理，实现负载均衡。还能用作==正向代理来进行上网==等功能。 正向代理：如果把局域网外的Internet想象成一个巨大的资源库，则局域网中的客户端要访问Internet，则需要通过代理服务器来访问，这种代理服务就称为正向（Http）代理。

![image-20220130141702164](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130141702164.png)

### 反向代理：

其实客户端对代理是无感知的，因为客户端不需要任何配置就可以访问，我们只需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，在返回给客户端，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器地址，隐藏了真实服务器IP地址。 

![image-20220130142139795](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130142139795.png)



>
> Nginx提供的负载均衡策略有2种：内置策略和扩展策略。内置策略为==轮询==，==加权轮询==，Ip hash。扩展策略，就天马行空，只有你想不到的没有他做不到的。
>

轮询

==ps:图中像云一样的东西可以忽略==

![image-20220128225954403](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220128225954403.png)

加权轮询

![image-20220128230009163](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220128230009163.png)

iphash对客户端请求的ip进行hash操作，==然后根据hash结果将同一个客户端ip的请求分发给同一台服务器进行处理，可以解决session不共享的问题。==   

> 如果根据nginx的负载均衡规则，不同服务器上用的Session不可共享，这次请求到第一台服务器，下次请求别的服务器的话，就要重新登陆了，对客户很不友善，iphash的规则很好的解决了此问题，每个ip只能访问一个服务器。但实际开发中，如果某台服务器挂了，那这个用户就操作不了了，因此一般我们用redis来实现Session共享从而解决这个问题

![image-20220128230025783](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220128230025783.png)

### 动静分离：

>动静分离，在我们的软件开发中，有些请求是需要后台处理的，有些请求是不需要经过后台处理的（如：css、html、jpg、js等等静态文件），这些不需要经过后台处理的文件称为静态文件。==让动态网站里的动态网页根据一定规则把不变的资源和经常变的资源区分开来，动静资源做好了拆分以后，我们就可以根据静态资源的特点将其在Nginx做缓存操作。提高资源响应的速度。==

![image-20220131120351896](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220131120351896.png)

![image-20220128230124024](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220128230124024.png)

目前，通过使用Nginx大大提高了我们网站的响应速度，优化了用户体验，让网站的健壮性更上一层楼！

## Nginx的安装

### windows下安装

1、下载nginx

http://nginx.org/en/download.html 下载稳定版本。
以nginx/Windows-1.20.2为例，直接下载 nginx-1.20.2.zip。
下载后解压，解压后如下：

![image-20220128231940936](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220128231940936.png)

2、启动nginx

有很多种方法启动nginx

(1)直接双击nginx.exe，双击后一个黑色的弹窗一闪而过

==(2)打开cmd命令窗口，切换到nginx解压目录下，输入命令 nginx.exe ，回车即可==

3、检查nginx是否启动成功

直接在浏览器地址栏输入网址 http://localhost:80 回车，出现以下页面说明启动成功！

![image-20220128231840743](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220128231840743.png)

4、配置监听

nginx的配置文件是conf目录下的nginx.conf，默认配置的nginx监听的端口为80，如果80端口被占用可以修改为未被占用的端口即可。

![image-20220128232102508](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220128232102508.png)

==当我们修改了nginx的配置文件nginx.conf 时，不需要关闭nginx后重新启动nginx，只需要执行命令 nginx -s reload 即可让改动生效==

5、关闭nginx

如果==使用cmd命令窗口启动nginx， 关闭cmd窗口是不能结束nginx进程的，可使用两种方法关闭nginx==

(1)输入nginx命令` nginx -s stop(`快速停止nginx) 或 `nginx -s quit`(完整有序的停止nginx)

(2)使用`taskkill taskkill /f /t /im nginx.exe`

```
taskkill是用来终止进程的
/f是强制终止 .
/t终止指定的进程和任何由此启动的子进程。
/im示指定的进程名称 .
```

### linux下安装

1、安装gcc

安装 nginx 需要先将官网下载的源码进行编译，编译依赖 gcc 环境，如果没有 gcc 环境，则需要安装：

```aa
yum install gcc-c++
```

2、PCRE pcre-devel 安装

PCRE(Perl Compatible Regular Expressions) 是一个Perl库，包括 perl 兼容的正则表达式库。nginx 的 http 模块使用 pcre 来解析正则表达式，所以需要在 linux 上安装 pcre 库，pcre-devel 是使用 pcre 开发的一个二次开发库。nginx也需要此库。命令：

```
yum -y install pcre-devel openssl openssl-devel
```

手动下载.tar.gz安装包，地址：https://nginx.org/en/download.html

![image-20220128235557034](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220128235557034.png)

下载完毕上传到服务器上 `/opt`  ==这也是为了方便管理，统一的一个目录==

6、在/opt目录下解压

```
tar -zxvf nginx-1.20.2.tar.gz
cd nginx-1.20.2
```

![image-20220128235949050](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220128235949050.png)

7、配置

使用默认配置，在nginx-1.20.2根目录下执行

```
1 ./configure
2 make
3 make install
查找安装路径： whereis nginx
```

![image-20220129000119293](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220129000119293.png)

![image-20220129001525650](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220129001525650.png)

==注意：要将阿里云安全组对应的端口开放、服务器防火墙对应端口也要开放！不然外网无法访问==

查看nginx的默认端口：在==usr/local/conf/nginx.conf==文件中能看到

![image-20220129001909760](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220129001909760.png)

访问服务器ip的80端口：

![image-20220129000451680](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220129000451680.png)

> Nginx常用命令 （都要在whereis nginx显示的sbin目录下才能用）

```
1 cd /usr/local/nginx/sbin/
2 ./nginx  启动          #要进入whereis nginx显示的sbin目录
3 ./nginx -s stop  强制停止
4 ./nginx -s quit  安全退出,把所有的进程一项一项的退出，也就是慢慢退出
5 ./nginx -s reload  重新加载配置文件
6 ps -ef|grep nginx  查看nginx进程

```

> 测试命令5（因为常用）

在usr/local/conf目录下修改nginx.conf文件，将端口改为81

执行： ./nginx -s reload  重新加载配置文件，接着执行./nginx 

访问测试：

![image-20220129003017047](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220129003017047.png)



## Nginx的配置文件

`usr/local/conf下的nginx.conf就是nginx的配置文件！`

配置文件中有很多# 开头的表示注释内容，我们去掉所有以 # 开头的段落，精简之后的内容如下：

```
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;

    keepalive_timeout  65;

    server {
        listen       80;
        server_name  localhost;

        location / {
            root   html;
            index  index.html index.htm;
        }  
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }    
    }
}

```

根据上述文件，我们可以很明显的将 nginx.conf 配置文件分为三部分：

==第一部分：全局块==

从配置文件开始到 events 块之间的内容，==主要会设置一些影响nginx 服务器整体运行的配置指令==，主要包括配置运行 Nginx 服务器的用户（组）、允许生成的 worker process 数，进程 PID 存放路径、日志存放路径和类型以及配置文件的引入等。

![image-20220131154840297](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220131154840297.png)

比如上面第一行配置的：   这是 Nginx 服务器并发处理服务的关键配置，worker_processes 值越大，可以支持的并发处理量也越多，但是会受到硬件、软件等设备的制约 

==第二部分：events块==

比如上面的配置： 

![image-20220131155000831](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220131155000831.png)

​	events 块涉及的指令==主要影响 Nginx 服务器与用户的网络连接==，常用的设置包括是否开启对多 work process 下的网络连接进行序列化，是否允许同时接收多个网络连接，选取哪种事件驱动模型来处理连接请求，每个 word process 可以同时支持的最大连接数等。  	上述例子就表示每个 work process 支持的最大连接数为 1024.  

​	这部分的配置对 Nginx 的性能影响较大，在实际中应该灵活配置。 

==第三部分：http块==

![image-20220131155116574](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220131155116574.png)

​	==这算是 Nginx 服务器配置中最频繁的部分，代理、缓存和日志定义等绝大多数功能和第三方模块的配置都在这里。==

​	需要注意的是：http 块也可以包括 ==http全局块、server 块==。

==①http 全局块== 

 http全局块配置的指令包括文件引入、MIME-TYPE 定义、日志自定义、连接超时时间、单链接请求数上限等。 

==②server 块==

​	`这块和虚拟主机有密切关系，虚拟主机从用户角度看，和一台独立的硬件主机是完全一样的，该技术的产生是为了节省互联网服务器硬件成本`  

​	==每个 http 块可以包括多个 server 块，而每个 server 块就相当于一个虚拟主机。==

​	==而每个 server 块也分为全局 server 块，以及可以同时包含多个 locaton 块==

==1、全局 server 块==  

​	==最常见的配置是本虚拟机主机的监听配置和本虚拟主机的名称或IP配置。==

==2、location 块==

​	一个 server 块可以配置多个 location 块。  

​	`这块的主要作用是基于 Nginx 服务器接收到的请求字符串（例如 server_name/uri-string），对虚拟主机名称（也可以是IP别名）之外的字符串（例如 前面的 /uri-string）进行匹配，对特定的请求进行处理。地址定向、数据缓存和应答控制等功能，还有许多第三方模块的配置也在这里进行。`



## 实操：

### 反向代理实操（1）：

实现效果：

![image-20220130150103921](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130150103921.png)

启动后测试看看是否启动成功

![image-20220130150227345](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130150227345.png)

 如果访问的到tomcat 说明启动成功啦！！！

![image-20220130150241709](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130150241709.png)

![image-20220130150324284](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130150324284.png)

 添加内容到HOSTS文件中

![image-20220130150343942](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130150343942.png)

 配置完成之后，我们便可以通过 www.123.com:8080 访问到第一步出现的 Tomcat初始界面。那么如何只需要输入 www.123.com 便可以跳转到 Tomcat初始界面呢？便用到 nginx的反向代理。

如何设置呢？

打开nginx的配置文件nginx.conf

添加

![image-20220130150416845](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130150416845.png)

 如上配置，我们监听80端口，访问域名为www.123.com，不加端口号时默认为80端口，故访问该域名时会跳转到127.0.0.1:8080路径上。在浏览器端输入 www.123.com 结果如下： 

![image-20220130150434637](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130150434637.png)

这样就完成啦！！！

### 反向代理实操（2）：

实现效果：使用nginx反向代理，根据访问的路径跳转到不同 端口的服务中

nginx监听端口为9001，

访问 http://127.0.0.1:9001/edu/ 直接跳转到127.0.0.1:8081

访问 http://127.0.0.1:9001/vod/ 直接跳转到127.0.0.1:8082

实验代码

第一步，准备两个tomcat，一个8001端口，一个8002端口，并准备好测试的页面

第二步，修改nginx的配置文件 在http块中添加server{}

![image-20220130151324885](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130151324885.png)

抱歉！！上面那个端口写错了应该是 localhost:8081和8082；

 location指令说明  

该指令用于匹配 URL。  语法如下： 

![image-20220130151408373](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130151408373.png)

![image-20220130151353294](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130151353294.png)

==在两个tomcat的webapp下新建`edu和vod文件夹`，然后导入一个html页面 内容为8080！！和8081！！==

测试访问！！！

![image-20220130151421713](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130151421713.png)

记得8080,8081,9001端口都要开放，不然访问不成功滴！！！

 这样就实现了反向代理啦！！

### Nginx之配置负载均衡：

![image-20220130152018749](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130152018749.png)

a.html里面对应的一个是8080一个是8081

在Nginx的配置文件中进行负载均衡配置

分别是在==http代码块中加入upstream myserver的内容和location中加入proxy_pass==  http://myserver

![image-20220130152046819](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130152046819.png)

 启动两个tomcat然后访问如下地址：

![image-20220130152055492](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130152055492.png)

![image-20220130152122920](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130152122920.png)

 发现两者交替打印！！！

随着互联网信息的爆炸性增长，负载均衡（load balance）已经不再是一个很陌生的话题，顾名思义，负载均衡即是将负载分摊到不同的服务单元，既保证服务的`可用性`，又保证`响应足够快`，给用户很好的体验。快速增长的访问量和数据流量催生了各式各样的负载均衡产品，很多专业的负载均衡硬件提供了很好的功能，但却价格不菲，这使得负载均衡软件大受欢迎，nginx就是其中的一个，在linux下有Nginx、LVS、Haproxy等等服务可以提供负载均衡服务，而且Nginx提供了几种分配方式(策略)： 

 nginx分配服务器的策略！！！！ 

1、轮询（默认） 每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。

2、weight weight代表权,重默认为1,权重越高被分配的客户端越多 指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。 例如：

![image-20220130152204831](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130152204831.png)

 3、ip_hash

每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。 例如： 

![image-20220130152213050](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130152213050.png)

4、fair（第三方）

按后端服务器的响应时间来分配请求，响应时间短的优先分配。 

![image-20220130152220540](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220130152220540.png)

### Nginx实现动静分离

#### 1.动静分离是什么

![image-20220131111523407](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220131111523407.png)

通过` location`指定不同的后缀名`实现不同的请求转发`。通过 expires 参数设置，可以使浏览器缓存过期时间，减少与服务器之间的请求和流量。具体 Expires 定义：是给一个资源设定一个过期时间，也就是说无需去服务端验证，直接通过浏览器自身确认是否过期即可，所以不会产生额外的流量。==此种方法非常适合不经常变动的资源==。（如果经常更新的文件，不建议使用 Expires 来缓存），我这里设置 3d，表示在这 3 天之内访问这个 URL，发送一个请求，比对服务器该文件最后更新时间没有变化，则不会从服务器抓取，返回状态码 304，如果有修改，则直接从服务器重新下载，返回状态码 200。  

####  2准备工作：

（1）在liunx系统中准备静态资源，用于进行访问 

![image-20220131111605916](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220131111605916.png)

#### 3.具体配置

（1）在[nginx](https://so.csdn.net/so/search?q=nginx&spm=1001.2101.3001.7020)配置文件中进行配置

![image-20220131111724576](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220131111724576.png)

#### 4、最终测试

（1）浏览器中输入地址 

![image-20220131111802989](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220131111802989.png)

==这个能显示目录内容是因为配置了autoindex on;==

![image-20220131111822611](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220131111822611.png)

![image-20220131111836341](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220131111836341.png)

### Nginx配置高可用集群

![image-20220131111953116](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220131111953116.png)

==环境搭建== 两台liunx系统，环境配置一样（有[nginx](https://so.csdn.net/so/search?q=nginx&spm=1001.2101.3001.7020)和其依赖的条件以及tomcat）！！！

需要在两台服务器中安装keepalived

这里实操一下：

通过yum install keepalived -y 指令

![image-20220131112016052](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220131112016052.png)

安装完查看一下版本确保真的安装成功了，使用命令：

==rpm -q -a keepalived==

安装成功后在`/etc会生成一个keepalived文件夹`

里面有一个文件`keepalived.conf ` 配置高可用的集群操作都是在这里修改

添加以下内容：

主服务器的keepalived.conf

```
global_defs {
   notification_email {
     acassen@firewall.loc
     failover@firewall.loc
     sysadmin@firewall.loc
   }   
   notification_email_from Alexandre.Cassen@firewall.loc
   smtp_server 192.168.229.128     #服务器ip
   smtp_connect_timeout 30
   router_id LVS_DEVEL        #访问到主机
}
 
vrrp_script chk_http_port {
    
    script "/usr/local/nginx/nginx_check.sh"  #检测脚本位置
 
    interval 2              # 检测脚本执行的间隔
 
    weight 2            #权重
 
}
 
vrrp_instance VI_1 {
    state MASTER # 备份服务器上将 MASTER 改为 BACKUP
    interface eth0         # 网卡
    virtual_router_id 51    # 主、备机的 virtual_router_id 须相同
    priority 100          # 主、备机取不同的优先级，主机值较大，备机值较小
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }   
    virtual_ipaddress {
        192.168.229.50     # VRRP H 虚拟地址
    }   
}
```

在/usr/local/nginx添加检测脚本 ：

名称为：nginx_check.sh（和上面保持一致）

具体内容：

```
#!/bin/bash
A=`ps -C nginx ¨Cno-header |wc -l`
if [ $A -eq 0 ];then
    /usr/local/nginx/sbin/nginx
    sleep 2
    if [ `ps -C nginx --no-header |wc -l` -eq 0 ];then
        killall keepalived
    fi
fi
```

从服务器两个对应的文件内容：

```

global_defs {
   notification_email {
     acassen@firewall.loc
     failover@firewall.loc
     sysadmin@firewall.loc
   }   
   notification_email_from Alexandre.Cassen@firewall.loc
   smtp_server 192.168.229.130
   smtp_connect_timeout 30
   router_id LVS_DEVEL
}
 
vrrp_script chk_http_port {
    
    script "/usr/local/nginx/nginx_check.sh"
 
    interval 2              # 检测脚本执行的间隔
 
    weight 2
 
}
 
vrrp_instance VI_1 {
    state BACKUP          # 备份服务器上将 MASTER 改为 BACKUP
    interface eth0         # 网卡
    virtual_router_id 51    # 主、备机的 virtual_router_id 须相同
    priority 90           # 主、备机取不同的优先级，主机值较大，备机值较小
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }   
    virtual_ipaddress {
        192.168.229.50     # VRRP H 虚拟地址
    }   
}

```

nginx_check.sh文件内容和上面一样！！！

把两台服务器上nginx和keepalived启动

启动nginx：./nginx

启动keepalived：service start keepalived

最终测试 （1）在浏览器地址栏输入 虚拟ip地址 192.168.229.50 

![image-20220131112251088](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220131112251088.png)

![image-20220131112301798](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220131112301798.png)

 把主服务器关了

再次访问

![image-20220131112320942](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220131112320942.png)

测试完毕！！！
