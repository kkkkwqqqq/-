# Java Web

## 1、基本概念

### 1.1、前言

web开发：

- web，网页的意思 ， www.baidu.com
- 静态web
  - html，css
  - 提供给所有人看的数据始终不会发生变化！
- 动态web
  - 淘宝，几乎是所有的网站；
  - 提供给所有人看的数据始终会发生变化，每个人在不同的时间，不同的地点看到的信息各不相同！
  - 技术栈：Servlet/JSP

在Java中，动态web资源开发的技术统称为JavaWeb；



### 1.2、web应用程序

web应用程序：可以提供浏览器访问的程序；

- a.html、b.html......多个web资源，这些web资源可以被外界访问，对外界提供服务；
- 你们能访问到的任何一个页面或者资源，==都存在于这个世界的某一个角落的计算机上。==
- URL
- 这个统一的web资源会被放在同一个文件夹(`代表当前web应用`)下，web应用程序-->Tomcat：服务器
- 一个web应用由多部分组成 （静态web，动态web）
  - html，css，js
  - jsp，servlet
  - Java程序
  - jar包
  - 配置文件 （Properties）

web应用程序编写完毕后，若想提供给外界访问：需要一个`服务器`来统一管理；

### 1.3、静态web

- *.htm, *.html,这些都是网页的后缀，如果服务器上一直存在这些东西，我们就可以直接进行读取。==通过网络的方式==；

![1567822802516](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204221503612.png)

- 静态web存在的缺点
  - Web页面无法动态更新，所有用户看到都是同一个页面
    - 轮播图，点击特效：伪动态
    - JavaScript [实际开发中，它用的最多]
    - VBScript
  - 它无法和数据库交互（数据无法持久化，用户无法交互）

### 1.4、动态web

页面会动态展示： “Web的页面展示的效果因人而异”；==下图的WebServer Plugin指的是过滤某些请求的插件==

![1567823191289](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204221504497.png)

缺点：

- 加入服务器的动态web资源出现了错误，我们需要重新编写我们的后台程序
  - 停机维护

优点：

- Web页面可以动态更新，所有用户看到都不是同一个页面
- 它可以与数据库交互 （数据持久化：注册，商品信息，用户信息........）

![1567823350584](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204221504414.png)



## 2、web服务器

### 2.1、技术讲解

**ASP:**

- 微软：国内最早流行的就是ASP(`类似jsp`)；

- 在HTML中嵌入了VB的脚本， ASP + COM；

- 在ASP开发中，基本一个页面都有几千行的业务代码，页面极其换乱(`html页面参杂后端语言代码`)

- 维护成本高！

  ```html
  <h1>
      <h1><h1>
          <h1>
              <h1>
                  <h1>
          <h1>
              <%
              System.out.println("hello")
              %>
              <h1>
                  <h1>
     <h1><h1>
  <h1>
  ```

**php：**

- PHP开发速度很快，功能很强大，跨平台，代码很简单 
- 无法承载大访问量的情况（`局限性`）

**JSP(本质是Servlet)/Servlet : **

B/S：浏览和服务器(`web应用：网页版`)

C/S: 客户端和服务器   （`桌面级应用：手机和电脑的应用程序`）

B/S和C/S具体详解参考：https://m.html.cn/qa/other/21685.html

- sun公司主推的B/S架构
- 基于Java语言的 (所有的大公司，或者一些开源的组件，都是用Java写的)
- 可以承载三高（`高并发，高可用，高性能`）问题带来的影响；
- 语法像ASP ， ASP-->JSP , 加强市场强度；



### 2.2、web服务器

服务器是一种被动的操作，用来处理用户的一些请求和给用户一些响应信息；

**IIS**

微软的； ASP...,Windows中自带的

**Tomcat**

面向百度编程；

Tomcat是Apache 软件基金会（Apache Software Foundation）的Jakarta 项目中的一个核心项目，最新的Servlet 和JSP 规范总是能在Tomcat 中得到体现，因为Tomcat `技术先进`、`性能稳定`，而且**免费**，因而深受Java 爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的`Web 应用服务器。`

Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，属于`轻量级应用服务器`，在`中小型系统`和`并发访问用户`不是很多的场合下被普遍使用，是开发和调试JSP 程序的首选。对于一个Java初学web的人来说，它是最佳的选择

Tomcat 实际上运行JSP 页面和Servlet。

....

**工作3-5年之后，可以尝试手写Tomcat服务器；**

下载tomcat：

1. 安装 or 解压
2. 了解配置文件及目录结构
3. 这个东西的作用



## 3、Tomcat

### 3.1、 安装tomcat

tomcat官网：http://tomcat.apache.org/

![1567825600842](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204221550683.png)

![1567825627138](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204221550304.png)

### 3.2、Tomcat启动和配置

文件夹作用：

![1567825763180](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204221550382.png)

**启动。关闭Tomcat**

![1567825840657](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213522858-1659849952.png)

访问测试：http://localhost:8080/

可能遇到的问题：

1. Java环境变量没有配置
2. 闪退问题：需要配置兼容性
3. 乱码问题：配置文件中设置

### 3.3、配置

![1567825967256](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213523061-1509783086.png)

==默认情况下，tomcat自带3个应用（3个文件夹代表3个应用）==

![image-20220422161005963](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204221610096.png)

==我们访问localhost:8080实际上就是访问tomcat9/webapps/ROOT应用的中的index.jsp==

![image-20220422160828550](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204221608969.png)

==那我们访问docs文件夹==  默认访问docs下的index.html页面

![image-20220422164115955](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204221641419.png)

可以配置启动的端口号

- tomcat的默认端口号为：8080（可以修改。像下面这样【==在tomcat的conf/server.xml中==】）

```xml
<Connector port="8081" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

可以配置主机的名称

- 默认的主机名为：localhost->127.0.0.1 ==下面修改成==www.qinjiang.com 所以我们的hosts文件里面也要配置
- 默认网站应用存放的位置为：webapps（`可以修改文件夹名字,那么你tomcat实际的webapps也要修改`）

```xml
  <Host name="www.qinjiang.com"  appBase="webapps"
        unpackWARs="true" autoDeploy="true">
```

在`C:\Windows\System32\drivers\etc`  中的hosts中添加域名映射` www.qinjiang.com`  然后可以通过:www.qinjiang.com:8080 访问到tomcat

![image-20220422161853610](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204221618725.png)

#### 高难度面试题：

请你谈谈网站是如何进行访问的！

1. 输入一个域名；回车

2. 检查本机的 ==C:\Windows\System32\drivers\etc\hosts==配置文件下有没有这个域名映射；

   1. 有：直接返回对应的ip地址，这个地址中，有我们需要访问的web程序，可以直接访问

      ```java
      127.0.0.1       www.qinjiang.com
      ```

   2. 没有：去DNS服务器找，找到的话就返回，找不到就返回找不到；

   ![1567827057913](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213523265-1829055418.png)

   

### 3.4、发布一个web网站

不会就先模仿

- 将自己写的网站（==web应用==），放到服务器(Tomcat)中指定的web应用的文件夹（webapps）下，就可以访问了

> 案例：在webapps中新建一个文件夹 写个index.html页面

![image-20220422164558208](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204221645432.png)

==浏览器打开：==

![image-20220422164501686](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204221645845.png)

网站应该有的结构

```java
 --webapps ：Tomcat服务器的web目录
	 -ROOT
		- WEB-INF
			-classes : java程序
			-lib：web应用所依赖的jar包
			-web.xml ：网站配置文件
		- index.html 默认的首页
		- static 
            -css
            	-style.css
            -js
            -img
         -.....
```



## 4、Http

### 4.1、什么是HTTP

HTTP（`超文本传输协议`）是一个简单的请求-响应协议，它通常运行在TCP之上。

- 文本：html，字符串，~ ….
- 超文本：图片，音乐，视频，定位，地图…….
- 80

Https：安全的（==s是安全的意思==）

- 443（==默认端口==）

### 4.2、两个时代

- http1.0
  - HTTP/1.0：客户端可以与web服务器连接后，只能获得一个web资源，然后就会断开连接，想再次访问资源，又得再连接一次。
- http2.0
  - HTTP/1.1：客户端可以与web服务器连接后，可以获得多个web资源。‘

### 4.3、Http请求

- 客户端---发请求（Request）---服务器

> 请求百度（查看网络）：

```java
Request URL:https://www.baidu.com/   请求地址
Request Method:GET    get方法/post方法
Status Code:200 OK    状态码：200
Remote（远程） Address:14.215.177.39:443
Accept:text/html  
Accept-Encoding:gzip, deflate, br
Accept-Language:zh-CN,zh;q=0.9    语言
Cache-Control:max-age=0
Connection:keep-alive
```

#### 1、请求行

- 请求行中的请求方式：GET

- 请求方式：

  Get，Post

  ，HEAD,DELETE,PUT,TRACT…

  - ==get：请求能够携带的参数比较少，大小有限制，会在浏览器的URL地址栏显示数据内容，不安全，但高效==
  - post：请求能够携带的参数没有限制，大小没有限制，==不会在浏览器的URL地址栏显示数据内容，安全，但不高效。==

#### 2、消息头

```java
Accept：告诉浏览器，它所支持的数据类型
Accept-Encoding：支持哪种编码格式  GBK   UTF-8   GB2312  ISO8859-1
Accept-Language：告诉浏览器，它的语言环境
Cache-Control：缓存控制
Connection：告诉浏览器，请求完成是断开还是保持连接
HOST：主机..../.
```

### 4.4、Http响应

- 服务器---响应-----客户端

百度：

```java
Cache-Control:private    缓存控制
Connection:Keep-Alive    连接
Content-Encoding:gzip    编码
Content-Type:text/html   类型
```

#### 1.响应体

```java
Accept：告诉浏览器，它所支持的数据类型
Accept-Encoding：支持哪种编码格式  GBK   UTF-8   GB2312  ISO8859-1
Accept-Language：告诉浏览器，它的语言环境
Cache-Control：缓存控制
Connection：告诉浏览器，请求完成是断开还是保持连接
HOST：主机..../.
Refresh：告诉客户端，多久刷新一次；
Location：让网页重新定位；
```

#### 2、响应状态码

200：请求响应成功 200

3xx：请求重定向

- 重定向：你重新到我给你新位置去（==资源不存在，服务端可以设置调到指定位置==）；

4xx：找不到资源 404

- 资源不存在；

![image-20220422173347563](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204221733096.png)

5xx：服务器代码错误 500 502:网关错误



## 5、Maven

**我为什么要学习这个技术？**

1. 在Javaweb开发中，需要使用大量的jar包，我们手动去导入；

2. 如何能够让一个东西自动帮我导入和配置这个jar包。

   由此，Maven诞生了！

### 5.1 Maven项目架构管理工具

我们目前用来就是方便导入jar包的！

Maven的核心思想：**约定大于配置**

- 有约束，不要去违反。

Maven会规定好你该如何去编写我们的Java代码（`通过指定的目录来约束`），必须要按照这个规范来；

### 5.2 下载安装Maven

官网;https://maven.apache.org/

![1567842350606](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204221754278.png)

下载完成后，解压即可；

小狂神友情建议：电脑上的所有环境都放在一个文件夹下，方便管理；

### 5.3 配置环境变量

在我们的系统环境变量中

配置如下配置：

- M2_HOME:maven目录下的bin目录
- MAVEN_HOME:maven的目录
- 在系统的path中配置 %MAVEN_HOME%\bin

![image-20220422205135521](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222051614.png)

测试Maven是否安装成功，保证必须配置完毕！

### 5.4 阿里云镜像

![image-20220422205355415](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222053661.png)

- 镜像：mirrors
  - 作用：加速我们的下载
- 国内建议使用阿里云的镜像
- conf/setting.xml中添加

```xml
<mirror>
    <id>nexus-aliyun</id>  
    <mirrorOf>*,!jeecg,!jeecg-snapshots</mirrorOf>  
    <name>Nexus aliyun</name>  
    <url>http://maven.aliyun.com/nexus/content/groups/public</url> 
</mirror>
```

![image-20220422205046629](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222050242.png)

### 5.5 本地仓库

**建立一个本地仓库：**D:\Environment\apache-maven-3.6.2\maven-repo    ==maven-repo是仓库名==

  ```
  <localRepository>D:\Environment\apache-maven-3.6.2\maven-repo</localRepository>  conf/setting.xml中添加
  ```



![image-20220422205109919](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222051235.png)

![image-20220422205007866](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222050007.png)

### 5.6、在IDEA中使用Maven

1. 启动IDEA

2. 创建一个MavenWeb项目（==其实就是在一个什么模板都没有的maven项目帮我们自动添加东西，比如webapp文件夹这些==）

   ![1567844785602](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222008049.png)

   ![1567844841172](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222008862.png)

   ![1567844917185](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213524806-337576310.png)

   ![1567844956177](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222008285.png)

   ![image-20220422201522158](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222015355.png)

3. 等待项目初始化完毕

   ![1567845105970](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213525489-780870148.png)

   ![1567845137978](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222008743.png)

4. 观察maven仓库中多了什么东西？

5. IDEA中的Maven设置

   注意：IDEA项目创建成功后，看一眼Maven的配置

   ![1567845341956](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222008300.png)

   > 自动导入源码不勾选

   ![1567845413672](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222008740.png)

6. 到这里，Maven在IDEA中的配置和使用就OK了!

### 5.7、创建一个普通的Maven项目

![image-20220422210932144](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222109302.png)

![image-20220422211207891](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222112046.png)

==maven的约定大于配置就是说每个文件夹下放什么，如上图文件夹==

### 5.8、标记文件夹功能

![image-20220422211438730](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222114180.png)

下面这个和上面这个功能是一样的

![image-20220422212444099](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222124354.png)

> 这样web项目结构就完整了

![image-20220422211556910](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222115182.png)

### 5.9 在 IDEA中配置Tomcat

![1567846140348](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213528180-1681352374.png)

![1567846179573](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213528400-378162968.png)

![1567846234175](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222125996.png)

![1567846369751](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213528834-743243549.png)

解决警告问题

必须要的配置：**为什么会有这个问题：我们访问一个网站，需要指定一个文件夹名字；**

![1567846421963](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213529071-1808646856.png)

![1567846546465](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222125344.png)

![1567846559111](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213529550-931923879.png)

![1567846640372](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213529779-8220418.png)

==上图是因为application.context写了/kuang==

### 5.10 pom文件

pom.xml 是Maven的核心配置文件  ==插件可以删掉因为和maven生命周期的功能一样==

![1567846784849](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213530017-2003692168.png)

> pom：

```java
<?xml version="1.0" encoding="UTF-8"?>
<!--maven版本和头文件-->
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
<!--这里是我们刚刚配置的GAV-->
  <groupId>com.kwq</groupId>
  <artifactId>javaweb-01-maven</artifactId>
  <version>1.0-SNAPSHOT</version>
 <!--packing 打包方式：
 jar: java应用
 war：javaweb应用
 -->
  <packaging>jar</packaging>

  <name>javaweb-01-maven Maven Webapp</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>
<!--配置-->
  <properties>
    <!--项目的默认构建编码-->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--编码版本-->
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>
<!--项目依赖-->
  <dependencies>
    <!--具体jar-->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-resources-plugin</artifactId>
      <version>2.6</version>
    </dependency>


  </dependencies>

  <build>
    <finalName>javaweb-01-maven</finalName>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>2.5</version>
        </plugin>
        <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->

        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-war-plugin</artifactId>
          <version>3.2.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>

```

> 建一个不加模板的maven项目  

![image-20220422220211603](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222202095.png)

maven由于他的约定大于配置，我们之后可能会遇到我们写的配置文件，无法被导出或者生效的问题（==mybatis中会遇到==），解决方案：

```xml
<!--在build中配置resources，来防止我们资源导出失败的问题-->
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

### 5.12 IDEA操作

![1567847630808](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222158606.png)

![1567847662429](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222158575.png)

### 5.13、解决遇到的问题

1. maven默认web项目中的web.xml版本问题

   ![image-20220422223537565](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222235880.png)

2. 替换为webapp4.0版本和tomcat一致（==在tomcat中的webapp里面的root的web.xml复制过来的，因为root就是一个web应用==）

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0"
            metadata-complete="true">
   
   
   
   </web-app>
   ```



## 6、Servlet

> 初始Servlet：

==导入servlet的依赖：==

```
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>4.0.1</version>
  <scope>provided</scope>
</dependency>
```

servlet类

```
package com.kwq;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //响应的类型 ：html
        resp.setContentType("text/html");
        //获取响应的输出流
        PrintWriter out = resp.getWriter();
        out.println("<html>");
        out.println("<head>");
        out.println("<title>hello World</title>");
        out.println("</head>");
        out.println("<body>");
        out.println("<h1>hello world</h1>");
        out.println("</body>");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

> web.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         metadata-complete="true">

<!--web.xml 中是我们web的核心配置-->
    <!--注册Servlet-->
    <servlet>
        <!--名字随便取-->
        <servlet-name>HelloServlet</servlet-name>
        <servlet-class>com.kwq.HelloServlet</servlet-class>
    </servlet>
    <!--Servlet的请求路径-->
    <servlet-mapping>
        <!--名字要和上面一样-->
        <servlet-name>HelloServlet</servlet-name>
        <url-pattern>/kwq</url-pattern>
    </servlet-mapping>



</web-app>
```

> 在webapp下建一个html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>我是导航栏</h1>
</body>
</html>
```

> 启动tomcat

访问静态资源

![image-20220422232232188](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222322730.png)

![image-20220422232433812](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222324197.png)

响应头的内容就是我们servlet，输出流输出的。

修改输出内容为中文

![image-20220422232737329](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222327486.png)

![image-20220422232653348](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222326719.png)

==出现上诉原因是因为请求头字符集的问题，我们可以修改请求头字符集==

![image-20220422232845492](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222328705.png)

==servlet中修改字符集==

![image-20220422233033456](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222330887.png)

再次启动输出：

![image-20220422233222533](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222332684.png)

==查看：==

![image-20220422233332059](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222333230.png)

### 6.1、Servlet简介

- Servlet就是sun公司开发动态web的一门技术
- Sun在这些API中提供一个接口叫做：Servlet，如果你想开发一个Servlet程序，只需要完成两个小步骤：
  - 编写一个类，实现Servlet接口
  - 把开发好的Java类部署到web服务器中。

**把实现了Servlet接口的Java程序叫做，Servlet**

### 6.2、HelloServlet

Serlvet接口Sun公司有两个默认的实现类：HttpServlet，GenericServlet

1. 构建一个普通的Maven项目，删掉里面的src目录，以后我们的学习就在这个项目里面建立Moudel；这个空的工程就是Maven主工程；

   ![image-20220423093738068](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204230937207.png)

发现某个依赖导入不进去

![image-20220423094650433](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204230946537.png)

==是因为我们的本地仓库里面没有这个jar==

![image-20220423094741139](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204230947242.png)

我们可以去远程maven仓库，把依赖弄进来，然后我们的本地仓库就会有这个jar

```java
<!-- https://mvnrepository.com/artifact/javax.servlet.jsp/jsp-api -->
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>jsp-api</artifactId>
    <version>2.2</version>
    <scope>provided</scope>
</dependency>

```

![image-20220423100141540](../AppData/Roaming/Typora/typora-user-images/image-20220423100141540.png)

==我们也可以直接在远程仓库把对应版本jar下载下来放进仓库对应位置，然后，原先那个依赖就不会爆红了就不会==

![image-20220423100244545](../AppData/Roaming/Typora/typora-user-images/image-20220423100244545.png)

接着我们新建一个子项目，选择webapp的模板

![image-20220423100430754](../AppData/Roaming/Typora/typora-user-images/image-20220423100430754.png)

1. 关于Maven父子工程的理解：

   父项目中会有

   ```xml
       <modules>
           <module>servlet-01</module>
       </modules>
   ```

   子项目会有

   ```xml
      <parent>
           <artifactId>javaweb-02-servlet</artifactId>
           <groupId>com.kwq</groupId>
           <version>1.0-SNAPSHOT</version>
       </parent>
   ```

   父项目中的依赖，子项目可以直接使用

   ```java
   son extends father
   ```

2. Maven环境优化

   1. 修改web.xml为最新的

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0"
            metadata-complete="true">
     
   </web-app>
   ```

   1. 将maven的结构搭建完整  ==java源文件，配置文件目录==

   ![image-20220423100832987](../AppData/Roaming/Typora/typora-user-images/image-20220423100832987.png)

3. 编写一个Servlet程序

> 继承关系

![1567911804700](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222224100.png)

1. 编写一个普通类

2. 实现Servlet接口，这里我们直接继承HttpServlet

   ```java
   package com.kwq.servlet;
   
   import javax.servlet.ServletException;
   import javax.servlet.http.HttpServlet;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   import java.io.PrintWriter;
   
   public class HelloServlet extends HttpServlet {
       
       //由于get或者post只是请求实现的不同的方式，可以相互调用，业务逻辑都一样；
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //ServletOutputStream outputStream = resp.getOutputStream();
           PrintWriter writer = resp.getWriter(); //响应流
           writer.print("Hello,Serlvet");
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req, resp);
       }
   }
   ```
   
3. 编写Servlet的映射（==web.xml中写==）

   为什么需要映射：我们写的是JAVA程序，但是要通过浏览器访问，而浏览器需要连接web服务器，所以我们需要再web服务中注册我们写的Servlet，还需给他一个浏览器能够访问的路径；

   ```xml
       <!--注册Servlet-->
       <servlet>
           <servlet-name>hello</servlet-name>
           <servlet-class>com.kuang.servlet.HelloServlet</servlet-class>
       </servlet>
       <!--Servlet的请求路径-->
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
   ```

4. 配置Tomcat

![image-20220423102222001](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231022142.png)

注意：配置项目发布的路径就可以了

> 最后启动测试：

![image-20220423102316606](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231023658.png)

### 6.3、Servlet原理

> 源码刨铣：

==**httpServlet的源码**==    `httq请求进来先进入这个方法`

```java
public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
    if (req instanceof HttpServletRequest && res instanceof HttpServletResponse) {
        HttpServletRequest request = (HttpServletRequest)req;
        HttpServletResponse response = (HttpServletResponse)res;
        this.service(request, response);
    } else {
        throw new ServletException("non-HTTP request or response");
    }
}
```

接着走这个方法

```
protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    String method = req.getMethod();
    long lastModified;
    if (method.equals("GET")) {
        lastModified = this.getLastModified(req);
        if (lastModified == -1L) {
            this.doGet(req, resp);
        } else {
            long ifModifiedSince = req.getDateHeader("If-Modified-Since");
            if (ifModifiedSince < lastModified) {
                this.maybeSetLastModified(resp, lastModified);
                this.doGet(req, resp);
            } else {
                resp.setStatus(304);
            }
        }
    } else if (method.equals("HEAD")) {
        lastModified = this.getLastModified(req);
        this.maybeSetLastModified(resp, lastModified);
        this.doHead(req, resp);
    } else if (method.equals("POST")) {
        this.doPost(req, resp);
    } else if (method.equals("PUT")) {
        this.doPut(req, resp);
    } else if (method.equals("DELETE")) {
        this.doDelete(req, resp);
    } else if (method.equals("OPTIONS")) {
        this.doOptions(req, resp);
    } else if (method.equals("TRACE")) {
        this.doTrace(req, resp);
    } else {
        String errMsg = lStrings.getString("http.method_not_implemented");
        Object[] errArgs = new Object[]{method};
        errMsg = MessageFormat.format(errMsg, errArgs);
        resp.sendError(501, errMsg);
    }

}
```

==**最后进入我们重写的doGet和doPost方法**==



Servlet是由Web服务器（==Tomcat==）调用，web服务器在收到浏览器请求之后，会：

`首次访问servlet会在target下生成一个Servlet.class`

![1567913793252](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222224840.png)

> 补充上图箭头流程

![image-20220423112126658](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231121986.png)

### 6.4、Mapping问题

1. 一个Servlet可以指定一个映射路径

   ```xml
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
   ```

2. 一个Servlet可以指定多个映射路径

   ```xml
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello2</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello3</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello4</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello5</url-pattern>
       </servlet-mapping>
   ```

3. 一个Servlet可以指定通用映射路径

   ```xml
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello/*</url-pattern>
       </servlet-mapping>
   ```

4. 默认请求路径

   ```xml
       <!--默认请求路径，这样的话启动就不会走index.jsp了-->
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/*</url-pattern>
       </servlet-mapping>
   ```

5. 指定一些后缀或者前缀等等….

   ```xml
   <!--可以自定义后缀实现请求映射
       注意点，*前面不能加项目映射的路径，就是不能写/*.qinjiang
       hello/sajdlkajda.qinjiang 这种写法也能访问到
       -->
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>*.qinjiang</url-pattern>
   </servlet-mapping>
   ```

6. 优先级问题
   指定了固有的映射路径优先级最高，如果找不到就会走默认的处理请求；

> 再写一个ErrorServlet

```
package com.kwq.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class ErrorServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html");
        resp.setCharacterEncoding("utf-8");
        PrintWriter writer = resp.getWriter();
        writer.print("<h1></h1>");
        writer.print("<h1>404</h1>");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

> 配置映射路径

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         metadata-complete="true">
    <!--注册Servlet-->
    <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>com.kwq.servlet.HelloServlet</servlet-class>
    </servlet>
    <!--Servlet的请求路径-->
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>error</servlet-name>
        <servlet-class>com.kwq.servlet.ErrorServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>error</servlet-name>
        <url-pattern>/*</url-pattern>
    </servlet-mapping>
</web-app>

```

启动：

![image-20220423111220665](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231112907.png)

![image-20220423111710115](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231117345.png)

### 6.5、ServletContext

> 在父工程下新建一个maven的webapp项目：servlet-02

==servlet==

```
package com.kwq.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("hello");
    }


}
```

==配置映射==

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         metadata-complete="true">
    <!--注册Servlet-->
    <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>com.kwq.servlet.HelloServlet</servlet-class>
    </servlet>
    <!--Servlet的请求路径-->
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>
```

==配置tomcat==

`web容器在启动的时候，它会为每个web程序都创建一个对应的ServletContext对象，它代表了当前的web应用；`



#### 1、共享数据

> 原理图

![image-20220423120524486](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231205716.png)

我在这个Servlet中保存的数据，可以在另外一个servlet中拿到；

```java
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        
        //this.getInitParameter()   初始化参数
        //this.getServletConfig()   Servlet配置
        //this.getServletContext()  Servlet上下文
        ServletContext context = this.getServletContext();

        String username = "秦疆"; //数据
        context.setAttribute("username",username); //将一个数据保存在了ServletContext中，名字为：username 。值 username

    }

}
public class GetServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();
        String username = (String) context.getAttribute("username");

        resp.setContentType("text/html");
        resp.setCharacterEncoding("utf-8");//不设置这个的话 页面会出现中文乱码，和上面这个配套的
        resp.getWriter().print("名字"+username);

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
    <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>com.kuang.servlet.HelloServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>


    <servlet>
        <servlet-name>getc</servlet-name>
        <servlet-class>com.kuang.servlet.GetServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>getc</servlet-name>
        <url-pattern>/getc</url-pattern>
    </servlet-mapping>
```

测试访问结果；==需要先访问helloServlet来对username进行赋值==

> 这个是不加GetServlet中不加`resp.setContentType("text/html")的结果：`    你也可以在浏览器中选择编码方式 手动转码（但是用户不会呀）

![image-20220424110619687](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241106811.png)

> 加了以后。

![image-20220423115520723](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231155048.png)

`这样就能做到，某个客户端访问servlet，在servlet上下文中保留数据，然后其他客户端来拿，实现数据共享`

#### 2、获取初始化参数

```
package com.kwq.servlet;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class ServletDemo03 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();
        String url = context.getInitParameter("url");
        resp.getWriter().print(url);

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

> web.xml

```xml
    <!--配置一些web应用初始化参数-->
    <context-param>
        <param-name>url</param-name>
        <param-value>jdbc:mysql://localhost:3306/mybatis</param-value>
    </context-param>
    <servlet>
        <servlet-name>gp</servlet-name>
        <servlet-class>com.kwq.servlet.ServletDemo03</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>gp</servlet-name>
        <url-pattern>/gp</url-pattern>
    </servlet-mapping>
```

> 输出：

![image-20220423144817571](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231448673.png)

#### 3、请求转发

```java
package com.kwq.servlet;

public class ServletDemo04 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();
        System.out.println("进入了ServletDemo04");
        //RequestDispatcher requestDispatcher = context.getRequestDispatcher("/gp"); //转发的请求路径
        //requestDispatcher.forward(req,resp); //调用forward实现请求转发；
        context.getRequestDispatcher("/gp").forward(req,resp);
    }

}

```

> web.xml

```xml
<!--配置一些web应用初始化参数-->
<context-param>
    <param-name>url</param-name>
    <param-value>jdbc:mysql://localhost:3306/mybatis</param-value>
</context-param>
<servlet>
    <servlet-name>gp</servlet-name>
    <servlet-class>com.kwq.servlet.ServletDemo03</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>gp</servlet-name>
    <url-pattern>/gp</url-pattern>
</servlet-mapping>

<servlet>
    <servlet-name>sd4</servlet-name>
    <servlet-class>com.kwq.servlet.ServletDemo04</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>sd4</servlet-name>
    <url-pattern>/sd4</url-pattern>
</servlet-mapping>
```

> 测试，发现转发后，uri没有发生变化，重定向才会发生变化

![image-20220423145509231](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231455417.png)

> 图解：上面是请求转发，下面是重定向  

![1567924457532](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213533253-713150891.png)

==上图 a是浏览器，访问b，b去调用c，然后返回b再返回给a。下图是a访问b，b响应让a去找c，然后c返回a。==

#### 4、读取资源文件

Properties

- 在java目录下新建properties
- 在resources目录下新建properties   内容都是` username=root password=123456`
- ![image-20220423151014801](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231510909.png)

`maven小知识：当我们点击maven的clean，target就会消失,对应的tomcat下webapp对应的web应用也就消失了。然后启动tomcat就会出现，我们的uri访问的都是tomcat的webapp下的web应用资源`

查看发现aa的配置文件没有出现

![image-20220423151112536](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231511643.png)

原因是maven的约定大于配置的原因，导致资源导出失败。

我们在pom添加如下代码即可：

```xml
<!--在build中配置resources，来防止我们资源导出失败的问题-->
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

发现：都被打包到了同一个路径下：target/classes里面，我们俗称这个路径为`classpath`:。

![image-20220423151601552](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231516666.png)

思路：需要一个文件流；

```java
package com.kwq.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

public class ServletDemo05 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
// /代表当前web项目的路径
        InputStream is = this.getServletContext().getResourceAsStream("/WEB-INF/classes/db.properties");

        Properties prop = new Properties();
        prop.load(is);
        String user = prop.getProperty("username");
        String pwd = prop.getProperty("password");

        resp.getWriter().print(user+":"+pwd);

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

> web.xml

```
<servlet>
    <servlet-name>sd5</servlet-name>
    <servlet-class>com.kwq.servlet.ServletDemo05</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>sd5</servlet-name>
    <url-pattern>/sd5</url-pattern>
</servlet-mapping>
```

访问测试即可ok；

![image-20220423152232862](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231522979.png)

![image-20220423203002679](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204232030852.png)

`/拿到的就是tomcat里的web应用的路径，指的是servlet_02_war这个路径。结构和下面的文件上传是一样的。只是这个在tomcat中，在不在tomcat中取决于我们的web目录是否在main文件夹下。在的话tomcat中会有不在的话，在原本项目路径会有。`

![image-20220423203301231](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204232033387.png)

### 6.6、HttpServletResponse

web服务器接收到客户端的http请求，针对这个请求，分别创建一个代表请求的HttpServletRequest对象，代表响应的一个HttpServletResponse；

- 如果要获取客户端请求过来的参数：找HttpServletRequest
- 如果要给客户端响应一些信息：找HttpServletResponse

#### 1、简单分类

负责向浏览器发送数据的方法

```java
ServletOutputStream getOutputStream() throws IOException;
PrintWriter getWriter() throws IOException;
```

负责向浏览器发送响应头的方法

```java
    void setCharacterEncoding(String var1);

    void setContentLength(int var1);

    void setContentLengthLong(long var1);

    void setContentType(String var1);

    void setDateHeader(String var1, long var2);

    void addDateHeader(String var1, long var2);

    void setHeader(String var1, String var2);

    void addHeader(String var1, String var2);

    void setIntHeader(String var1, int var2);

    void addIntHeader(String var1, int var2);
```

响应的状态码

```java
    int SC_CONTINUE = 100;
    int SC_SWITCHING_PROTOCOLS = 101;
    int SC_OK = 200;
    int SC_CREATED = 201;
    int SC_ACCEPTED = 202;
    int SC_NON_AUTHORITATIVE_INFORMATION = 203;
    int SC_NO_CONTENT = 204;
    int SC_RESET_CONTENT = 205;
    int SC_PARTIAL_CONTENT = 206;
    int SC_MULTIPLE_CHOICES = 300;
    int SC_MOVED_PERMANENTLY = 301;
    int SC_MOVED_TEMPORARILY = 302;
    int SC_FOUND = 302;
    int SC_SEE_OTHER = 303;
    int SC_NOT_MODIFIED = 304;
    int SC_USE_PROXY = 305;
    int SC_TEMPORARY_REDIRECT = 307;
    int SC_BAD_REQUEST = 400;
    int SC_UNAUTHORIZED = 401;
    int SC_PAYMENT_REQUIRED = 402;
    int SC_FORBIDDEN = 403;
    int SC_NOT_FOUND = 404;
    int SC_METHOD_NOT_ALLOWED = 405;
    int SC_NOT_ACCEPTABLE = 406;
    int SC_PROXY_AUTHENTICATION_REQUIRED = 407;
    int SC_REQUEST_TIMEOUT = 408;
    int SC_CONFLICT = 409;
    int SC_GONE = 410;
    int SC_LENGTH_REQUIRED = 411;
    int SC_PRECONDITION_FAILED = 412;
    int SC_REQUEST_ENTITY_TOO_LARGE = 413;
    int SC_REQUEST_URI_TOO_LONG = 414;
    int SC_UNSUPPORTED_MEDIA_TYPE = 415;
    int SC_REQUESTED_RANGE_NOT_SATISFIABLE = 416;
    int SC_EXPECTATION_FAILED = 417;
    int SC_INTERNAL_SERVER_ERROR = 500;
    int SC_NOT_IMPLEMENTED = 501;
    int SC_BAD_GATEWAY = 502;
    int SC_SERVICE_UNAVAILABLE = 503;
    int SC_GATEWAY_TIMEOUT = 504;
    int SC_HTTP_VERSION_NOT_SUPPORTED = 505;
```

#### 2、常见应用

1. 向浏览器输出消息 （一直在讲，就不说了）
2. 下载文件
   1. 要获取下载文件的路径
   2. 下载的文件名是啥？
   3. 设置想办法让浏览器能够支持下载我们需要的东西
   4. 获取下载文件的输入流
   5. 创建缓冲区
   6. 获取OutputStream对象
   7. 将FileOutputStream流写入到buffer缓冲区
   8. 使用OutputStream将缓冲区中的数据输出到客户端！

`新建一个maven项目，选择webapp模板，然后我们先放一张图片进入我们当前的web应用:`

![image-20220423161658837](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231616977.png)

![image-20220423161142713](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231611009.png)

`重点：`

ServletContext得到的路径是我们本地tomcat的webapps下对应的文件夹（==这个文件夹就是我们的当前的应用==）路径

![image-20220423161341260](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231613417.png)

![image-20220423161614788](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231616937.png)

`因此我们要修改路径:`

```java
package com.kwq.servlet;

import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.FileInputStream;
import java.io.IOException;
import java.net.URLEncoder;

public class FileServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 1. 要获取下载文件的路径
        String realPath = "D:\\Environment\\tomcat9\\webapps\\ROOT\\WEB-INF\\classes\\1.png";
        System.out.println("下载文件的路径："+realPath);
        // 2. 下载的文件名是啥？
        String fileName = realPath.substring(realPath.lastIndexOf("\\") + 1); //这个\\是转义
        // 3. 设置想办法让浏览器能够支持(Content-Disposition)下载我们需要的东西,中文文件名URLEncoder.encode编码，否则有可能出现中文乱码
        resp.setHeader("Content-Disposition","attachment;filename="+ URLEncoder.encode(fileName,"UTF-8"));
        // 4. 获取下载文件的输入流
        FileInputStream in = new FileInputStream(realPath);
        // 5. 创建缓冲区
        int len = 0;
        byte[] buffer = new byte[1024];
        // 6. 获取OutputStream对象
        ServletOutputStream out = resp.getOutputStream();
        // 7. 将FileOutputStream流写入到buffer缓冲区,使用OutputStream将缓冲区中的数据输出到客户端！
        while ((len=in.read(buffer))>0){
            out.write(buffer,0,len);
        }

        in.close();
        out.close();
    }

}

```

> web.xml

```
<servlet>
    <servlet-name>filedown</servlet-name>
    <servlet-class>com.kwq.servlet.FileServlet</servlet-class>
</servlet>
    
    <servlet-mapping>
        <servlet-name>filedown</servlet-name>
        <url-pattern>/down</url-pattern>
    </servlet-mapping>
```

> 测试：

![image-20220423162218777](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231622881.png)

#### 3、验证码功能

验证怎么来的？

- 可以前端实现
- 也可以后端实现，需要用到 Java 的图片类，生产一个图片

```java
package com.kwq.servlet;

import javax.imageio.ImageIO;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.Random;

public class ImageServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //如何让浏览器3秒自动刷新一次;
        resp.setHeader("refresh","3");
        
        //在内存中创建一个图片
        BufferedImage image = new BufferedImage(80,20,BufferedImage.TYPE_INT_RGB);
        //得到图片
        Graphics2D g = (Graphics2D) image.getGraphics(); //笔
        //设置图片的背景颜色
        g.setColor(Color.white);
        g.fillRect(0,0,80,20);
        //给图片写数据
        g.setColor(Color.BLUE);
        g.setFont(new Font(null,Font.BOLD,20));
        g.drawString(makeNum(),0,20);

        //告诉浏览器，这个请求用图片的方式打开
        resp.setContentType("image/jpeg");
        //网站存在缓存，不让浏览器缓存
        resp.setDateHeader("expires",-1);
        resp.setHeader("Cache-Control","no-cache");
        resp.setHeader("Pragma","no-cache");

        //把图片写给浏览器
        ImageIO.write(image,"jpg", resp.getOutputStream());

    }

    //生成随机数
    private String makeNum(){
        Random random = new Random();
        String num = random.nextInt(9999999) + "";
        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < 7-num.length() ; i++) {
            sb.append("0");
        }
        num = sb.toString() + num;
        return num;
    }


    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

> 小插曲，我商场项目百度生成的验证码，其实和上面这个差不多

![image-20220423165906148](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231659342.png)



> web.xml

```
<servlet>
    <servlet-name>ImageServlet</servlet-name>
    <servlet-class>com.kwq.servlet.ImageServlet</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>ImageServlet</servlet-name>
    <url-pattern>/img</url-pattern>
</servlet-mapping>
```

> 测试：

![image-20220423165511085](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231655250.png)

每隔五秒会自动刷新，==也就是每隔五秒让浏览器重新访问一次localhost:8080/img这个uri==

![image-20220423165536929](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231655042.png)

![image-20220423165635969](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231656119.png)



#### 4、实现重定向

![1567931587955](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213533511-1405171237.png)

B一个web资源收到客户端A请求后，B他会通知A客户端去访问另外一个web资源C，这个过程叫重定向

常见场景：

- 用户登录   （==比如token，我们打开登录页面的uri就直接给我们跳到首页了==）

```java
void sendRedirect(String var1) throws IOException;
```

我们在tomcat里修改路径多加个`/r`，如果我们不加r，那么RedireServlet的重定向直接写`/img`即可

![image-20220423185533769](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231855034.png)

测试：

```java
package com.kwq.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class RedireServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        resp.sendRedirect("/img");//重定向
    }
}

```

> web.xml

```
<servlet>
    <servlet-name>RedireServlet</servlet-name>
    <servlet-class>com.kwq.servlet.RedireServlet</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>RedireServlet</servlet-name>
    <url-pattern>/red</url-pattern>
</servlet-mapping>
```

> 测试访问：==可以发现重定向uri马上发生了变化==

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231858408.png" alt="image-20220423185852301" style="zoom:100%;" />

![image-20220423185902217](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231859344.png)

`我们需要写成这样才不会404`

![image-20220423185940452](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231859534.png)

![image-20220423190233463](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231902562.png)

> 访问：

![image-20220423190212003](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231902087.png)

![image-20220423190200191](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231902342.png)



> 面试题：请你聊聊重定向和转发的区别？

相同点

- 页面都会实现跳转

不同点

- 请求转发的时候，url不会产生变化
- 重定向时候，url地址栏会发生变化；

![1567932163430](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222224798.png)

> 原理剖析  http://localhost:8080/r/red后

![image-20220423190904313](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231909524.png)

```java
resp.setHeader("Location","/r/img");
resp.setStatus(302);  
//前两步等价于
 resp.sendRedirect("/r/img");//重定向*
```

> 将302改为303于是乎

![image-20220423191426816](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231914041.png)

#### 5、简单实现登录重定向

```jsp
package com.kwq.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class RequestTest extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //处理请求    从请求中那参数，我们实际开发项目也这样写
        String username = req.getParameter("username");
        String password = req.getParameter("password");

        System.out.println(username+":"+password);


    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}

```

> web.xml

```java
<servlet>
    <servlet-name>RequestTest</servlet-name>
    <servlet-class>com.kwq.servlet.RequestTest</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>RequestTest</servlet-name>
    <url-pattern>/login</url-pattern>
</servlet-mapping>
```

> index.jsp

```
<html>
<body>
<h2>Hello World!</h2>

<%--这里提交的路径，需要寻找到项目的路径,即：localhost:8080/r--%>
<%--${pageContext.request.contextPath}代表当前的项目--%>

<form action="${pageContext.request.contextPath}/login" method="get">
    用户名：<input type="text" name="username"> <br>
    密码：<input type="password" name="password"> <br>
    <input type="submit">
</form>
</body>
</html>
```

> 一点开发现乱码，法一：

```jsp
<%@ page language="java"  contentType="text/html;charset=UTF-8"%> // index.jsp中添加这个头文件  
```

> 法二：

![image-20220423193621068](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231936158.png)

先去安装一个charset插件,然后选择utf-8

![image-20220423193456808](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231934985.png)

![image-20220423192300584](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231923799.png)

![image-20220423193557146](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231935311.png)

==提交表单，因为是get请求，所以会直接在浏览器展示出来==

![image-20220423194000873](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204231940988.png)

==在webapp下新建success.jsp==

```
<html>
<body>
<h2>title</h2>
<h1>Success</h1>
</body>
</html>
```

![image-20220423200608946](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204232006053.png)

> 测试输出：提交表单后

![image-20220423200520933](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204232005061.png)

### 6.7、HttpServletRequest

HttpServletRequest代表客户端的请求，用户通过Http协议访问服务器，HTTP请求中的所有信息会被封装到HttpServletRequest，通过这个HttpServletRequest的方法，获得客户端的所有信息；

![1567933996830](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213533944-1857016429.png)

![1567934023106](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213534221-80302344.png)

#### 1、获取参数，请求转发

![1567934110794](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204222224020.png)

==新建一个webapp模板的子项目：request==

> servlet

```java
package com.kwq.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Arrays;

public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String[] hobbys = req.getParameterValues("hobbys");
        System.out.println("=============================");
        //后台接收中文乱码问题
        System.out.println(username);
        System.out.println(password);
        System.out.println(Arrays.toString(hobbys));
        System.out.println("=============================");


        System.out.println(req.getContextPath());
        //通过请求转发
        //这里的 / 代表当前的web应用  servletContext也可以实现这个功能
        req.getRequestDispatcher("/success.jsp").forward(req,resp);

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}

```

> web.xml

```xml
<servlet>
    <servlet-name>LoginServlet</servlet-name>
    <servlet-class>com.kwq.servlet.LoginServlet</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>LoginServlet</servlet-name>
    <url-pattern>/login</url-pattern>
</servlet-mapping>
```

> index.jsp

```
<%@ page language="java"  contentType="text/html;charset=UTF-8"%>
<html>
<head>
<title>登录</title>
</head>

<body>
<h1>登录</h1>

<div style="text-align: center">
    <%--这里表单表示的意思：以post方式提交表单，提交到我们的/login--%>
    <form action="${pageContext.request.contextPath}/login" method="post">
        用户名：<input type="text" name="username"> <br>
        密码：<input type="password" name="password"> <br>
        爱好：
        <input type="checkbox" name="hobbys" value="女孩">女孩
        <input type="checkbox" name="hobbys" value="代码">代码
        <input type="checkbox" name="hobbys" value="唱歌">唱歌
        <input type="checkbox" name="hobbys" value="电影">电影
        <br>
        <input type="submit">
    </form>
</div>
</body>
</html>
```

> success.html

```
<%@ page contentType="text/html; utf-8" language="java"%>
<html>
<head>
<title>Title</title>
<body>
<h1>登录成功</h1>
</body>
</head>

</html>
```

> 访问提交：

![image-20220423223214840](../AppData/Roaming/Typora/typora-user-images/image-20220423223214840.png)

![image-20220423225002641](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204232250750.png)

> 控制台输出：乱码

![image-20220423223055647](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204232230045.png)

> 解决方法：

1.先将项目的文件编码都改为utf-8

2.Java编译器加如下代码

![image-20220423223714469](../AppData/Roaming/Typora/typora-user-images/image-20220423223714469.png)

3.tomcat配置vm选项

![image-20220423223616394](../AppData/Roaming/Typora/typora-user-images/image-20220423223616394.png)

==重启tomcat再次，访问后台就正常啦==

![image-20220423224844293](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204232248389.png)

我们还可以设置一个环境变量
变量名`JAVA_TOOL_OPTIONS`
变量值`-Dfile.encoding=UTF-8`。这样我们tomcat里就不用每次加上`-Dfile.encoding=UTF-8`了。删掉刚刚tomcat配置的Vm选项。依旧不会出现乱码问题。

==再测试一下这个重定向==

![image-20220423225335563](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204232253639.png)

servlet

```
package com.kwq.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class testServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html");
        resp.setCharacterEncoding("utf-8");
        PrintWriter writer = resp.getWriter();
        writer.print("<h1></h1>");
        writer.print("<h1>test</h1>");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

web.xml

```
<servlet>
    <servlet-name>testServlet</servlet-name>
    <servlet-class>com.kwq.servlet.testServlet</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>testServlet</servlet-name>
    <url-pattern>/test</url-pattern>
</servlet-mapping>
```

![image-20220423225431146](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204232254216.png)

**面试题：请你聊聊重定向和转发的区别？**

相同点

- 页面都会实现跳转

不同点

- 请求转发的时候，url不会产生变化 307
- 重定向时候，url地址栏会发生变化； 302



## 7、Cookie、Session

### 7.1、会话

**会话**：用户打开一个浏览器，点击了很多超链接，访问多个web资源，关闭浏览器，这个过程可以称之为会话；

**有状态会话**：一个同学来过教室，下次再来教室，我们会知道这个同学，曾经来过，称之为有状态会话；

**你能怎么证明你是西开的学生？**

你 西开

1. 发票 西开给你发票
2. 学校登记 西开标记你来过了

**一个网站，怎么证明你来过？**

客户端（你） 服务端（西开）

1. 服务端给客户端一个 信件，客户端下次访问服务端带上信件就可以了； cookie
2. 服务器登记你来过了，下次你来的时候我来匹配你； seesion

### 7.2、保存会话的两种技术

**cookie**

- 客户端技术，因为cookies保存在浏览器 （`服务端通过响应【response】发给客户端cookie，客户端通过请求【request】带给服务端cookie`）

**session**

- 服务器技术，利用这个技术，可以保存用户的会话信息？ 我们可以把信息或者数据放在Session中！

常见常见：网站登录之后，你下次不用再登录了，第二次访问直接就上去了！

### 7.3、Cookie

![image-20220424122039735](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241220893.png)

新建一个maven项目（`带webapp模板`）：javaweb-session-cookie

> servlet

1. 从请求中拿到cookie信息
2. 服务器响应给客户端cookie

```
package com.kwq.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Date;

// 保存用户上一次访问的时间
public class CookieDemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       //服务器，告诉你 你来的时间，把这个时间封装成一个信件 你下次带来，我就知道你来了

        //解决中文乱码
       req.setCharacterEncoding("utf-8");
       //下面这两句要配合使用，否则下面的out.write（）输出到浏览器页面会出现中文乱码
       resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html");
        PrintWriter out = resp.getWriter();

        //Cookie,服务器端从客户端获取呀
        Cookie[] cookies = req.getCookies();
        //判断Cookies是否存在
        if (cookies!=null){
            //如果存在
            out.write("你上次访问的时间是:");
            for (int i=0;i<cookies.length;i++){
                Cookie cookie = cookies[i];
                //获取cookie的名称
                if (cookie.getName().equals("lastLoginTime")){
                    //获取cookie的值
                    long lastLoginTime = Long.parseLong(cookie.getValue());
                    Date date = new Date(lastLoginTime);
                    out.write(date.toLocaleString());
                }
            }
        }else {
            out.write("这是您第一次访问本站");
        }
        //服务端给客户端响应一个cookie
        Cookie cookie = new Cookie("lastLoginTime", System.currentTimeMillis()+"");
        resp.addCookie(cookie);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

> 请求映射

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         metadata-complete="true">


    <servlet>
        <servlet-name>CookieDemo01</servlet-name>
        <servlet-class>com.kwq.servlet.CookieDemo01</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>CookieDemo01</servlet-name>
        <url-pattern>/c1</url-pattern>
    </servlet-mapping>
</web-app>
```

配置tomcat！

> 测试：

![image-20220424095421266](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204240954882.png)

再访问一次

![image-20220424100022390](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241000792.png)

`此时，我们将浏览器关闭再打开：`

![image-20220424100259318](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241002665.png)

`然后打开浏览器再次访`

![image-20220424100142043](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241001181.png)

`修改代码，多加一步：`

![image-20220424100602977](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241006163.png)

启动访问：

![image-20220424100918264](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241009538.png)

关闭浏览器再访问一次：

![image-20220424101039170](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241010597.png)



**cookie：一般会保存在本地的 用户目录下 appdata；**

一个网站cookie是否存在上限！**聊聊细节问题**

- 一个Cookie只能保存一个信息；
- 一个web站点可以给浏览器发送多个cookie，最多存放20个cookie；
- Cookie大小有限制4kb；
- 300个cookie浏览器上限

**删除Cookie；**

- 不设置有效期，关闭浏览器，自动失效；
- 设置有效期时间为 0 ；

> 有效时间设置为0的cookie 的小Demo

servlet

```
package com.kwq.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class CookieDemo02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //创建一个cookie 名字必须要和删除的名字一致
        Cookie cookie = new Cookie("lastLoginTime", System.currentTimeMillis() + "");
        //将cookie有效期设置为0，立马过期
        cookie.setMaxAge(0);
        resp.addCookie(cookie);
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

资源映射

```
<servlet>
    <servlet-name>CookieDemo02</servlet-name>
    <servlet-class>com.kwq.servlet.CookieDemo02</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>CookieDemo02</servlet-name>
    <url-pattern>/c2</url-pattern>
</servlet-mapping>
```

> 测试：

![image-20220424104022131](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241040517.png)

==一访问/c2：发现cookies就消失了==

![image-20220424104058891](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241040227.png)

还有一点就是，cookies中的value设置为中文。可能会出现乱码。(`下面这张是没有出现乱码的情况·`)

![image-20220424105334221](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241053672.png)

> 如果出现：解决方法如下

**编码解码：**

```java
Cookie cookie=new Cookie("name",URLEncoder.encode("柯维钦","utf-8"))
out.write(URLDecoder.decode(cookie.getValue(),"UTF-8"));
```

### 7.4、Session（重点）

![1568344560794](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213534870-1879002525.png)

什么是Session：

- 服务器会给每一个用户（`浏览器`）创建一个Seesion对象；
- 一个Seesion独占一个浏览器，只要浏览器没有关闭，这个Session就存在；
- 用户登录之后，整个网站它都可以访问！--> 我们可以在服务端用Session`保存用户的信息`；`保存购物车的信息`…..

![1568342773861](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204240913162.png)

Session和cookie的区别：

- Cookie是把用户的数据写给用户的浏览器，浏览器保存 （可以保存多个）
- Session把用户的数据写到用户独占Session中，服务器端保存 （保存重要的信息，减少服务器资源的浪费）
- Session对象由服务创建；

使用场景：

- 保存一个登录用户的信息；
- 购物车信息(`无需登录，也能看到那种`)；
- 在整个网站中经常会使用的数据，我们将它保存在Session中；

> Session技术说明： 下图也说明了客户端并没有报错session，而是保存了cookie，服务端才保存了session

![image-20220424123901450](../AppData/Roaming/Typora/typora-user-images/image-20220424123901450.png)

使用Session：

```java
package com.kuang.servlet;

import com.kuang.pojo.Person;

import javax.servlet.ServletException;
import javax.servlet.http.*;
import java.io.IOException;

public class SessionDemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        
        //解决乱码问题
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html");
        
        //得到Session
        HttpSession session = req.getSession(); 
        //给Session中存东西
        session.setAttribute("name","秦疆");
        //获取Session的ID
        String sessionId = session.getId();

        //判断Session是不是新创建
        if (session.isNew()){
            resp.getWriter().write("session创建成功,ID:"+sessionId);
        }else {
            resp.getWriter().write("session以及在服务器中存在了,ID:"+sessionId);
        }

        //Session创建的时候做了什么事情；
//        Cookie cookie = new Cookie("JSESSIONID",sessionId);
//        resp.addCookie(cookie);

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

> 配置映射

```
<servlet>
    <servlet-name>SessionDemo01</servlet-name>
    <servlet-class>com.kwq.servlet.SessionDemo01</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>SessionDemo01</servlet-name>
    <url-pattern>/se</url-pattern>
</servlet-mapping>
```

> 访问：

![image-20220424114106594](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241141948.png)

`新建servlet`

```
package com.kwq.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

public class SessionDemo02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //解决乱码问题
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html");

        //得到Session
        HttpSession session = req.getSession();

        System.out.println((String) session.getAttribute("name"));


    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

`配置映射`

```
<servlet>
    <servlet-name>SessionDemo02</servlet-name>
    <servlet-class>com.kwq.servlet.SessionDemo02</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>SessionDemo02</servlet-name>
    <url-pattern>/s2</url-pattern>
</servlet-mapping>
```

> 先访问上/s2,再访问/se,再访问/s2，控制台输出如下

![image-20220424114608536](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241146895.png)

`新添加一个类`

```
package com.kwq.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Person {
    private String name;
    private int age;


}
```

`修改第一个/se对应的servlet`

![image-20220424125003576](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241250886.png)

> 先访问上/s2,再访问/se,再访问/s2，控制台输出如下

![image-20220424115634834](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241156954.png)

`新建一个servlet`

```java
package com.kwq.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

public class SessionDemo03 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        HttpSession session = req.getSession();
        session.removeAttribute("name");
        session.invalidate();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

> 配置映射关系

```
<servlet>
    <servlet-name>SessionDemo03</servlet-name>
    <servlet-class>com.kwq.servlet.SessionDemo03</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>SessionDemo03</servlet-name>
    <url-pattern>/s3</url-pattern>
</servlet-mapping>
```

> 测试。下图是浏览器已经访问了两次/se后的截图

![image-20220424120458594](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241204737.png)

==接着访问/s3再访问se，会发现注销后，浏览器会生成一个新的sessionID（也就是cookies中JSESSIONID的值）==   关掉浏览器，再打开也会生成一个新的sessionID，因为关闭浏览器，cookies就没了，也就是JSESSIONID删除了，那么再次访问就无法根据JSESSIONID的值来找到服务端对应保存的session，因此服务端会重新生成一个cookie返回给客户端。这个cookie就是新的`JSESSIONID`。

![image-20220424120529677](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204241205794.png)



**会话自动过期：web.xml配置**

```xml
<!--设置Session默认的失效时间-->
<session-config>
    <!--15分钟后Session自动失效，以分钟为单位-->
    <session-timeout>15</session-timeout>
</session-config>
```

这样的话，服务端创建的session在15min后就会消失。然后客户端再访问，即使携带了sessionID（`JSESSIONID中的值`），也找不到服务端的保存的对应的session，因此服务端会重新生成一个cookie返回给客户端。这个cookie就是新的`JSESSIONID`



![1568344679763](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204240913134.png)

## 8、JSP

### 8.1、什么是JSP

Java Server Pages ： Java服务器端页面，也和Servlet一样，用于动态Web技术！

最大的特点：

- 写JSP就像在写HTML
- 区别：
  - HTML只给用户提供静态的数据
  - JSP页面中可以嵌入JAVA代码，为用户提供动态数据；

### 8.2、JSP原理

思路：JSP到底怎么执行的！

- 代码层面没有任何问题

- 服务器内部工作

  tomcat中有一个work目录；

  ![image-20220424193101052](../AppData/Roaming/Typora/typora-user-images/image-20220424193101052.png)

  IDEA中使用Tomcat会在IDEA的tomcat中生产一个work目录

  ![image-20220424202710567](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204242027788.png)

  我电脑的地址：

  ```java
  C:\Users\19395\AppData\Local\JetBrains\IntelliJIdea2020.3\tomcat\724c3ad5-0cff-420a-b4c5-96345a32fa0f\work\Catalina\localhost\ROOT\org\apache\jsp
  ```

  发现页面（`jsp`）转变成了Java程序！说明jsp本质就是一个类（`Servlet类`）

  `将其删掉，再访问此jsp页面，会重新生成.java->.class`

  ![image-20220424202807996](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204242028129.png)

**浏览器向服务器发送请求，不管访问什么资源，其实都是在访问Servlet！**

JSP最终也会被转换成为一个Java类！

**JSP 本质上就是一个Servlet**

> web项目默认的index.jsp

```
<%@ page language="java"  contentType="text/html;charset=UTF-8"%>
<html>
<body>
<h2>Hello World!</h2>
</body>
</html>
```

> 查看index.jsp.java 

![image-20220424201827784](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204242018150.png)

> 三个方法：

```java
//初始化
  public void _jspInit() {
      
  }
//销毁
  public void _jspDestroy() {
  }
//JSPService
  public void _jspService(.HttpServletRequest request,HttpServletResponse response)
      
```

1. 判断请求 （`可以发现和我们写的servlet是一模一样呀，先去判断是什么请求然后再进入对应的方法`）

![image-20220424201920784](../AppData/Roaming/Typora/typora-user-images/image-20220424201920784.png)

1. 内置一些对象

   ```java
   final javax.servlet.jsp.PageContext pageContext;  //页面上下文
   javax.servlet.http.HttpSession session = null;    //session
   final javax.servlet.ServletContext application;   //applicationContext
   final javax.servlet.ServletConfig config;         //config
   javax.servlet.jsp.JspWriter out = null;           //out
   final java.lang.Object page = this;               //page：当前
   HttpServletRequest request                        //请求
   HttpServletResponse response                      //响应
   ```

2. 输出页面前增加的代码

   ```java
   response.setContentType("text/html");       //设置响应的页面类型
   pageContext = _jspxFactory.getPageContext(this, request, response,
                                             null, true, 8192, true);
   _jspx_page_context = pageContext;
   application = pageContext.getServletContext();
   config = pageContext.getServletConfig();
   session = pageContext.getSession();
   out = pageContext.getOut();
   _jspx_out = out;
   ```

3. 以上的这些个对象我们可以在JSP页面中直接使用！

![1568347078207](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213535946-443756809.png)

在JSP页面中；

只要是 JAVA代码就会原封不动的输出；

如果是HTML代码，就会被转换为：

```java
out.write("<html>\r\n");
```

这样的格式，输出到前端！

> 演示

修改默认的index.jsp页面

```java
<%@ page language="java"  contentType="text/html;charset=UTF-8"%>
<html>
<body>
<%
    String name="qinjiang";
%>
</body>
</html>
```

==查看.java文件==

![image-20220424203630923](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204242036183.png)



### 8.3、JSP基础语法

> 新建一个普通的maven项目

项目结构：

![image-20220424204056362](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204242040501.png)

> 添加框架支持

![image-20220424204136923](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204242041094.png)

> 再次查看项目结构

![image-20220424204201520](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204242042732.png)

==**我们的webapp原本是在main文件夹里面的，用这种方式是在外面的。但是这样也可以写web应用。推荐还是直接选择webapp的模板来创建maven项目。这样比较规范**==

> 导入依赖

```

<dependencies>
    <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/jsp-api -->
    <!--servlet的依赖-->
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.2</version>
        <scope>provided</scope>
    </dependency>
    <!--jsp的依赖-->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/javax.servlet.jsp.jstl/jstl-api -->
    <!--JSTL表达式的依赖-->
    <dependency>
        <groupId>javax.servlet.jsp.jstl</groupId>
        <artifactId>jstl-api</artifactId>
        <version>1.2</version>
    </dependency>
<!--standard标签库的依赖-->
    <!-- https://mvnrepository.com/artifact/taglibs/standard -->
    <dependency>
        <groupId>taglibs</groupId>
        <artifactId>standard</artifactId>
        <version>1.1.2</version>
    </dependency>
</dependencies>
```

任何语言都有自己的语法，JAVA中有。 JSP 作为java技术的一种应用，它拥有一些自己扩充的语法（了解，知道即可！），Java所有语法都jsp支持！

#### **JSP表达式**

```jsp
index.jsp页面
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <%--JSP表达式
作用：用来将程序的输出，输出到客户端
<%= 变量或者表达式%>
--%>
  <%= new java.util.Date()%>
  </body>
</html>

```

> 启动tomcat

![image-20220424210031247](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204242100448.png)

> jsp对应的servlet

![image-20220424210234178](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204242102498.png)

#### **jsp脚本片段**

```jsp
index.jsp页面
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <%--JSP表达式
作用：用来将程序的输出，输出到客户端
<%= 变量或者表达式%>
--%>
  <%= new java.util.Date()%>

  <%--jsp脚本片段--%>
  <%
    int sum = 0;
    for (int i = 1; i <=100 ; i++) {
      sum+=i;
    }
    /*内置对象out可以直接使用*/
    out.println("<h1>Sum="+sum+"</h1>");
  %>
  </body>
</html>

```

![image-20220424210443170](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204242104304.png)

**脚本片段的再实现**

```jsp
index.jsp页面
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <%--JSP表达式
作用：用来将程序的输出，输出到客户端
<%= 变量或者表达式%>
--%>
  <%= new java.util.Date()%>

  <%--jsp脚本片段--%>
  <%
    int sum = 0;
    for (int i = 1; i <=100 ; i++) {
      sum+=i;
    }
    /*内置对象out可以直接使用*/
    out.println("<h1>Sum="+sum+"</h1>");
  %>

  <%
    int x = 10;
    out.println(x);
  %>
  <p>这是一个JSP文档</p>
  <%
    int y = 2;
    out.println(y);
  %>

  <hr>


  <%--在代码嵌入HTML元素--%>
  <%
    for (int i = 0; i < 5; i++) {
  %>
  <h1>Hello,World  <%=i%> </h1>   
      
  <%
    }
  %>
  </body>
</html>

```

![image-20220424210725498](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204242107721.png)

==**上面对应的变量和代码都是一个方法内的局部变量和普通代码**==

![image-20220425093005526](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250930626.png)

`那我们要定义一个全局变量或者方法怎么实现，JSP声明就出来了`

#### JSP声明

```jsp
  <%!
    static {
      System.out.println("Loading Servlet!");
    }

    private int globalVar = 0;

    public void kuang(){
      System.out.println("进入了方法Kuang！");
    }
  %>
```

![image-20220425093017519](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250930752.png)

==JSP声明：会被编译到JSP生成Java的类中！其他的，就会被生成到_jspService方法中！==

在JSP，嵌入Java代码即可！

```jsp
<%%>
<%=%>
<%!%>

<%--注释--%>
```

JSP的注释，不会在客户端显示，HTML就会！

> 测试

index.jsp新加两行代码

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250930073.png" alt="image-20220425093028902" style="zoom:67%;" />

查看网页源代码：

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250930593.png" alt="image-20220425093044450" style="zoom: 67%;" />

### 8.4、JSP指令

结构：

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250930205.png" alt="image-20220425093056000" style="zoom: 50%;" />

jsp2.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

<%--定制错误页面--%>
<%@ page errorPage="error/500.jsp" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%
    int x=1/0;
%>
</body>
</html>
```

500.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>自定义500错误的页面</h1>
</body>
</html>
```

访问：

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250931046.png" alt="image-20220425093107937" style="zoom:67%;" />

项目结构： 多了一个img目录，但是这个目录得标记为`资源目录` 不然浏览器中看不到图片

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250931830.png" alt="image-20220425093120650" style="zoom:50%;" />

500.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<img src="../img/500.png" alt="500">
</body>
</html>
```

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250931437.png" alt="image-20220425093138211" style="zoom:50%;" />

项目结构：

<img src="../AppData/Roaming/Typora/typora-user-images/image-20220425093201622.png" alt="image-20220425093201622" style="zoom:50%;" />

404.jsp

```
<%--
  Created by IntelliJ IDEA.
  User: 19395
  Date: 2022/4/24
  Time: 22:23
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<img src="../img/404.png" alt="404">
</body>
</html>
```

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250932616.png" alt="image-20220425093225325" style="zoom:50%;" />

下面这样写也ok 就不用在jsp页面写`<img>标签`

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250932715.png" alt="image-20220425093241509" style="zoom:67%;" />

结构：

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250929573.png" alt="image-20220425092938342" style="zoom: 67%;" />

footer.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<h1>我是Footer</h1>
```

header.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<h1>我是Header</h1>
```

jsp3.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
 <%@include file="common/header.jsp" %>
    <h1>网页主体</h1>
<%@include file="common/footer.jsp"%>
</body>
</html>
```

访问：

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250928144.png" alt="image-20220425092857032" style="zoom:67%;" />

修改jsp3.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
 <%@include file="common/header.jsp" %>
    <h1>网页主体</h1>
<%@include file="common/footer.jsp"%>

<hr>

<%--jsp标签--%>
<jsp:include page="common/header.jsp"/>
 <h1>网页主体</h1>
<jsp:include page="common/footer.jsp"/>
</body>
</html>
```

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250928425.png" alt="image-20220425092844156" style="zoom:67%;" />

探究源码：

![image-20220425092821060](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250928246.png)

![image-20220425092801186](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250928389.png)

```jsp
<%@page args.... %>
<%@include file=""%>

<%--@include会将两个页面合二为一--%>

<%@include file="common/header.jsp"%>
<h1>网页主体</h1>

<%@include file="common/footer.jsp"%>

<hr>

<%--jSP标签
    jsp:include：拼接页面，本质还是三个
    --%>
<jsp:include page="/common/header.jsp"/>
<h1>网页主体</h1>
<jsp:include page="/common/footer.jsp"/>
```

==补充一个小常识：WEB-INF文件下下的资源，客户端不可见== 这是因为maven的约定大于配置



### 8.5、9大内置对象

- PageContext 存东西
- Request 存东西
- Response
- Session 存东西
- Application 【SerlvetContext】 存东西
- config 【SerlvetConfig】
- out
- page ，不用了解
- exception

```java
pageContext.setAttribute("name1","秦疆1号"); //保存的数据只在一个页面中有效
request.setAttribute("name2","秦疆2号"); //保存的数据只在一次请求中有效，请求转发会携带这个数据
session.setAttribute("name3","秦疆3号"); //保存的数据只在一次会话中有效，从打开浏览器到关闭浏览器
application.setAttribute("name4","秦疆4号");  //保存的数据只在服务器中有效，从打开服务器到关闭服务器
```

结构：

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250927616.png" alt="image-20220425092739343" style="zoom:67%;" />

添加jsp页面：

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
        <%--内置对象--%>
<%
    pageContext.setAttribute("name1","秦疆1号");//保存的数据只在一个页面中有效
    request.setAttribute("name2","秦疆2号");//保存的数据只在一次请求中有效，请求转发会携带这个数据
    session.setAttribute("name3","秦疆3号");//保存的数据只在一次会话中有效，从打开浏览器到关闭浏览器
    application.setAttribute("name4","秦疆4号");//保存的数据只在服务器中有效，从打开服务器到关闭服务器 application其实就是ServletContext
%>
</body>

<%--脚本片段中的代码，会被原封不动的生成到.jsp.java
要求：这里面的代码：必须保证java语法的正确性--%>

<%
    //从pageContext取出，我们通过寻找的方式来
    //从底层到高层（作用域） 先去pageContext找 找不到去request 再找不到去session，最后去application找 找到的话即刻返回
    String name1 = (String)pageContext.findAttribute("name1");
    String name2 =(String) pageContext.findAttribute("name2");
    String name3 = (String)pageContext.findAttribute("name3");
    String name4 =(String) pageContext.findAttribute("name4");
    String name5 =(String) pageContext.findAttribute("name5");

%>

<%--使用EL表达式输出 ${}--%>
<h1>取出的值为：</h1>
<h3>${name1}</h3>
<h3>${name2}</h3>
<h3>${name3}</h3>
<h3>${name4}</h3>
<h3>${name5}</h3>
</html>
```

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250927016.png" alt="image-20220425092724773" style="zoom:67%;" />

修改：

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250927343.png" alt="image-20220425092700180" style="zoom:67%;" />

发现会有个null

<img src="https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250926774.png" alt="image-20220425092635526" style="zoom:67%;" />

新加个页面

![image-20220425092621707](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250926851.png)

jsp页面内容：

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>


<%--脚本片段中的代码，会被原封不动的生成到.jsp.java
要求：这里面的代码：必须保证java语法的正确性--%>

<%
    //从pageContext取出，我们通过寻找的方式来
    //从底层到高层（作用域）
    String name1 = (String)pageContext.findAttribute("name1");
    String name2 =(String) pageContext.findAttribute("name2");
    String name3 = (String)pageContext.findAttribute("name3");
    String name4 =(String) pageContext.findAttribute("name4");
    String name5 =(String) pageContext.findAttribute("name5");

%>

<%--使用EL表达式输出 ${}--%>
<h1>取出的值为：</h1>
<h3>${name1}</h3>
<h3>${name2}</h3>
<h3>${name3}</h3>
<h3>${name4}</h3>

<h3><%=name5%></h3>
</body>
</html>
```

依次访问：

![image-20220425092555644](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250925883.png)



> 扩展

```jsp
<%--
  Created by IntelliJ IDEA.
  User: 19395
  Date: 2022/4/25
  Time: 0:15
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%--   查看源码
    public void setAttribute(String name, Object attribute, int scope) {
        switch(scope) {
        case 1:
            this.mPage.put(name, attribute);
            break;
        case 2:
            this.mRequest.put(name, attribute);
            break;
        case 3:
            this.mSession.put(name, attribute);
            break;
        case 4:
            this.mApp.put(name, attribute);
            break;
        default:
            throw new IllegalArgumentException("Bad scope " + scope);
        }
--%>
<%
    pageContext.setAttribute("hello","hello1",PageContext.SESSION_SCOPE);
    //上面等价于， session.setAttribute("hello","hello1")
%>
</body>
</html>

```

加一个jsp页面

![image-20220425092540365](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250925542.png)

内容：

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%
    pageContext.forward("/index.jsp");
    //等价于
    //request.getRequestDispatcher("/index.jsp").forward(request,response);
%>
</body>
</html>

```

访问跳转到了index.jsp页面

![image-20220425092507958](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250925084.png)

request：客户端向服务器发送请求，产生的数据，用户看完就没用了，比如：新闻，用户看完没用的！

session：客户端向服务器发送请求，产生的数据，用户用完一会还有用，比如：购物车；

application：客户端向服务器发送请求，产生的数据，一个用户用完了，其他用户还可能使用，比如：聊天数据；

### 8.6、JSP标签、JSTL标签、EL表达式

```xml
<!-- JSTL表达式的依赖 -->
<dependency>
    <groupId>javax.servlet.jsp.jstl</groupId>
    <artifactId>jstl-api</artifactId>
    <version>1.2</version>
</dependency>
<!-- standard标签库 -->
<dependency>
    <groupId>taglibs</groupId>
    <artifactId>standard</artifactId>
    <version>1.1.2</version>
</dependency>
```

EL表达式： ${ }

- **获取数据**
- **执行运算**
- **获取web开发的常用对象**（`内置对象`）

**JSP标签**

在web文件夹下新建jsptag.jsp和jsptag2.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>1</h1>
    <%--
http://localhost:8080/jsptag.jsp?name=kuangshen&age=12
--%>
<jsp:forward page="/jsptag2.jsp">
    <jsp:param name="name" value="kuangshen"></jsp:param>
    <jsp:param name="age" value="12"></jsp:param>
</jsp:forward>
</body>
</html>

```

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>2</h1>
<%--取出参数--%>
名字：<%=request.getParameter("name")%>
年龄：<%=request.getParameter("age")%>
</body>
</html>

```

测试:

![image-20220425103519127](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251035263.png)



**JSTL表达式**

JSTL标签库的使用就是为了弥补HTML标签的不足；它自定义许多标签，可以供我们使用，标签的功能和Java代码一样！

**格式化标签**(要用自行百度查看用法)

**SQL标签**(要用自行百度查看用法)

**XML 标签(要用自行百度查看用法)**

**核心标签** （掌握部分）

![1568362473764](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204250953774.png)

**JSTL标签库使用步骤**

- 引入对应的 taglib
- 使用其中的方法
- **在Tomcat 也需要引入 jstl的包，否则会报错：JSTL解析错误**

> c：if   在web目录下新建一个coreif.jsp,内容如下

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<head>
    <title>Title</title>
</head>
<body>


<h4>if测试</h4>

<hr>

<form action="coreif.jsp" method="get">
    <%--
    EL表达式获取表单中的数据
    ${param.参数名}
    --%>
    <input type="text" name="username" value="${param.username}">
    <input type="submit" value="登录">
</form>

<%--判断如果提交的用户名是管理员，则登录成功--%>
<c:if test="${param.username=='admin'}" var="isAdmin">
    <c:out value="管理员欢迎您！"/>
</c:if>

<%--自闭合标签--%>
<c:out value="${isAdmin}"/>

</body>

```

将两个jar添加进tomcat的lib目录

![image-20220425104858824](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251048982.png)

> 测试：

![image-20220425104919002](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251049192.png)



![image-20220425104935885](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251049014.png)

> c:choose c:when

在web目录下新建一个corewhen.jsp，内容如下

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<%--定义一个变量score，值为85--%>
<c:set var="score" value="55"/>

<c:choose>
    <c:when test="${score>=90}">
        你的成绩为优秀
    </c:when>
    <c:when test="${score>=80}">
        你的成绩为一般
    </c:when>
    <c:when test="${score>=70}">
        你的成绩为良好
    </c:when>
    <c:when test="${score<=60}">
        你的成绩为不及格
    </c:when>
</c:choose>

</body>
</html>

```

![image-20220425105441869](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251054070.png)





> c:foreach

在web目录下新建一个foreach.jsp

```jsp
<%@ page import="java.util.ArrayList" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%

    ArrayList<String> people = new ArrayList<>();
    people.add(0,"张三");
    people.add(1,"李四");
    people.add(2,"王五");
    people.add(3,"赵六");
    people.add(4,"田六");
    request.setAttribute("list",people);
%>


<%--
var , 每一次遍历出来的变量
items, 要遍历的对象
begin,   哪里开始
end,     到哪里
step,   步长
--%>
<c:forEach var="people" items="${list}">
    <c:out value="${people}"/> <br>
</c:forEach>

<hr>

<c:forEach var="people" items="${list}" begin="1" end="3" step="1" >
    <c:out value="${people}"/> <br>
</c:forEach>
</body>
</html>

```

![image-20220425105808426](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251058634.png)

扩展：

如果把这个页面放到`WEB-INF`目录下：

![image-20220425110330577](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251103725.png)

`会发现访问不到   因为WEB-INF对用户是不可见的`

![image-20220425110440345](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251104825.png)



## 9、JavaBean

实体类

JavaBean有特定的写法：

- 必须要有一个无参构造
- 属性必须私有化
- 必须有对应的get/set方法；

一般用来和数据库的字段做映射 ORM；

ORM ：对象关系映射

- 表--->类
- 字段-->属性
- 行记录---->对象

**people表**

| id   | name    | age  | address |
| ---- | ------- | ---- | ------- |
| 1    | 秦疆1号 | 3    | 西安    |
| 2    | 秦疆2号 | 18   | 西安    |
| 3    | 秦疆3号 | 100  | 西安    |

```java
package com.kwq.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
class People{
    private int id;
    private String name;
    private int age;
    private String address;
}

```

建库建表

![image-20220425135157968](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251351242.png)

在web目录下新建一个javaBean.jsp

```
<%@ page import="com.kwq.pojo.People" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<%
    //    People people = new People();
//    people.setId(1);
//    people.setName("张三");
//    people.setAge(19);
//    people.setAddress("广西");   
//这些写法等价于下面的写法
%>
<jsp:useBean id="people" class="com.kwq.pojo.People"/>
<jsp:setProperty name="people" property="address" value="广西"/>
<jsp:setProperty name="people" property="id" value="1"/>
<jsp:setProperty name="people" property="age" value="19"/>
<jsp:setProperty name="people" property="name" value="张三"/>

姓名：<jsp:getProperty name="people" property="name"/>
id：<jsp:getProperty name="people" property="id"/>
年龄：<jsp:getProperty name="people" property="age"/>
地址：<jsp:getProperty name="people" property="address"/>
</body>
</html>
```

输出：

![image-20220425135815509](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251358615.png)

## 10、MVC三层架构

什么是MVC： Model view Controller 模型（`实体类`）、视图（`jsp页面`）、控制器（`servlet`）

### 10.1、早些年

![1568423664332](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251105490.png)

用户直接访问控制层，控制层就可以直接操作数据库；但是这样层次不太清晰，代码会十分臃肿

```java
servlet--CRUD-->数据库
弊端：程序十分臃肿，不利于维护  
servlet的代码中：处理请求、响应、视图跳转、处理JDBC、处理业务代码、处理逻辑代码   因此加个service和dao层

架构：没有什么是加一层解决不了的！
程序猿调用
|
JDBC
|
Mysql Oracle SqlServer ....
```

### 10.2、MVC三层架构

![1568424227281](https://img2020.cnblogs.com/blog/2117227/202107/2117227-20210713213536769-1967925380.png)

Model

- 业务处理 ：业务逻辑（Service）
- 数据持久层：CRUD （Dao）

View

- 展示数据
- 提供链接发起Servlet请求 （a，form，img…）

Controller （Servlet）

- 接收用户的请求 ：（req：请求参数、Session信息….）

- 交给业务层处理对应的代码

- 控制视图的跳转

  ```java
  登录--->接收用户的登录请求--->处理用户的请求（获取用户登录的参数，username，password）---->交给业务层处理登录业务（判断用户名密码是否正确：事务）--->Dao层查询用户名和密码是否正确-->数据库
  ```



## 11、Filter （重点）

比如 Shiro安全框架技术就是用Filter来实现的

Filter：过滤器 ，用来过滤网站的数据；

- 处理中文乱码(`这样就不用每次都在servlet设置resp的字符集了`)
- 登录验证（`session`）….

==其实就是加一层的思想==

（比如用来过滤网上骂人的话，我***我自己 0-0）  

![10](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251456189.png)

Filter开发步骤：

1. 导包
2. 编写过滤器
   1. 导包不要错 **（注意）**

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251600766.png)

新建一个空的maven项目（`不选模板`）:javaweb-filter

先添加框架支持

> 导入依赖

```
    <dependencies>
        <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/jsp-api -->
        <!--servlet的依赖-->
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
            <scope>provided</scope>
        </dependency>
        <!--jsp的依赖-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/javax.servlet.jsp.jstl/jstl-api -->
        <!--JSTL表达式的依赖-->
        <!-- https://mvnrepository.com/artifact/javax.servlet.jsp.jstl/jstl-api -->
        <dependency>
            <groupId>javax.servlet.jsp.jstl</groupId>
            <artifactId>jstl-api</artifactId>
            <version>1.2</version>
        </dependency>

        <!--standard标签库的依赖-->
        <!-- https://mvnrepository.com/artifact/taglibs/standard -->
        <dependency>
            <groupId>taglibs</groupId>
            <artifactId>standard</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>RELEASE</version>
            <scope>compile</scope>
        </dependency>
    <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    <!--连接数据库-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.47</version>
    </dependency>
    </dependencies>

```

servlet并且注册路径配置

```
package com.kwq.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class ShowServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       resp.getWriter().write("你好呀，世界");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

```
    <servlet>
        <servlet-name>ShowServlet</servlet-name>
        <servlet-class>com.kwq.servlet.ShowServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>ShowServlet</servlet-name>
        <url-pattern>/servlet/show</url-pattern>
    </servlet-mapping>

    <servlet-mapping>
        <servlet-name>ShowServlet</servlet-name>
        <url-pattern>/show</url-pattern>
    </servlet-mapping>
```

![image-20220425154538982](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251545517.png)

发现响应会出乱码。

实现Filter接口，重写对应的方法即可😁

```java
package com.shen.filter;

import javax.servlet.*;
import java.io.IOException;

public class CharacterEncodingFilter implements Filter {
    //初始化:web服务器启动，就已经初始化了，随时等待过滤对象出现！
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("CharacterEncodingFilter初始化");
    }
    //chain :链
    /*
    1.过滤器中所有代码，在过滤特定请求的时候都会执行
    2.必须要让过滤器继续同行
     */
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain filterChain) throws IOException, ServletException {
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        response.setContentType("text/html;charset=UTF-8");

        System.out.println("CharacterEncodingFilter执行前....");
        filterChain.doFilter(request,response);//让我们的请求继续走，如果不写，程序到这里就被拦截停止了
        System.out.println("CharacterEncodingFilter执行后....");
    }
    //销毁：web服务器关闭的时候，过滤器会被销毁
    @Override
    public void destroy() {
        System.out.println("CharacterEncodingFilter销毁");
    }
}
```

1. 在web.xml中配置 Filter

```java
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>com.shen.filter.CharacterEncodingFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <!--只要是servlet的任何请求，会经过这个过滤器-->
        <url-pattern>/servlet/*</url-pattern>
        <!--<url-pattern>/*</url-pattern>-->
    </filter-mapping>
```

启动tomcat

![image-20220425154950525](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251549770.png)

输入uri后

![image-20220425155026247](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251550395.png)

控制台：

![image-20220425155040562](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251550670.png)

再次输入uri，很明显没有走过滤器。这是因为我们web.xml没有疲累到`/show`

![image-20220425155109771](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251551952.png)



## 12、监听器

实现一个监听器的接口；（有n种监听器）

1. 编写一个监听器

   实现监听器的接口…

```java
package com.kwq.listen;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

//统计网站在线人数：统计session
public class OnlineCountListener implements HttpSessionListener {
    //创建session监听:看你的一举一动
    //一旦创建session就会触发这个事件！
    @Override
    public void sessionCreated(HttpSessionEvent se) {
        System.out.println(se.getSession().getId());
        ServletContext ctx = se.getSession().getServletContext();
        Integer onlineCount = (Integer) ctx.getAttribute("OnlineCount");
        if (onlineCount==null){
            onlineCount=new Integer(1);
        }else{
            int count = onlineCount.intValue();
            onlineCount = new Integer(count+1);
        }
        ctx.setAttribute("OnlineCount",onlineCount);
    }
    //销毁session监听
    //一旦销毁session就会触发这个事件！
    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        ServletContext ctx = se.getSession().getServletContext();
        Integer onlineCount = (Integer) ctx.getAttribute("OnlineCount");
        if (onlineCount==null){
            onlineCount=new Integer(0);
        }else{
            int count = onlineCount.intValue();
            onlineCount = new Integer(count-1);
        }
        ctx.setAttribute("OnlineCount",onlineCount);
    }
    /*
    Session销毁：
    1. 手动销毁     getSession().invalidate();
    2. 自动销毁     在web.xml中设置
    */
}

```

1. web.xml中注册监听器

```java
   <!--注册监听器-->
    <listener>
        <listener-class>com.kwq.listen.OnlineCountListener</listener-class>
    </listener>
```

修改index.jsp页面

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <h1>当前有 <span><%=this.getServletConfig().getServletContext().getAttribute("OnlineCount")%></span>人在线</h1>
  </body>
</html>
```

> 访问uri

![image-20220425163823856](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251638045.png)

前两个的tomcat启动时的问题 所以打印了三次（说明访问了三次）

![image-20220425163756794](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251637046.png)

重新部署下

![image-20220425163909676](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251639843.png)

再次访问：

![image-20220425163933100](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251639407.png)

## 13、过滤器、监听器常见应用

**监听器：GUI编程中经常使用；**

```java
package com.kwq.listen;

import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class TestPanel {
    public static void main(String[] args) {
        Frame frame = new Frame("中秋节快乐"); //新建一个窗体
        Panel panel = new Panel(null); //面板
        frame.setLayout(null);//设置窗体的布局

        frame.setBounds(300,300,500,500);
        frame.setBackground(new Color(0,0,255));//设置背景颜色
        panel.setBounds(50,50,300,300);
        panel.setBackground(new Color(0,255,0));//设置背景颜色
        frame.add(panel);

        frame.setVisible(true);

        //监听事件：监听关闭事件
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.out.println("监听到关闭窗口事件");
                System.exit(0);
            }
        });
    }
}

```

测试

![image-20220425165451706](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251654135.png)

点击右上角的X

![image-20220425165518866](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204251655017.png)



**过滤器实现权限：**

用户登录之后才能进入主页！用户注销后就不能进入成功主页了！

1. 用户登录之后，向Sesison中放入用户的数据
2. 进入主页的时候要判断用户是否已经登录；要求：在过滤器中实现！

结构：

![image-20220425200707614](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252007735.png)

> servlet

```
package com.kwq.servlet;

import com.kwq.util.Constant;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取前端请求的参数
        String username = req.getParameter("username");
        if (username.equals("admin")){
            //登录成功
            req.getSession().setAttribute(Constant.USER_SESSION,req.getSession().getId());
            resp.sendRedirect("/sys/success.jsp");
        }else {
            //登录失败
            resp.sendRedirect("/error.jsp");
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

Login.jsp

```
<%--
  Created by IntelliJ IDEA.
  User: 19395
  Date: 2022/4/25
  Time: 19:27
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>登录</h1>
<form action="/servlet/login" method="get">
    <input type="text" name="username">
    <input type="submit">
</form>
</body>
</html>
```

error.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>错误</h1>
<h3>没有权限，用户名错误</h3>
</body>
</html>
```

success.jsp

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>主页</h1>
</body>
</html>
```

配置资源映射

```java
<servlet>
    <servlet-name>LoginServlet</servlet-name>
    <servlet-class>com.kwq.servlet.LoginServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>LoginServlet</servlet-name>
    <url-pattern>/servlet/login</url-pattern>
</servlet-mapping>
```

> 测试

![image-20220425200523096](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252005229.png)

![image-20220425200510737](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252005978.png)

`输入admin`

![image-20220425200534430](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252005555.png)

添加servlet

```java
package com.kwq.servlet;

import com.kwq.util.Constant;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class LogoutServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Object user_session = req.getSession().getAttribute(Constant.USER_SESSION);
        if (user_session!=null){
            req.getSession().removeAttribute(Constant.USER_SESSION);
            resp.sendRedirect("/Login.jsp");
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}

```

配置资源映射

```
<servlet>
    <servlet-name>LogoutServlet</servlet-name>
    <servlet-class>com.kwq.servlet.LogoutServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>LogoutServlet</servlet-name>
    <url-pattern>/servlet/logout</url-pattern>
</servlet-mapping>
```

修改error.jsp

![image-20220425202059757](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252020917.png)

修改success.jsp

![image-20220425202125047](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252021291.png)

访问填写错误的用户名，然后跳到下面页面

![image-20220425201232745](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252012864.png)

紧接着调到登录页面

![image-20220425201303475](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252013600.png)

成功登录进去

![image-20220425201326941](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252013060.png)

注销后显示页面

![image-20220425202007840](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252020078.png)

添加过滤器

```java
package com.kwq.filter;

import com.kwq.util.Constant;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.FileFilter;
import java.io.IOException;

public class SysFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;

        if (request.getSession().getAttribute(Constant.USER_SESSION)==null){
            response.sendRedirect("/error.jsp");
        }

        filterChain.doFilter(request,response);
    }

    @Override
    public void destroy() {

    }
}
```

配置

```xml
<filter>
    <filter-name>SysFilter</filter-name>
    <filter-class>com.kwq.filter.SysFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>SysFilter</filter-name>
    <url-pattern>/sys/*</url-pattern>
</filter-mapping>
```



## 14、JDBC

### 什么是JDBC ： Java连接数据库！

![[(img-rZzTXmtn-1588757845422)(JavaWeb.assets/1568439601825.png)]](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252048086.png)

### 需要jar包的支持：

- java.sql
- javax.sql
- mysql-conneter-java… 连接驱动（必须要导入）

### **实验环境搭建**

新建一个不带maven模板的maven项目：`javaweb-jdbc`

```sql
新建一个数据库：名称为jdbc
CREATE TABLE users(
    id INT PRIMARY KEY,
    `name` VARCHAR(40),
    `password` VARCHAR(40),
    email VARCHAR(60),
    birthday DATE
);

INSERT INTO users(id,`name`,`password`,email,birthday)
VALUES(1,'张三','123456','zs@qq.com','2000-01-01');
INSERT INTO users(id,`name`,`password`,email,birthday)
VALUES(2,'李四','123456','ls@qq.com','2000-01-01');
INSERT INTO users(id,`name`,`password`,email,birthday)
VALUES(3,'王五','123456','ww@qq.com','2000-01-01');


SELECT	* FROM users;
```

### 导入数据库依赖

```xml
<!--mysql的驱动-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>
```

### **JDBC 固定步骤：**

1. 加载驱动
2. 连接数据库,代表数据库
3. 向数据库发送SQL的对象Statement : CRUD
4. 编写SQL （根据业务，不同的SQL）
5. 执行SQL
6. 关闭连接（先开的后关）

```java
package com.kwq;

import java.sql.*;

public class TestJdbc {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //配置信息
        //useUnicode=true&characterEncoding=utf-8 解决中文乱码
        String url="jdbc:mysql://127.0.0.1:3306/jdbc?useSSL=false&useUnicode=true&characterEncoding=utf8";
        String username = "root";
        String password = "123123";

        //1.加载驱动
        Class.forName("com.mysql.jdbc.Driver");
        //2.连接数据库,代表数据库
        Connection connection = DriverManager.getConnection(url, username, password);

        //3.向数据库发送SQL的对象Statement,PreparedStatement : CRUD
        Statement statement = connection.createStatement();

        //4.编写SQL
        String sql = "select * from users";

        //5.执行查询SQL，返回一个 ResultSet  ： 结果集
        ResultSet rs = statement.executeQuery(sql);

        while (rs.next()){
            System.out.println("id="+rs.getObject("id"));
            System.out.println("name="+rs.getObject("name"));
            System.out.println("password="+rs.getObject("password"));
            System.out.println("email="+rs.getObject("email"));
            System.out.println("birthday="+rs.getObject("birthday"));
        }

        //6.关闭连接，释放资源（一定要做） 先开后关
        rs.close();
        statement.close();
        connection.close();
    }
}

```

> 输出：

![image-20220425212632460](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252126611.png)

### **预编译SQL**

```java
package com.kwq;

import java.sql.Connection;
import java.sql.Date;
import java.sql.DriverManager;
import java.sql.PreparedStatement;

public class TestJDBC2 {
    public static void main(String[] args) throws Exception {
        //配置信息
        //useUnicode=true&characterEncoding=utf-8 解决中文乱码
        String url="jdbc:mysql://127.0.0.1:3306/jdbc?useSSL=false&useUnicode=true&characterEncoding=utf8";
        String username = "root";
        String password = "123123";

        //1.加载驱动
        Class.forName("com.mysql.jdbc.Driver");
        //2.连接数据库,代表数据库
        Connection connection = DriverManager.getConnection(url, username, password);

        //3.编写SQL
        String sql = "insert into  users(id, name, password, email, birthday) values (?,?,?,?,?);";

        //4.预编译
        PreparedStatement preparedStatement = connection.prepareStatement(sql);

        preparedStatement.setInt(1,4);//给第一个占位符？ 的值赋值为4；
        preparedStatement.setString(2,"狂神说Java");//给第二个占位符？ 的值赋值为狂神说Java；
        preparedStatement.setString(3,"123456");//给第三个占位符？ 的值赋值为123456；
        preparedStatement.setString(4,"24736743@qq.com");//给第四个占位符？ 的值赋值为1；
        preparedStatement.setDate(5,new Date(new java.util.Date().getTime()));//给第五个占位符？ 的值赋值为new Date(new java.util.Date().getTime())；  第一个Data是sql包下的

        //5.执行SQL
        int i = preparedStatement.executeUpdate();

        if (i>0){
            System.out.println("插入成功@");
        }

        //6.关闭连接，释放资源（一定要做） 先开后关
        preparedStatement.close();
        connection.close();
    }
}

```

![image-20220425212836100](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252128209.png)

### **事务**

要么都成功，要么都失败！

`ACID原则`：保证数据的安全。

```less
开启事务
事务提交  commit()
事务回滚  rollback()
关闭事务

转账：
A:1000
B:1000
    
A(900)   --100-->   B(1100) 

START TRANSACTION;  #开启事务
UPDATE `account` SET money=money-100 WHERE `name`='A';
UPDATE `account` SET money=money+100 WHERE `name`='B';

ROLLBACK; #事务回滚
COMMIT; #事务提交        
回滚再提交 也不会有变化  提交再回滚就是真提交了
```

### **Junit单元测试**

依赖

```xml
<!--单元测试-->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>
```

简单使用

@Test注解只有在方法上有效，只要加了这个注解的方法，就可以直接运行！失败的时候是红色：

```java
@Test
public void test(){
    System.out.println("Hello");
}
```

### **搭建一个环境**

```sql
package com.kwq;

import org.junit.Test;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class TestJDBC3 {
    @Test
    public void test() {
        //配置信息
        //useUnicode=true&characterEncoding=utf-8 解决中文乱码
        String url="jdbc:mysql://127.0.0.1:3306/jdbc?useSSL=false&useUnicode=true&characterEncoding=utf8";
        String username = "root";
        String password = "123123";

        Connection connection = null;

        //1.加载驱动
        try {
            Class.forName("com.mysql.jdbc.Driver");
            //2.连接数据库,代表数据库
            connection = DriverManager.getConnection(url, username, password);

            //3.通知数据库开启事务,false 开启
            connection.setAutoCommit(false);

            String sql = "update account set money = money-100 where name = 'A'";
            connection.prepareStatement(sql).executeUpdate();

            //制造错误
            //int i = 1/0;

            String sql2 = "update account set money = money+100 where name = 'B'";
            connection.prepareStatement(sql2).executeUpdate();

            connection.commit();//以上两条SQL都执行成功了，就提交事务！
            System.out.println("success");
        } catch (Exception e) {
            try {
                //如果出现异常，就通知数据库回滚事务
                connection.rollback();
            } catch (SQLException e1) {
                e1.printStackTrace();
            }
            e.printStackTrace();
        }finally {
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}

```

执行此方法：

![image-20220425215246401](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252152629.png)

![image-20220425215257761](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252152024.png)

`把int i=1/0的注释打开`

测试：

![image-20220425215352268](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252153466.png)

![image-20220425215410843](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204252154041.png)



## 15、实战Sbms超市订单管理系统

### 项目搭建准备工作

#### 1.项目架构

![image-20220426085048054](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260850242.png)

#### 2.数据库设计

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260849310.png)

sql在idea项目下。

#### 3.项目搭建

##### 1、项目如何搭建？

- 是否使用maven？

  - 使用maven要去网上找依赖
  - 不使用maven要自己手动导jar包

  为了方便，这个项目使用maven搭建

##### 2、创建项目

- 使用maven模板创建一个maven项目  项目名为`smbms`
  ![img](https://img2020.cnblogs.com/blog/1747479/202009/1747479-20200908232742879-1072455214.png)



1. 补全maven项目结构
   ![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260849539.png)
2. 更新WEB.XML的配置版本

```XML
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         metadata-complete="true">
</web-app>
```

1. 清理pom.xml

   只需要保留GAV+项目的打包方式即可

```XML
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.kwq</groupId>
  <artifactId>smbms</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  

</project>

```

3、配置Tomcat
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260849648.png)
![img](https://img2020.cnblogs.com/blog/1747479/202009/1747479-20200908232939014-1125660442.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260849165.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260849265.png)

#### 4、测试项目是否搭建完成

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260849047.png)

![image-20220426090211908](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260902040.png)

测试通过，项目搭建完成！

#### 5、导入依赖

```undefined
用什么导入什么，不一定要一次性全部导入
  <dependencies>
    <!--servlet依赖-->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
      <version>2.5</version>
    </dependency>
    <!--JSP依赖-->
    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>jsp-api</artifactId>
      <version>2.2.1-b03</version>
    </dependency>
    <!--数据库连接依赖-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.47</version>
    </dependency>
    <!--JSTL标签-->
    <dependency>
      <groupId>javax.servlet.jsp.jstl</groupId>
      <artifactId>jstl-api</artifactId>
      <version>1.2</version>
    </dependency>
    <!--JSTL标签的依赖-->
    <dependency>
      <groupId>taglibs</groupId>
      <artifactId>standard</artifactId>
      <version>1.1.2</version>
    </dependency>
  </dependencies>
```

#### 6、创建项目包结构

- 首先创建一个总的包，命名"com.公司名"

![image-20220426090425054](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260904197.png)

- 在创建这个项目会使用的包

![image-20220426090539496](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260905017.png)

#### 7、创建实体类/JavaBean/ORM映射

- 使用IDEA连接数据库，按照数据库中的表的结构来创建实体类
  
- 数据库中有5张表，但是地址表没有什么用处，所以我们暂时不创建地址表对应的实体类

```
package com.kwq.pojo;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Bill {
    private Integer id;   //id
    private String billCode; //账单编码
    private String productName; //商品名称
    private String productDesc; //商品描述
    private String productUnit; //商品单位
    private BigDecimal productCount; //商品数量
    private BigDecimal totalPrice; //总金额
    private Integer isPayment; //是否支付
    private Integer providerId; //供应商ID
    private Integer createdBy; //创建者
    private Date creationDate; //创建时间
    private Integer modifyBy; //更新者
    private Date modifyDate;//更新时间

    private String providerName;//供应商名称

}
```

```
package com.kwq.pojo;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Provider {

    private Integer id;   //id
    private String proCode; //供应商编码
    private String proName; //供应商名称
    private String proDesc; //供应商描述
    private String proContact; //供应商联系人
    private String proPhone; //供应商电话
    private String proAddress; //供应商地址
    private String proFax; //供应商传真
    private Integer createdBy; //创建者
    private Date creationDate; //创建时间
    private Integer modifyBy; //更新者
    private Date modifyDate;//更新时间


}
```

```
package com.kwq.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.util.Date;
@Data
@AllArgsConstructor
@NoArgsConstructor

public class User {
    private Integer id; //id
    private String userCode; //用户编码
    private String userName; //用户名称
    private String userPassword; //用户密码
    private Integer gender;  //性别
    private Date birthday;  //出生日期
    private String phone;   //电话
    private String address; //地址
    private Integer userRole;    //用户角色
    private Integer createdBy;   //创建者
    private Date creationDate; //创建时间
    private Integer modifyBy;     //更新者
    private Date modifyDate;   //更新时间

    private Integer age;//年龄

    private String userRoleName;    //用户角色名称
    
}
```

```
package com.kwq.pojo;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Role {

        private Integer id;   //id
        private String roleCode; //角色编码
        private String roleName; //角色名称
        private Integer createdBy; //创建者
        private Date creationDate; //创建时间
        private Integer modifyBy; //更新者
        private Date modifyDate;//更新时间

        
}
```

#### 8、编写数据库操作的基础公共类BaseDao

- 数据库配置文件，这是一个资源文件，应该创建在maven项目的resources文件中

```PROPERTIES
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/smbms?useSSL=false&useUnicode=true&characterEncoding=utf8
userName=root
password=123123

```

- 编写数据库操作的公共方法

```JAVA
package com.kwq.dao;

import java.io.IOException;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

public class BaseDao {
    private static String DRIVER;
    private static String URL;
    private static String USERNAME;
    private static String PASSWORD;

    static {//静态代码块，在调用这个类的地方优先自动执行
        //读取配置文件
        //1、创建properties对象
        Properties properties = new Properties();
        //2、通过类加载器加载资源文件为字节输入流
        InputStream in = BaseDao.class.getClassLoader().getResourceAsStream("db.properties");
        try {
            properties.load(in);
        } catch (IOException e) {
            e.printStackTrace();
        }
        DRIVER = properties.getProperty("driver");
        URL = properties.getProperty("url");
        USERNAME = properties.getProperty("userName");
        PASSWORD = properties.getProperty("password");
    } //1、编写获取数据库的连接对象的公共方法
    public static Connection getConnection(){
        Connection conn= null;
        try {
            //1、加载驱动类
            Class.forName(DRIVER);
            //2、获取连接对象
            conn = DriverManager.getConnection(URL,USERNAME,PASSWORD);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        return conn;
    }

    //2、编写查询公共方法 —— 注意查询的结果返回为ResultSet结果集
    /**
     * 用于查询数据的公共方法，注意：使用发送SQL语句的对象为PreparedStatement
     * @param sql:查询的sql语句，由前端传递
     * @param params:sql语句中占位符的值
     *
     *===============这下面的3个参数之所以在调用的时候传递原因就在于这3个都是资源，我们需要关闭，如果我们直接在这个方法里获取资源对象的话，那么我们就应该在这个方法中关闭资源===============
     *===============但是调用处还在等待这个方法返回结果集，所以我们不应该在这个地方获取这3个对象，而应该由调用出传递，这样可以统一管理和关闭资源===============
     *
     * @param conn：调用出使用BaseDao.getConnection()获取到数据库连接对象传入
     * @param pstmt：调用出只是传入null的引用。这个对象真正的实例化放在这个方法里面
     * @param rs：返回的结果集，和pstmt只是传入null的引用。这个对象真正的实例化放在这个方法里面
     *
     * @return：返回查询到的结果集
     */
    public static ResultSet executeQuery(Connection conn, String sql, Object[] params, ResultSet rs, PreparedStatement pstmt){
        try {
            pstmt = conn.prepareStatement(sql);
            for (int i=1;i<= params.length;i++){//循环遍历参数数组，并将参数设入SQL中
                pstmt.setObject(i,params[i-1]);//注意：数组的index从0开始，而PreparedStatement中设置占位符的值的index从1开始
            }
            rs = pstmt.executeQuery();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        return  rs;
    }

    //3、编写修改公共方法
    /**
     * 用于修改数据的公共方法，注意：使用发送SQL语句的对象为PreparedStatement
     * @param sql：修改数据的sql语句模板
     * @param params：模板中占位符对应的值
     *
     * =====下面两个对象需要调用出传入也是为了统一管理和关闭资源=====
     * @param conn：调用出使用BaseDao.getConnection()获取到数据库连接对象传入
     * @param pstmt：调用出只是传入null的引用。这个对象真正的实例化放在这个方法里面
     *
     * @return 返回受影响的行数
     */
    public static int executeUpdate(Connection conn, String sql, Object[] params, ResultSet rs, PreparedStatement pstmt){
        int result = 0;
        try {
            pstmt = conn.prepareStatement(sql);
            for (int i=1;i< params.length;i++){//循环遍历参数数组，并将参数设入SQL中
                pstmt.setObject(i,params[i-1]);//注意：数组的index从0开始，而PreparedStatement中设置占位符的值的index从1开始
            }
            result = pstmt.executeUpdate();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        return  result;
    }

    //4、编写关闭资源公共方法
    /**
     * 关闭资源
     * @param conn：调用出使用BaseDao.getConnection()获取到数据库连接对象传入
     * @param pstmt：调用出只是传入null的引用。这个对象真正的实例化放在这个方法里面
     * @param rs：返回的结果集，和pstmt只是传入null的引用。这个对象真正的实例化放在这个方法里面
     * @return：返回关闭资源的结果
     *
     * 注意:关闭资源的时候要倒着关
     */
    public static boolean close(Connection conn, PreparedStatement pstmt, ResultSet rs){
        boolean flag = true;
        if (rs!=null){
            try {
                rs.close();
                rs = null;//让这个变量为null，gc就会自动对其进行回收
            } catch (SQLException throwables) {
                throwables.printStackTrace();
                flag = false;//关闭失败就将flag设置false
            }
        }
        if (pstmt!=null){
            try {
                pstmt.close();
                pstmt = null;
            } catch (SQLException throwables) {
                throwables.printStackTrace();
                flag = false;
            }
        }
        if (conn!=null){
            try {
                conn.close();
                conn = null;
            } catch (SQLException throwables) {
                throwables.printStackTrace();
                flag = false;
            }
        }
        return flag;//返回关闭结果
    }

    
}


```

#### 9、编写字符编码过滤器

```JAVA
//编写过滤器

package com.thhh.filter;

import javax.servlet.*;
import java.io.IOException;

public class CharacterEncoding implements Filter {
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        response.setContentType("text/html;charset=utf-8");

        chain.doFilter(request,response);
    }

    public void destroy() {

    }
}
```

配置

```xml
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         metadata-complete="true">
    <!--注册字符编码过滤器-->
    <filter>
        <filter-name>CharacterEncoding</filter-name>
        <filter-class>com.kwq.filter.CharacterEncoding</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

#### 10、导入静态资源

```css
包括HTML、CSS、JS等，注意：这些资源都是网站的，所以我们应该将这些资源放在webapp下面
```

![img](https://img2020.cnblogs.com/blog/1747479/202009/1747479-20200908233607614-1290599201.png)

**通过以上的步骤，一个WEB项目的搭建工作就算基本完成了**

### 登录功能的实现

页面实现逻辑/流程

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260954410.png)

#### 1、编写前端页面

 前端页面直接使用已有的，我们主要编写网页背后的执行代码  这两个页面都在webapp下，分别是error.jsp和login.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
<h1>请登录后再访问该页面！</h1>
<a href="login.jsp">返回</a>
</body>
</html>
```

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" isELIgnored="false" %>
<html>
<head lang="en">

    <meta charset="UTF-8">
    <title>系统登录 - 超市订单管理系统</title>
    <link type="text/css" rel="stylesheet" href="${pageContext.request.contextPath}/css/style.css" />
    <script type="text/javascript">
   /* if(top.location!=self.location){
         top.location=self.location;
    } */
    </script>
</head>
<body class="login_bg">
    <section class="loginBox">
        <header class="loginHeader">
            <h1>超市订单管理系统</h1>
        </header>
        <section class="loginCont">
           <form class="loginForm" action="${pageContext.request.contextPath}/login.do"  name="actionForm" id="actionForm"  method="post" >
            <div class="info">${error}</div>
            <div class="inputbox">
                    <label >用户名：</label>
               <input type="text" class="input-text" id="userCode" name="userCode" placeholder="请输入用户名" required/>
            </div>
            <div class="inputbox">
                    <label >密码：</label>
                    <input type="password" id="userPassword" name="userPassword" placeholder="请输入密码" required/>
                </div>
            <div class="subBtn">
                    <input type="submit" value="登录"/>
                    <input type="reset" value="重置"/>
                </div>
         </form>
        </section>
    </section>
</body>
</html>
```

项目结构：

![image-20220426100627605](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261006757.png)

#### 2、设置首页

在web.xml中  启动就不走默认的index.jsp。 而是走webapp下的login.jsp

```XML
    <!--设置欢迎页/首页-->
    <welcome-file-list>
        <welcome-file>login.jsp</welcome-file>
    </welcome-file-list>
```

启动tomcat：

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260954381.png)

#### 3、编写Dao层用户登陆的接口

- 创建一个UserDao接口，然后我们按照"面向接口编程"的原则去接口UserDaoImpl中实现这个接口中的方法
  - 使用面向接口编程的好处就在与我们在接口中只需要定义方法，而不需要实现方法，整个结构和思路都很清晰
  - 其次就是将设计和实现分离，保证了设计专注于设计，实现专注于实现

```JAVA
package com.kwq.dao.user;

public interface UserDao {
        //查询登录用户
        User getLoginUser(Connection con, String userCode) throws SQLException;

}

```

#### 4、编写Dao接口的实现类

```JAVA
package com.kwq.dao.user;


public class UserDaoImpl implements UserDao {

    //得到要登陆的用户
    public User getLoginUser(Connection con, String userCode) {
        PreparedStatement pre = null;
        ResultSet rs = null;
        User user = null;
        try {
            if (con != null) {
                String sql = "select * from smbms_user where userCode=?";
                Object[] params = {userCode};

                rs = BaseDao.executeQuery(con,sql,params,rs, pre);
                if (rs.next()) {
                    user = new User();
                    user.setId(rs.getInt("id"));
                    user.setUserCode(rs.getString("userCode"));
                    user.setUserName(rs.getString("userName"));
                    user.setUserPassword(rs.getString("userPassword"));
                    user.setGender(rs.getInt("gender"));
                    user.setBirthday(rs.getDate("birthday"));
                    user.setPhone(rs.getString("phone"));
                    user.setAddress(rs.getString("address"));
                    user.setUserRole(rs.getInt("userRole"));
                    user.setCreatedBy(rs.getInt("createdBy"));
                    user.setCreationDate(rs.getTimestamp("creationDate"));
                    user.setModifyBy(rs.getInt("modifyBy"));
                    user.setModifyDate(rs.getTimestamp("modifyDate"));
                }
                BaseDao.close(null, pre, rs);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return user;
    }
}
```

#### 5、编写业务层接口

```java
package com.kwq.service.user;


import com.kwq.pojo.User;

public interface UserService {
    /**
     * 用户登陆身份验证
     * @param userCode：用户账号
     * @param userPassword：用户密码，注意，密码判断我们在service层进行；
     *                      在Dao层只是简单的操作数据库，没有其他的逻辑代码；在servlet层中只是接收和转发请求以及控制视图跳转
     *                      而对于业务层(service)就是用来实现业务逻辑代码的
     * @return
     */
    public User login(String userCode, String userPassword);
}

```

#### 6、业务层实现类

```java
package com.kwq.service.user;
/**
 * 业务层主要就是编写业务代码，在编写业务代码的时候经常会调用数据库
 * 所以在业务层中需要使用到我们一开始编写好的DAO的代码
 */



import com.kwq.dao.BaseDao;
import com.kwq.dao.user.UserDao;
import com.kwq.dao.user.UserDaoImpl;
import com.kwq.pojo.User;
import org.junit.Test;

import java.sql.Connection;
import java.sql.SQLException;

public class UserServiceImpl implements UserService{
    private UserDao userDao;//业务层需要使用Dao，所以直接将Dao作为一个成员变量来使用

    public UserServiceImpl() {
        this.userDao = new UserDaoImpl();//在业务层被实例化的时候就让它得到Dao对象，后面就可以直接去用
    }


    public User login(String userCode, String userPassword) {
        Connection conn = null;
        User user = null;

        conn = BaseDao.getConnection();//获取数据库连接对象
        //通过业务层调用Dao层
        try {
            user = userDao.getLoginUser(conn,userCode);//调用userDao中的获取用户信息的方法
        } catch (SQLException throwables) {
            throwables.printStackTrace();}
        finally {
            BaseDao.close(conn,null,null);
        }


        return user;
    }
   
}

```

测试是否能够成功运行

```JAVA
  @Test
    public void test(){
        UserServiceImpl userService = new UserServiceImpl();
        User admin = userService.login("admin","12bfasbka");
        System.out.println(admin.getUserPassword());
    }
```

![image-20220426105012266](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261050615.png)

#### 7、编写servlet

首先在webapp下导入我们也用到的JSP页面

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260954061.png)

> frame.jsp

```
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@include file="/jsp/common/head.jsp"%>
<div class="right">
    <img class="wColck" src="${pageContext.request.contextPath }/images/clock.jpg" alt=""/>
    <div class="wFont">
        <h2>${userSession.userName }</h2>
        <p>欢迎来到超市订单管理系统!</p>
    </div>
</div>
</section>
<%@include file="/jsp/common/foot.jsp" %>
```

 其中frame.jsp是登陆成功之后跳转的页面，而common中是页面的头部和底部

 注意：我们在编写servlet和注册servlet的时候一定要注意我们使用的前端页面上面写的地址，为了不出错最后时与前端页面上写的地址保持一致；其次就是注意前端使用的JSP内置对象和EL表达式，我们在后端需要使用对应的变量名，否则是取不到值的
**servlet编写**

> 常量类

```
package com.kwq.utils;

public class Constants {
    public  final static   String USER_SESSION="userSession";
}
```

```JAVA
package com.kwq.servlet.user;

import com.kwq.pojo.User;
import com.kwq.service.user.UserServiceImpl;
import com.kwq.utils.Constants;

import javax.servlet.ServletException;
import javax.servlet.http.*;
import java.io.IOException;


public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("LoginServlet start...");
        //获取用户名和密码
        String userCode = request.getParameter("userCode");
        String userPassword = request.getParameter("userPassword");
        //和数据库中的密码进行对比
        UserServiceImpl userService = new UserServiceImpl();
        User user = userService.login(userCode, userPassword);
        if (user != null) {
            //将用户信息存入session中
            request.getSession().setAttribute(Constants.USER_SESSION, user);
            //跳转到主页
            response.sendRedirect("jsp/frame.jsp");
        } else {
            /**
             查无此人，无法登录
             转发给登录页面，提示用户名或密码错误
             */
            request.setAttribute("error", "用户或密码不正确");

            //转发路径相对于webapp内第一层
            request.getRequestDispatcher("login.jsp").forward(request, response);
        }
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }
}

```

#### 8、注册servlet

```xml
    <!--注册登陆的servlet-->
    <servlet>
        <servlet-name>LoginServlet</servlet-name>
        <servlet-class>com.thhh.servlet.user.LoginServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>LoginServlet</servlet-name>
        <url-pattern>/login.do</url-pattern>
    </servlet-mapping>
```

注意：这里的映射与我们使用的前端页面表单提交的action要保持一致
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260954419.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260954284.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260955372.png)

#### 9、测试功能，确保上面的代码正确

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260955662.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260955500.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260955497.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204260955199.png)

测试完成！



### 注销功能的实现

#### 1、注销功能

 思路：移除session对象+返回登陆页面

1.编写servlet

```java
package com.kwq.servlet.user;


import com.kwq.utils.Constants;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class LogoutServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.getSession().removeAttribute(Constants.USER_SESSION);//移除用户session
        resp.sendRedirect(req.getContextPath()+"/login.jsp");//重定向回登陆页面
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261118034.png)

#### 2.注册servlet

```XML
  <!--注册注销servlet-->
    <servlet>
        <servlet-name>LogoutServlet</servlet-name>
        <servlet-class>com.kwq.servlet.user.LogoutServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>LogoutServlet</servlet-name>
        <url-pattern>/jsp/logout.do</url-pattern>
    </servlet-mapping>
```




#### 3、登陆拦截

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261118100.png)
1.编写过滤器

```java
package com.kwq.filter;



import com.kwq.pojo.User;
import com.kwq.utils.Constants;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class SysFilter implements Filter {
 
    public void init(FilterConfig filterConfig) throws ServletException {

    }

 
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest)req;//父类强转子类,用于获取session
        HttpServletResponse response = (HttpServletResponse) resp;//父类强转子类，用于重定向
        User user = (User) request.getSession().getAttribute(Constants.USER_SESSION);

        if (user==null){//用户处于未登陆状态
            response.sendRedirect(request.getContextPath()+"/error.jsp");//重定向到登陆页面
        }else{
            chain.doFilter(req,resp);//过滤器放行
        }
    }

 
    public void destroy() {

    }
}
```

2.注册过滤器

```xml
  <!--注册未登录时请求后台页面过滤器-->
    <filter>
        <filter-name>SysFilter</filter-name>
        <filter-class>com.kwq.filter.SysFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>SysFilter</filter-name>
        <url-pattern>/jsp/*</url-pattern>
        <!--注意：我们将所有需要登陆之后才能访问的功能性页面给都放在了/jsp文件夹下，所以过滤器应该过滤这个文件夹下的请求-->
    </filter-mapping>
```

> 查看error.jsp


![img](https://img2020.cnblogs.com/blog/1747479/202009/1747479-20200909202043164-1357858823.png)

4.测试功能

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261118229.png)

> 随后的响应

![image-20220426113713473](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261137674.png)

点击返回跳到首页

![image-20220426113743841](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261137087.png)



### 密码修改

分析：很明显，要修改用户密码我们还是需要和数据库交互，那么就还是前面我们写登陆功能的代码编写步骤 —— DAO层、service层、servlet层，前端页面直接使用现成的，但是注意servlet的地址映射注意和前端页面保持一致

 **为什么要按照DAO层、service层、servlet层，JSP页面的顺序来编写呢？**
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261142420.png)
​ 原因在上图展示的很清楚，开发JSP需要填写servlet在服务器上的映射路径，开发servlet需要调用service中的方法完成业务逻辑，开发service需要调用Dao中对数据库的操作来操作数据库，而只有Dao中使用的JDBC我们是数据库厂商实现了的，所以我们可以直接使用；所以为了开发的完整性，我们就应该从Dao开始-->service-->servlet-->JSP

 **分析实现步骤/模块功能划分(很重要)**

 只有我们先想好了怎么做，然后再去编写代码才会快，且有条不紊，切忌看完要求之后马上开始写代码
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261142516.png)

#### 1.导入前端素材

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261142674.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261142544.png)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
   pageEncoding="UTF-8"%>
<%@include file="/jsp/common/head.jsp"%>
<div class="right">
            <div class="location">
                <strong>你现在所在的位置是:</strong>
                <span>密码修改页面</span>
            </div>
            <div class="providerAdd">
                <form id="userForm" name="userForm" method="post" action="${pageContext.request.contextPath }/jsp/user.do">
                    <input type="hidden" name="method" value="savepwd">
                    <!--div的class 为error是验证错误，ok是验证成功-->
                    <div class="info">${message}</div>
                    <div class="">
                        <label for="oldPassword">旧密码：</label>
                        <input type="password" name="oldpassword" id="oldpassword" value="">
                  <font color="red"></font>
                    </div>
                    <div>
                        <label for="newpassword">新密码：</label>
                        <input type="password" name="newpassword" id="newpassword" value=""> 
                  <font color="red"></font>
                    </div>
                    <div>
                        <label for="rnewpassword">确认新密码：</label>
                        <input type="password" name="rnewpassword" id="rnewpassword" value=""> 
                  <font color="red"></font>
                    </div>
                    <div class="providerAddBtn">
                        <!--<a href="#">保存</a>-->
                        <input type="button" name="save" id="save" value="保存" class="input-button">
                    </div>
                </form>
            </div>
        </div>
    </section>
<%@include file="/jsp/common/foot.jsp" %>
<script type="text/javascript" src="${pageContext.request.contextPath }/js/pwdmodify.js"></script>
```



#### 2.Dao接口

```java
package com.kwq.dao.user;

import com.kwq.pojo.User;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;

public interface UserDao {

        //查询登录用户
        User getLoginUser(Connection con, String userCode) throws SQLException;
        //修改当前用户密码
        int updatePwd(Connection con, int id, String password) throws SQLException;


}

```

 只需要看方法2

#### 3.Dao接口实现

```java
package com.kwq.dao.user;



import com.kwq.dao.BaseDao;
import com.kwq.pojo.User;
import com.mysql.jdbc.StringUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;



public class UserDaoImpl implements UserDao {

    //得到要登陆的用户
    public User getLoginUser(Connection con, String userCode) {
        PreparedStatement pre = null;
        ResultSet rs = null;
        User user = null;
        try {
            if (con != null) {
                String sql = "select * from smbms_user where userCode=?";
                Object[] params = {userCode};

                rs = BaseDao.executeQuery(con,sql,params,rs, pre);
                if (rs.next()) {
                    user = new User();
                    user.setId(rs.getInt("id"));
                    user.setUserCode(rs.getString("userCode"));
                    user.setUserName(rs.getString("userName"));
                    user.setUserPassword(rs.getString("userPassword"));
                    user.setGender(rs.getInt("gender"));
                    user.setBirthday(rs.getDate("birthday"));
                    user.setPhone(rs.getString("phone"));
                    user.setAddress(rs.getString("address"));
                    user.setUserRole(rs.getInt("userRole"));
                    user.setCreatedBy(rs.getInt("createdBy"));
                    user.setCreationDate(rs.getTimestamp("creationDate"));
                    user.setModifyBy(rs.getInt("modifyBy"));
                    user.setModifyDate(rs.getTimestamp("modifyDate"));
                }
                BaseDao.close(null, pre, rs);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return user;
    }

    public int updatePwd(Connection con, int id, String password) throws SQLException {
        //修改当前用户密码
            PreparedStatement pstm = null;
            int execute = 0;
            if (con != null) {
                String sql = "update smbms_user set userPassword = ? where id = ?";
                Object[] params = {password, id};
                execute = BaseDao.executeUpdate(con, sql, params ,pstm);
                BaseDao.close(null, pstm, null);
            }
            return execute;
        }


}


```

 只需要看方法2

#### 4.service接口

```java
package com.kwq.service.user;


import com.kwq.pojo.User;

public interface UserService {
    /**
     * 用户登陆身份验证
     * @param userCode：用户账号
     * @param userPassword：用户密码，注意，密码判断我们在service层进行；
     *                      在Dao层只是简单的操作数据库，没有其他的逻辑代码；在servlet层中只是接收和转发请求以及控制视图跳转
     *                      而对于业务层(service)就是用来实现业务逻辑代码的
     * @return
     */
    public User login(String userCode, String userPassword);

    //根据用户id修改密码
    boolean updatePwd(int id, String password);

}

```

 只看方法2

#### 5.service接口实现

```java
package com.kwq.service.user;
/**
 * 业务层主要就是编写业务代码，在编写业务代码的时候经常会调用数据库
 * 所以在业务层中需要使用到我们一开始编写好的DAO的代码
 */



import com.kwq.dao.BaseDao;
import com.kwq.dao.user.UserDao;
import com.kwq.dao.user.UserDaoImpl;
import com.kwq.pojo.User;
import org.junit.Test;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class UserServiceImpl implements UserService{
    private UserDao userDao;//业务层需要使用Dao，所以直接将Dao作为一个成员变量来使用

    public UserServiceImpl() {
        this.userDao = new UserDaoImpl();//在业务层被实例化的时候就让它得到Dao对象，后面就可以直接去用
    }


    public User login(String userCode, String userPassword) {
        Connection conn = null;
        User user = null;

        conn = BaseDao.getConnection();//获取数据库连接对象
        //通过业务层调用Dao层
        try {
            user = userDao.getLoginUser(conn,userCode);//调用userDao中的获取用户信息的方法
        } catch (SQLException throwables) {
            throwables.printStackTrace();}
        finally {
            BaseDao.close(conn,null,null);
        }


        return user;
    }
    
    public boolean updatePwd(int id, String password) {
        Connection con = null;
        PreparedStatement pstm = null;
        con = BaseDao.getConnection();
        boolean flag = false;
        try {
            if (userDao.updatePwd(con, id, password) > 0) {
                flag = true;
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            BaseDao.close(con, pstm, null);
        }
        return flag;
    }
}

```

 

#### 6.servlet编写(写的很巧妙实现了复用)

```java
package com.kwq.servlet.user;


import com.kwq.pojo.User;
import com.kwq.service.user.UserService;
import com.kwq.service.user.UserServiceImpl;
import com.kwq.utils.Constants;
import com.mysql.jdbc.StringUtils;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class UserServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String method = req.getParameter("method");
        if (method.equals("savepwd") && method != null) {
            this.updatePwd(req, resp);

        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }

    //修改密码
    public void updatePwd(HttpServletRequest req, HttpServletResponse resp) {
        //从session里拿id
        Object o = req.getSession().getAttribute(Constants.USER_SESSION);
        String newpassword = req.getParameter("newpassword");

        System.out.println("UserServlet" + newpassword);
        boolean flag = false;
        if (o != null && !StringUtils.isNullOrEmpty(newpassword)) {
            UserService userService = new UserServiceImpl();
            flag = userService.updatePwd(((User) o).getId(), newpassword);
            if (flag) {
                req.setAttribute("message", "修改密码成功，请使用新密码登录！");
                req.getSession().removeAttribute(Constants.USER_SESSION);
            } else {
                req.setAttribute("message", "密码修改失败！");
            }
        } else {
            req.setAttribute("message", "密码修改失败！");
        }
        try {
            req.getRequestDispatcher("/jsp/pwdmodify.jsp").forward(req, resp);
        } catch (ServletException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }



}

```

#### 7.注册servlet

```XML
 <!--注册用户修改密码的servlet-->
    <servlet>
        <servlet-name>UserServlet</servlet-name>
        <servlet-class>com.kwq.servlet.user.UserServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>UserServlet</servlet-name>
        <url-pattern>/jsp/user.do</url-pattern>
    </servlet-mapping>
```

#### 8.测试

 bug：

```javascript
//前端页面上，编写的时候要求输入旧密码，但是实际测试的时候输入旧密码有BUG，我们直接不使用输入旧密码，使用新密码+重复新密码输入框来修改密码
//但是前端使用的JS控制了提交表单的按钮，即需要3个输入框输入都满足要求的时候才能提交表单数据，所以我们需要把判断旧密码输入框的判断语句注释了
//这样才能只通过新密码+重复新密码实现密码修改

saveBtn.on("click",function(){
   oldpassword.blur();
   newpassword.blur();
   rnewpassword.blur();
   // oldpassword.attr("validateStatus") == "true"
   // &&
   if( newpassword.attr("validateStatus") == "true"
      && rnewpassword.attr("validateStatus") == "true"){
      if(confirm("确定要修改密码？")){
         $("#userForm").submit();
      }
   }
   
});
```

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261142613.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261142569.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261142008.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261142303.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261142330.png)
![img](https://img2020.cnblogs.com/blog/1747479/202009/1747479-20200909202527871-890979702.png)



### 使用Ajax优化密码修改功能

 前面实现的密码修改功能是直接输入两遍新密码进行的修改，这显然是不安全的，所以我们应该在修改密码的时候加入验证旧密码的操作，而这个操作根据前端素材就需要使用到Ajax了

#### 2.怎么实现旧密码验证？

 直接的做法就是去数据库中查找，但是这样的画我们又要写一条线：Dao开始-->service-->servlet-->JSP

 我们可以采取一种很简单的方式进行：在用户登陆的时候我们将整个用户对象都存储到session中了，所以我们可以利用Ajax的异步请求+session中的user对象实现旧密码的验证

 做法就是在后端复用的servlet中再定义一个方法，这个方法专门负责对比Ajax传递给后端的旧密码是否和session中User对象的密码一致，并通过对比的情况和结果来返回JSON数据给前端

#### 3.编写servlet

```javascript
pwdmodify.js的失去焦点事件  失去焦点去发一条ajax请求

oldpassword.on("blur",function(){
   $.ajax({
      type:"GET",
      url:path+"/jsp/user.do",//Ajax提交的URL，可以和我们修改密码的JSP页面的表单提交的URL一样，所以我们应该复用servlet
      data:{method:"pwdmodify",oldpassword:oldpassword.val()},//提交的参数
      //上面两句话等价于 = path+/jsp/user.do ? method=pwdmodify & oldpassword=oldpassword.val()

           dataType:"json",
      success:function(data){
         if(data.result == "true"){//旧密码正确
            validateTip(oldpassword.next(),{"color":"green"},imgYes,true);
         }else if(data.result == "false"){//旧密码输入不正确
            validateTip(oldpassword.next(),{"color":"red"},imgNo + " 原密码输入不正确",false);
         }else if(data.result == "sessionerror"){//当前用户session过期，请重新登录
            validateTip(oldpassword.next(),{"color":"red"},imgNo + " 当前用户session过期，请重新登录",false);
         }else if(data.result == "error"){//旧密码输入为空
            validateTip(oldpassword.next(),{"color":"red"},imgNo + " 请输入旧密码",false);
         }
      },
      error:function(data){
         //请求出错
         validateTip(oldpassword.next(),{"color":"red"},imgNo + " 请求错误",false);
      }
   });
```

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261901602.png)

导入依赖

```
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>fastjson</artifactId>
  <version>1.2.79</version>
</dependency>
```

配置session过期时间

```
<session-config>
    <session-timeout>30</session-timeout>
</session-config>
```

```JAVA
userSevlet添加方法：    
//2.验证旧密码
    //直接与session中的user对象的密码进行对比即可，不用再去查数据库
    public void pwdModify(HttpServletRequest req, HttpServletResponse resp){
        Object user = req.getSession().getAttribute(Constants.USER_SESSION);
        String oldpassword = req.getParameter("oldpassword");

        //使用Map集合存储返回给前端的数据
        Map<String,String> resultMap = new HashMap<String, String>();

        if (user==null){//session中的user对象失效了
            resultMap.put("result","sessionerror");
        }else if (StringUtils.isNullOrEmpty(oldpassword)){//输入的密码为空
            resultMap.put("result","error");
        }else {
            String userPassword = ((User)user).getUserPassword();
            if (userPassword.equals(oldpassword)){//输入的密码匹配
                resultMap.put("result","true");
            }else {//输入的密码不匹配
                resultMap.put("result","false");
            }
        }

        //将Map集合中的数据转为JSON格式传输给前端
        try {
            resp.setContentType("application/JSON");//设置返回数据是一个JSON，这样浏览器拿到这个数据之后就会按照JSON的数据格式来解析它
            PrintWriter writer = resp.getWriter();//获取输出流
            writer.write(JSONArray.toJSONString(resultMap));//使用阿里巴巴提供的一个JSON工具类，直接将其他数据格式的数据转为JSON数据格式，然后直接将其写出去
            writer.flush();//刷新缓冲区
            writer.close();//关闭资源
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
```

#### 4.测试

![img](https://img2020.cnblogs.com/blog/1747479/202009/1747479-20200909202751837-335071298.png)
![img](https://img2020.cnblogs.com/blog/1747479/202009/1747479-20200909202806389-393400149.png)

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204261901539.png)



### 用户管理功能实现


![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204262018120.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204262018702.png)

#### 1.导入分页工具类

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204262018304.png)

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204262018630.png)
![img](https://img2020.cnblogs.com/blog/1747479/202009/1747479-20200909203016663-107565294.png)
​ 

OOP的3大特性：封装、继承、多态，其中封装 = 属性私有+属性的get/set() + **在set中限制一些不安全的赋值操作**(这一步可以留到service层再做，但是在封装的时候做更好，这样减少了service层的代码，且体现了封装的特性)

#### 2.用户列表页面导入

 首先是jsp文件

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8" %>
<%@include file="/jsp/common/head.jsp" %>
<div class="right">
    <div class="location">
        <strong>你现在所在的位置是:</strong>
        <span>用户管理页面</span>
    </div>
    <div class="search">
        <form method="get" action="${pageContext.request.contextPath }/jsp/user.do">
            <input name="method" value="query" class="input-text" type="hidden">
            <span>用户名：</span>
            <input name="queryname" class="input-text" type="text" value="${queryUserName }">

            <span>用户角色：</span>
            <select name="queryUserRole">
                <c:if test="${roleList != null }">
                    <option value="0">--请选择--</option>
                    <c:forEach var="role" items="${roleList}">
                        <option
                                <c:if test="${role.id == queryUserRole }">selected="selected"</c:if>
                                value="${role.id}">${role.roleName}</option>
                    </c:forEach>
                </c:if>
            </select>

            <input type="hidden" name="pageIndex" value="1"/>
            <input value="查 询" type="submit" id="searchbutton">
            <a href="${pageContext.request.contextPath}/jsp/useradd.jsp">添加用户</a>
        </form>
    </div>
    <!--用户-->
    <table class="providerTable" cellpadding="0" cellspacing="0">
        <tr class="firstTr">
            <th width="10%">用户编码</th>
            <th width="20%">用户名称</th>
            <th width="10%">性别</th>
            <th width="10%">年龄</th>
            <th width="10%">电话</th>
            <th width="10%">用户角色</th>
            <th width="30%">操作</th>
        </tr>
        <c:forEach var="user" items="${userList }" varStatus="status">
            <tr>
                <td>
                    <span>${user.userCode }</span>
                </td>
                <td>
                    <span>${user.userName }</span>
                </td>
                <td>
               <span>
                  <c:if test="${user.gender==1}">男</c:if>
                  <c:if test="${user.gender==2}">女</c:if>
               </span>
                </td>
                <td>
                    <span>${user.age}</span>
                </td>
                <td>
                    <span>${user.phone}</span>
                </td>
                <td>
                    <span>${user.userRoleName}</span>
                </td>
                <td>
                    <span><a class="viewUser" href="javascript:;" userid=${user.id } username=${user.userName }><img
                            src="${pageContext.request.contextPath }/images/read.png" alt="查看" title="查看"/></a></span>
                    <span><a class="modifyUser" href="javascript:;" userid=${user.id } username=${user.userName }><img
                            src="${pageContext.request.contextPath }/images/xiugai.png" alt="修改" title="修改"/></a></span>
                    <span><a class="deleteUser" href="javascript:;" userid=${user.id } username=${user.userName }><img
                            src="${pageContext.request.contextPath }/images/schu.png" alt="删除" title="删除"/></a></span>
                </td>
            </tr>
        </c:forEach>
    </table>
    <input type="hidden" id="totalPageCount" value="${totalPageCount}"/>
    <c:import url="rollpage.jsp">
        <c:param name="totalCount" value="${totalCount}"/>
        <c:param name="currentPageNo" value="${currentPageNo}"/>
        <c:param name="totalPageCount" value="${totalPageCount}"/>
    </c:import>
</div>
</section>

<!--点击删除按钮后弹出的页面-->
<div class="zhezhao"></div>
<div class="remove" id="removeUse">
    <div class="removerChid">
        <h2>提示</h2>
        <div class="removeMain">
            <p>你确定要删除该用户吗？</p>
            <a href="#" id="yes">确定</a>
            <a href="#" id="no">取消</a>
        </div>
    </div>
</div>

<%@include file="/jsp/common/foot.jsp" %>
<script type="text/javascript" src="${pageContext.request.contextPath }/js/userlist.js"></script>
```

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204262018540.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204262018407.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204262018343.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204262018402.png)

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
   
</script>
</head>
<body>
      <div class="page-bar">
         <ul class="page-num-ul clearfix">
            <li>共${param.totalCount }条记录&nbsp;&nbsp; ${param.currentPageNo }/${param.totalPageCount }页</li>
            <c:if test="${param.currentPageNo > 1}">
               <a href="javascript:page_nav(document.forms[0],1);">首页</a>
               <a href="javascript:page_nav(document.forms[0],${param.currentPageNo-1});">上一页</a>
            </c:if>
            <c:if test="${param.currentPageNo < param.totalPageCount }">
               <a href="javascript:page_nav(document.forms[0],${param.currentPageNo+1 });">下一页</a>
               <a href="javascript:page_nav(document.forms[0],${param.totalPageCount });">最后一页</a>
            </c:if>
            &nbsp;&nbsp;
         </ul>
       <span class="page-go-form"><label>跳转至</label>
        <input type="text" name="inputPage" id="inputPage" class="page-key" />页
        <button type="button" class="page-btn" onClick='jump_to(document.forms[0],document.getElementById("inputPage").value)'>GO</button>
      </span>
      </div> 
</body>
<script type="text/javascript" src="${pageContext.request.contextPath }/js/rollpage.js"></script>
</html>
```

分析：

- Dao层使用聚合函数COUNT(*)查询用户表一共多少条记录
- service层用于统计总共有多少条数据
- servlet层用于向前端返回总共的数据条数

#### 3.获取用户数量

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204262018006.png)

##### 1、UserDao接口

 因为是获取用户的数量，所以和用户表有关，那这个接口方法就放在前面已经创建好的UserDao中

 方法定义的时候需要哪些参数？

 查看前端素材：
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204262018153.png)
​ 可见，查询界面我们需要实现按照用户名查询、按照角色名称查询和整表查询

 再去看看数据库中的数据表
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204262018224.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204262018308.png)

联表查询的基本SQL语句

```sql
SELECT COUNT(1)
FROM smbms_user u,smbms_role r
WHERE u.userRole = r.id;
```

但是从前面的需求我们可以看出，前端素材提供了按照姓名查询、按照职位查询和整表查询，所以我们需要在上面联表查询的基础上再添加一些筛选条件，添加的手段就是使用"AND 条件"来实现

在Java中实现我们可以使用StringBuffer来实现sql语句的拼接

```java
package com.kwq.dao.user;

import com.kwq.pojo.User;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;

public interface UserDao {

        //查询登录用户
        User getLoginUser(Connection con, String userCode) throws SQLException;
        //修改当前用户密码
        int updatePwd(Connection con, int id, String password) throws SQLException;

        //查询用户整数
        int getUserCount(Connection con, String username, int userRole) throws SQLException;
}

```

 只看方法3

##### 2、UserDaoImpl接口实现

```java
package com.kwq.dao.user;



import com.kwq.dao.BaseDao;
import com.kwq.pojo.User;
import com.mysql.jdbc.StringUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;



public class UserDaoImpl implements UserDao {

    //得到要登陆的用户
    public User getLoginUser(Connection con, String userCode) {
        PreparedStatement pre = null;
        ResultSet rs = null;
        User user = null;
        try {
            if (con != null) {
                String sql = "select * from smbms_user where userCode=?";
                Object[] params = {userCode};

                rs = BaseDao.executeQuery(con,sql,params,rs, pre);
                if (rs.next()) {
                    user = new User();
                    user.setId(rs.getInt("id"));
                    user.setUserCode(rs.getString("userCode"));
                    user.setUserName(rs.getString("userName"));
                    user.setUserPassword(rs.getString("userPassword"));
                    user.setGender(rs.getInt("gender"));
                    user.setBirthday(rs.getDate("birthday"));
                    user.setPhone(rs.getString("phone"));
                    user.setAddress(rs.getString("address"));
                    user.setUserRole(rs.getInt("userRole"));
                    user.setCreatedBy(rs.getInt("createdBy"));
                    user.setCreationDate(rs.getTimestamp("creationDate"));
                    user.setModifyBy(rs.getInt("modifyBy"));
                    user.setModifyDate(rs.getTimestamp("modifyDate"));
                }
                BaseDao.close(null, pre, rs);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return user;
    }

    public int updatePwd(Connection con, int id, String password) throws SQLException {
        //修改当前用户密码
            PreparedStatement pstm = null;
            int execute = 0;
        System.out.println(id);
        System.out.println(password);
            if (con != null) {
                String sql = "update smbms_user set userPassword = ? where id = ?";
                Object[] params = {password,id};
                execute = BaseDao.executeUpdate(con, sql, params ,pstm);
                BaseDao.close(null, pstm, null);
            }
            return execute;
        }
    //3、根据用户名/角色名获取用户总数
    public int getUserCount(Connection conn, String userName, int userRole) throws SQLException {
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        int count = 0;
        if (conn!=null){
            StringBuffer sql = new StringBuffer();//使用字符串缓冲区，这样就可以动态的在sql后面追加AND条件了
            sql.append("SELECT COUNT(1) COUNT FROM smbms_user u,smbms_role r WHERE u.userRole = r.id");//基本的联表查询SQL语句
            List<Object> list = new ArrayList<Object>();//创建一个list来存储我们要拼接的筛选条件，由于我们不能限制传入的参数的数据类型，所以泛型指定为object
            if (!StringUtils.isNullOrEmpty(userName)){//判断传入的用户名是不是空，如果不是空表明前端指定了按照姓名查询的参数
                sql.append(" AND userName like ?");
                list.add("%"+userName+"%");
            }
            if (userRole>0){
                sql.append(" AND userRole = ?");
                list.add(userRole);
            }
            Object[] params = list.toArray();//获得BaseDao中为pstmt对象设置参数的参数列表
            rs = BaseDao.executeQuery(conn,sql.toString(), params,rs, pstmt);//调用查询定义好的查询方法
            if (rs.next()){//获取查询结果
                count = rs.getInt("COUNT");//COUNT是在SQL语句中为查询结果取的别名
            }
            BaseDao.close(null,pstmt,rs);//关闭资源
        }
        return count;
    }

}


```

##### 3、UserService接口

```java
package com.kwq.service.user;


import com.kwq.pojo.User;

public interface UserService {
    /**
     * 用户登陆身份验证
     * @param userCode：用户账号
     * @param userPassword：用户密码，注意，密码判断我们在service层进行；
     *                      在Dao层只是简单的操作数据库，没有其他的逻辑代码；在servlet层中只是接收和转发请求以及控制视图跳转
     *                      而对于业务层(service)就是用来实现业务逻辑代码的
     * @return
     */
    public User login(String userCode, String userPassword);

    //根据用户id修改密码
    boolean updatePwd(int id, String password);

    //查询记录数
    int getUserCount(String username, int userRole);

}

```

 只看方法3

##### 4、UserServiceImpl接口实现

```java
package com.kwq.service.user;
/**
 * 业务层主要就是编写业务代码，在编写业务代码的时候经常会调用数据库
 * 所以在业务层中需要使用到我们一开始编写好的DAO的代码
 */



import com.kwq.dao.BaseDao;
import com.kwq.dao.user.UserDao;
import com.kwq.dao.user.UserDaoImpl;
import com.kwq.pojo.User;
import org.junit.Test;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class UserServiceImpl implements UserService{
    private UserDao userDao;//业务层需要使用Dao，所以直接将Dao作为一个成员变量来使用

    public UserServiceImpl() {
        this.userDao = new UserDaoImpl();//在业务层被实例化的时候就让它得到Dao对象，后面就可以直接去用
    }


    public User login(String userCode, String userPassword) {
        Connection conn = null;
        User user = null;

        conn = BaseDao.getConnection();//获取数据库连接对象
        //通过业务层调用Dao层
        try {
            user = userDao.getLoginUser(conn,userCode);//调用userDao中的获取用户信息的方法
        } catch (SQLException throwables) {
            throwables.printStackTrace();}
        finally {
            BaseDao.close(conn,null,null);
        }


        return user;
    }
    @Test
    public void test(){
        UserServiceImpl userService = new UserServiceImpl();
        User admin = userService.login("admin","12bfasbka");
        System.out.println(admin.getUserPassword());
    }
    public boolean updatePwd(int id, String password) {
        System.out.println("aa");
        Connection con = null;
        PreparedStatement pstm = null;
        con = BaseDao.getConnection();
        boolean flag = false;
        try {
            if (userDao.updatePwd(con, id, password) > 0) {
                flag = true;
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            BaseDao.close(con, pstm, null);
        }
        return flag;
    }
    //查询记录数

    public int getUserCount(String username, int userRole) {
        Connection con = null;
        int count = 0;
        try {
            con = BaseDao.getConnection();
            count = userDao.getUserCount(con, username, userRole);
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            BaseDao.close(con, null, null);
        }
        return count;
    }

}

```

5、测试



#### 4.获取用户列表

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204262018776.png)

##### 1、UserDao接口

```java
/**
 * 4、用户获取用户列表
 * @param conn：数据库连接对象
 * ===========后面两个参数用于条件查询用户数据
 * @param userName：按照用户名查找
 * @param userRole：按照角色名称查找
 * ===========后面两个参数用于对按照上面条件查询出来的结果进行分页处理
 * @param currentPageNo：翻到第多少页
 * @param pageSize：每一页多少条数据
 * @return：返回满足条件的user对象集合
 */
  //获取用户列表
        List<User> getUserList(Connection con, String userName, int userRole, int currentPageNo, int pageSize) throws Exception;
```

##### 2、UserDaoImpl接口实现

```java
 //查询用户列表
    public List<User> getUserList(Connection con, String userName, int userRole, int currentPageNo, int pageSize) throws Exception {
        PreparedStatement pstm = null;
        ResultSet rs = null;
        List<User> userList = new ArrayList<User>();
        if (con != null) {
            StringBuffer sql = new StringBuffer();
            sql.append("select u.*,r.roleName as userRoleName from smbms_user u,smbms_role r where u.userRole = r.id");
            List<Object> list = new ArrayList<Object>();
            if (!StringUtils.isNullOrEmpty(userName)) {
                sql.append(" and u.userName like ?");
                list.add("%" + userName + "%");
            }
            if (userRole > 0) {
                sql.append(" and u.userRole = ?");
                list.add(userRole);
            }
            //在mysql数据库中，分页使用 limit startIndex，pageSize ; 总数
            sql.append(" order by creationDate DESC limit ?,?");
            currentPageNo = (currentPageNo - 1) * pageSize;
            list.add(currentPageNo);
            list.add(pageSize);

            Object[] params = list.toArray();
//            System.out.println("sql ----> " + sql.toString());
            rs = BaseDao.executeQuery(con,sql.toString(), params, rs,pstm);
            while (rs.next()) {
                User _user = new User();
                _user.setId(rs.getInt("id"));
                _user.setUserCode(rs.getString("userCode"));
                _user.setUserName(rs.getString("userName"));
                _user.setGender(rs.getInt("gender"));
                _user.setBirthday(rs.getDate("birthday"));
                _user.setPhone(rs.getString("phone"));
                _user.setUserRole(rs.getInt("userRole"));
                _user.setUserRoleName(rs.getString("userRoleName"));
                userList.add(_user);
            }
            BaseDao.close(null, pstm, rs);
        }
        return userList;
    }
```

##### 3、UserService接口

```java
/**
 * 4、根据用户名/用户角色名称来查询数据，返回一个User对象集合，而currentPageNo+pageSize用于前端做分页操作
 * @param userName
 * @param userRole
 * @param currentPageNo
 * @param pageSize
 * @return：满足条件+limit的User对象集合
 */
  //根据条件查询用户列表
    List<User> getUserList(String queryUserName, int queryUserRole, int currentPageNo, int pageSize);
```

##### 4、Userservice接口实现

```java
/**
 * 4、根据用户名/用户角色名称来查询数据，返回一个User对象集合，而currentPageNo+pageSize用于前端做分页操作
 * @param userName
 * @param userRole
 * @param currentPageNo
 * @param pageSize
 * @return：满足条件+limit的User对象集合
 */
//根据条件查询用户列表

    public List<User> getUserList(String queryUserName, int queryUserRole, int currentPageNo, int pageSize) {
        Connection con = null;
        List<User> userList = null;
//        System.out.println("queryUserName ----> " + queryUserName);
//        System.out.println("queryUserName ----> " + queryUserRole);
//        System.out.println("currentPageNo ----> " + currentPageNo);
//        System.out.println("pageSize ----> " + pageSize);
        try {
            con = BaseDao.getConnection();
            userList = userDao.getUserList(con, queryUserName, queryUserRole, currentPageNo, pageSize);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            BaseDao.close(con, null, null);
        }
        return userList;
    }
```

##### 5、测试

```java
@Test
public void test(){
    List userList = new UserServiceImpl().getUserList(null,0,2,5);
    //(3-1)*5 = 10,所以展示的是10~14条数据，但是一共只有12条,注意：MYSQL中结果index从0开始
    for (Object o : userList) {
        System.out.println(((User)o).getUserName());
    }
}
```

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204262018148.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204262018364.png)
测试完成！



### 获取角色列表

#### 1、RoleDao接口

```java
public interface RoleDao {
        //获取角色列表
        List<Role> getRoleList(Connection con) throws SQLException;
}
```

#### 2、RoleDaoImpl接口实现

```java
package com.kwq.dao.role;


import com.kwq.dao.BaseDao;
import com.kwq.pojo.Role;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

/**
 * @Description
 * @Author:PrinceHan
 * @CreateTime:2022/3/9 22:18
 */
public class RoleDaoImpl implements RoleDao{

    //获取角色列表

    public List<Role> getRoleList(Connection con) throws SQLException {
        PreparedStatement pstm = null;
        ResultSet rs = null;
        ArrayList<Role> roleList = new ArrayList<Role>();
        if (con != null) {
            String sql = "select * from smbms_role";
            Object[] params = {};
            rs = BaseDao.executeQuery(con, sql,params,rs,pstm);
            while (rs.next()) {
                Role role = new Role();
                role.setId(rs.getInt("id"));
                role.setRoleCode(rs.getString("roleCode"));
                role.setRoleName(rs.getString("roleName"));
                roleList.add(role);
            }
            BaseDao.close(null, pstm, rs);
        }
        return roleList;
    }
}

```

#### 3、RoleService接口

```java
package com.kwq.service.role;


import com.kwq.pojo.Role;

import java.sql.Connection;
import java.util.List;

/**
 * @Description
 * @Author:PrinceHan
 * @CreateTime:2022/3/9 22:22
 */
public interface RoleService {

    List<Role> getRoleList();
}

```

#### 4、RoleService接口实现

```java
package com.kwq.service.role;


import com.kwq.dao.BaseDao;
import com.kwq.dao.role.RoleDao;
import com.kwq.dao.role.RoleDaoImpl;
import com.kwq.pojo.Role;
import org.junit.Test;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;

/**
 * @Description
 * @Author:PrinceHan
 * @CreateTime:2022/3/9 22:22
 */
public class RoleServiceImpl implements RoleService{

    //引入Dao
    private RoleDao roleDao;

    public RoleServiceImpl() {
        roleDao = new RoleDaoImpl();
    }


    public List<Role> getRoleList() {
        Connection con = null;
        List<Role> roleList = null;
        try {
            con = BaseDao.getConnection();
            roleList = roleDao.getRoleList(con);
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            BaseDao.close(con, null, null);
        }
        return roleList;
    }

    @Test
    public void test() {
        RoleServiceImpl roleService = new RoleServiceImpl();
        List<Role> roleList = roleService.getRoleList();
        for (Role role : roleList) {
            System.out.println(role.getRoleName());
        }
    }
}

```

#### 5、测试

```java
@Test
public void test(){
    RoleService roleService = new RoleServiceImpl();
    List<Role> list = roleService.getRoleList();
    for (Role role:list) {
        System.out.println(role.getId()+"\t"+role.getRoleName()+"\t"+role.getRoleCode());
    }
}
```

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271012360.png)

#### 6、编写servlet

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271012650.png)

```java
href="${pageContext.request.contextPath }/jsp/user.do?method=query"

/jsp/user.do
//这个URL映射的servlet我们在前面已经注册过了，所以区别就是它后面跟的参数
    
method=query
这个参数就是在传递指定servlet应该调用的方法
```

 这一次编写的servlet比较的复杂，我们把它单独拎出来看

```java
    //3、按照用户名/职位名称查询用户列表或整表查询
    //【重点&难点】
    public void quary(HttpServletRequest req, HttpServletResponse resp){
        //1、从前端获取数据
        String queryname = req.getParameter("queryname");
        String queryUserRole = req.getParameter("queryUserRole");
        String pageIndex = req.getParameter("pageIndex");//通过隐藏域进行的提交，默认 = 1

        int UserRole = 0;//我们先让UserRole = 0，因为从前端接收到的queryUserRole可能就是一个NULL，此时我们就需要将其指定为0
        int pageSize = 5;//这个数字最好是写在配置文件中，这样以后想要修改一页上面显示的行数，我们就不用再从新编译代码和测试了
        int currentPageNo = 1;//先给当前页设置一个默认的值

        //2、通过判断参数决定哪些请求需要处理
        if (queryname == null){
            queryname = "";//如果前端没有按照用户名查询，我们就将用户名设置""
        }
        if (queryUserRole!=null && queryUserRole!=""){
            UserRole = Integer.parseInt(queryUserRole);//当前端传过来的queryUserRole有数据的时候我们才对其进行解析
        }
        if (pageIndex!=null){
            currentPageNo = Integer.parseInt(pageIndex);
        }

        //3、为了实现分页，需要使用工具类PageSupport并传入总用户数，计算出totalPageCount
        //参数totalCount由getUserCount得出；pageSize是固定死了的；currentPageNo默认设为1
        UserService userService = new UserServiceImpl();
        int totalCount = userService.getUserCount(queryname,UserRole);
        //使用最开始导入的工具类
        PageSupport pageSupport = new PageSupport();
        pageSupport.setPageSize(pageSize);//设置一页多少行数据
        pageSupport.setTotalCount(totalCount);//设置总页数
        pageSupport.setCurrentPageNo(currentPageNo);//设置当前页数


        int totalPageCount = 0;
        totalPageCount = pageSupport.getTotalPageCount();

        //4、控制翻页
        if (currentPageNo<1){//在第一页的时候还想点击上一页
            currentPageNo = 1;
        }else if(currentPageNo>pageSupport.getTotalPageCount()) {//在第最后一页的时候还想点击下一页
            currentPageNo = totalPageCount;
        }

        //5、用户列表展示
        List<User> userList = userService.getUserList(queryname,UserRole,currentPageNo,pageSize);
        //将集合返回给前端进行解析显示
        req.setAttribute("userList",userList);

        //6、角色列表展示
        List<Role> roleList = new RoleServiceImpl().getRoleList();
        req.setAttribute("roleList",roleList);

        //7、将参数返回前端
        req.setAttribute("queryUserName",queryname);
        req.setAttribute("queryUserRole",queryUserRole);
        req.setAttribute("totalPageCount",totalPageCount);
        req.setAttribute("totalCount",totalCount);
        req.setAttribute("currentPageNo",currentPageNo);

        //8、重定向刷新页面
        try {
            System.out.println("=======================进入到servlet，且i调用method = query");
            req.getRequestDispatcher("userlist.jsp").forward(req,resp);
        } catch (ServletException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```

 首先，这个servlet直接和前端的用户管理页面进行交互，且用户管理页面所有展示的数据都是由这个servlet的query()返回的，所以我们应该按照前端素材来编写servlet的query()

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271012663.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271012550.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271012620.png)
​ 通过上面的分析我们可以发现：我们需要**从前端页面获取3个参数，并给前端返回7个参数**

- 获取前端参数
  - queryname：按照姓名查询的姓名
  - queryUserRole：按照职位查询的职位名称
  - pageIndex：当前的页面index
- 传递给前端的参数
  - userList：存储前端展示的用户(user)对象集合
  - roleList：存储前端展示的角色(role)对象集合
  - queryname：再将这个参数传递回去是为了前端搜索之后搜索框中还保留着用户搜索的值
  - queryUserRole：作用同queryname
  - totalCount：总共多少条用户记录
  - currentPageNo：当前所在页数
  - totalPageCount：总页数参数，注意：这个参数来自于工具类pageSupport
    ![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271012582.png)

#### 7、测试

![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271013086.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271013426.png)
![img](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271013894.png)
测试完成！



## 文件上传：

### 1、文件上传准备

![image-20220427112803064](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271128357.png)

IDEA中建个空项目，在其中new一个模块file

采用apache的开源工具common-fileupload文件上传组件

common-fileupload依赖于commin-io这个jar包

直接去Maven仓库搜索、下载即可！

模块中新建lib目录，把两个包复制过去，右键add as Library即可~

![image-20220427112856618](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271128998.png)

> 重点！！！
>
> 1、ServletFileUpload：负责处理上传的文件数据，并将表单中每个输入项封装成一个FileItem对象
>
> 2、在使用ServletFileUpload对象解析请求时需要DiskFileItemFactory对象
>
> 3、所以，我们需要在进行解析工作前构造好DiskFileItemFactory对象，通过ServletFileUpload对象的构造方法或setFileItemFactory()方法设置ServletFileUpload对象的FileItemFactory属性



### 2、正戏开始

1、IDEA中建个空项目，里面建个maven项目（file），添加webapp支持

2、在file中建个lib文件夹，把fileupload和common-io的jar包拉进去，Add as Library，不行的话在结构导入也可以！

在HTML页面input 必须有 name <input type="file" name="filename">

**表单如果包含一个文件上传输入项的话，这个表单的enctype属性就必须设置为multipart/form-data**

浏览器表单的类型如果为multipart/form-data , 在服务器端想获取数据就要通过流。

> 【常用方法介绍】

```java

//isFormField方法用于判断FileItem类对象封装的数据是一个普通文本表单
//还是一个文件表单，如果是普通表单字段则返回true，否则返回false
boolean isFormField();

//getFieldName方法用于返回表单标签name属性的值。
String getFieldName();

//getString方法用于将FileItem对象中保存的数据流内容以一个字符串返回
String getString();

//getName方法用于获得文件上传字段中的文件名。
String getName();

//以流的形式返回上传文件的数据内容。
InputStream getInputStream()

//delete方法用来清空FileItem类对象中存放的主体内容
//如果主体内容被保存在临时文件中，delete方法将删除该临时文件。
void delete();
```

**ServletFileUpload 类**
ServletFileUpload负责处理上传的文件数据,并**将表单中每个输入项封装成一个FileItem对象中** . 使用其**parseRequest(HttpServletRequest)** 方法可以将通过表单中每一个HTML标签提交的数据封装成一个FileItem对象，然后以List列表的形式返回。使用该方法处理上传文件简单易用。

**代码编写UploadServlet**

```java
public class UploadServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        try {
            //判断上传的文件是普通的表单还是带文件的表单
            if (!ServletFileUpload.isMultipartContent(request)) {
                return;//如果是普通文件，我们可以直接返回
            } //如果通过了这个if，说明我们的表单是带文件上传的；

            //创建上传文件的保存路径，建议在WEB-INF路径下，安全，用户无法直接访问上传的文件； /代表tomcat启动后生成的文件夹的WEB-INF的上级目录
            String uploadPath = this.getServletContext().getRealPath("/WEB-INF/upload");
            File uploadFile = new File(uploadPath);
            if (!uploadFile.exists()) {//如果目录不存在，创建这样一个目录；
                uploadFile.mkdir();
            }

            //临时路径，假如文件超过了预期的大小，我们就把他放到一个临时文件中，过几天自动删除，或者提醒用户转存为永久
            String tmpPath = this.getServletContext().getRealPath("/WEB-INF/tmp");
            File file = new File(tmpPath);
            if (!file.exists()) {//如果目录不存在，创建这样一个目录；
                file.mkdir();
            }

            //处理上传的文件，一般都需要通过流来获取，我们可以使用request.getInputStream(),原生态的文件上传流获取，十分麻烦
            //但是我们都建议使用 Apache的文件上传组件来实现，common-fileupload，它需要依赖于 commons-io组件；

            //1.创建DiskFileItemFactory对象，处理文件上传路径或者大小限制的；
            DiskFileItemFactory factory = new getDiskFileItemFactory(file);
            //2.获取ServletFileUpload
            ServletFileUpload upload = new getServletFileUpload(factory);
            //3.处理上传的文件
            String msg = uploadParseRequest(upload, request, uploadPath);

            //servlet请求转发消息
            request.setAttribute("msg",msg);
            request.getRequestDispatcher("msg.jsp").forward(request,response);

        } catch (FileUploadException e) {
            e.printStackTrace();
        }

    }


    public static DiskFileItemFactory getDiskFileItemFactory(File file) {
        DiskFileItemFactory factory = new DiskFileItemFactory();
        //通过这个工厂设置一个缓冲区，当上传的文件大于这个缓冲区的时候，将他放到临时文件中；
        factory.setSizeThreshold(1024 * 1024); //缓存区大小为1M
        factory.setRepository(file);//临时目录的保存目录，需要一个File
        return factory;
    }

    public static ServletFileUpload getServletFileUpload(DiskFileItemFactory factory) {
        ServletFileUpload upload = new ServletFileUpload(factory);
        //监听文件上传进度；
        upload.setProgressListener(new ProgressListener() {
            @Override
            //pBytesRead:已经读取到的文件大小
            //pContentLength ： 文件大小
            public void update(long pBytesRead, long pContentLength, int pItems) {
                System.out.println("总大小：" + pContentLength + "已上传：" + pBytesRead);
            }
        });

        //处理乱码问题
        upload.setHeaderEncoding("UTF-8");
        //设置单个文件的最大值
        upload.setFileSizeMax(1024 * 1024 * 10);
        //设置总共能够上传文件的大小
        //1024 = 1kb * 1024 = 1M * 10 = 10M
        upload.setSizeMax(1024 * 1024 * 10);

        return upload;
    }


    public static String uploadParseRequest(ServletFileUpload upload,HttpServletRequest request,String uploadPath)
            throws FileUploadException, IOException {

        String msg = "";

        //3.把前端请求解析，封装成一个FileItem对象
        List<FileItem> fileItems = upload.parseRequest(request);
        for (FileItem fileItem : fileItems) {
            if (fileItem.isFormField()){ //判断上传的文件是普通的表单还是带文件的表单
                //getFieldName指的是前端表单控件的name；
                String name = fileItem.getFieldName();
                String value = fileItem.getString("UTF-8"); //处理乱码
                System.out.println(name+":"+value);
            }else { //判断它是上传的文件

                //=======================处理文件===============================//

                //拿到文件名字
                String uploadFileName = fileItem.getName();
                System.out.println("上传的文件名："+uploadFileName);

                if (uploadFileName.trim().equals("")||uploadFileName==null){
                    continue;
                }

                //获得上传的文件名  /images/girl/paojie.png
                String fileName = uploadFileName.substring(uploadFileName.lastIndexOf("\\") + 1);
                //获得文件的后缀名
                String fileExtName = uploadFileName.substring(uploadFileName.lastIndexOf(".") + 1);
                    /*
                        如果文件后缀名 fileExtName 不是我们所需要的
                        就直接return，不处理，告诉用户文件类型不对。
                    */

                System.out.println("文件信息 [件名："+fileName+"---文件类型"+fileExtName+"]");

                //可以使用UUID（唯一识别的通用码），保证文件名唯一；
                //UUID.randomUUID()，随机生一个唯一识别的通用码；
                String uuidPath = UUID.randomUUID().toString();

                //=======================处理文件完毕===============================//

                //存到哪？ uploadPath
                //文件真实存在的路径 realPath
                String realPath =   uploadPath+"\\"+uuidPath;
                //给每个文件创建一个对应的文件夹
                File realPathFile = new File(realPath);
                if (!realPathFile.exists()){
                    realPathFile.mkdir();
                }

                //=======================存放地址完毕===============================//

                //获得文件上传的流
                InputStream inputStream = fileItem.getInputStream();

                //创建一个文件输出流
                //realPath = 真实的文件夹；
                //差了一个文件; 加上输出文件的名字+"/"+uuidFileName
                FileOutputStream fos = new FileOutputStream(realPath+"\\"+fileName);

                //创建一个缓冲区
                byte[] buffer = new byte[1024*1024];

                //判断是否读取完毕
                int len = 0;
                //如果大于0说明还存在数据；
                while ((len=inputStream.read(buffer))>0){
                    fos.write(buffer,0,len);
                }

                //关闭流
                fos.close();
                inputStream.close();

                msg = "文件上传成功！";
                fileItem.delete(); //上传成功，清除临时文件
                //=======================文件传输完毕===============================//
            }
        }

        return msg;
    }
}
```

update.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>文件上传</title>
</head>
<body>
    <form action="${pageContext.request.contextPath}/upload.do" enctype="multipart/form-data" method="post">
        上传用户：<input type="text" name="username"><br/>
        上传文件1：<input type="file" name="file1"><br/>
        上传文件2：<input type="file" name="file2"><br/>
        <input type="submit" value="提交">
    </form>
</body>
</html>
```

msg.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>消息提示</title>
</head>
<body>
    ${msg}
</body>
</html>
```

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>upload</servlet-name>
        <servlet-class>com.kwq.servlet.UploadFileServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>upload</servlet-name>
        <url-pattern>/upload.do</url-pattern>
    </servlet-mapping>
</web-app>
```

> 测试，上传文件

![image-20220427154850611](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271548099.png)

![image-20220427160545083](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271605403.png)

`这个out文件夹是启动tomcat就会生成的，而其中的file_war_exploded则代表当前应用`。

查看是否我们本地存在上传的文件

![image-20220427155732152](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271557304.png)

![image-20220427155655717](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271556099.png)

![image-20220427155809514](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271558775.png)

我们的项目结构：

![image-20220427191452637](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271914787.png)

![image-20220427191936096](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271919361.png)

![image-20220427191947959](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271919173.png)

> bug提醒：

需要将这两个jar放在tomcat的lib目录下，不然tomcat启动不了

![image-20220427155530283](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204271555495.png)

## 电子邮件

要在网络上实现邮件功能，必须要有专门的邮件服务器。

这些邮件服务器类似于现实生活中的邮局，它主要负责接收用户投递过来的邮件，并把邮件投递到邮件接收者的电子邮箱中。

[SMTP](https://so.csdn.net/so/search?q=SMTP&spm=1001.2101.3001.7020)服务器地址：一般是 [smtp.xxx.com](http://smtp.xxx.com/)，[比如163邮箱是smtp.163.com](http://xn--163smtp-i22mg65gz1lpq6bfw2c.163.com/)，[qq邮箱是smtp.qq.com](http://xn--qqsmtp-j02m143lvq1b.qq.com/)。

电子邮箱(E-Mail地址)的获得需要在邮件服务器上进行申请。比如我们要使用QQ邮箱，就需要开通邮箱功能；
![在这里插入图片描述](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204272028183.png)

传输协议

SMTP协议
发送邮件：

我们通常把处理用户smtp请求(邮件发送请求)的服务器称之为SMTP服务器(邮件发送服务器)。

POP3协议
接收邮件：

我们通常把处理用户pop3请求(邮件接收请求)的服务器称之为POP3服务器(邮件接收服务器)。

邮件收发原理  `里面的邮箱空间指的就是我们注册的邮箱在服务器的某块区域`
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019071910351769.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNzM0,size_16,color_FFFFFF,t_70)

1. 大狂神通过smtp协议连接到Smtp服务器，然后发送一封邮件给网易的邮件服务器
2. 网易分析发现需要去QQ的邮件服务器，通过smtp协议将邮件转投给QQ的Smtp服务器
3. QQ将接收到的邮件存储在24736743@qq.com这个邮件账号的空间中
4. 小狂神通过Pop3协议连接到Pop3服务器收取邮件
5. 从24736743@qq.com这个邮件账号的空间中取出邮件
6. Pop3服务器将取出来的邮件送到小狂神手中

【注意】有可能你收件人地址，发件人地址等信息都正确了，控制台也打印了正确的信息，但是在收件箱就是收不到信息。这是因为可能收件箱服务器拒收了你发的邮件（比如认为你的邮件是广告），这时候可能在垃圾箱里能找到，可能找不到。解决办法是重复的邮件内容不要多次发送，或者更换收件箱试试

### Java发送邮件

### 概述

我们将用代码完成邮件的发送。这在实际项目中应用的非常广泛，比如注册需要发送邮件进行账号激活，再比如OA项目中利用邮件进行任务提醒等等。

使用Java发送 E-mail 十分简单，但是首先你应该准备 JavaMail API 和Java Activation Framework 。

得到两个jar包：

- mail.jar
- activation.jar

JavaMail 是sun公司（现以被甲骨文收购）为方便Java开发人员在应用程序中实现邮件发送和接收功能而提供的一套标准开发包，它支持一些常用的邮件协议，如前面所讲的SMTP，POP3，IMAP,还有MIME等。我们在使用JavaMail API 编写邮件时，无须考虑邮件的底层实现细节，只要调用JavaMail 开发包中相应的API类就可以了。

我们可以先尝试发送一封简单的邮件，确保电脑可以连接网络。

- 创建包含邮件服务器的网络连接信息的Session对象。
- 创建代表邮件内容的Message对象
- 创建Transport对象，连接服务器，发送Message，关闭连接

主要有四个核心类，我们在编写程序时，记住这四个核心类，就很容易编写出Java邮件处理程序。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204272028988.png)

### 纯文本邮件

创建一个空的java项目

![image-20220427204757771](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204272047091.png)

先在项目中导入jar包

![image-20220427205233952](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204272052356.png)

QQ邮箱中获取对应的权限

QQ邮箱需要安全验证，我们需要获取他对应的权限；

QQ邮箱–>邮箱设置–>账户

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204272028562.png)
![image-20220427205452857](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204272054154.png)

编写测试代码

```java
package com.kwq;

import com.sun.mail.util.MailSSLSocketFactory;

import java.util.*;
import javax.mail.*;
import javax.mail.internet.*;

public class SendEmail {

    public static void main(String[] args) throws Exception {

        Properties prop = new Properties();
        prop.setProperty("mail.host", "smtp.qq.com");  //设置QQ邮件服务器
        prop.setProperty("mail.transport.protocol", "smtp"); // 邮件发送协议
        prop.setProperty("mail.smtp.auth", "true"); // 需要验证用户名密码

        // 关于QQ邮箱，还要设置SSL加密，加上以下代码即可
        MailSSLSocketFactory sf = new MailSSLSocketFactory();
        sf.setTrustAllHosts(true);
        prop.put("mail.smtp.ssl.enable", "true");
        prop.put("mail.smtp.ssl.socketFactory", sf);

        //使用JavaMail发送邮件的5个步骤

        //创建定义整个应用程序所需的环境信息的 Session 对象

        Session session = Session.getDefaultInstance(prop, new Authenticator() {
            public PasswordAuthentication getPasswordAuthentication() {
                //发件人邮件用户名、授权码
                return new PasswordAuthentication("347858300@qq.com", "ddfhwnifziggcbbb");
            }
        });


        //开启Session的debug模式，这样就可以查看到程序发送Email的运行状态
        session.setDebug(true);

        //2、通过session得到transport对象
        Transport ts = session.getTransport();

        //3、使用邮箱的用户名和授权码连上邮件服务器
        ts.connect("smtp.qq.com", "347858300@qq.com", "ddfhwnifziggcbbb");

        //4、创建邮件

        //创建邮件对象
        MimeMessage message = new MimeMessage(session);

        //指明邮件的发件人
        message.setFrom(new InternetAddress("347858300@qq.com"));

        //指明邮件的收件人，现在发件人和收件人是一样的，那就是自己给自己发
        message.setRecipient(Message.RecipientType.TO, new InternetAddress("347858300@qq.com"));

        //邮件的标题
        message.setSubject("只包含文本的简单邮件");

        //邮件的文本内容
        message.setContent("你好啊！", "text/html;charset=UTF-8");

        //5、发送邮件
        ts.sendMessage(message, message.getAllRecipients());

        ts.close();
    }

}

```

> 查看邮箱

![image-20220427205820723](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204272058127.png)

> 把这个改成对应的邮箱地址，就能实现跨邮箱发送

![image-20220427210816824](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204272108918.png)

### 带图片和附件的邮件

先认识两个类一个名词：

`MIME（多用途互联网邮件扩展类型）`

MimeBodyPart类

javax.mail.internet.MimeBodyPart类 表示的是一个MIME消息，它和MimeMessage类一样都是从Part接口继承过来。

MimeMultipart类

javax.mail.internet.MimeMultipart是抽象类 Multipart的实现子类,它用来组合多个MIME消息。一个MimeMultipart对象可以包含`多个代表MIME消息的MimeBodyPart对象。`  ==附件就是.md等 内嵌资源就是.png等==

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204272205023.png)

**创建包含内嵌图片的邮件**

前面的例子中是单独的使用HTML或者是纯文本内容，`但是有时候我们需要在纯文本中使用内嵌的方式显示一些图片，因此就要将纯文本和内嵌图片单独存放在MimeBodyPart中然后再将其存放在一个Mimemultipart对象中即可。`

```java
package com.kwq;

import com.sun.mail.util.MailSSLSocketFactory;

import javax.activation.DataHandler;
import javax.activation.FileDataSource;
import javax.mail.*;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeBodyPart;
import javax.mail.internet.MimeMessage;
import javax.mail.internet.MimeMultipart;
import java.util.Properties;

public class SendImageEmail {
    public static void main(String[] args) throws Exception {

        Properties prop = new Properties();
        prop.setProperty("mail.host", "smtp.qq.com");  //设置QQ邮件服务器
        prop.setProperty("mail.transport.protocol", "smtp"); // 邮件发送协议
        prop.setProperty("mail.smtp.auth", "true"); // 需要验证用户名密码

        // 关于QQ邮箱，还要设置SSL加密，加上以下代码即可
        MailSSLSocketFactory sf = new MailSSLSocketFactory();
        sf.setTrustAllHosts(true);
        prop.put("mail.smtp.ssl.enable", "true");
        prop.put("mail.smtp.ssl.socketFactory", sf);

        //使用JavaMail发送邮件的5个步骤

        //1、创建定义整个应用程序所需的环境信息的 Session 对象
        Session session = Session.getDefaultInstance(prop, new Authenticator() {
            public PasswordAuthentication getPasswordAuthentication() {
                //发件人邮件用户名、授权码
                return new PasswordAuthentication("347858300@qq.com", "jiewyzmibennbgjc");
            }
        });


        //开启Session的debug模式，这样就可以查看到程序发送Email的运行状态
        session.setDebug(true);

        //2、通过session得到transport对象
        Transport ts = session.getTransport();

        //3、使用邮箱的用户名和授权码连上邮件服务器
        ts.connect("smtp.qq.com", "347858300@qq.com", "jiewyzmibennbgjc");

        //4、创建邮件

        //创建邮件
        MimeMessage message = new MimeMessage(session);

        // 设置邮件的基本信息
        //发件人
        message.setFrom(new InternetAddress("347858300@qq.com"));
        //收件人
        message.setRecipient(Message.RecipientType.TO, new InternetAddress("347858300@qq.com"));
        //邮件标题
        message.setSubject("带图片的邮件");

        // 准备邮件数据

        // 准备图片数据
        MimeBodyPart image = new MimeBodyPart();//图片需要经过数据处理。。。DataHandler:数据处理
        DataHandler dh = new DataHandler(new FileDataSource("D:\\java文件夹汇总\\javaWeb\\main-java\\src\\com\\kwq\\1.png"));
        image.setDataHandler(dh);//在我们的body中放入这个处理的图片数据
        image.setContentID("bz.jpg");//给图片设置一个ID

        // 准备正文数据
        MimeBodyPart text = new MimeBodyPart();
        text.setContent("这是一封邮件正文带图片<img src='cid:bz.jpg'>的邮件", "text/html;charset=UTF-8");//cid(对应的是setContentID缩写):bz.jsp

        // 描述数据关系，multi封装图片数据和正文数据,mixed最大封装，
        MimeMultipart mm = new MimeMultipart();
        mm.addBodyPart(text);
        mm.addBodyPart(image);
        mm.setSubType("related");//可用mixed参数

        //设置到消息中，保存修改
        message.setContent(mm);
        message.saveChanges();

        //5.发送邮件
        ts.sendMessage(message, message.getAllRecipients());
        ts.close();
    }
}

```

> 测试：

![image-20220427224823830](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204272248192.png)

### 带图片和附件的复杂邮件发送

```java
package com.kwq;

import com.sun.mail.util.MailSSLSocketFactory;

import javax.activation.DataHandler;
import javax.activation.FileDataSource;
import javax.mail.*;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeBodyPart;
import javax.mail.internet.MimeMessage;
import javax.mail.internet.MimeMultipart;
import java.security.GeneralSecurityException;
import java.util.Properties;

public class SendFileMail {
    public static void main(String[] args) throws MessagingException, GeneralSecurityException {

        //创建一个配置文件保存并读取信息
        Properties properties = new Properties();

        //设置qq邮件服务器
        properties.setProperty("mail.host","smtp.qq.com");
        //设置发送的协议
        properties.setProperty("mail.transport.protocol","smtp");
        //设置用户是否需要验证
        properties.setProperty("mail.smtp.auth", "true");


        //=================================只有QQ存在的一个特性，需要建立一个安全的链接
        // 关于QQ邮箱，还要设置SSL加密，加上以下代码即可
        MailSSLSocketFactory sf = new MailSSLSocketFactory();
        sf.setTrustAllHosts(true);
        properties.put("mail.smtp.ssl.enable", "true");
        properties.put("mail.smtp.ssl.socketFactory", sf);

        //=================================准备工作完毕

        //1.创建一个session会话对象；
        Session session = Session.getDefaultInstance(properties, new Authenticator() {
            @Override
            protected PasswordAuthentication getPasswordAuthentication() {
                return new PasswordAuthentication("347858300@qq.com", "jiewyzmibennbgjc");
            }
        });

        //可以通过session开启Dubug模式，查看所有的过程
        session.setDebug(true);


        //2.获取连接对象，通过session对象获得Transport，需要捕获或者抛出异常；
        Transport tp = session.getTransport();

        //3.连接服务器,需要抛出异常；
        tp.connect("smtp.qq.com","347858300@qq.com","jiewyzmibennbgjc");

        //4.连接上之后我们需要发送邮件；
        MimeMessage mimeMessage = imageMail(session);

        //5.发送邮件
        tp.sendMessage(mimeMessage,mimeMessage.getAllRecipients());

        //6.关闭连接
        tp.close();

    }


    public static MimeMessage imageMail(Session session) throws MessagingException {

        //消息的固定信息
        MimeMessage mimeMessage = new MimeMessage(session);

        //邮件发送人
        mimeMessage.setFrom(new InternetAddress("347858300@qq.com"));
        //邮件接收人，可以同时发送给很多人，我们这里只发给自己；
        mimeMessage.setRecipient(Message.RecipientType.TO, new InternetAddress("347858300@qq.com"));
        mimeMessage.setSubject("我也不知道是个什么东西就发给你了"); //邮件主题


        /*
        编写邮件内容
        1.图片
        2.附件
        3.文本
         */

        //图片
        MimeBodyPart body1 = new MimeBodyPart();
        body1.setDataHandler(new DataHandler(new FileDataSource("D:\\java文件夹汇总\\javaWeb\\main-java\\src\\com\\kwq\\1.png")));
        body1.setContentID("yhbxb.png"); //图片设置ID

        //文本
        MimeBodyPart body2 = new MimeBodyPart();
        body2.setContent("请注意，我不是广告<img src='cid:yhbxb.png'>","text/html;charset=utf-8");

        //附件
        MimeBodyPart body3 = new MimeBodyPart();
        body3.setDataHandler(new DataHandler(new FileDataSource("D:\\java文件夹汇总\\javaWeb\\main-java\\src\\com\\kwq\\Spring面经.md")));
        body3.setFileName("log4j.properties"); //附件设置名字

        MimeBodyPart body4 = new MimeBodyPart();
        body4.setDataHandler(new DataHandler(new FileDataSource("D:\\java文件夹汇总\\javaWeb\\main-java\\src\\com\\kwq\\2022暑期面试八股文.md")));
        body4.setFileName(""); //附件设置名字

        //拼装邮件正文内容
        MimeMultipart multipart1 = new MimeMultipart();
        multipart1.addBodyPart(body1);
        multipart1.addBodyPart(body2);
        multipart1.setSubType("related"); //1.文本和图片内嵌成功！

        //new MimeBodyPart().setContent(multipart1); //将拼装好的正文内容设置为主体
        MimeBodyPart contentText =  new MimeBodyPart();
        contentText.setContent(multipart1);

        //拼接附件
        MimeMultipart allFile =new MimeMultipart();
        allFile.addBodyPart(body3); //附件
        allFile.addBodyPart(body4); //附件
        allFile.addBodyPart(contentText);//正文
        allFile.setSubType("mixed"); //正文和附件都存在邮件中，所有类型设置为mixed；


        //放到Message消息中
        mimeMessage.setContent(allFile);
        mimeMessage.saveChanges();//保存修改


        return mimeMessage;

    }

}

```

> 输出

![image-20220427225648872](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204272256123.png)

`修改上面代码：收到的邮件和上图一模一样`

```
package com.kwq;

import com.sun.mail.util.MailSSLSocketFactory;

import javax.activation.DataHandler;
import javax.activation.FileDataSource;
import javax.mail.*;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeBodyPart;
import javax.mail.internet.MimeMessage;
import javax.mail.internet.MimeMultipart;
import java.security.GeneralSecurityException;
import java.util.Properties;

public class SendFileMail {
    public static void main(String[] args) throws MessagingException, GeneralSecurityException {

        //创建一个配置文件保存并读取信息
        Properties properties = new Properties();

        //设置qq邮件服务器
        properties.setProperty("mail.host","smtp.qq.com");
        //设置发送的协议
        properties.setProperty("mail.transport.protocol","smtp");
        //设置用户是否需要验证
        properties.setProperty("mail.smtp.auth", "true");


        //=================================只有QQ存在的一个特性，需要建立一个安全的链接
        // 关于QQ邮箱，还要设置SSL加密，加上以下代码即可
        MailSSLSocketFactory sf = new MailSSLSocketFactory();
        sf.setTrustAllHosts(true);
        properties.put("mail.smtp.ssl.enable", "true");
        properties.put("mail.smtp.ssl.socketFactory", sf);

        //=================================准备工作完毕

        //1.创建一个session会话对象；
        Session session = Session.getDefaultInstance(properties, new Authenticator() {
            @Override
            protected PasswordAuthentication getPasswordAuthentication() {
                return new PasswordAuthentication("347858300@qq.com", "jiewyzmibennbgjc");
            }
        });

        //可以通过session开启Dubug模式，查看所有的过程
        session.setDebug(true);


        //2.获取连接对象，通过session对象获得Transport，需要捕获或者抛出异常；
        Transport tp = session.getTransport();

        //3.连接服务器,需要抛出异常；
        tp.connect("smtp.qq.com","347858300@qq.com","jiewyzmibennbgjc");

        //4.连接上之后我们需要发送邮件；
        MimeMessage mimeMessage = imageMail(session);

        //5.发送邮件
        tp.sendMessage(mimeMessage,mimeMessage.getAllRecipients());

        //6.关闭连接
        tp.close();

    }


    public static MimeMessage imageMail(Session session) throws MessagingException {

        //消息的固定信息
        MimeMessage mimeMessage = new MimeMessage(session);

        //邮件发送人
        mimeMessage.setFrom(new InternetAddress("347858300@qq.com"));
        //邮件接收人，可以同时发送给很多人，我们这里只发给自己；
        mimeMessage.setRecipient(Message.RecipientType.TO, new InternetAddress("347858300@qq.com"));
        mimeMessage.setSubject("我也不知道是个什么东西就发给你了"); //邮件主题


        /*
        编写邮件内容
        1.图片
        2.附件
        3.文本
         */

        //图片
        MimeBodyPart body1 = new MimeBodyPart();
        body1.setDataHandler(new DataHandler(new FileDataSource("D:\\java文件夹汇总\\javaWeb\\main-java\\src\\com\\kwq\\1.png")));
        body1.setContentID("yhbxb.png"); //图片设置ID

        //文本
        MimeBodyPart body2 = new MimeBodyPart();
        body2.setContent("请注意，我不是广告<img src='cid:yhbxb.png'>","text/html;charset=utf-8");

        //附件
        MimeBodyPart body3 = new MimeBodyPart();
        body3.setDataHandler(new DataHandler(new FileDataSource("D:\\java文件夹汇总\\javaWeb\\main-java\\src\\com\\kwq\\Spring面经.md")));
        body3.setFileName("log4j.properties"); //附件设置名字

        MimeBodyPart body4 = new MimeBodyPart();
        body4.setDataHandler(new DataHandler(new FileDataSource("D:\\java文件夹汇总\\javaWeb\\main-java\\src\\com\\kwq\\2022暑期面试八股文.md")));
        body4.setFileName(""); //附件设置名字

        //拼装邮件正文内容
        MimeMultipart multipart1 = new MimeMultipart();
        multipart1.addBodyPart(body1);
        multipart1.addBodyPart(body2);
        multipart1.addBodyPart(body3);
        multipart1.addBodyPart(body4);
        multipart1.setSubType("mixed"); //1.文本和图片内嵌成功！

       /* //new MimeBodyPart().setContent(multipart1); //将拼装好的正文内容设置为主体
        MimeBodyPart contentText =  new MimeBodyPart();
        contentText.setContent(multipart1);

        //拼接附件
        MimeMultipart allFile =new MimeMultipart();
        allFile.addBodyPart(body3); //附件
        allFile.addBodyPart(body4); //附件
        allFile.addBodyPart(contentText);//正文
        allFile.setSubType("mixed"); //正文和附件都存在邮件中，所有类型设置为mixed；

*/
        //放到Message消息中
        mimeMessage.setContent(multipart1);
        mimeMessage.saveChanges();//保存修改


        return mimeMessage;

    }

}
```

> 项目结构

![image-20220427225829326](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204272258658.png)

### JavaWeb发送邮件

现在很多的网站都提供有用户注册功能， 通常我们注册成功之后就会收到一封来自注册网站的邮件。邮件里面的内容可能包含了我们的注册的用户名和密码以及一个激活账户的超链接等信息。今天我们也来实现一个这样的功能，用户注册成功之后，就将用户的注册信息以Email的形式发送到用户的注册邮箱当中，实现发送邮件功能就得借助于JavaMail了。

代码实现：

1. 新建一个JavaWeb项目(`maven空项目,添加web支持也可以直接使用maven模板`)，配置tomcat

选择maven模板的结构（`会在tomcat的webapp目录下生成一个对应的文件夹代表当前应用`）

==带有webapp支持的maven模板原始结构：==

![image-20220428094336732](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204280943055.png)

完善项目结构：

![image-20220428094612302](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204280946464.png)

==项目结构==

![image-20220428003102685](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204280031851.png)

![image-20220428093136124](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204280931463.png)

> 不选择模板的结构(`不会在tomcat的webapp下生成该应用的目录`)

==不选maven模板的初始化结构==

![image-20220428093740504](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204280937748.png)

==添加web框架支持后==

![image-20220428093824717](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204280938922.png)

==项目结构==

![image-20220428003201431](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204280032576.png)

> pom

```
<dependencies>
  <dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>RELEASE</version>
    <scope>compile</scope>
  </dependency>
  <dependency>
    <groupId>com.sun.mail</groupId>
    <artifactId>jakarta.mail</artifactId>
    <version>1.6.7</version>
  </dependency>
  <!--jsp的依赖-->
  <dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>servlet-api</artifactId>
    <version>2.5</version>
  </dependency>
  <!-- https://mvnrepository.com/artifact/javax.activation/activation -->
  <dependency>
    <groupId>javax.activation</groupId>
    <artifactId>activation</artifactId>
    <version>1.1.1</version>
  </dependency>

</dependencies>
```

2.编写对应的用户实体类

```java
package com.kwq.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String username;
    private String password;
    private String email;
}

```

3.前端注册页面index.jsp

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
  <title>注册</title>
</head>
<body>
<%--/registerServlet.do对应servlet--%>
<form action="${pageContext.request.contextPath}/RegisterServlet.do" method="post">
  用户名：<input type="text" name="username"><br/>
  密码：<input type="password" name="password"><br/>
  邮箱：<input type="text" name="email"><br/>
  <input type="submit" value="注册">
</form>

</body>
</html>
```

4.邮件发送工具类

```java
package com.kwq.utils;

import com.kwq.pojo.User;
import com.sun.mail.util.MailSSLSocketFactory;

import javax.mail.*;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
import java.util.Properties;

public class Sendmail extends Thread {

    //用于给用户发送邮件的邮箱
    private String from = "347858300@qq.com";
    //邮箱的用户名
    private String username = "柯维钦";
    //邮箱的密码
    private String password = "mfowxggcftigbhec";
    //发送邮件的服务器地址
    private String host = "smtp.qq.com";

    private User user;
    public Sendmail(User user){
        this.user = user;
    }

    //重写run方法的实现，在run方法中发送邮件给指定的用户
    @Override
    public void run() {
        try{
            Properties prop = new Properties();
            prop.setProperty("mail.host", host);
            prop.setProperty("mail.transport.protocol", "smtp");
            prop.setProperty("mail.smtp.auth", "true");

            // 关于QQ邮箱，还要设置SSL加密，加上以下代码即可
            MailSSLSocketFactory sf = new MailSSLSocketFactory();
            sf.setTrustAllHosts(true);
            prop.put("mail.smtp.ssl.enable", "true");
            prop.put("mail.smtp.ssl.socketFactory", sf);

            //1、创建定义整个应用程序所需的环境信息的 Session 对象
            Session session = Session.getDefaultInstance(prop, new Authenticator() {
                public PasswordAuthentication getPasswordAuthentication() {
                    //发件人邮件用户名、授权码
                    return new PasswordAuthentication("347858300@qq.com", "mfowxggcftigbhec");
                }
            });

            //开启Session的debug模式，这样就可以查看到程序发送Email的运行状态
            session.setDebug(true);

            //2、通过session得到transport对象
            Transport ts = session.getTransport();

            //3、使用邮箱的用户名和授权码连上邮件服务器
            ts.connect(host, username, password);

            //4、创建邮件
            MimeMessage message = new MimeMessage(session);
            message.setFrom(new InternetAddress(from)); //发件人
            message.setRecipient(Message.RecipientType.TO, new InternetAddress(user.getEmail())); //收件人
            message.setSubject("用户注册邮件"); //邮件的标题

            String info = "恭喜您注册成功，您的用户名：" + user.getUsername() + ",您的密码：" + user.getPassword() + "，请妥善保管，如有问题请联系网站客服!!";

            message.setContent(info, "text/html;charset=UTF-8");
            message.saveChanges();

            //发送邮件
            ts.sendMessage(message, message.getAllRecipients());
            ts.close();
        }catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}

```

5,servlet

```
package com.kwq.servlet;


import com.kwq.pojo.User;
import com.kwq.utils.Sendmail;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class Register extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        try {
            //接收用户请求，封装成对象
            String username = request.getParameter("username");
            String password = request.getParameter("password");
            String email = request.getParameter("email");
            User user = new User(username,password,email);
			//3s钟原则，我们应当在用户点击某个功能之后三秒内给用户一个页面响应，否则用户百分之99的可能关掉此网页
            //写完工具类后，用户注册成功之后，给用户发送一封邮件
            //我们使用线程（发送的同时返回页面）来专门发送邮件，防止出现耗时，和网站注册人数过多的情况；
            Sendmail send = new Sendmail(user);
            //启动线程，线程启动之后就会执行run方法来发送邮件
            send.start();

            //注册用户
            request.setAttribute("message", "注册成功，我们已经发了一封带了注册信息的电子邮件，请查收！如网络不稳定，可能过会儿才能收到！！");
            //和上面的发送邮件同时进行，提高用户体验
            request.getRequestDispatcher("info.jsp").forward(request, response);
        } catch (Exception e) {
            e.printStackTrace();
            request.setAttribute("message", "注册失败！！");
            request.getRequestDispatcher("info.jsp").forward(request, response);
        }
    }


    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

6.web.xml中注册

```
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         metadata-complete="true">
    <servlet>
        <servlet-name>re</servlet-name>
        <servlet-class>com.kwq.servlet.Register</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>re</servlet-name>
        <url-pattern>/RegisterServlet.do</url-pattern>
    </servlet-mapping>


</web-app>
```

> info.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>提示信息</title>
</head>
<body>
${message}
</body>
</html>
```

测试输出：

![image-20220428001916277](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204280019396.png)

![image-20220428001923714](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204280019841.png)

![image-20220428001903131](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204280019374.png)

### Springboot中实现发送邮件功能

> 新建一个springboot项目：

![image-20220428095554245](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204280955444.png)

![image-20220428095656809](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204280956058.png)

> 导入依赖

```pom
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

> 查看依赖   ==发现就是封装了我们之前创建maven实现邮件发送用的依赖==

![image-20220428100028102](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204281000331.png)

> 配置文件

```
# 应用名称
spring.application.name=springboot-mail

spring.mail.username=347858300@qq.com
spring.mail.password=mfowxggcftigbhec
spring.mail.host=smtp.qq.com
spring.mail.properties.mail.smtl.ssl.enable=true
```

> 测试

```
package com.kwq;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.mail.SimpleMailMessage;
import org.springframework.mail.javamail.JavaMailSenderImpl;

@SpringBootTest
class SpringbootMailApplicationTests {
    @Autowired
    JavaMailSenderImpl mailSender;

    @Test
    void contextLoads() {
        //发送邮件
        //收件人
        //内容

        SimpleMailMessage message = new SimpleMailMessage();
        message.setSubject("西开Java");
        message.setText("Hello");

        message.setFrom("347858300@qq.com");
        message.setTo("347858300@qq.com");

        mailSender.send(message);
    }

}
```

> 效果

![image-20220428101117824](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204281011061.png)



`代码二（带有附件）`

```java
@Test
void test2() throws MessagingException {
    MimeMessage mimeMessage = mailSender.createMimeMessage();
    MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, true);

    helper.setSubject("西开java");
    helper.setText("hello",true);

    //附件
    helper.addAttachment("1.jpg",new File("D:\\java文件夹汇总\\javaWeb\\springboot-mail\\1.png"));
    helper.addAttachment("1.jpg",new File("D:\\java文件夹汇总\\javaWeb\\springboot-mail\\1.png"));
    helper.addAttachment("1.jpg",new File("D:\\java文件夹汇总\\javaWeb\\springboot-mail\\1.png"));
    helper.addAttachment("1.jpg",new File("D:\\java文件夹汇总\\javaWeb\\springboot-mail\\1.png"));

    helper.setFrom("347858300@qq.com");
    helper.setTo("347858300@qq.com");//也可以一次发给多个人
    mailSender.send(mimeMessage);
}
```

效果：

![image-20220428101904324](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204281019574.png)

