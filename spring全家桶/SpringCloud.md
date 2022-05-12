# SpringCloud

## 什么是微服务架构？

微服务架构是一种==架构模式==，它提倡将单一应用程序划分成一组小的服务，服务之间互相协调、互相配合，为用户提供最终价值。每个服务运行在其独立的进程中，==服务于服务之间采用轻量级的通信机制互相协作（通常是基于HTTP协议的RESTful API）==。每个服务都围绕着具体业务进行构建，并且能够被独立的部署到生产环境、类生产环境等。==另外，应当尽量避免统一的、集中式的服务管理机制，对具体的一个服务而言，应根据业务上下文，选择合适的语言、工具对其进行构建。==

==SpringCloud是分布式微服务架构的一站式解决方案，是多种微服务架构落地技术的集合体，俗称微服务全家桶。==

![image-20220319105236337](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191052765.png)

## Boot和Cloud版本选型（技术选型）

springboot的github源码地址：https://github.com/spring-projects/spring-boot/releases ==可以看到当前已经是2.7.0版本了，更新迭代的十分快==

springboot官网地址：https://spring.io/

![image-20220319110947994](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191109130.png)

查看boot2.0版本的介绍https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Release-Notes（稍稍修改，其他版本的boot介绍也可以通过此地址查看）

==官网强烈推荐我们从1.5之前的版本升级到2.0 也就是我们不要再使用1.5之前的版本进行开放了==

![image-20220319111601895](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191116018.png)

==截至2022.3.19日 当前springboot最稳定版本是2.6.4==

![image-20220319112033556](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191120791.png)

springcloud版本是以英文字母来命名的：

![image-20220319124643023](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191246161.png)

![image-20220319124739468](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191247552.png)

springcloud的源码地址：https://github.com/spring-projects/spring-cloud/wiki

![image-20220319112306705](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191123886.png)

官网地址：https://spring.io/ 目前2021.0.1是最新最稳定的版本

![image-20220319112918249](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191129610.png)

### boot和cloud的版本依赖关系

打开此地址：https://spring.io/projects/spring-cloud#overview 可以看到cloud和boot版本对应关系（博客找对应关系资料也行）

![image-20220319114830237](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191148346.png)



更详细的boot和cloud版本对应关系 https://start.spring.io/actuator/info 复制内容通过json解析工具可以看到如下：我们选型的时候要严格按照这个要求来配置

![image-20220319125447175](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191254259.png)

虽说在范围之内的都是可以用的，但是点开某个版本的cloud，他会给我们推荐使用哪个版本，我们就用它推荐的版本即可

![image-20220319125833549](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191258634.png)

![image-20220319125928088](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191259162.png)

### 本教程的技术选型：

![image-20220319130007162](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191300222.png)

## Cloud各种组件的停更替换

![image-20220319141838602](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191418701.png)

## 创建父工程

> 1.新建maven项目

![image-20220319143255870](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191432981.png)

把src文件夹删掉；

> pom

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.atgugui.springcloud</groupId>
  <artifactId>cloud2021</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>pom</packaging>  //表示是一个总的父工程 手动添加上去
 
  <!--统一管理jar包版本-->
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <junit.version>4.12</junit.version>
    <log4j.version>1.2.17</log4j.version>
    <lombok.version>1.16.18</lombok.version>
    <mysql.version>5.1.47</mysql.version><!--数据库版本，根据自己的改-->
    <druid.version>1.1.16</druid.version>
    <mybatis.spring.boot.version>1.3.0</mybatis.spring.boot.version>
  </properties>
  <!--子模块继承之后，提供作用：锁定版本+子module不用写groupId和version 继承了父类的pom中的groupId和version-->
  <dependencyManagement>
    <dependencies>
      <!--spring boot 2.2.2-->
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.2.2.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!--spring cloud Hoxton.SR1-->
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
        <version>Hoxton.SR1</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!--spring cloud 阿里巴巴-->
      <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-alibaba-dependencies</artifactId>
        <version>2.1.0.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!--mysql-->
      <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>${mysql.version}</version>
        <!--      <scope>runtime</scope>-->
      </dependency>
      <!-- druid-->
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>${druid.version}</version>
      </dependency>
      <!--mybatis-->
      <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>${mybatis.spring.boot.version}</version>
      </dependency>
      <!--junit-->
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>${junit.version}</version>
      </dependency>
      <!--log4j-->
      <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>${log4j.version}</version>
      </dependency>

      <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>${lombok.version}</version>
      </dependency>


      <!--spring cloud alibaba 2.1.0.RELEASE-->
      <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-alibaba-dependencies</artifactId>
        <version>2.1.0.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>

      </dependency>


    </dependencies>
  </dependencyManagement>
  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>

        <configuration>
          <fork>true</fork>
          <addResources>true</addResources>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

==技术选型的时候改改这些版本号即可==

![image-20220319144252267](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191442364.png)

### 复习DependecyManagement和Depebdencies

![image-20220319144841674](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191448828.png)

![image-20220319144853745](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191448852.png)

==父工程创建完成后执行mvn中的install将父工程发布到仓库方便子工程继承==



## 微服务模块

### 支付模块

> 构建微服务模块五部曲：

1.创建一个子模块cloud-provider-payment8001  查看父工程pom变化

![image-20220319150432678](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203191504796.png)

2.改pom文件

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>cloud2021</artifactId>
        <groupId>com.atgugui.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-provider-payment8001</artifactId>

    <dependencies>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>


        <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-jdbc -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifacAt/org.springframework.boot/spring-boot-devtools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <!-- https://mvnrepository.com/artifacAt/org.springframework.boot/spring-boot-devtools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
    </dependencies>
</project>
```

3.写yml

```java
server:
  port: 8001


spring:
  application:
    name: cloud-payment-service
  datasource:
    url: jdbc:mysql://localhost:3306/db2019?useSSL=false&useUnicode=true&characterEncoding=utf-8
    driver-class-name: org.gjt.mm.mysql.Driver   #mysql驱动包
    username: root
    password: 123123
    type: com.alibaba.druid.pool.DruidDataSource

#配置mybatis
mybatis:
  #设置别名
  mapperLocations: classpath:mapper/*.xml
  type-aliases-package: com.atguigu.springcloud.entities
  configuration:
    map-underscore-to-camel-case: true #开启这个的作用是可以让数据库中的p_Addr与pojo中的pAddr对应




```

4.主启动类

```java
package com.atguigu.springcloud;


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;


/**
 * @author KWQ
 */
@SpringBootApplication

public class PaymentMain8001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8001.class,args);
    }
}

```

5.业务类

```
package com.atguigu.springcloud.entities;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

@AllArgsConstructor
@NoArgsConstructor
@Data
public class Payment implements Serializable {
    private Long id;
    private String serial;
}

```

```
package com.atguigu.springcloud.entities;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class CommonResult <T>{
    private Integer code;
    private String message;
    private T data;
    public CommonResult(Integer code,String message){
        this(code,message,null);
    }
}

```

dao

```
package com.atguigu.springcloud.dao;


import com.atguigu.springcloud.entities.Payment;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;

@Mapper
public interface PaymentDao {
    public int create(Payment payment);

    public Payment getpaymentById(@Param("id") Long id);

}

```

mapper的映射文件mapper.xml（写sql）

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.atguigu.springcloud.dao.PaymentDao">
    <insert id="create" parameterType="com.atguigu.springcloud.entities.Payment" useGeneratedKeys="true" keyProperty="id">
        insert into payment(serial) values(#{serial});
    </insert>

    <!--   配置结果集的映射-->
    <resultMap id="BaseResultMap" type="com.atguigu.springcloud.entities.Payment">
        <id column="id" property="id" jdbcType="BIGINT"/>
        <id column="serial" property="serial" jdbcType="VARCHAR"/>
    </resultMap>
    <select id="getpaymentById" parameterType="Long" resultMap="BaseResultMap">
        select * from payment where id=#{id};
    </select>

</mapper>

```

service

```
package com.atguigu.springcloud.service;


import com.atguigu.springcloud.entities.Payment;
import org.apache.ibatis.annotations.Param;

public interface PaymentService {
    public int create(Payment payment);

    public Payment getpaymentById(@Param("id") Long id);

}

```

```
package com.atguigu.springcloud.service.impl;


import com.atguigu.springcloud.dao.PaymentDao;
import com.atguigu.springcloud.entities.Payment;
import com.atguigu.springcloud.service.PaymentService;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;

@Service
public class PaymentServiceImpl implements PaymentService {
    @Resource
    private PaymentDao paymentDao;
    @Override
    public int create(Payment payment) {
        return paymentDao.create(payment);
    }

    @Override
    public Payment getpaymentById(Long id) {
        return paymentDao.getpaymentById(id);
    }
}

```

controller

```
package com.atguigu.springcloud.controller;


import com.atguigu.springcloud.entities.Payment;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.*;
import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.service.PaymentService;

import javax.annotation.Resource;

@RestController
@Slf4j     //打印日志信息
public class PaymentController
{
    @Resource
    private PaymentService paymentService;

    @PostMapping(value ="/payment/create" )
    public CommonResult create( Payment payment){
        int result = paymentService.create(payment);
        log.info("插入的结果是"+result);
        if(result>0){
            return new CommonResult(200,"插入数据库成功",result);
        }else {
            return new CommonResult(444,"插入数据库失败",null);
        }

    }

    @GetMapping(value ="/payment/get/{id}" )
    public CommonResult getPaymentById(@PathVariable("id") Long id){
        Payment payment = paymentService.getpaymentById(id);
        log.info("插入的结果是"+payment);
        if(payment!=null){
            return new CommonResult(200,"查询成功",payment);
        }else {
            return new CommonResult(444,"没有对应记录",null);
        }

    }

}

```

启动测试ok！！！

#### 开启热部署

> 8001模块添加热部署依赖：

```java
<!-- https://mvnrepository.com/artifacAt/org.springframework.boot/spring-boot-devtools -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
    <optional>true</optional>
</dependency>
```

> 父工程pom中添加：

```
<build>
  <plugins>
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
      <configuration>
        <fork>true</fork>
        <addResources>true</addResources>
      </configuration>
    </plugin>
  </plugins>
</build>
```

> 对应位置打钩

![image-20220321170628203](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203211706517.png)

> 按住快捷键然后在注册里这两个东西打上勾

![image-20220321170706261](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203211707375.png)

==最后重启idea就可以生效了==

### 订单模块

新建订单模块：cloud-sumer-order80

> pom

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-sumer-order80</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web  -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
```

> yml：

```
server:
  port: 80
```

> 启动类

```
package com.atguigu.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class OrderMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderMain80.class,args);
    }
}
```

> 实体类：

```
package com.atguigu.springcloud.entities;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class CommonResult <T>{
    private Integer code;
    private String message;
    private T data;
    public CommonResult(Integer code,String message){
        this(code,message,null);
    }
}
```

```
package com.atguigu.springcloud.entities;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

@AllArgsConstructor
@NoArgsConstructor
@Data
public class Payment implements Serializable {
    private Long id;
    private String serial;
}
```

#### RestTemplate介绍：

![image-20220321190759373](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203211907653.png)

官网地址：
https://docs.spring.io/spring-framework/docs/5.2.2.RELEASE/javadoc-api/org/springframework/web/client/RestTemplate.html

![image-20220321190933863](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203211909931.png)

>  config配置类

```
package com.atguigu.springcloud.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ApplicationContextConfig {
    @Bean
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
```

>  controller：

```
package com.atguigu.springcloud.controller;

import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.entities.Payment;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;

@RestController
@Slf4j
public class OrderController {
    public static final String PAYMENT_URL = "http://localhost:8001";

    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("/consumer/payment/create")
    public CommonResult<Payment>   create(Payment payment){
        return restTemplate.postForObject(PAYMENT_URL+"/payment/create",payment, CommonResult.class);  //写操作
    }

    @GetMapping("/consumer/payment/get/{id}")
    public CommonResult<Payment> getPayment(@PathVariable("id") Long id){
        return restTemplate.getForObject(PAYMENT_URL+"/payment/get/"+id,CommonResult.class);
    }

}
```

> 测试：同时开启两个服务

测试：http://localhost/consumer/payment/get/2  完美输出

测试：

![image-20220321192424445](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203211924511.png)

我们查看我们的数据库，发现数据是空的

![image-20220321192454009](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203211924077.png)

加个注解就好了

![image-20220321192542133](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203211925205.png)

再次测试就正常！！！

`细节`：

当我们同时开启两个服务的时候就会看到这个   点进去

![image-20220321191506802](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203211915864.png)

![image-20220321191436949](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203211914049.png)

最后就能看到这样的效果：

![image-20220321191537772](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203211915836.png)

==如果启动两个服务没出现这个，百度查查如何让其出来！！！==



### 工程重构

新建一个模块：cloud-api-commons

> pom：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-api-commons</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/cn.hutool/hutool-all -->
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.1.0</version>
        </dependency>
    </dependencies>

</project>
```

新建一个包建两个重复的实体类（entities里面的类）

执行maven命令 clean 然后 install

> 改造80和8001

1.先在两个服务中加入如下依赖

```
<dependency>
    <groupId>com.atguigu.cloud</groupId>
    <artifactId>cloud-api-commons</artifactId>
    <version>${project.version}</version>
</dependency>
```

2.删除两个服务各自的entities包

3.测试运行：

http://localhost/consumer/payment/get/2  完美输出

localhost/consumer/payment/create?serial=1111342141 完美输出



## Eureka

==实现服务注册功能的服务器==

==**什么是服务治理?**==

![image-20220321204941684](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212049890.png)

==**什么是服务注册？**==

![image-20220321205021709](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212050901.png)

![image-20220321205045369](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212050498.png)

### Eureka两大组件

![image-20220321205120772](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212051080.png)

### EurekaServer服务端的安装

> 新建模块：cloud-eureka-server7001

pom：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-eureka-server7001</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-eureka-server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>

        <dependency>
            <groupId>com.atgugui.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web  -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>

    </dependencies>

</project>
```

> yml：

```
server:
  port: 7001

eureka:
  instance:
    hostname: localhost #eureka服务端实例名称
  client:
    register-with-eureka: false #表示不像注册中心注册自己
    fetch-registry: false #false表示自己就是注册中心，我的职责就是维护服务实例,并不区检索服务
    service-url:
      #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

> 主启动类

```
package com.atguigu.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer
public class EurekaMain7001 {
    public static void main(String[] args) {
        SpringApplication.run(EurekaMain7001.class,args);
    }
}
```

访问：[Eureka](http://localhost:7001/)

> 输出：

![image-20220321211551579](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212115979.png)

### 支付微服务8001入驻eurekaServer

==**8001就是eurekaClient**==

> 8001的pom添加如下依赖

```java
<!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-eureka-server -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

> yml中增加eureka的配置：

```
eureka:
  client:
    register-with-eureka: true
    fetchRegistry: true
    service-url:
      defaultZone: http://localhost:7001/eureka 
```

> 主启动类添加注解   @EnableEurekaClient

```
package com.atguigu.springcloud;


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;


/**
 * @author KWQ
 */
@SpringBootApplication
@EnableEurekaClient
public class PaymentMain8001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8001.class,args);
    }
}
```

> 启动8001和7001

访问：[Eureka](http://localhost:7001/)

![image-20220321215113435](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212151554.png)

==说明8001注册进去了==   上面的名称和我们8001的名称是一样的

![image-20220321213911792](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212139928.png)

### 将80注册进eurekaServer

> 80模块添加依赖：

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

> yml

```
server:
  port: 80
spring:
  application:
    name: cloud-order-service

eureka:
  client:
    register-with-eureka: true
    fetchRegistry: true
    service-url:
      defaultZone: http://localhost:7001/eureka
```

==主启动类添加注解：@EnableEurekaClient==

启动三个服务：访问localhost:7001   ==成功入驻页面==

![image-20220321215317093](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212153197.png)



### Eureka集群说明

![image-20220321215923512](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212159618.png)

![image-20220321220006627](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212200713.png)

### Eureka集群环境构建

新建一个模块：cloud-eureka-server7002 除了yml不同其他保持一致

> 修改映射配置：

![image-20220321221720133](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212217211.png)

> 7001的yml：

```
server:
  port: 7001

eureka:
  instance:
    hostname: eureka7001.com #eureka服务端实例名称
  client:
    register-with-eureka: false #表示不像注册中心注册自己
    fetch-registry: false #false表示自己就是注册中心，我的职责就是维护服务实例,并不区检索服务
    service-url:
      #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址
      defaultZone: http://eureka7002.com:7002/eureka/
```

> 7002的yml

```
server:
  port: 7002

eureka:
  instance:
    hostname: eureka7002.com #eureka服务端实例名称
  client:
    register-with-eureka: false #表示不像注册中心注册自己
    fetch-registry: false #false表示自己就是注册中心，我的职责就是维护服务实例,并不区检索服务
    service-url:
      #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址 注册进7001
      defaultZone: http://eureka7001.com:7001/eureka/
```

![image-20220321221427841](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212214903.png)

> 启动7001和7002

![image-20220321222032740](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212220835.png)

![image-20220321222127195](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212221297.png)

### 订单支付两位服务注册进Eureka集群

> 将支付服务8001微服务发布到上面两台Eureka集群配置中

修改8001的yml即可

```
service-url:
  defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  #集群版
```

> 将支付服务80微服务发布到上面两台Eureka集群配置中

修改80的yml即可

```
service-url:
  defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  #集群版
```

> 测试：

先启动7001，7002.再启动8001，最后启动80

访问：[localhost/consumer/payment/get/31](http://localhost/consumer/payment/get/31)  成功访问即可



### 支付微服务集群配置

参考8001新建一个cloud-provider-payment8001模块（内容和8001一样） 就改下端口号就好了

修改8001和8002的controller

![image-20220321230551420](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212305761.png)

启动7001/7002/8001/8002/80

![image-20220321230843534](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212308623.png)

发现每次都是8001，没有做到对支付模块的负载均衡

修改order的controller的==PAYMENT_URL==

![image-20220321231909746](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203212319874.png)在order模块的config中添加注解==@LoadBalanced==

```
package com.atguigu.springcloud.config;

import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ApplicationContextConfig {
    @Bean
    @LoadBalanced   //开启RestTemplate的负载均衡
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
    
}
```

> 再次测试

发现可以成功出现轮询效果



### actuator微服务信息完善

> 所需要依赖：

```
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

==当前问题：应当修改服务名称==

![image-20220322141456247](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203221414539.png)

在8001和8002中yml添加：![image-20220322142038292](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203221420355.png)

效果图：

![image-20220322141719642](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203221417755.png)

==当前问题：访问信息没有ip提示==

8001、8002同时添加

![image-20220322142143199](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203221421263.png)

效果图：

![image-20220322142000584](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203221420676.png)



### 服务发现Discovery

> 对于注册进eureka里面的微服务，可以通过服务发现来获取该服务的信息

在8001的controller加入：

```
@Resource
private DiscoveryClient discoveryClient;

@GetMapping(value = "/payment/discovery")
public Object discovery(){
    List<String> services = discoveryClient.getServices();
    for (String element : services) {
        log.info("***** element:"+element);
    }
    List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
    for (ServiceInstance instance : instances) {
        log.info(instance.getServiceId()+"\t"+instance.getHost()+"\t"+instance.getPort()+"\t"+instance.getUri());
    }
    return this.discoveryClient;
}
```

8001主启动类新加注解

```
@EnableDiscoveryClient
```

> 测试：

![image-20220322144126779](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203221441871.png)

> 控制台输出：

![image-20220322144157950](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203221441007.png)



### Eureka自我保护理论知识

==Eureka默认是开启自我保护机制的==

> 故障现象概述：

![image-20220322145634847](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203221456054.png)

![image-20220322145705128](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203221457240.png)

![image-20220322145723597](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203221457695.png)

==**一句话总结就是：某时刻某一个微服务不可用了，Eureka不会立刻清理，依旧会对该微服务的信息进行保存**==

==**Eurka的自我保护属于CAP里面的AP分支**==



### 如何禁止Eureka的自我保护

==一般生产环境中不会禁止自我保护==

注册中心eurekaServer端7001默认是开启自我保护机制的，何以见得？

![image-20220322151133094](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203221511183.png)

在7001的yml中添加

```
 server:
    enable-self-preservation: false
    #每两秒要接收到心跳 不然从注册中心上剔除
    eviction-interval-timer-in-ms: 2000
```

修改8001的yml，添加如下：

```
instance:
  instance-id: payment8001
  prefer-ip-address: true
  #Eureka客户端向服务端发送心跳的时间间隔，单位是s （默认是30s）
  lease-renewal-interval-in-seconds: 1
    #Eureka服务端在收到最后一次心跳后等待时间上限，单位是s （默认是90s），超时将剔除服务
  lease-expiration-duration-in-seconds: 2
```

启动7001和8001

![image-20220322151934337](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203221519461.png)

将8001服务关掉，发现服务被剔除了

![image-20220322152031764](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203221520852.png)

### 补充：

Eureka现在已经停更了，不建议使用了（部分老公司可能还在用），上面对eureka的学习，也足够我们来应对老项目的挑战了！！！



## Zookeeper

==zookeeper这里我们用来替代Eureka==

### 8004模块入驻Zk

新建模块：cloud-provider-payment8004

> pom：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-provider-payment8004</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>

        <dependency>
            <groupId>com.atgugui.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-zookeeper-discovery -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
           </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>


    </dependencies>

</project>
```

> yml：

```java
server:
  port: 8004

spring:
  application:
    name: cloud-provider-payment
  cloud:
    zookeeper:
      connect-string: 120.24.210.204:2181   #服务器ip加zk端口
```

> 主启动类

```
package com.atguigu.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient  //该注解用于向使用consul或者zookeeper作为服务注册中心时注册服务
public class Payment8004 {
    public static void main(String[] args) {
        SpringApplication.run(Payment8004.class,args);
    }
}
```

> controller

```
package com.atguigu.springcloud.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.UUID;

@RestController
@Slf4j
public class PaymentController {

    @Value("${server.port}")
    private String serverPort;

    @GetMapping(value = "/payment/zk")
    public String paymentzk(){
        return "springcloud with zookeeper:"+serverPort+"\t"+ UUID.randomUUID().toString();
    }

}
```

> 开启zk

![image-20220323200055129](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232000649.png)



==发现启动报错==

![image-20220323200138611](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232001756.png)

![image-20220323200217791](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232002016.png)

> jar冲突解决方案：添加依赖:

```
<!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-zookeeper-discovery -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
    <!--排除zk3.5.3-->
    <exclusions>
        <exclusion>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<!--添加zk 3.4,9版本-->
<!-- https://mvnrepository.com/artifact/org.apache.zookeeper/zookeeper -->
<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.4.9</version>
</dependency>
```

> 测试：

![image-20220323200501277](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232005384.png)

![image-20220323200401179](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232004405.png)

==对应起来了，说明8004成功入驻Eureka了==

解析json字符串：

![image-20220323200653866](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232006012.png)

### Zk是临时还是持久结点

==Zk是临时结点。eureka默认是持久结点==

我们停止8004服务 zk并不会马上剔除8004服务 而是等待一段时间 再剔除，当我们再次启动，就会生成一个新的id流水号

![image-20220323201650131](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232016297.png)

### 订单服务注册进Zk

新建模块cloud-consumerZk-order80

> pom

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-consumerZk-order80</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>

        <dependency>
            <groupId>com.atgugui.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-zookeeper-discovery -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

> yml

```
server:
  port: 80

spring:
  application:
    name: cloud-consumer-order
  cloud:
    zookeeper:
      connect-string: 120.24.210.204:2181
```

> 主启动类

```
package com.atguigu.springcloud;


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class OrderZKMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderZKMain80.class,args);
    }
}
```

> 配置类：

```
package com.atguigu.springcloud.config;

import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ApplicationContextConfig {

    @LoadBalanced
    @Bean
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }

}
 
 
```

> controller

```
package com.atguigu.springcloud.controller;

import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.entities.Payment;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;

@RestController
@Slf4j
public class OrderZKController {

    public static final String INVOME_URL = "http://cloud-provider-payment";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping("/consumer/payment/zk")
    public String payment (){
      String result = restTemplate.getForObject(INVOME_URL+"/payment/zk",String.class);
      return result;
    }


}
 
 
```

==启动8004和80来测试==

![image-20220323203443819](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232034069.png)

![image-20220323203537404](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232035497.png)

==因为很少公司用Zk来做服务注册，因此这里就不介绍集群了。想要了解并学会使用找篇博客快速上手即可==



## Consul

### Consul概述

> 是什么？

![image-20220323205117699](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232051855.png)

> 能干嘛？ 官方文档翻译下来就是以下内容

![image-20220323205710932](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232057115.png)

> 下载地址：

[下载|HashiCorp的领事 (consul.io)](https://www.consul.io/downloads)

> 怎么玩

[Spring Cloud Consul 中文文档 参考手册 中文版](https://www.springcloud.cc/spring-cloud-consul.html)



### 安装Consul

官网安装就行。

安装完毕只有一个consul.exe

> 查看版本号

![image-20220323211630622](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232116720.png)

> 使用开发者模式启动

![image-20220323211714078](../AppData/Roaming/Typora/typora-user-images/image-20220323211714078.png)

访问localhost:8500出现下面界面表示安装成功

![image-20220323211754936](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232117096.png)



### 服务提供方注册进Consul

> 新建一个模块：

cloud-providerconsul-payment8006

> pom

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-providerconsul-payment8006</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
        <dependencies>
            <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-consul-discovery -->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-starter-consul-discovery</artifactId>
            </dependency>

            <dependency>
                <groupId>com.atguigu.springcloud</groupId>
                <artifactId>cloud-api-commons</artifactId>
                <version>${project.version}</version>
            </dependency>


            <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
            </dependency>

            <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-actuator</artifactId>
            </dependency>

            <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-devtools</artifactId>
                <scope>runtime</scope>
                <optional>true</optional>
            </dependency>

            <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <optional>true</optional>
            </dependency>

            <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-test</artifactId>
                <scope>test</scope>
            </dependency>



        </dependencies>

    </project>


</project>
```

> yml：

```
server:
  port: 8006


spring:
  application:
    name: consul-provider-payment
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```

> 主启动类：

```
package com.atguigu.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class PaymentMain8006 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8006.class,args);
    }
}
```

> controller：

```
package com.atguigu.springcloud.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.UUID;

@RestController
@Slf4j
public class PaymentController {

    @Value("${server.port}")
    private String serverPort;

    @GetMapping(value = "/payment/consul")
    public String paymentConsul(){
        return "springcloud with consul: "+serverPort+"\t"+ UUID.randomUUID().toString();
    }
}
```

> 启动服务测试效果

![image-20220323213029877](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232130112.png)

点进去：

![image-20220323213252429](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232132913.png)

再点进去能看到健康状态

![image-20220323213335723](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232133822.png)

访问：[localhost:8006/payment/consul](http://localhost:8006/payment/consul)

![image-20220323213125726](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232131967.png)



### 服务消费者注册进Consul

> 新建模块   

cloud-consumerconsul-order80   

> pom

```

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-consumerconsul-order80</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-consul-discovery -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
        </dependency>

        <dependency>
            <groupId>com.atguigu.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>

```

> yml

```
server:
  port: 80


spring:
  application:
    name: consul-consumer-order
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: ${spring.application.name}
 
 
```

> 主启动类

```java
package com.atguigu.springcloud;

  import org.springframework.boot.SpringApplication;
  import org.springframework.boot.autoconfigure.SpringBootApplication;
  import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

  @SpringBootApplication
  @EnableDiscoveryClient
  public class OrderConsulMain80 {
  public static void main(String[] args) {
  SpringApplication.run(OrderConsulMain80.class,args);
  }
}
```

> 配置Bean

```
package com.atguigu.springcloud.config;

import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ApplicationContextConfig {

    @LoadBalanced
    @Bean
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }

}
 
 
```

> controller：

```
package com.atguigu.springcloud.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;

@RestController
@Slf4j
public class OrderConsulController {

    public static final String INVOME_URL = "http://consul-provider-payment";

    @Resource 
    private RestTemplate restTemplate;

    @GetMapping("/consumer/payment/consul")
    public String payment (){
      String result = restTemplate.getForObject(INVOME_URL+"/payment/consul",String.class);
      return result;
    }


}
 
 
```

> 测试

启动两个consul服务（80和8006）

![image-20220323220055113](../AppData/Roaming/Typora/typora-user-images/image-20220323220055113.png)

上图可看出已注册进consul

[localhost/consumer/payment/consul](http://localhost/consumer/payment/consul)

![image-20220323220322252](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203232203358.png)



## 三个注册中心异同点

### CAP

首先先来介绍一下什么是==CAP==

C：Consistency（强一致性）

A：Availability（可用性）

P：Partition tolerance（分区容错性）

==CAP理论关注粒度是数据，而不是整体系统设计的策略==

经典CPA图

![image-20220324101428540](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241014953.png)

![image-20220324101442124](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241014288.png)

### 异同点

> Eureka满足（AP）：

![image-20220324101533367](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241015646.png)![image-20220324101548809](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241015959.png)



> Zookeeper和consul（满足CP）

![image-20220324101624377](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241016624.png)

![image-20220324101648725](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241016963.png)



## Ribbon

### 概述：

![image-20220324104629460](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241046590.png)

> 从官网可以看到现在已经进入了维护模式

![image-20220324104732282](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241047576.png)

> 未来的替换方案：

![image-20220324104758643](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241047051.png)



> 能干嘛：实现负载均衡

![image-20220324104843256](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241048546.png)

> 负载均衡（LB）包含集中式LB和进程式LB：

![image-20220324104918887](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241049029.png)

![image-20220324104952246](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241049496.png)

前面我们讲解过了80通过轮询负载访问8001、8002

==一句话总结就是：==  ==**Ribbon=负载均衡+RestTemplate调用**==



### Ribbon负载均衡演示

> 架构：

![image-20220324150014437](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241500649.png)

![image-20220324150031522](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241500784.png)

==**总结：Ribbon其实就是一个软负载均衡的客户端组件，他可以和其他所需请求的客户端结合使用，和eureka结合只是其中的一个实例**==

![image-20220324150153613](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241501840.png)![image-20220324150206262](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241502556.png)

> 二说restTemplate

![image-20220324150240187](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241502490.png)

==两个常用方法==

![image-20220324150315217](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241503392.png)

![image-20220324150337415](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241503582.png)

> 具体演示：

在80模块的controller添加如下

```java
@GetMapping("/consumer/payment/getForEntity/{id}")
public CommonResult<Payment> getPayment2(@PathVariable("id") long id){
    ResponseEntity<CommonResult> entity = restTemplate.getForEntity(PAYMENT_URL + "/payment/get/" + id, CommonResult.class);
    if(entity.getStatusCode().is2xxSuccessful()){
        CommonResult body = entity.getBody();
        log.info(entity.getStatusCode()+"\t"+entity.getHeaders());
        return body;

    }else {
        return new CommonResult<>(444,"操作失败");
    }
}
```

> 测试：

![image-20220324151418614](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241514909.png)

![image-20220324151435251](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241514389.png)



### Ribbon默认自带的负载规则

> Ribbon核心组件IRule：

![image-20220324152023887](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241520025.png)

![image-20220324152049085](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241520197.png)

### Ribbon负载规则替换

修改cloud-consumer-order80模块

> 注意配置细节：

![image-20220324152707513](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241527861.png)

![image-20220324152725164](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241527391.png)

> 新建包：

![image-20220324153204518](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241532637.png)

> 配置类

```java
package com.atguigu.myrule;

import com.netflix.loadbalancer.IRule;
import com.netflix.loadbalancer.RandomRule;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MySelfRule {
    @Bean
    public IRule myRule(){
        return new RandomRule();  //定义为随机
    }
}
```

> 主启动类添加注解   @RibbonClient(name = "CLOUD-PAYMENT-SERVICE",configuration = MySelfRule.class)

```java
package com.atguigu.springcloud;

import com.atguigu.myrule.MySelfRule;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.cloud.netflix.ribbon.RibbonClient;

@SpringBootApplication
@EnableEurekaClient
@RibbonClient(name = "CLOUD-PAYMENT-SERVICE",configuration = MySelfRule.class)
public class OrderMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderMain80.class,args);
    }
}
```

> 测试：这波测出来就是随机得了          restTemplate的模板类负载均衡注解不能删掉（固定的）

![image-20220324153604300](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241536403.png)



### Ribbon默认负载轮询算法原理

> 原理：
>

![image-20220324155027596](../AppData/Roaming/Typora/typora-user-images/image-20220324155027596.png)

### Ribbon手写轮询算法

8001、8002的controller加一个

```
@GetMapping(value="/payment/lb")
public String getPaymentLB(){
    return serverPort;
}
```

> 80的配置类去掉  //   @LoadBalanced            使得原先默认的失效，手写的才能生

```
package com.atguigu.springcloud.config;

import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ApplicationContextConfig {
    @Bean
  //  @LoadBalanced   //开启RestTemplate的负载均衡
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }

}

```

80写一个接口

![image-20210909131404720](../AppData/Roaming/Typora/typora-user-images/image-20210909131404720.png)

```
package com.atguigu.springcloud.lb;

import org.springframework.cloud.client.ServiceInstance;

import java.util.List;

/*提供具体哪个服务器*/
public interface LoadBalancer {
    ServiceInstance instances(List<ServiceInstance> serviceInstances);
}
```

提供实现类MyLb

```
package com.atguigu.springcloud.lb;

import org.springframework.cloud.client.ServiceInstance;
import org.springframework.stereotype.Component;

import java.util.List;
import java.util.concurrent.atomic.AtomicInteger;

@Component
public class MyLB implements LoadBalancer{

    private AtomicInteger atomicInteger=new AtomicInteger(0);

    public final int getAndIncrement(){
        int current;
        int next;
        do{
            current=this.atomicInteger.get();
            next=current>=2147483647?0:current+1;
        }while (!this.atomicInteger.compareAndSet(current,next));
        System.out.println("****第几次访问，次数next:"+next);
        return next;
    }
    @Override
    public ServiceInstance instances(List<ServiceInstance> serviceInstances) {
        int index = getAndIncrement() % serviceInstances.size();
        return serviceInstances.get(index);
    }
}
```

==**ps：lb要放在和主启动类同一包下，否则@ComponentScan扫描不到@Component**==

修改80的controller

```
package com.atguigu.springcloud.controller;

import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.entities.Payment;
import com.atguigu.springcloud.lb.LoadBalancer;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;
import java.net.URI;
import java.util.List;

@RestController
@Slf4j
public class OrderController {
 /*   public static final String PAYMENT_URL = "http://localhost:8001";*/
    public static final String PAYMENT_URL = "http://CLOUD-PAYMENT-SERVICE";//微服务名称

    @Autowired
    private RestTemplate restTemplate;
    //额外加的
    @Resource
    private LoadBalancer loadBalancer;
    //额外加的
    @Resource
    private DiscoveryClient discoveryClient;

    @GetMapping("/consumer/payment/create")
    public CommonResult<Payment>   create( Payment payment){
        return restTemplate.postForObject(PAYMENT_URL+"/payment/create",payment, CommonResult.class);  //写操作
    }

    @GetMapping("/consumer/payment/get/{id}")
    public CommonResult<Payment> getPayment(@PathVariable("id") Long id){
        return restTemplate.getForObject(PAYMENT_URL+"/payment/get/"+id,CommonResult.class);
    }


    @GetMapping("/consumer/payment/getForEntity/{id}")
    public CommonResult<Payment> getPayment2(@PathVariable("id") long id){
        ResponseEntity<CommonResult> entity = restTemplate.getForEntity(PAYMENT_URL + "/payment/get/" + id, CommonResult.class);
        if(entity.getStatusCode().is2xxSuccessful()){
            CommonResult body = entity.getBody();
            log.info(entity.getStatusCode()+"\t"+entity.getHeaders());
            return body;

        }else {
            return new CommonResult<>(444,"操作失败");
        }
    }
    //额外加的
    @GetMapping(value = "/consumer/payment/lb")
    public String getPaymentLB(){
        List<ServiceInstance> instances=discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
        if(instances==null||instances.size()<=0){
            return  null;
        }
        ServiceInstance serviceInstance = loadBalancer.instances(instances);
        URI uri = serviceInstance.getUri();
        return restTemplate.getForObject(uri+"/payment/lb",String.class);

    }


}

```

手写完成！测试localhost/consumer/payment/lb

![image-20220324160254467](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241602592.png)

![image-20220324160654027](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241606110.png)



## OpenFeign

### 概述：

![image-20220324162134608](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241621163.png)

![image-20220324162918489](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241629713.png)

Feign是一个声明式的web服务客户端，让编写web服务客户端变得非常容易，只需要创建一个接口并且在接口上添加注解即可

github源码地址： https://github.com/spring-cloud/spring-cloud-OpenFeign



> 能干嘛？

![image-20220324162541720](../AppData/Roaming/Typora/typora-user-images/image-20220324162541720.png)

> 两者的区别

![image-20220324162636117](../AppData/Roaming/Typora/typora-user-images/image-20220324162636117.png)



### OpenFeign服务调用使用步骤

> 新建模块：cloud-consumer-feign-order80

pom

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-consumer-feign-order80</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <!--openfeign-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>com.atgugui.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
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
        </dependency>
    </dependencies>
</project>
```

==可以看到OpenFeign整合了Ribbon==  所以说Feign自带负载均衡配置项

![image-20220324192054317](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241920679.png)

> yml

```java
server:
  port: 80
eureka:
  client:
    register-with-eureka: false
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka, http://eureka7002.com:7002/eureka
```

> 主启动类   注意加了一个新注解@EnableFeignClients

```java
package com.atguigu.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.openfeign.EnableFeignClients;

@SpringBootApplication
@EnableFeignClients
public class OrderFeignMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderFeignMain80.class,args);
    }
}
```

> service

```
package com.atguigu.springcloud.service;

import com.atguigu.springcloud.entities.CommonResult;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@Component
@FeignClient(value = "CLOUD-PAYMENT-SERVICE")
public interface PaymentFeignService {
    @GetMapping(value = "/payment/get/{id}")
    public CommonResult getPaymentById (@PathVariable("id") Long id);

}
```

> controller

```java
package com.atguigu.springcloud.controller;

import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.service.PaymentFeignService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

@RestController
@Slf4j
public class OrderFeignController {
    @Resource
    private PaymentFeignService paymentFeignService;

    @GetMapping(value = "/consumer/payment/get/{id}")
    public CommonResult getPaymentById(@PathVariable("id") Long id) {
        return paymentFeignService.getPaymentById(id);
    }

}
```

> 测试：

![image-20220324193852143](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241938533.png)

> 小结：

![image-20220324194025924](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203241940207.png)

`小柯总结：`

==**服务调用有两种解决方案，一种是ribbon+restTemplate，另外一种是OpenFeign**==

==**用OpenFeign更符合我们程序员的规范，因为OpenFeign来进行服务调用，采取的方式是controller调serivice**==



### OpenFeign的超时控制

> 模拟OpenFeign的超时报错

服务提供方8001端口添加

```
@GetMapping(value = "/payment/feign/timeout")
public String paymentFeignTimeout(){
    try { TimeUnit.SECONDS.sleep(3); }catch (Exception e) {e.printStackTrace();}
    return serverPort;
}
```

服务消费方80添加超时方法（service中）

```java
@GetMapping(value = "/payment/feign/timeout")
public String paymentFeignTimeout();
```

服务消费方80添加超时方法（controller中）

```
@GetMapping(value = "/consumer/payment/feign/timeout")
public String paymentFeignTimeout(){
    return paymentFeignService.paymentFeignTimeout();
}
```

> 测试

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203242006318.png)

> 解释

![image-20220324200730333](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203242007472.png)

> 在80的yml添加

```java
#设置feign客户端超时时间（OpenFeign默认支持Ribbon）
ribbon:
#指建立连接后从服务器读取到可用资源所用的时间
  ReadTimeout:  5000
#指的是建立连接所用的时间，适用于网络状况正常的情况下，两端连接所用的时间
  ConnectTimeout: 5000
```

> 这样就不再报错了

![image-20220324201503647](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203242015760.png)

### OpenFeign日志增强

> 是什么

![image-20220324202258581](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203242022673.png)

> 日志级别

![image-20220324202315062](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203242023221.png)

> 配置日志Bean

```java
package com.atguigu.springcloud.config;

import feign.Logger;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class FeignConfig {
   @Bean
    Logger.Level feignLoggerLevel(){
       return Logger.Level.FULL;
   }
}
```

> YML文件里需要开启Feign客户端

```
logging:
  level:
    #feign日志以什么级别监控哪个接口
    com.atguigu.springcloud.service.PaymentFeignService: debug
```

> 测试

![image-20220324202743721](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203242027822.png)

> 控制台可以看到日志

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203242029935.png)



## Hystrix（停更了）

### 概述

![image-20220325150541095](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203251505204.png)

![image-20220325150557469](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203251505653.png)

> 服务雪崩

![image-20220325150632620](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203251506750.png)

> Hystrix是什么

![image-20220325150657948](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203251506068.png)

> 能干嘛

- 服务降级
- 服务熔断
- 接近实时的监控
- ....

官网资料教你如何使用：[How To Use · Netflix/Hystrix Wiki (github.com)](https://github.com/Netflix/Hystrix/wiki/How-To-Use)

停更说明：https://github.com/Netflix/Hystrix

![image-20220325151934203](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203251519310.png)

### Hystrix的重要概念

==1.服务降级==：服务器忙，请稍后再试，不让客户端等待并且立刻返回一个友好提示，fallback

> 那些情况会触发降级：

- 程序运行异常
- 超时
- 服务熔断触发服务降级
- 线程池/信号量打满也会导致服务降级



==2.服务熔断==： 类比保险丝达到最大服务访问后，直接拒绝访问，拉闸限电，然后调用服务降级的方法并且返回友好提示

也可想象成保险丝: 服务的降级->进而熔断->恢复链路



==3.服务限流==： 秒杀高并发等操作，严禁一窝蜂的过来拥挤，大家排队，一秒钟N个，有序进行



### Hystrix支付微服务构建

新建模块：cloud-provider-hystrix-payment8001

> pom

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-provider-hystrix-payment8001</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
        <!--新增hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>


        <dependency>
            <groupId>com.atguigu.cloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>


        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
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
        </dependency>
    </dependencies>

</project>
```

> yml

```
server:
  port: 8001
  
eureka:
  client:
    register-with-eureka: true    #表识不向注册中心注册自己
    fetch-registry: true   #表示自己就是注册中心，职责是维护服务实例，并不需要去检索服务
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/

spring:
  application:
    name: cloud-provider-hystrix-payment
```

> 主启动类

```java
package springcloud;

import com.netflix.hystrix.contrib.metrics.eventstream.HystrixMetricsStreamServlet;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.cloud.client.circuitbreaker.EnableCircuitBreaker;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
@EnableEurekaClient
public class PaymentHystrixMain8001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentHystrixMain8001.class,args);
    }
    

}
```

> service

```java
package springcloud.service;

import cn.hutool.core.util.IdUtil;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;
import org.springframework.stereotype.Service;
import org.springframework.web.bind.annotation.PathVariable;

import java.util.concurrent.TimeUnit;

@Service
public class PaymentService {
    /*正常访问 一定ok*/
    public String paymentInfo_OK(Integer id){
        return "线程池，"+Thread.currentThread().getName()+" payment_ok,id"+id+"\t"+"哈哈";
    }

    
    public String paymentInfo_TimeOut(Integer id){
        int timeNumber=3;
        try {
            TimeUnit.SECONDS.sleep(timeNumber);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        return "线程池，"+Thread.currentThread().getName()+" payment_timeout,id"+id+"\t"+"哈哈"+"耗时"+timeNumber;
    }

   

}
```

> controller

```
package springcloud.controller;


import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import springcloud.service.PaymentService;

import javax.annotation.Resource;

@RestController
@Slf4j
public class PaymentController {
   @Resource
    private PaymentService paymentService;

   @Value("${server.port}")
    private String serverPort;

    @GetMapping("/payment/hystrix/ok/{id}")
    public String paymentInfo_OK(@PathVariable("id") Integer id){
        String result = paymentService.paymentInfo_OK(id);
        log.info("*******result:"+result);
        return result;
    }
    @GetMapping("/payment/hystrix/timeout/{id}")
    public String paymentInfo_TimeOut(@PathVariable("id") Integer id){
        String result = paymentService.paymentInfo_TimeOut(id);
        log.info("*******result:"+result);
        return result;
    }





}
```

> 启动7001,8001 测试

![image-20220325191050701](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203251910102.png)

![image-20220325191123388](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203251911506.png)



### Jmeter高并发测压后卡顿

![image-20220325200445562](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252004132.png)

> Jmeter开启后（200个线程，100个循环），浏览器测试ok的接口也出现了卡顿。

![image-20220325201234110](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252012300.png)

> 为什么会被卡死？

==tomcat的默认线程全部用到timeout接口上去了，没有线程来服务ok接口==

![image-20220325201334961](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252013080.png)

==上面还是服务提供者8001字节测试，加入此时外部的消费者80也来访问，那消费者只能干等，最终导致消费端80不满意，服务端8001直接被拖死==

### 订单服务调用支付服务出现卡顿

> 新建模块

cloud-consumer-feign-hystrix-order80

> pom

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-consumer-feign-hystrix-order80</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <!--新增hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>com.atguigu.cloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
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
        </dependency>
    </dependencies>

</project>


    
```

> yml：

```
server:
  port: 80


eureka:
  client:
    register-with-eureka: true    #表识不向注册中心注册自己
    fetch-registry: true   #表示自己就是注册中心，职责是维护服务实例，并不需要去检索服务
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/

spring:
  application:
    name: cloud-provider-hystrix-order
```

> 主启动类

```
package springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.hystrix.EnableHystrix;
import org.springframework.cloud.openfeign.EnableFeignClients;

@SpringBootApplication
@EnableFeignClients
public class OrderHystrixMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderHystrixMain80.class,args);
    }
}
```

> service

```java
package springcloud.service;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@Component
@FeignClient(value = "CLOUD-PROVIDER-HYSTRIX-PAYMENT")
public interface PaymentHystrixService {
    @GetMapping("/payment/hystrix/ok/{id}")
    public String paymentInfo_OK(@PathVariable("id") Integer id);

    @GetMapping("/payment/hystrix/timeout/{id}")
    public String paymentInfo_TimeOut(@PathVariable("id") Integer id);
}
```

> controller

```
package springcloud.controller;


import com.netflix.hystrix.contrib.javanica.annotation.DefaultProperties;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import springcloud.service.PaymentHystrixService;

import javax.annotation.Resource;

@RestController
@Slf4j
public class OrderHystrixController {
    @Resource
    private PaymentHystrixService paymentHystrixService;

    @GetMapping("/consumer/payment/hystrix/ok/{id}")
    public String paymentInfo_ok(@PathVariable("id") Integer id){
        String result=paymentHystrixService.paymentInfo_OK(id);
        return result;
    }

    @HystrixCommand
    @GetMapping("/consumer/payment/hystrix/timeout/{id}")
    public String paymentInfo_TimeOut(@PathVariable("id") Integer id){
        String result=paymentHystrixService.paymentInfo_TimeOut(id);
        return result;
    }


}
```

开启7001，8001，80

> 测试：2w个线程压8001 消费端80微服务再去访问正常的ok微服务8001地址

![image-20220325202236292](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252022385.png)

或者（OpenFeign超时不再等待）：

![image-20220325202859794](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252028896.png)



### 降级容错解决的维度要求

==正因为有上诉故障或不加表现，才有我们的降级/容错/限流等技术诞生==

> 如何解决？解决的要求

超时导致服务器变慢（转圈）  ------------------超时不再等待

出错（宕机或程序运行出错） -------------------出错要兜底

> 解决

对方服务（8001）超时了，调用者（80）不能一直卡死等待，必须有服务降级

对方服务（8001）down机了，调用者（80）不能一直卡死等待，必须有服务降级

对方服务（8001）ok，调用者（80）自己出故障或有自我要求（自己等待时间小于服务提供者），自己处理降级



### Hystrix之服务支付侧fallback

> 修改8001的service 改成五秒

```java
package springcloud.service;

import cn.hutool.core.util.IdUtil;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;
import org.springframework.stereotype.Service;
import org.springframework.web.bind.annotation.PathVariable;

import java.util.concurrent.TimeUnit;

@Service
public class PaymentService {
    /*正常访问 一定ok*/
    public String paymentInfo_OK(Integer id){
        return "线程池，"+Thread.currentThread().getName()+" payment_ok,id"+id+"\t"+"哈哈";
    }


    public String paymentInfo_TimeOut(Integer id){
        int timeNumber=5;
        try {
            TimeUnit.SECONDS.sleep(timeNumber);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        return "线程池，"+Thread.currentThread().getName()+" payment_timeout,id"+id+"\t"+"哈哈"+"耗时"+timeNumber;
    }



}
```

> 8001主启动类新加注解@EnableCircuitBreaker      

```java
package springcloud;

import com.netflix.hystrix.contrib.metrics.eventstream.HystrixMetricsStreamServlet;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.cloud.client.circuitbreaker.EnableCircuitBreaker;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
@EnableEurekaClient
@EnableCircuitBreaker      //新加的注解
public class PaymentHystrixMain8001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentHystrixMain8001.class,args);
    }


}
```

> 修改8001的controller

```
package springcloud.controller;


import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import springcloud.service.PaymentService;

import javax.annotation.Resource;

@RestController
@Slf4j
public class PaymentController {
   @Resource
    private PaymentService paymentService;

   @Value("${server.port}")
    private String serverPort;

    @GetMapping("/payment/hystrix/ok/{id}")
    public String paymentInfo_OK(@PathVariable("id") Integer id){
        String result = paymentService.paymentInfo_OK(id);
        log.info("*******result:"+result);
        return result;
    }
    @HystrixCommand(fallbackMethod = "paymentInfo_TimeOutHandler",
            commandProperties =
                    //三秒钟以内是正常逻辑 超过三秒走兜底方法
                    {@HystrixProperty(name="execution.isolation.thread.timeoutInMilliseconds",value = "3000")})
    @GetMapping("/payment/hystrix/timeout/{id}")
    public String paymentInfo_TimeOut(@PathVariable("id") Integer id){
        String result = paymentService.paymentInfo_TimeOut(id);
        log.info("*******result:"+result);
        return result;
    }
    //兜底方法
    public String paymentInfo_TimeOutHandler(Integer id){
        return "线程池"+Thread.currentThread().getName()+"系统繁忙或者运行报错，请稍后再试,id"+id+"\t"+"o(╥﹏╥)o";
    }
}

```

> 测试： 无论是消费者端开始服务者端都可以开启服务降级

![image-20220325204825341](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252130881.png)



> 将timeout接口写成运行异常错误：

修改8001的service

```java
package springcloud.service;

import cn.hutool.core.util.IdUtil;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;
import org.springframework.stereotype.Service;
import org.springframework.web.bind.annotation.PathVariable;

import java.util.concurrent.TimeUnit;

@Service
public class PaymentService {
    /*正常访问 一定ok*/
    public String paymentInfo_OK(Integer id){
        return "线程池，"+Thread.currentThread().getName()+" payment_ok,id"+id+"\t"+"哈哈";
    }


    public String paymentInfo_TimeOut(Integer id){
        int timeNumber=5;
      /*  try {
            TimeUnit.SECONDS.sleep(timeNumber);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }*/
        int age=10/0;

        return "线程池，"+Thread.currentThread().getName()+" payment_timeout,id"+id+"\t"+"哈哈"+"耗时"+timeNumber;
    }

}
```

> 再次启动测试

![image-20220325204825341](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252116865.png)

==上面故意制造了两个异常：==

1 int age=10/0

2 我们能接受3s，它运行5s，超时异常

无论是哪种异常都会走我们的兜底方法



### Hystrix之服务降级订单侧fallback

80订单微服务，也可以更好地保护自己，照样画葫芦进行客户端保护（Hystrix的服务降级一般用在客户端）

> yml中添加：

```
feign:
  hystrix:
    enabled: true #如果处理自身的容错就开启。开启方式与生产端不一样。
```

> 主启动类新增注解：

```
@EnableHystrix
```

> controller添加

```
@GetMapping("/consumer/payment/hystrix/timeout/{id}")
@HystrixCommand(fallbackMethod = "paymentTimeOutFallbackMethod",commandProperties = {
        @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "3000")  //3秒钟以内就是正常的业务逻辑
})
public String paymentInfo_TimeOut(@PathVariable("id") Integer id){
    String result = paymentHystrixService.paymentInfo_TimeOut(id);
    return result;
}

//兜底方法
public String paymentTimeOutFallbackMethod(@PathVariable("id") Integer id){
    return "我是消费者80，对付支付系统繁忙请10秒钟后再试或者自己运行出错请检查自己,(┬＿┬)";
}
```

> 输出

![image-20220325215251768](../AppData/Roaming/Typora/typora-user-images/image-20220325215251768.png)

> 写成这样也会显示上面的输出

![image-20220325215425350](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252154447.png)

### Hystrix之全局服务降级DefaultProperties

==目前问题：==

每个业务方法对应一个兜底的方法，代码膨胀，我们需要将统一的和自定义的分开

![image-20220325221216958](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252212159.png)

![image-20220325221058248](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252210587.png)

![image-20220325221112982](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252211181.png)

> 修改80的controller

```
package springcloud.controller;


import com.netflix.hystrix.contrib.javanica.annotation.DefaultProperties;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import springcloud.service.PaymentHystrixService;

import javax.annotation.Resource;

@RestController
@Slf4j
@DefaultProperties(defaultFallback = "payment_Global_FallbackMethod")  //全局的
public class OrderHystrixController {

    @Resource
    private PaymentHystrixService paymentHystrixService;

    @GetMapping("/consumer/payment/hystrix/ok/{id}")
    public String paymentInfo_OK(@PathVariable("id") Integer id){
        String result = paymentHystrixService.paymentInfo_OK(id);
        return result;
    }

//    @GetMapping("/consumer/payment/hystrix/timeout/{id}")
//    public String paymentInfo_TimeOut(@PathVariable("id") Integer id){
//        String result = paymentHystrixService.paymentInfo_TimeOut(id);
//        return result;
//    }

    @GetMapping("/consumer/payment/hystrix/timeout/{id}")
//    @HystrixCommand(fallbackMethod = "paymentTimeOutFallbackMethod",commandProperties = {
//            @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "1500")  //1.5秒钟以内就是正常的业务逻辑
//    })
    @HystrixCommand
    public String paymentInfo_TimeOut(@PathVariable("id") Integer id){
        int age = 10/0;
        String result = paymentHystrixService.paymentInfo_TimeOut(id);
        return result;
    }

    //兜底方法
    public String paymentTimeOutFallbackMethod(@PathVariable("id") Integer id){
        return "我是消费者80，对付支付系统繁忙请10秒钟后再试或者自己运行出错请检查自己,(┬＿┬)";
    }

    //下面是全局fallback方法
    public String payment_Global_FallbackMethod(){
        return "Global异常处理信息，请稍后再试,(┬＿┬)";
    }
}
 
 
```

> 测试输出：

![image-20220325221720222](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252217393.png)

### Hystrix之通配服务降级FeignFallback

==问题：上面虽然有全局服务降级，但是兜底方法和业务逻辑混在一块，很混乱==、

==**本次的案例服务降级处理实在客户端80实现完成的，与服务端8001没有关系，只需要为Feign客户端定义的接口添加一个服务降级处理的实现类即可实现解耦**==

> 未来我们要面对的异常：

- 运行（运行时异常，比如除以0）
- 超时（执行时间过长）
- 宕机（服务挂掉不可用）

> 在80的service添加一个类 用来写对应方法出错时候的兜底方法

```java
package com.atguigu.springcloud.service;

import org.springframework.stereotype.Component;

@Component
public class PaymentFallbackService implements PaymentHystrixService {
    @Override
    public String paymentInfo_OK(Integer id) {
        return "-----PaymentFallbackService fall back-paymentInfo_OK , (┬＿┬)";
    }

    @Override
    public String paymentInfo_TimeOut(Integer id) {
        return "-----PaymentFallbackService fall back-paymentInfo_TimeOut , (┬＿┬)";
    }
}
 
 
```

> 修改service

```
package springcloud.service;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import springcloud.service.PaymentFallbackService;

@Component
@FeignClient(value = "CLOUD-PROVIDER-HYSTRIX-PAYMENT",fallback = PaymentFallbackService.class)
public interface PaymentHystrixService {

    @GetMapping("/payment/hystrix/ok/{id}")
    public String paymentInfo_OK(@PathVariable("id") Integer id);

    @GetMapping("/payment/hystrix/timeout/{id}")
    public String paymentInfo_TimeOut(@PathVariable("id") Integer id);


}
```

启动7001，8001，80

> 测试：

![image-20220325223626033](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252236178.png)

> 故意关闭微服务8001

![image-20220325223439195](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252236229.png)

==此时服务端provider已经down了，但是我们做了服务降级处理，让客户端在服务端不可用时也会获得提升信息而不会挂起耗死服务器==



### Hystrix之服务熔断理论

断路器：一句话就是家里的保险丝

Hystrix是springcloud里面的一个断路器

> 熔断是什么？

![image-20220325225225755](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252252162.png)

==大神论文==      [CircuitBreaker (martinfowler.com)](https://martinfowler.com/bliki/CircuitBreaker.html)



### Hystrix之服务熔断案例

> 修改微服务8001的service

```java
package springcloud.service;

import cn.hutool.core.util.IdUtil;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;
import org.springframework.stereotype.Service;
import org.springframework.web.bind.annotation.PathVariable;

import java.util.concurrent.TimeUnit;

@Service
public class PaymentService {
    /*正常访问 一定ok*/
    public String paymentInfo_OK(Integer id){
        return "线程池，"+Thread.currentThread().getName()+" payment_ok,id"+id+"\t"+"哈哈";
    }


    public String paymentInfo_TimeOut(Integer id){
        int timeNumber=5;
      /*  try {
            TimeUnit.SECONDS.sleep(timeNumber);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }*/
        int age=10/0;

        return "线程池，"+Thread.currentThread().getName()+" payment_timeout,id"+id+"\t"+"哈哈"+"耗时"+timeNumber;
    }


    //服务熔断
    @HystrixCommand(fallbackMethod = "paymentCircuitBreaker_fallback",commandProperties = {
            @HystrixProperty(name = "circuitBreaker.enabled",value = "true"),  //是否开启断路器
            @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold",value = "10"),   //请求次数
            @HystrixProperty(name = "circuitBreaker.sleepWindowInMilliseconds",value = "10000"),  //时间范围
            @HystrixProperty(name = "circuitBreaker.errorThresholdPercentage",value = "60"), //失败率达到多少后跳闸
    })
    public String paymentCircuitBreaker(@PathVariable("id") Integer id){
        if (id < 0){
            throw new RuntimeException("*****id 不能负数");
        }
        String serialNumber = IdUtil.simpleUUID();

        return Thread.currentThread().getName()+"\t"+"调用成功,流水号："+serialNumber;
    }
    public String paymentCircuitBreaker_fallback(@PathVariable("id") Integer id){
        return "id 不能负数，请稍候再试,(┬＿┬)/~~     id: " +id;
    }





}

```



> 为什么配置这些参数？

官网说断路器的关闭和打开过程发生在以下几步

![image-20220325231259667](../AppData/Roaming/Typora/typora-user-images/image-20220325231259667.png)

1.请求次数满足阈值条件

2.错误百分比超过了阈值

3.满足上面两个条件就是从关闭状态到打开状态

4.所有的请求，短期内都无法正常使用

5.一段时间（时间窗口期）后，下一个请求尝试让他通过，若没通过，则继续保持打开状态在一个窗口期（重新进入完整的休眠期）内，如果通过了，则转为关闭状态

> 8001的controller

```java
package springcloud.controller;


import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import springcloud.service.PaymentService;

import javax.annotation.Resource;

@RestController
@Slf4j
public class PaymentController {
   @Resource
    private PaymentService paymentService;

   @Value("${server.port}")
    private String serverPort;

    @GetMapping("/payment/hystrix/ok/{id}")
    public String paymentInfo_OK(@PathVariable("id") Integer id){
        String result = paymentService.paymentInfo_OK(id);
        log.info("*******result:"+result);
        return result;
    }
    @HystrixCommand(fallbackMethod = "paymentInfo_TimeOutHandler",
            commandProperties =
                    //三秒钟以内是正常逻辑 超过三秒走兜底方法
                    {@HystrixProperty(name="execution.isolation.thread.timeoutInMilliseconds",value = "3000")})
    @GetMapping("/payment/hystrix/timeout/{id}")
    public String paymentInfo_TimeOut(@PathVariable("id") Integer id){
        String result = paymentService.paymentInfo_TimeOut(id);
        log.info("*******result:"+result);
        return result;
    }
    //兜底方法
    public String paymentInfo_TimeOutHandler(Integer id){
        return "线程池"+Thread.currentThread().getName()+"系统繁忙或者运行报错，请稍后再试,id"+id+"\t"+"o(╥﹏╥)o";
    }
    //===服务熔断
    @GetMapping("/payment/circuit/{id}")
    public String paymentCircuitBreaker(@PathVariable("id") Integer id){
        String result = paymentService.paymentCircuitBreaker(id);
        log.info("*******result:"+result);
        return result;
    }
}
```

> 启动7001和8001

![image-20220325231911734](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252319895.png)

![image-20220325231936120](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252319292.png)

> 疯狂uri中写负数 再写正数

![image-20220325232023876](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252320072.png)

> 一直正数 后面就正常了

![image-20220325232056857](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252320143.png)

==多次错误，然后慢慢正确，发现刚开始不满足条件，就算是正确的访问地址也不能进行访问，需要慢慢的恢复链路==

### Hystrix之服务熔断总结

> 大神的结论：

![image-20220325232753526](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252327727.png)

> 官网断路器流程图

![image-20220325232904004](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252329324.png)

> 官网步骤

![image-20220325232939562](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252329945.png)

> 断路器在什么情况下起作用

![image-20220325233023936](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252330199.png)

![image-20220325233057286](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252330979.png)

> 断路器打开之后：

![image-20220326001238102](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203260012252.png)

> 断路器还有其他的很多配置  用到的话再自行研究吧

![image-20220325233226814](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252332091.png)

![image-20220325233256430](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203252332771.png)

### 服务限流

后面高级篇讲解alibaba的Sentinel再讲解

### Hystrix工作流程最后总结

在官网可以看到工作流程：[它是如何运作的 ·Netflix/Hystrix Wiki (github.com)](https://github.com/Netflix/Hystrix/wiki/How-it-Works)

![image-20220326083816833](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203260838932.png)

> 官网流程图：

![image-20220326082613350](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203260826512.png)

==5.如果断路器打开（4）了/线程池（tomcat中的）或者信号量满了（5）/抛异常了（6），就走getfallback方法==

> 步骤说明：

![image-20220326082444462](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203260824623.png)



### Hystrix图形化Dashboard搭建

![image-20220326084641067](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203260846350.png)

> 新建模块：cloud-consumer-hystrix-dashboard9001

pom

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-consumer-hystrix-dashboard9001</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <!--新增hystrix dashboard-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
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
        </dependency>
    </dependencies>

</project>
```

> yml

```
server:
  port: 9001
 
```

> 主启动类   新加注解@EnableHystrixDashboard

```
package springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.hystrix.dashboard.EnableHystrixDashboard;

@SpringBootApplication
@EnableHystrixDashboard
public class HystrixDashboardMain9001 {
    public static void main(String[] args) {
        SpringApplication.run(HystrixDashboardMain9001.class,args);
    }
}
```

> 启动9001

![image-20220326085109683](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203260851843.png)



### Hystrix图形化Dashboard监控实战

==所有Provider微服务提供类（8001）都需要监控依赖配置==

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

> 注意：新版本Hystrix需要在主启动类中8001（被监控的服务）中指定监控路径

```java
package springcloud;

import com.netflix.hystrix.contrib.metrics.eventstream.HystrixMetricsStreamServlet;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.cloud.client.circuitbreaker.EnableCircuitBreaker;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
@EnableEurekaClient
@EnableCircuitBreaker      //新加的注解
public class PaymentHystrixMain8001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentHystrixMain8001.class,args);
    }
    @Bean
    public ServletRegistrationBean getServlet(){
        HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
        ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);
        registrationBean.setLoadOnStartup(1);
        registrationBean.addUrlMappings("/hystrix.stream");
        registrationBean.setName("HystrixMetricsStreamServlet");
        return registrationBean;
    }
}
```

> 启动7001，8001，9001

![image-20220326090353611](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203260903754.png)

> 疯狂点击正确的地址 [localhost:8001/payment/circuit/31](http://localhost:8001/payment/circuit/31)

![image-20220326090542024](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203260905222.png)

> 疯狂点击错误的地址 [localhost:8001/payment/circuit/-31](http://localhost:8001/payment/circuit/-31)

![image-20220326090749680](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203260907792.png)

> 如何看？

7色

==1圈==

![image-20220326091131601](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203260911698.png)

==1线==

![image-20220326091155648](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203260911729.png)==整图说明1==

![image-20220326091239462](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203260912601.png)

==整图说明2==

![image-20220326091422304](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203260914462.png)

==搞懂一个才能搞懂复杂的==

![image-20220326091449726](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203260914896.png)



## GateWay

> 微服务架构中，网关在哪？

![image-20220326122215003](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261222167.png)

> GateWay是什么？

![image-20220326121345770](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261213875.png)

![image-20220326121420957](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261214088.png)

zuul官网：[首页 ·Netflix/zuul Wiki (github.com)](https://github.com/Netflix/zuul/wiki)

GateWay官网：[Spring Cloud Gateway](https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.2.1.RELEASE/reference/html/)

> 一句话概述：

![image-20220326115518220](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261155501.png)

![image-20220326115549659](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261155783.png)

> 源码架构

![image-20220326121514175](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261215320.png)

> Gateway具有如下特性

![image-20220326141439889](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261414191.png)

> Gateway和zuul的区别

![image-20220326141430866](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261414129.png)

![image-20220326141415186](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261414520.png)



> zuul1.x模型

![image-20220326141524770](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261415039.png)

![image-20220326141544648](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261415917.png)

> GateWay模型

WebFlux是什么？

![image-20220326141645387](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261416605.png)

![image-20220326141656318](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261416486.png)

![image-20220326141711584](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261417918.png)

### 三大核心概念

三大核心概念

- Route(路由)：
   ==路由是构建网关的基本模块==，它由==ID==，目标==URI==，==一系列的断言==和==过滤器==组成，如果断言为true则匹配该路由

- Predicate（断言）：
   参考的是java8的java.util.function.Predicate。开发人员可以匹配HTTP请求中的所有内容（例如请求头或请求参数），如果请求与断言相匹配则进行路由

- Filter(过滤)：
   指的是Spring框架中GatewayFilter的实例，使用过滤器，可以在请求被==路由前或者之后对请求==进行修改。

![image-20220326141940201](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261419698.png)

### GateWay的工作流程

> 官网总结

![image-20220326142106649](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261421821.png)

![image-20220326142118606](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261421898.png)

==核心逻辑：==

路由转发+执行过滤链

### GateWay9527搭建

```java
新建模块：cloud-gateway-gateway9527
```

> pom

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-gateway-gateway9527</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <!--新增gateway-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>
        <dependency>
            <groupId>com.atguigu.cloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
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
        </dependency>



    </dependencies>


</project>
```

> yml

```java
server:
  port: 9527
spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      routes:
        - id: payment_routh #路由的ID，没有固定规则但要求唯一，建议配合服务名
          uri: http://localhost:8001   #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**   #断言,路径相匹配的进行路由

        - id: payment_routh2
          uri: http://localhost:8001
          predicates:
            - Path=/payment/lb/**   #断言,路径相匹配的进行路由


eureka:
  instance:
    hostname: cloud-gateway-service
  client:
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka
 
 

```

> 主启动类

```
package com.atguigu.springcloud;


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient
public class GateWayMain9527 {
    public static void main(String[] args) {
        SpringApplication.run(GateWayMain9527.class,args);
    }
 }
```

> 测试：启动7001，8001，9527

![image-20220326145810173](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261458477.png)

![image-20220326145840953](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261458122.png)

> 说明

![image-20220326151952924](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261519355.png)



> Gateway网关路由有两种配置方式一种是上面的yml，一种是编码的方式，接下来我们来讲讲编码的方式

官网案例：

![image-20220326153612078](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261536349.png)

> 需求：通过9527网关访问到外网的百度新闻地址

编码（新增一个配置类）：

```java
package com.atguigu.springcloud;

import org.springframework.cloud.gateway.route.RouteLocator;
import org.springframework.cloud.gateway.route.builder.RouteLocatorBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class GateWayConfig {

    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder routeLocatorBuilder) {
        RouteLocatorBuilder.Builder routes = routeLocatorBuilder.routes();
        //localhost：9527/guonei ---》http://news.baidu.com/guonei
        routes.route("path_rote_atguigu", r -> r.path("/guonei").uri("http://news.baidu.com/guonei")).build();  
        return routes.build();
    }
}
```

![image-20220326154037973](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261540878.png)

> 改成这样也行

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261546147.png)

![image-20220326154539109](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261545496.png)



### GateWay配置动态路由

> 上述存在问题：地址写死了，无法做到负载均衡。实际开发中都是集群

![image-20220326155227595](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261552019.png)

==**以前是用ribbon实现的负载均衡，现在我们不用ribbon，而是用GateWay网关进行动态路由，实现负载均衡**==

==默认情况下GateWay会根据注册中心的服务列表，以注册中心上微服务名为路径创建动态路由进行转发，从而实现动态路由的功能==

> 修改9527的yml

```java
server:
  port: 9527
spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true  #开启从注册中心动态创建路由的功能，利用微服务名进行路由
      routes:
        - id: payment_routh #路由的ID，没有固定规则但要求唯一，建议配合服务名
          #uri: http://localhost:8001   #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service
          predicates:
            - Path=/payment/get/**   #断言,路径相匹配的进行路由

        - id: payment_routh2
          #uri: http://localhost:8001   #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service
          predicates:
            - Path=/payment/lb/**   #断言,路径相匹配的进行路由


eureka:
  instance:
    hostname: cloud-gateway-service
  client:
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka
 
 
```

==需要注意的是uri的协议为lb，表示启用GateWay的负载均衡功能。lb://cloud-payment-service是Gateway在微服务中自动为我们创建的负载均衡uri==

> 测试：启动7001，8001，8002，9527 访问：[localhost:9527/payment/lb](http://localhost:9527/payment/lb)

发现两个端口轮流出现

![image-20220326160515092](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261605288.png)

### GateWay常用的predicate

> 刚刚启动9527控制台出现的东西    这是用来精确匹配的

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261621489.png)

> 文档说明：

![image-20220326163615857](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261636305.png)

![image-20220326163905763](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261639951.png)

> 常用的路由断言

![image-20220326164819904](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261650711.png) 

因为属实太多了 这里只列举一个例子 若实际开发中用到其他的 百度一下即可，都大同小异

> 修改9527的yml

```java
server:
  port: 9527
spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true  #开启从注册中心动态创建路由的功能，利用微服务名进行路由
      routes:
        - id: payment_routh #路由的ID，没有固定规则但要求唯一，建议配合服务名
          #uri: http://localhost:8001   #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service
          predicates:
            - Path=/payment/get/**   #断言,路径相匹配的进行路由

        - id: payment_routh2
          #uri: http://localhost:8001   #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service
          predicates:
            - Path=/payment/lb/**   #断言,路径相匹配的进行路由
            - After=2022-03-31T09:54:17.093+08:00[Asia/Shanghai]
            #- Cookie=username,zzyy
            #  - Header=X-Request-Id, \d+  #请求头要有X-Request-Id 属性并且值为整数的正则表达式
            # - Host=**.atguigu.com
            #- Method=GET



eureka:
  instance:
    hostname: cloud-gateway-service
  client:
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka
```

> 只能在设置的时间后面访问才能匹配到，否则报错

![image-20220326165414325](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261654650.png)

==**小总结：说白了，Predicate就是为了实现一组匹配规则，让请求过来找到对应的Route进行处理**==

### GateWay的Filter

> 是什么？

![image-20220326170607837](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261706176.png)

Filter的生命周期：

1. 在业务逻辑之前
2. 在业务逻辑之后

Filter的种类：

单一  GatewayFilter   自带的有31种

全局  GlobalFilter      自带的有11种

> 接下来来介绍常用的GatewayFilter ：AddRequestParameter 

![image-20220326170833509](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261708846.png)

我们也可以自定义过滤器：

下面我们来自定义一个全局的过滤器GlobalFilter：

它能干嘛呢？

- 全局日志记录
- 统一网关鉴权
- ........

> 9527新加配置类

```java
package com.atguigu.springcloud.config;

import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Component;

@Component
@Slf4j
public class MyLogGateWatFilter implements GlobalFilter, Ordered {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        log.info("*********come in MyLogGateWayFilter: "+new Date());
        String uname = exchange.getRequest().getQueryParams().getFirst("uname");
        if(StringUtils.isEmpty(uname)){
            log.info("*****用户名为Null 非法用户,(┬＿┬)");
            exchange.getResponse().setStatusCode(HttpStatus.NOT_ACCEPTABLE);//给人家一个回应
            return exchange.getResponse().setComplete();
        }
        return chain.filter(exchange);//执行下一个过滤链
    } 
    @Override
    public int getOrder() {
        return 0;
    }
}
```

使用了这个之后

我们的请求如果是没有带有uname这个参数就会被过滤,可以用来作为一些必要参数的筛选和鉴权.

 启动700,8001,8002,9257进行测试 ，访问localhost:9527/payment/lb/uname=z3即可看见效果！！！！

![image-20220326171733106](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261717422.png)

![image-20220326171702400](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203261717692.png)



## Config（分布式配置中心）

==**已经过时了被阿里巴巴的Nacos替代了**==

如果有很多个微服务，每个微服务对应同一个数据库，那么迁移数据库或者修改数据库的名称，这么多个微服务不得改疯掉

> 分布式系统面临的问题？

![image-20220327105820924](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203271058412.png)



> Config是什么？

![image-20220327110317341](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203271103580.png)



> 如何玩？

SpringCloud Config 为微服务架构中的微服务提供集中化的外部配置支持，配置服务器为各个不同微服务应用的所有环境提供了一个中心化的外部配置

SpringCloud Config 分为==服务端==和==客户端==两部分，==服务端也称为分布式配置中心==，==他是一个独立的微服务应用，用来连接配置服务器并为客户端提供获取配置信息，加密解密信息等访问接口==

客户端则是通过指定的配置中心来管理应用资源，以及业务相关的配置内容，==并在启动的时候从配置中心加载配置获取配置信息，====配置服务器默认采用git来存储配置信息。这样就有助于对环境配置进行版本管理，并且通过git客户端工具来方便的管理和访问配置内容。==

> 能干嘛

- 集中管理配置文件
- 不同环境不同配置，动态化的配置更新，分环境部署比如 dev/test/prod/beta/release
  运行期间动态调整配置，不再需要在每个服务部署的机器上编写配置文件，服务会向配置中心统一拉去自己的配置信息
- 当配置发生变动时，服务不需要重启就可以感知到配置的变化并应用新的配置
- 将配置信息以REST接口的形式暴露

> 与github整合配置：

![image-20220327110635136](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203271106331.png)



> 官网：

[Spring Cloud Config](https://docs.spring.io/spring-cloud-config/docs/current/reference/html/)

![image-20220327110951082](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203271109427.png)



### config服务端配置与测试

现在github建一个仓库：名字任意：springcloud-config2

新建三个配置文件

![image-20220327141204485](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203271412790.png)

> 新建模块

```java
cloud-config-center-3344
```

> pom

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-config-center-3344</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>


        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>com.atguigu.cloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
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
        </dependency>
    </dependencies>


</project>
```

> yml

```java
server:
  port: 3344
spring:
  application:
    name: cloud-config-center
  cloud:
    config:
      server:
        git:
          uri: https://github.com/kkkkwqqqq/springcloud-config2.git   #填写你自己的github路径
          search-paths:
            - springcloud-config2   #仓库名字
      label: main   #分支名字
eureka:
  client:
    service-url:
      defaultZone:  http://localhost:7001/eureka


```

> 主启动类

```
package springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.config.server.EnableConfigServer;

@SpringBootApplication
@EnableConfigServer
public class MainAppConfigCenter3344 {
    public static void main(String[] args) {
        SpringApplication.run(MainAppConfigCenter3344.class,args);
    }
}
```

> 测试通过Config微服务是否可以从Github上获取配置内容

启动微服务7001和3344 

![image-20220327150857307](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203271508552.png)

==配置读取规则：下面的config-3344.com一律改成localhost   master改为main==

![image-20220327143009155](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203271430586.png)



### config客户端配置与测试

> 新建模块：cloud-config-client-3355

pom

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-config-client-3355</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>com.atguigu.cloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
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
        </dependency>
    </dependencies>

</project>


    
```

> bootstrap.yml是什么？

![image-20220327144826984](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203271448262.png)

> boostrap.yml

```java
server:
  port: 3355

spring:
  application:
    name: config-client
  cloud:
    config:
      label: main
      name: config
      profile: dev
      uri: http://localhost:3344
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka
 
```

> 说明    config-3344.com就是localhost 不过是加了资源隐射罢了 三个综合就是：localhost:3344/main/conf-dev.yml

![image-20220327144955319](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203271449639.png)

> 主启动类

```java
package springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@EnableEurekaClient
@SpringBootApplication
public class ConfigClientMain3355 {
    public static void main(String[] args) {
        SpringApplication.run(ConfigClientMain3355.class,args);
    }
}
```

> 业务类

```
package springcloud.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ConfigClientController {

    @Value("${config.info}")  //这里的config.info是从github的配置文件种拿的
    private String configInfo;

    @GetMapping("/configInfo")
    public String getConfigInfo(){
        return configInfo;
    }
}



```

> 测试：启动7001，3344，3355

![image-20220327150655142](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203271506261.png)



==这样就实现了客户端3355访问SpringCloud Config3344通过Github获取配置信息==

> 可是问题随之而来

==**linux运维修改github上的配置文件(将版本号改为2)，刷新3344发现configserver配置立刻做出响应。刷新3355，发现ConServer客户都安没有任何响应。除非3355字节重启。难道每次运维修改配置，客户端都需要重启？？？噩梦（实际开发种，每一个微服务重启都十分花时间）！！！**==



### config之动态刷新之手动版

> 3355添加监控依赖：

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

> 修改yml，暴露监控端口

```
server:
  port: 3355

spring:
  application:
    name: config-client
  cloud:
    config:
      label: main
      name: config
      profile: dev
      uri: http://localhost:3344
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka
#暴露监控端点
management:
  endpoints:
    web:
      exposure:
        include: "*"
```

> controller新加一个刷新注解

```
package springcloud.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.context.config.annotation.RefreshScope;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RefreshScope //新加的注解 具备刷新能力
public class ConfigClientController {

    @Value("${config.info}")  //这里的config.info是从github的配置文件种拿的
    private String configInfo;

    @GetMapping("/configInfo")
    public String getConfigInfo(){
        return configInfo;
    }
}
```

> 启动3344,3355,7001，访问

![image-20220327194427579](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203271944053.png)

![image-20220327194459808](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203271944923.png)

> 更改版本号为4   再次访问

![image-20220327194617588](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203271946761.png)

![image-20220327194632600](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203271946788.png)

==发现还是没用，这个时候需要运维多做一步，发个post请求==

![image-20220327195016367](../AppData/Roaming/Typora/typora-user-images/image-20220327195016367.png)

![image-20220327195025663](../AppData/Roaming/Typora/typora-user-images/image-20220327195025663.png)

==**这样就避免了服务重启**==

> 一个遗留问题：

==加入有多个微服务客户端3355/3366/3377..... 每个微服务都要执行一次post请求，手动刷新属实麻烦，可否广播，一次通知，处处生效呢？我们想要扩大范围的自动刷新，求方法？这就引出了我们的Bus消息总线啦==



## Bus消息总线

### 概念

> 是什么？

![image-20220327200817007](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203272008279.png)

==可以实现分布式自动刷新配置功能==

>  产生原因：

配合==SpringCloud Config==来实现配置的动态刷新（单单使用config来刷新过于繁琐） 

> 设计思想：

Bus支持两种消息代理：RabbtiMq和Kafka

> 能干嘛？

![image-20220327200948282](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203272009434.png)

![image-20220327201049517](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203272010764.png)

![image-20220327201106871](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203272011093.png)

### Bus之RabbitMQ环境安装

1.安装Erlang

2.安装RabbitMQ   这两个安装百度吧

安装完后进入sbin目录

![image-20220327212655839](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203272126088.png)

在这个目录下cmd输入圈起来的内容

![image-20220327212834118](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203272128364.png)

> 双击start启动Rabbitmq，成功启动访问页面效果  默认账号密码都是：guest

![image-20220327213046567](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203272130831.png)

### Bus动态刷新全局广播的设计思想和选型

新建模块：3366（内容和3355一样）

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-config-client-3366</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>com.atguigu.cloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
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
        </dependency>
    </dependencies>
</project>

```

> yml

```
server:
  port: 3366

spring:
  application:
    name: config-client
  cloud:
    config:
      label: main
      name: config
      profile: dev
      uri: http://localhost:3344
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka
#暴露监控端点
management:
  endpoints:
    web:
      exposure:
        include: "*"
```

controller

```
package springcloud.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.context.config.annotation.RefreshScope;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RefreshScope //新加的注解 具备刷新能力
public class ConfigClientController {

    @Value("${config.info}")  //这里的config.info是从github的配置文件种拿的
    private String configInfo;

    @GetMapping("/configInfo")
    public String getConfigInfo(){
        return configInfo;
    }
}
```

> 主启动类

```java
package springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@EnableEurekaClient
@SpringBootApplication
public class ConfigClientMain3366 {
    public static void main(String[] args) {
        SpringApplication.run(ConfigClientMain3366.class,args);
    }
}
```

> 设计思想

1.

![image-20220327215153611](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203272151961.png)

2.

![image-20220327215232148](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203272152450.png)

![image-20220327215318144](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203272153305.png)



### Bus动态刷新全局广播配置实现

> 给3344配置中心服务端添加消息总线支持

pom添加：

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
```

yml：添加

```
server:
  port: 3344
spring:
  application:
    name: cloud-com.atguigu.springcloud.alibaba.config-center
  cloud:
    config:
      server:
        git:
          uri: https://gitee.com/kekekeke111/springcloud-com.atguigu.springcloud.alibaba.config.git
          search-paths:
            - springcloud-com.atguigu.springcloud.alibaba.config
          username: kekekeke111
          password: 990325ab
      label: main

#Rabitmq相关配置
rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka
#Rabitmq相关配置,暴露bus刷新配置的端点
management:
  endpoints:  #暴露bus刷新配置的端点
    web:
      exposure:
        include: 'bus-refresh'




```

> 给3355客户端添加消息总线支持

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
```

yml添加：

```
server:
  port: 3355

spring:
  application:
    name: config-client
  cloud:
    config:
      label: main
      name: config
      profile: dev
      uri: http://localhost:3344
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka
#暴露监控端点
management:
  endpoints:
    web:
      exposure:
        include: "*"





```

> 给3355客户端添加消息总线支持

pom添加：

```java
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
```

yml

```
server:
  port: 3356

spring:
  application:
    name: config-client
  cloud:
    config:
      label: main
      name: config
      profile: dev
      uri: http://localhost:3344
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka
#暴露监控端点
management:
  endpoints:
    web:
      exposure:
        include: "*"





```

> 测试

启动这些微服务   浏览器中看到的都是version=4

![image-20220327224042805](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203272240220.png)

> 运维工程师修改为version=5，再测试

![image-20220327224227026](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203272242199.png)

![image-20220327224240577](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203272242893.png)



> 手动刷新

![image-20220327225116646](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203272251976.png)

再次访问（无需重启微服务）就都变成5了

### SpringCloud Bus动态刷新定点通知

![image-20220327231322234](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203272313580.png)

==这里的destination是微服务名称:端口号==

### 总结：

![image-20220327231458158](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203272314296.png)



## SpringCloud Stream(消息驱动)

### Stream概述

> Stream为什么被引入

现在消息中间件太多，你学了这个，公司用那个，那么程序员学习负担就很重

前端---java ee ---大数据平台          java ee 用rabbitmq 大数据用kafka      会出现的问题 切换 维护 开发

有没有一种新的技术诞生，让我们不再关注具体MQ的细节 我们只需要用一种适配的方式，自动的给我在各种MQ内切换

Stream就诞生了，通过操作stream可以操纵底层不同的mq

> 是什么？

![image-20220328104024715](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281040100.png)

`一句话:` 屏蔽底层消息中间件的差异，降低切换版本，统一消息的编程模型

> 官网文档：[Spring Cloud Stream](https://spring.io/projects/spring-cloud-stream)

![image-20220328110701558](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281107777.png)

![image-20220328110754197](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281107403.png)



### Stream的设计思想

> 标准的MQ

![image-20220328111829679](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281118890.png)![image-20220328111854858](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281118001.png)



> 为什么用Stream

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281120515.png)

> Stream凭什么可以屏蔽底层差异

![image-20220328112033824](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281120502.png)

> Binder

![image-20220328112117725](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281121115.png)

![image-20220328112133229](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281121483.png)

==**INPUT对应消费者，OUTPUT对应生产者**==

![image-20220328112235977](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281122115.png)



### Stream编码常用注解简介

> Stream的标准流程套路：

![image-20220328145005425](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281450826.png)

![image-20220328145039157](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281450321.png)

==对一些名词的解释：==

![image-20220328145109090](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281451443.png)

> 编码API和常用注解：

![image-20220328145423629](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281454887.png)

### Stream消息驱动之生产者

新建模块：

```
cloud-stream-rabbitmq-provider8801
```

> pom

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-stream-rabbitmq-provider8801</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>


        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-eureka-server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>

        <dependency>
            <groupId>com.atguigu.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>



    </dependencies>

</project>


   
```

下面这个依赖如果用的是kafaka就改成kafaka就行了

```java
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
</dependency>
```

> yml

```
server:
  port: 8801

spring:
  application:
    name: cloud-stream-provider

  cloud:
    stream:
      binders: # 在此处配置要绑定的rabbitmq的服务信息；
        defaultRabbit: # 表示定义的名称，用于于binding整合
          type: rabbit # 消息组件类型
          environment: # 设置rabbitmq的相关的环境配置
            spring:
              rabbitmq:
                host: localhost
                port: 5672
                username: guest
                password: guest
      bindings: # 服务的整合处理
        output: # 这个名字是一个通道的名称
          destination: studyExchange # 表示要使用的Exchange名称定义
          content-type: application/json # 设置消息类型，本次为json，文本则设置“text/plain”
          binder: defaultRabbit  # 设置要绑定的消息服务的具体设置

eureka:
  client: # 客户端进行Eureka注册的配置
    service-url:
      defaultZone: http://localhost:7001/eureka
  instance:
    lease-renewal-interval-in-seconds: 2 # 设置心跳的时间间隔（默认是30秒）
    lease-expiration-duration-in-seconds: 5 # 如果现在超过了5秒的间隔（默认是90秒）
    instance-id: send-8801.com  # 在信息列表时显示主机名称
    prefer-ip-address: true     # 访问的路径变为IP地址
```

> 主启动类

```java
package com.atguigu.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class StreamMQMain8801 {
    public static void main(String[] args) {
        SpringApplication.run(StreamMQMain8801.class,args);
    }
}
```

> 写一个service接口

```
package com.atguigu.springcloud.service;

public interface IMessageProvider {
    public String send();
}
```

> 实现类

```java
package com.atguigu.springcloud.service.impI;


import com.atguigu.springcloud.service.IMessageProvider;
import org.springframework.cloud.stream.annotation.EnableBinding;
import org.springframework.cloud.stream.messaging.Source;
import org.springframework.integration.support.MessageBuilder;
import org.springframework.messaging.MessageChannel;

import javax.annotation.Resource;
import java.util.UUID;


@EnableBinding(Source.class) //定义消息的推送管道   
public class MessageProviderImpI implements IMessageProvider {
    @Resource
        private MessageChannel output;//消息发送管道

    @Override
    public String send()
    {
        String serial = UUID.randomUUID().toString();
      output.send(MessageBuilder.withPayload(serial).build());//往消息中间件发送信息
        System.out.println(serial);
      return null;
    }

}
```



> 测试  疯狂点击：[localhost:8801/sendMessage](http://localhost:8801/sendMessage) 停一会 再疯狂点击  访问localhost:15672就能看到下面的东西了

![image-20220328151544727](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281515319.png)

后台：

![image-20220328151754198](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281517522.png)



### Stream消息驱动之消费者

新建模块cloud-stream-rabbitmq-consumer8802

> pom

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-stream-rabbitmq-consumer8802</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>


        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-eureka-server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>

        <dependency>
            <groupId>com.atguigu.cloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>



    </dependencies>

</project>

   
```

> yml

```java
server:
  port: 8802

spring:
  application:
    name: cloud-stream-consumer
  cloud:
    stream:
      binders: # 在此处配置要绑定的rabbitmq的服务信息；
        defaultRabbit: # 表示定义的名称，用于于binding整合
          type: rabbit # 消息组件类型
          environment: # 设置rabbitmq的相关的环境配置
            spring:
              rabbitmq:
                host: localhost
                port: 5672
                username: guest
                password: guest
      bindings: # 服务的整合处理
        input: # 这个名字是一个通道的名称
          destination: studyExchange # 表示要使用的Exchange名称定义
          content-type: application/json # 设置消息类型，本次为json，文本则设置“text/plain”
          binder: defaultRabbit  # 设置要绑定的消息服务的具体设置


eureka:
  client: # 客户端进行Eureka注册的配置
    service-url:
      defaultZone: http://localhost:7001/eureka
  instance:
    lease-renewal-interval-in-seconds: 2 # 设置心跳的时间间隔（默认是30秒）
    lease-expiration-duration-in-seconds: 5 # 如果现在超过了5秒的间隔（默认是90秒）
    instance-id: receive-8802.com  # 在信息列表时显示主机名称
    prefer-ip-address: true     # 访问的路径变为IP地址
```

> 主启动类

```java
package springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class StreamMQMain8802 {
    public static void main(String[] args) {
        SpringApplication.run(StreamMQMain8802.class,args);
    }
}
```

> controller

```java
package springcloud.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.stream.annotation.EnableBinding;
import org.springframework.cloud.stream.annotation.StreamListener;
import org.springframework.cloud.stream.messaging.Sink;
import org.springframework.messaging.Message;
import org.springframework.stereotype.Component;


@Component
@EnableBinding(Sink.class)  //source是发送者 sink是接受者
public class ReceiveMessageListenerController {

    @Value("${server.port}")
    private String serverPort;

    @StreamListener(Sink.INPUT)
    public void input(Message<String> message) {  //发的是message 所以受到的也是message
        System.out.println("消费者1，-------" + message.getPayload() + "\t port:" + serverPort);
    }
}
```

![image-20220328153055028](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281530291.png)

==上面的studyExchange取决于我们配置的名字destination==

点击五次

[localhost:8801/sendMessage](http://localhost:8801/sendMessage)

查看控制台

8801

![image-20220328153208721](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281532882.png)

8802

![image-20220328153226571](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281532713.png)

点进去studyExchange

![image-20220328153400994](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281534450.png)



### Stream之消息重复消费

依照8802，clone出来一份8803

> 启动这五个微服务

![image-20220328154853542](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281548825.png)

> 访问两次

[localhost:8801/sendMessage](http://localhost:8801/sendMessage)

![image-20220328154957028](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281549298.png)

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281550444.png)

==**目前是8802/8803同时都收到了，存在重复消费问题**==

> 生产实际案例：

![image-20220328155113627](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281551858.png)

![image-20220328155139858](../AppData/Roaming/Typora/typora-user-images/image-20220328155139858.png)

==**原因是不在一个组所以会出现重复消费的问题**==   下面的TO：就是8803和8802对应的组

![image-20220328155316653](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281553949.png)

### Stream之group解决消息重复消费

==**上图To的两个流水号（组）是系统给8802和8803分配的，因为组不一样，所以会出现重复消费的问题**==

> 自定义组一个是kwqa一个是kwqb

![image-20220328160343783](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281603928.png)

![image-20220328160319925](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281603071.png)

==上面这样还是在同一个组==

那我们统一将group改为kwqa再访问两次这个地址：[localhost:8801/sendMessage](http://localhost:8801/sendMessage)

![image-20220328160544554](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281605724.png)

![image-20220328160558916](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281605199.png)

==**这样就避免了重复消费了**==



### Stream之消息持久化

> 通过上述，我们解决了重复消费的问题，再看看持久化

==**我们停掉8802,8803，并去掉8802的分组group:kwqa,8803的分组信息没有去掉，接着向localhost:8801/sendMessage发送四条消息，再启动8802和8803。发现8802后台没有打印出消息，8803打印出了MQ上的消息。**==



## Sleuth（分布式请求链路跟踪）

### 概述

> 为什么会出现这个技术？解决了什么问题？

![image-20220328162512008](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281625316.png)

==**Spring Cloud Sleuth提供了一套完整的服务跟踪的解决方案，并且兼容支持了zipkin**==

> 解决：

![image-20220328162831394](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281628698.png)



### Sleuth之zipkin搭建安装

自行百度安装，安装完后会有一个jar

![image-20220328163553099](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281635258.png)

> 启动zipkin，出现下面页面表示启动成功

![image-20220328163646117](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281636493.png)

> 访问会看到这么一个页面

![image-20220328163809206](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281638418.png)

> 完整的调用链路：

![image-20220328163841492](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281638715.png)

> 对上图的再解释

![image-20220328163916156](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281639518.png)

> 名词解释：

==Trace==：类似于树结构的Span集合，表示一条调用链路，存在唯一标识

==span==：表示调用链路来源，通俗的理解span就是一次请求信息



### Sleuth链路监控展现

在我们的cloud-provider-payment8001中 添加依赖

```java
<!--包含了sleuth+zipkin-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
```

> yml

```java
spring:
  application:
    name: cloud-payment-service
    zipkin:
      base-url: http://localhost:9411
      sleuth:
        sampler:   #采样率 值位于0-1之间 1的话监控的比较细致
          probability: 1
```

> 业务类controller

```java
@GetMapping("/payment/zipkin")
public String paymentZipkin()
{
    return "hi ,i'am paymentzipkin server fall back，welcome to atguigu，O(∩_∩)O哈哈~";
}
```



==改造cloud-consumer-order80==

> 添加pom

```java
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
```

> yml

```
spring:
  application:
    name: cloud-order-service
    zipkin:
      base-url: http://localhost:9411
      sleuth:
        sampler:
          probability: 1
```

> controller

```java
// ====================> zipkin+sleuth
@GetMapping("/consumer/payment/zipkin")
public String paymentZipkin()
{
    String result = restTemplate.getForObject("http://localhost:8001"+"/payment/zipkin/", String.class);
    return result;
}
```

> 启动三个服务来测试

![image-20220328170653845](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281706244.png)

> 打开浏览器访问：[localhost/consumer/payment/zipkin](http://localhost/consumer/payment/zipkin)

![image-20220328172346989](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281723410.png)

> 访问查看：http://localhost:9411/zipkin

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281726836.png)

![image-20220328172755909](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281727326.png)

![image-20220328172715768](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281727961.png)



> 注意点：

==**因为支付模块是单体，不是集群，因此80的配置类不能添加@LoadBalanced这个注解 否则报错**==

![image-20220328173036249](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203281730451.png)



# Cloud Alibaba

> 为什么会出现SpringCloud Aliabab

==Spring Cloud Netflix项目进入维护模式==

[Spring Cloud Greenwich.RC1 available now](https://spring.io/blog/2018/12/12/spring-cloud-greenwich-rc1-available-now)

![image-20220329100906296](../AppData/Roaming/Typora/typora-user-images/image-20220329100906296.png)

> 说明

![image-20220329101050784](../AppData/Roaming/Typora/typora-user-images/image-20220329101050784.png)

![image-20220329101107648](../AppData/Roaming/Typora/typora-user-images/image-20220329101107648.png)

> 什么是维护模式？

![image-20220329101320485](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291013694.png)

> 进入维护模式意味着什么？

![image-20220329101405359](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291014564.png)

> SpringCloud alibaba带来了什么？

==官网地址：==[spring-cloud-alibaba/README-zh.md at 2.2.x · alibaba/spring-cloud-alibaba (github.com)](https://github.com/alibaba/spring-cloud-alibaba/blob/2.2.x/README-zh.md)

==是什么？==

![image-20220329102407318](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291024610.png)

==官网介绍：==

![image-20220329102603854](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291026132.png)

==能干嘛？==

![image-20220329102959822](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291030028.png)

==怎么玩？==

![image-20220329103120984](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291031231.png)

==最后一句话的意思是：根据所需要的功能添加alibaba的相关组件依赖（比如nacos）==

> 组件介绍：

![image-20220329104304378](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291043621.png)

![image-20220329104333762](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291043026.png)

> springcloud alibaba学习资料获取

官网：[Spring Cloud Alibaba](https://spring.io/projects/spring-cloud-alibaba)

源码地址：

中文版：[spring-cloud-alibaba/README-zh.md at 2021.x · alibaba/spring-cloud-alibaba (github.com)](https://github.com/alibaba/spring-cloud-alibaba/blob/2021.x/README-zh.md)  

英文版：[alibaba/spring-cloud-alibaba: Spring Cloud Alibaba provides a one-stop solution for application development for the distributed solutions of Alibaba middleware. (github.com)](https://github.com/alibaba/spring-cloud-alibaba)



## Nacos（服务注册与配置中心）

### 概述

> nacos简介：

官网文档：

[home (nacos.io)](https://nacos.io/zh-cn/)

[Spring Cloud Alibaba Reference Documentation (spring-cloud-alibaba-group.github.io)](https://spring-cloud-alibaba-group.github.io/github-pages/greenwich/spring-cloud-alibaba.html)   教你快速上手

==是什么？==

一个更易于构建`云原生应用`的`动态服务发现`，`配置管理`和`服务管理中心`

Nacos就是注册中心+配置中心的组合==>Nacos=Eureka+Config+Bus 

> 能干嘛

替代Eureka做服务注册中心

替代Config做服务配置中心

> 去哪下？

自行百度

> 各注册中心比较？

![image-20220329111540813](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291115027.png)

### Nacos的安装

百度，安装完后，启动

![image-20220329111218638](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291117207.png)访问：http://localhost:8848/nacos   出现下面这个页面表示安装成功！

![image-20220329111735442](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291117631.png)



### Nacos之服务提供者注册

> 新建模块：  cloudalibaba-provider-payment9001

父pom添加依赖

```java
<!--spring cloud 阿里巴巴-->
<dependency>
  <groupId>com.alibaba.cloud</groupId>
  <artifactId>spring-cloud-alibaba-dependencies</artifactId>
  <version>2.1.0.RELEASE</version>
  <type>pom</type>
  <scope>import</scope>
</dependency>
```

> 本pom

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloudalibaba-provider-payment9001</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
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
        </dependency><dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.62</version>
    </dependency>
    </dependencies>


</project>
```

> yml

```
server:
  port: 9001

spring:
  application:
    name: nacos-payment-provider
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #配置Nacos地址

management:
  endpoints:
    web:
      exposure:
        include: '*'
        
```

> 主启动类和业务类

```
package alibaba;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@EnableDiscoveryClient
@SpringBootApplication
public class PaymentMain9001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain9001.class,args);
    }
}
```

```
package alibaba.controller;


import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;


@RestController
public class PaymentController
{
    @Value("${server.port}")
    private String serverPort;

    @GetMapping(value = "/payment/nacos/{id}")
    public String getPayment(@PathVariable("id") Integer id)
    {
        return "nacos registry, serverPort: "+ serverPort+"\t id"+id;
    }
}
```

> 测试：启动9001 

![image-20220329130931415](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291309680.png)

![image-20220329131012140](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291310280.png)==这样子nacos服务注册中心+服务提供者9001都ok了==

下一章节演示nacos的负载均衡，参照9001新建9002，启动9001和9002，打开nacos控制台，点击详情

![image-20220329132247038](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291322171.png)

> 如果不想新建重复体力劳动，可以直接拷贝虚拟端口映射

![image-20220329131408836](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291314966.png)

![image-20220329131451103](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291314210.png)



### Nacos之服务消费者注册和负载

新建模块cloudalibaba-consumer-nacos-order83

> pom

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloudalibaba-consumer-nacos-order83</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <!--SpringCloud ailibaba nacos -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <dependency>
            <groupId>com.atgugui.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
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
        </dependency>
    </dependencies>


</project>

```

> 为什么nacos支持负载均衡？

![image-20220329133015981](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291330103.png)

> yml

```java
server:
  port: 83


spring:
  application:
    name: nacos-order-consumer
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848


service-url:
  nacos-user-service: http://nacos-payment-provider





```

> 主启动类

```java
package alibaba;


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;


@EnableDiscoveryClient
@SpringBootApplication
public class OrderNacosMain83
{
    public static void main(String[] args)
    {
        SpringApplication.run(OrderNacosMain83.class,args);
    }
}



```

> 模板配置类

```java
package alibaba.config;
import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;


@Configuration
public class ApplicationContextConfig
{
    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate()
    {
        return new RestTemplate();
    }
}
```

> controller

```java
package alibaba.controller;


import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;


@RestController
@Slf4j
public class OrderNacosController
{
    @Resource
    private RestTemplate restTemplate;

    @Value("${service-url.nacos-user-service}")
    private String serverURL;

    @GetMapping(value = "/consumer/payment/nacos/{id}")
    public String paymentInfo(@PathVariable("id") Long id)
    {
        return restTemplate.getForObject(serverURL+"/payment/nacos/"+id,String.class);
    }

}
```

> 启动9001，9002和83

![image-20220329134214649](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291342856.png)

![image-20220329134305249](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291343337.png)

![image-20220329134318813](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291343914.png)

==这样负载均衡的效果就测试出来啦==



### 各注册中心对比

> Nacos全景图展示：

![image-20220329143947412](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291439882.png)![image-20220329144009359](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291440659.png)

![image-20220329144642242](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291446573.png)

> Nacos和CAP

![image-20220329144144920](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291441413.png)

==Nacos支持AP和CP模式的切换==

![image-20220329144851846](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291448209.png)

### Nacos之服务配置中心

==替代Config做配置中心，相当于config+Bus==

> 新建模块：cloudalibaba-config-nacos-client3377

pom：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloudalibaba-config-nacos-client3377</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <!--nacos-config-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        </dependency>
        <!--nacos-discovery-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!--web + actuator-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--一般基础配置-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
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
        </dependency>
    </dependencies>



</project>
```

> 新建两个配置文件 一个是bootstrap.yml 一个是application.yml

![image-20220329151541745](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291515950.png)

> boostrap.yml

```
     
server:
  port: 3377

spring:
  application:
    name: nacos-config-client

  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #服务注册中心地址
      config:
        server-addr: localhost:8848 #配置中心地址
        file-extension: yaml #指定yaml格式的配置



        
```

> application.yml

```java
spring:
  profiles:
    active: dev        #开发环境
   # active: test        #测试环境
  #  active: info        #测试环境
```

可以将application.yml的内容添加到boostrap.yml中，同时删掉application.yml文件

> 主启动类

```java
package springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;


@EnableDiscoveryClient
@SpringBootApplication
public class NacosConfigClientMain3377
{
    public static void main(String[] args) {
        SpringApplication.run(NacosConfigClientMain3377.class, args);
    }
}
```

> 业务类 controller   @RefreshScope

```java
package springcloud.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.context.config.annotation.RefreshScope;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;


@RestController
@RefreshScope
public class ConfigClientController
{
    @Value("${config.info}")
    private String configInfo;

    @GetMapping("/config/info")
    public String getConfigInfo() {
        return configInfo;
    }
}
```

> 在Nacos中添加配置信息

![image-20220329153008578](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291530947.png)

![image-20220329153028045](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291530261.png)

![image-20220329153048735](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291530907.png)

> Nacos的匹配规则（来自官网）：

==Nacos中dataid的组成格式与SpringBoot配置文件中的匹配规则==

![image-20220329152815248](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291528492.png)

![image-20220329152832563](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291528712.png)

> 小结总说明：

![image-20220329153114224](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291531624.png)

> 启动3377

![image-20220329152643311](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291526508.png)

> 将center修改成center1

![image-20220329152734312](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291527648.png)

==没重启微服务，却出现上图效果，说明自带自动刷新功能==



### Nacos之命名空间分组和DataID三者关系

> Nacos作为配置中心-------分类配置   实现多环境多项目管理

![image-20220329194424468](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291944859.png)

> Nacos的图形化管理界面

==配置管理==

![image-20220329194602024](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291946254.png)

==命名空间==

![image-20220329194623982](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291946174.png)

> Namespace+Group+Data ID三者的关系，为什么这样设计？

![image-20220329194708257](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291947533.png)![image-20220329194730230](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203291947487.png)



### Nacos之DataID配置

指定`spring.profile.active`和配置文件的`DataID`来`使不同环境下读取不同的配置`

默认空间+默认分组+新建dev和test两个DataID

==新建dev配置DataID==

==新建test配置DataID==

通过`spring.profile.active`属性就能进行多环境下配置文件的读取

![image-20220329200040637](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292000836.png)![image-20220329200208354](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292002547.png)

![image-20220329200235802](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292002975.png)

![image-20220329200251704](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292002929.png)

> 测试：

![image-20220329200339914](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292003331.png)

==改成test，就输出test==



### Nacos之Group分组方案

> 新建两个组   通过组实现环境区分

![image-20220329200945206](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292009391.png)

![image-20220329201115014](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292011373.png)

![image-20220329201238314](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292012556.png)

> boostrap+application

![image-20220329201314035](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292013319.png)

> 在config下增加一条group的配置即可，可配置为DEV_GROUP或TEST_GROUP

```
server:
  port: 3377

spring:
  application:
    name: nacos-config-client

  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #服务注册中心地址
      config:
        server-addr: localhost:8848 #配置中心地址
        file-extension: yaml #指定yaml格式的配置
        group: TEST_GROUP
```

> 测试输出：

![image-20220329211653214](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292116502.png)

### Nacos之Namespace空间方案

> 新建dev/test的Namespace

![image-20220329213501594](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292135870.png)

> 回到服务列表中查看

![image-20220329213623939](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292136170.png)

> 按照域名配置填写

![image-20220329213650855](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292136020.png)

在dev命名空间下新建这三个服务列表

![image-20220329214122504](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292141723.png)

![image-20220329214231758](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292142016.png)

![image-20220329214345361](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292143665.png)

![image-20220329214504399](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292145766.png)

> 在bootstrap.yml中加入namespace 对应的值是命名空间ID

```java
server:
  port: 3377

spring:
  application:
    name: nacos-config-client

  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #服务注册中心地址
      config:
        server-addr: localhost:8848 #配置中心地址
        file-extension: yaml #指定yaml格式的配置
        group: TEST_GROUP
        namespace: e508bf3b-c96d-4dcb-9c13-cb7bbb2923dc
```

> application.yml

```
spring:
  profiles:
    active: dev        #开发环境
```

> 测试：

![image-20220329214745163](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203292147347.png)

### Nacos集群_架构说明

官方文档地址：[集群部署说明 (nacos.io)](https://nacos.io/zh-cn/docs/cluster-mode-quick-start.html)

> 官方架构图：

![image-20220330111833399](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301118597.png)

> 上图真实情况

![image-20220330111910355](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301119543.png)

![image-20220330111917025](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301119216.png)

> 说明

![image-20220330111938686](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301119982.png)==按上述说明，我们需要mysql数据库==



> 重点说明：

![image-20220330112041717](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301120993.png)

![image-20220330112056654](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301120893.png)



### Nacos持久化配置（Mysql）

Nacos默认自带的是嵌入式数据库derby   [nacos/pom.xml at development ·阿里巴巴/纳科斯 (github.com)](https://github.com/alibaba/nacos/blob/develop/config/pom.xml)



> 从derby数据库切换到mysql数据库的步骤：

找到nacos目录下的conf文件夹，找到sql脚本，在mysql中执行sql脚本。

紧接着在conf文件下找到application.properties，在末尾添加：

```java
spring.datasource.platform=mysql

db.num=1
db.url.0=jdbc:mysql://localhost:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=123123
```

重新启动nacos

==访问：[Nacos](http://localhost:8848/nacos)  发现我们原来的配置列表的东西消失了 原因就是我们更改了数据库。新数据库中没有存储以前的消息==

![image-20220330135159826](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301352141.png)

> 我们随便添加一个配置：

![image-20220330135438810](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301354034.png)

数据库中查看：

![image-20220330135516814](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301355144.png)



### Nacos之Linux版本安装

预计需要：1个Nginx+3个nacos注册中心+1个mysql  实际开发中，==Nginx和mysql一定也是构建集群，这里是学Nacos的集群，因此nginx和mysql的集群搭配就不用了。== 还有一点就是，nginx、mysql和nacos都是在一台服务器上的，实际开发中一般都是在不同的机器上面,这篇博客比较接近企业级部署：[听句劝! Nacos集群搭建可以看看这篇教程 - 云+社区 - 腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1805561)

> Nacos下载Linux版本

下载地址：[版本 2.0.4 （ 2022 年 1 月 18 日） ·阿里巴巴/纳科斯 (github.com)](https://github.com/alibaba/nacos/releases/tag/2.0.4)

通过宝塔，将压缩包传到服务器的opt目录下并且解压

![image-20220330141404951](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301414087.png)

> 将nacos拷贝一份到mynacos目录下

![image-20220330141647802](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301416995.png)

备份一个文件

![image-20220330141951794](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301419968.png)



### Nacos集群配置

==这里避下坑：nacos一定要选1.14版本的（和视频一样）或者去官网中找与你所选的spring cloud alibaba版本所适配的nacos版本==

和win下一样，复制sql脚本到mysql数据库，如图

![image-20220330142548972](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301425291.png)

> 在nacos的conf目录下备份一份application.propertes 命名为application.propertes.init   application.propertes末尾加上这些

```
spring.datasource.platform=mysql
 
db.num=1
db.url.0=jdbc:mysql://localhost:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=12312
```

> 拷贝一份cluster.conf

![image-20220330143748281](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301437653.png)

> 查看主机ip

![image-20220330144056287](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301440527.png)

==这里面不能写localhost，必须是linux命令hostname -i能够识别的ip==

![image-20220330144118726](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301441902.png)



> 编辑Nacos的启动脚本startup.sh，使他能够接受不同的启动端

![image-20220330150140574](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301501823.png)



> 编辑startup.sh：

![image-20220330150213945](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301502247.png)

![image-20220330150236906](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301502078.png)

==将startup.sh下图原来对应地方的数据，改为下图这个。==

![image-20220331082411942](../AppData/Roaming/Typora/typora-user-images/image-20220331082411942.png)

> 执行方式

![image-20220330150411330](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301504538.png)

> 查看正确启动nacos的数量

`ps -ef | grep nacos | grep -v grep |wc -l`   显示3

> 修改nginx的nginx.conf，如下图  然后启动nginx

![image-20220330154130239](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301541551.png)

> 9002修改nacos的地址   将9002注册进nacos集群

![image-20220330160309230](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301603535.png)

![image-20220330160341394](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301603597.png)

> 新建一条数据

![image-20220330160610495](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301606752.png)

> 查看数据库

![image-20220330235431863](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203302354075.png)



> 高可用小总结

![image-20220330160403450](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203301604657.png)



## Sentinel

### 概述：

> 官网地址：

英文：[alibaba/Sentinel: A powerful flow control component enabling reliability, resilience and monitoring for microservices. (面向云原生微服务的高可用流控防护组件) (github.com)](https://github.com/alibaba/Sentinel)

中文： [介绍 · alibaba/Sentinel Wiki (github.com)](https://github.com/alibaba/Sentinel/wiki/介绍)

> 是什么？   一句话解释就是：Hystrix

![image-20220331135203224](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311352782.png)

![image-20220331135333468](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311353642.png)

> 能干嘛？

![image-20220331135512055](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311355304.png)

> Sentinel组件由两部分组成：后台，前台8080

![image-20220331141857179](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311418330.png)

> 去哪下？

百度，这里下载的是1.7的。下载完成是一个jar

![image-20220331141024396](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311410522.png)

运行jar，如果出现下面这个效果则表示安装和开启成功

![image-20220331141326598](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311413733.png)

> 访问：localhost:8080   账号密码均为：sentinel

![image-20220331141939687](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311419917.png)

### Sentinel初始化监控

新建模块：cloudablibaba-sentinel-service8401

> pom：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloudablibaba-sentinel-service8401</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <dependency>
            <groupId>com.atguigu.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
<!--sentienl持久化用的，这里暂时用不到-->
        <dependency>
            <groupId>com.alibaba.csp</groupId>
            <artifactId>sentinel-datasource-nacos</artifactId>
        </dependency>

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>4.6.3</version>
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
        </dependency>

    </dependencies>

</project>
```

> yml：   ==这里的8719就是一个新的监控服务要使用的端口号==

```java
server:
  port: 8401

spring:
  application:
    name: cloudalibaba-sentinel-service
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
    sentinel:
      transport:
        dashboard: localhost:8080
        port: 8719  #默认8719，假如被占用了会自动从8719开始依次+1扫描。直至找到未被占用的端口

management:
  endpoints:
    web:
      exposure:
        include: '*'
```

> 主启动类

```java
package alibaba;



import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;


@EnableDiscoveryClient
@SpringBootApplication
public class MainApp8401
{
    public static void main(String[] args) {
        SpringApplication.run(MainApp8401.class, args);
    }
}
```

> 业务类：

```java
package alibaba.controller;
import com.alibaba.csp.sentinel.annotation.SentinelResource;
import com.alibaba.csp.sentinel.slots.block.BlockException;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;


@RestController
@Slf4j
public class FlowLimitController
{
    @GetMapping("/testA")
    public String testA() {
        //暂停毫秒


        return "------testA";
    }

    @GetMapping("/testB")
    public String testB() {
        return "------testB";
    }

}
```

> 启动8401 启动sentinel  访问localhost:8080

![image-20220331143330122](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311433530.png)

发现空空如也，这是因为Sentiel采用懒加载的模式，我们执行一次就能看到啦。

![image-20220331143443002](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311434166.png)

![image-20220331143656262](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311436482.png)

==**结论：sentinel8080正在监控微服务8401**==



### Sentinel流控规则简介

![image-20220331144032267](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311440473.png)

==**进一步解释说明：**==

![image-20220331144059597](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311440940.png)

![image-20220331144110899](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311441098.png)



### Sentinel流控-QPS直接失败

==**QPS:每一秒的请求数**==

直接快速失败是系统默认的

> 配置及说明：

![image-20220331144928599](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311449883.png)

> 测试：多次快速点击：[localhost:8401/testA](http://localhost:8401/testA)

![image-20220331145032333](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311450487.png)

==直接调用默认报错信息，技术方面OK，but，是否应该有我们自己的后续处理？类似一个fallback的兜底方法，打印我们想要的信息在网页上==



### Sentinel流控-线程数直接失败

> 修改8401的/testA方法

![image-20220331151431029](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311514244.png)

![image-20220331151306742](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311513132.png)

==**这个和QPS直接失败的不同点就是，QPS是1s超过一个就把请求挡在外面。而线程数直接失败则是都放进来，如果设定是一个线程。那么阻塞的时候，其他请求进来就会请求失败。**==

![image-20220331152456236](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311524396.png)



### Sentinel流控-关联

> 是什么？

当关联的资源达到阈值时，就限流自己。当与A关联的资源B达到阈值后，就限流自己，B惹事，A挂了

==**实际应用场景**==

订单模块达到阈值后，限制支付模块。

> 配置：

![image-20220331153556691](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311535954.png)

> Postman模拟并发密集访问testB

![image-20220331155311532](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311553731.png)

![image-20220331155112949](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311551158.png)

### Sentinel流控-链路

> 多个请求调用同一个微服务，达到阈值上限，则限流资源名字对应的接口

![image-20220331160600437](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311606631.png)

> 测试：访问B在访问A

![image-20220331160814849](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311608041.png)

### Sentinel流控-预热

> 应用场景：

![image-20220331165033871](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311650032.png)

> 预热：

![image-20220331164021314](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311640480.png)

![image-20220331164030653](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311640813.png)

==**公司：阈值除以coldFactor（默认值为3），经过预热时长后才会达到阈值**==

==**默认coldFactor为3，即请求QPS从threshold/3开始，经预热时长逐渐升至设定的QPS阈值**==

> 实操：

![image-20220331164310799](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311643049.png)

`刚开始疯狂点击`

![image-20220331163925326](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311639643.png)

`后面疯狂点击也能正常访问`

### Sentinel流控-排队等待

==匀速排队，阈值必须设置成QPS，否则无效，根本没有匀速排队的选项==

> 官网：

![image-20220331165145883](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311651232.png)

==阈值QPS为2时，每隔500ms才允许通过下一个请求，QPS为1的时候，每隔1s才允许通过下一个请求==

![image-20220331165258700](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311652960.png)

> 实操：  截图截错了 下图所有testA都改成testB

![image-20220331165120913](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311651105.png)

> 测试： 每隔一秒允许进来一个   请求等待的超时时间如果超过了2000ms就返回错误信息

> 修改B方法：

```
@GetMapping("/testB")
public String testB() {
    log.info(Thread.currentThread().getName()+"\t"+"testB");
    return "------testB";
}
```

==用postman来每隔100ms发一个请求，一共发十个==

![image-20220331170150565](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311701795.png)

> 效果 每隔一秒输出一个

![image-20220331170432085](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311704237.png)



### Sentinel降级简介（熔断）

==**时间窗口指的是降级时间间隔，单位是秒**==

> 基本介绍：

![image-20220331171558954](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311715397.png)

==**注意Sentinel的断路器是没有半开状态的**==

==半开的状态系统自动去检测是否请求异常，没有异常就关闭断路器恢复使用，有异常则继续打开断路器不可用，具体可以参考Hystrix==

![image-20220331171823582](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311718747.png)



### Sentinel降级-RT

> RT是什么？

![image-20220331193548166](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311935415.png)

![image-20220331193556968](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311935292.png)

> 测试：

`在8401的controller加入`

    @GetMapping("/testD")
    public String testD()
    {
        try { TimeUnit.SECONDS.sleep(1); } catch (InterruptedException e) { e.printStackTrace(); }
        log.info("testD 测试RT");
    
        return "------testD";
    }

 `配置`

![image-20220331193715344](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311937599.png)

![image-20220331195749014](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311957190.png)

> 配置JMeter 

![image-20220331195457276](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311954679.png)

![image-20220331195519352](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311955665.png)

> 访问输出：

![image-20220331195440394](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203311954802.png)



### Sentinel降级-异常比例

> 是什么？

![image-20220331200727524](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312007724.png)

![image-20220331200747271](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312007467.png)



> 测试

`修改8401的controller`

```
@GetMapping("/testD")
public String testD()
{
    try { TimeUnit.SECONDS.sleep(1); } catch (InterruptedException e) { e.printStackTrace(); }
    int i = 10 / 0;

    return "------testD";
}
```

`sentinel配置`

![image-20220331200857413](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312008603.png)

`Jmeter`

![image-20220331201422342](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312014573.png)

启动压力测试然后浏览器访问testD

![image-20220331201256064](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312012289.png)

`压力测试结束后`

![image-20220331201350633](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312013877.png)

> 结论

![image-20220331200954598](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312009840.png)

### Sentinel降级-异常数

> 是什么？

![image-20220331202101939](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312021101.png)

==对上面的小补充，只要异常数在1分钟内（尽管是1s内）达到了阈值，直接熔断，如果时间窗口期设置成10s，那么就有可能下一次访问，因为又是异常，加上前面的时间也不到1分钟，又进入熔断状态==

![image-20220331202118250](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312021457.png)

==**异常数是按分钟统计的**==

> 代码：

```
@GetMapping("/testE")
public String testE()
{
    log.info("testE 测试异常数");
    int age = 10/0;
    return "------testE 测试异常数";
}
```

> 配置

![image-20220331202237808](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312022018.png)

> 测试

![image-20220331202459324](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312024471.png)



### Sentinel热点key限流

> 基本介绍：

![image-20220331224725392](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312247666.png)

官网： [热点参数限流 · alibaba/Sentinel Wiki (github.com)](https://github.com/alibaba/Sentinel/wiki/热点参数限流)

> 承上启下复习

![image-20220331225021536](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312250667.png)

> 8401里面添加controller

```java
@GetMapping("/testHotKey")
@SentinelResource(value = "testHotKey",blockHandler = "deal_testHotKey")
public String testHotKey(@RequestParam(value = "p1",required = false) String p1,
                         @RequestParam(value = "p2",required = false) String p2) {
    int age = 10/0;
    return "------testHotKey";
}

//兜底方法
public String deal_testHotKey (String p1, String p2, BlockException exception){
    return "------deal_testHotKey,o(╥﹏╥)o";
}
```

> 配置规则：

![image-20220331225439560](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312254707.png)

![image-20220331225507323](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312255423.png)

==**意思是：方法testHostKey里面第一个参数只要QPS超过每秒一次，马上降级处理，走我们自定义的降级方法**==

> 输出：

![image-20220331231001617](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312310719.png)

![image-20220331225616419](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312256519.png)

==因为多次携带p1，所以走兜底方法==



不带p1访问，一直点击始终是这个页面

![image-20220331225748232](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312257324.png)



如果把`blockHandler = "deal_testHotKey"`去掉。多次访问后（==**重启后需要重新设置热点规则之前设置的会被删掉**==）

![image-20220331231150178](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312311312.png)

> 修改代码，加个运行时异常

![image-20220331232049380](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312320618.png)

启动：第一次

![image-20220331232208238](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312322508.png)

疯狂点击：

![image-20220331232237204](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312322390.png)

### Sentinel热点key限流+参数例外项

> 问题描述：

==**上述案例演示了第一个参数p1，当QPS超过1s1次点击后马上被限流。但是我们想要实现某些特殊情况。我们期望p1参数当它是某个特殊值，他的限流值和平时不一样，例如当p1的值等于5时，他的阈值可以达到200**==

> controller

```
@GetMapping("/testHotKey")
@SentinelResource(value = "testHotKey",blockHandler = "deal_testHotKey")
public String testHotKey(@RequestParam(value = "p1",required = false) String p1,
                         @RequestParam(value = "p2",required = false) String p2) {
    
    return "------testHotKey";
}

//兜底方法
public String deal_testHotKey (String p1, String p2, BlockException exception){
    return "------deal_testHotKey,o(╥﹏╥)o";
}
```

> 配置。多加个参数例外项

![image-20220331233020654](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312330042.png)

`ps:参数类型只能是以下这些类型`

![image-20220331232828217](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312328466.png)

> 测试：

多次点击：

![image-20220331233328487](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312333680.png)

![image-20220331233342027](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312333209.png)

> 我们手动添加一个运行时异常看看

`没触发阈值时`

![image-20220331234734822](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312347066.png)

`触发阈值时：`

![image-20220331234807812](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312348998.png)

> 小总结：

![image-20220331233641182](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202203312336462.png)

### Sentinel系统规则

官网：[系统自适应限流 · alibaba/Sentinel Wiki (github.com)](https://github.com/alibaba/Sentinel/wiki/系统自适应限流)

> 各项配置参数说明

![image-20220401093720619](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204010937180.png)

> 配置规则：

![image-20220401093828170](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204010938395.png)

> 配置QPS

![image-20220401093858834](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204010938041.png)

> controller

```java
@GetMapping("/testA")
public String testA() {
    //暂停毫秒

    return "------testA";
}

@GetMapping("/testB")
public String testB() {
    log.info(Thread.currentThread().getName()+"\t"+"testB");
    return "------testB";
}
```

> 输出： 多次点击后

![image-20220401094023891](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204010940876.png)

![image-20220401094034496](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204010940756.png)



### SentinelResource配置

#### 按资源名称限流+后续处理：

==**资源名称指的是@SentinelResource的value值**==

> 8401加上我们封装的依赖

```java
<dependency>
    <groupId>com.atguigu.cloud</groupId>
    <artifactId>cloud-api-commons</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

> yml

```
server:
  port: 8401

spring:
  application:
    name: cloudalibaba-sentinel-service
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
    sentinel:
      transport:
        dashboard: localhost:8080
        port: 8719  #默认8719，假如被占用了会自动从8719开始依次+1扫描。直至找到未被占用的端口

management:
  endpoints:
    web:
      exposure:
        include: '*'
```

> pom：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloudablibaba-sentinel-service8401</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <dependency>
            <groupId>com.atguigu.cloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
<!--sentienl持久化用的-->
        <dependency>
            <groupId>com.alibaba.csp</groupId>
            <artifactId>sentinel-datasource-nacos</artifactId>
        </dependency>

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>4.6.3</version>
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
        </dependency>

    </dependencies>

</project>
```

> 业务类

```java
package alibaba.controller;

import com.alibaba.csp.sentinel.annotation.SentinelResource;
import com.alibaba.csp.sentinel.slots.block.BlockException;
import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.entities.Payment;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;


@RestController
public class RateLimitController {
    @GetMapping("/byResource")
    @SentinelResource(value = "byResource", blockHandler = "handleException")
    public CommonResult byResource() {
        return new CommonResult(200, "按资源名称限流测试OK", new Payment(2020L, "serial001"));
    }

    public CommonResult handleException(BlockException exception) {
        return new CommonResult(444, exception.getClass().getCanonicalName() + "\t 服务不可用");
    }
  
 
}
```

> 主启动类

```java
package alibaba;



import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;


@EnableDiscoveryClient
@SpringBootApplication
public class MainApp8401
{
    public static void main(String[] args) {
        SpringApplication.run(MainApp8401.class, args);
    }
}
```

> 配置流控规则

![image-20220401095800135](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204010958432.png)



> 启动8401

==刚开始：==

![image-20220401100158321](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011001485.png)

==多次快速点击之后：表示1s内查询次数大于1，就跑到我们自定义处限流==

![image-20220401100132811](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011001013.png)

==此时关闭微服务看看，再看看sentinel控制台，发现流控规则消失了，发现流控规则是个临时结点==

![image-20220401100433248](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011004451.png)



#### 按Url地址限流+后续处理

> 业务类加个方法：

```java
@GetMapping("/rateLimit/byUrl")
@SentinelResource(value = "byUrl")
public CommonResult byUrl()
{
    return new CommonResult(200,"按url限流测试OK",new Payment(2020L,"serial002"));
}
```

> 配置

![image-20220401100819904](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011008125.png)

> 测试

快速点击后：

![image-20220401100845050](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011008191.png)

==说明会返回Sentinel自带的限流处理结果==

> 修改 controller

```java
@GetMapping("/rateLimit/byUrl")
@SentinelResource(value = "byUrl",blockHandler = "handleException")
public CommonResult byUrl()
{
    return new CommonResult(200,"按url限流测试OK",new Payment(2020L,"serial002"));
}
```

==启动后：快速点击，依旧不会走我们自定义的限流方法，而是返回Sentinel自带的限流处理结果==

![image-20220401101216243](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011012542.png)



#### 解决上面兜底方法面临的问题

> 上面兜底方法面临的异常：

![image-20220401101823120](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011018429.png)

> 客户自定义限流逻辑

==创建customerBlockHandler类用于自定义限流处理逻辑==

```java
package alibaba.myHander;

import com.alibaba.csp.sentinel.slots.block.BlockException;
import com.atguigu.springcloud.entities.CommonResult;

public class CustomerBlockHandler {

    public static CommonResult handleException1(BlockException exception) {
        return new CommonResult(4444, "按客户自定义....exception1");

    }
    public static CommonResult handleException2(BlockException exception) {
        return new CommonResult(4444, "按客户自定义....exception2");

    }
}
```

> controller新加代码：

```java
@GetMapping("/rateLimit/customerBlockHandler")
@SentinelResource(value = "customerBlockHandler",
        blockHandlerClass = CustomerBlockHandler.class,
        blockHandler = "handlerException2")
public CommonResult customerBlockHandler()
{
    return new CommonResult(200,"按客戶自定义",new Payment(2020L,"serial003"));
}
```

> 启动微服务，并且配置Sentinel流控规则   ==**细节点：这里的资源名只能写@SentinelResource里value的值，填uri不生效，还是会返回Sentinel自带的**==

![image-20220401102928589](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011029918.png)

> 测试：

![image-20220401102854080](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011028230.png)

> 进一步说明

![image-20220401102523715](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011025019.png)



#### 更多注解属性说明（==不生效的时候来看看，大概是违反了约定==）

![image-20220401103926260](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011039464.png)

![image-20220401103947709](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011039913.png)



### Sentinel服务熔断

==sentinel整合ribbon+openFeign+fallback==

#### 环境搭建（sentinel整合ribbon）

> 启动nacos和sentinel         

`新建模块cloudalibaba-provider-payment9003/9004`

> 9003的yml

```java
server:
  port: 9003

spring:
  application:
    name: nacos-payment-provider
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #配置Nacos地址

management:
  endpoints:
    web:
      exposure:
        include: '*'
```

> 9003的pom

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloudalibaba-provider-payment9003</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <!--SpringCloud ailibaba nacos -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <dependency>
            <groupId>com.atgugui.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <!-- SpringBoot整合Web组件 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--日常通用jar包配置-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
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
        </dependency>
    </dependencies>
</project>
```

> 主启动类

```
package alibaba;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;


@SpringBootApplication
@EnableDiscoveryClient
public class PaymentMain9003
{
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain9003.class, args);
    }
}
```

> controller

```java
package alibaba.controller;
import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.entities.Payment;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import java.util.HashMap;


@RestController
public class PaymentController
{
    @Value("${server.port}")
    private String serverPort;

    public static HashMap<Long, Payment> hashMap = new HashMap<>();
    static{
        hashMap.put(1L,new Payment(1L,"28a8c1e3bc2742d8848569891fb42181"));
        hashMap.put(2L,new Payment(2L,"bba8c1e3bc2742d8848569891ac32182"));
        hashMap.put(3L,new Payment(3L,"6ua8c1e3bc2742d8848569891xt92183"));
    }

    @GetMapping(value = "/paymentSQL/{id}")
    public CommonResult<Payment> paymentSQL(@PathVariable("id") Long id){
        Payment payment = hashMap.get(id);
        CommonResult<Payment> result = new CommonResult(200,"from mysql,serverPort:  "+serverPort,payment);
        return result;
    }



}
```

==9004除了端口和9003不一样其他都一样==

> 测试是否搭建成功：

![image-20220401110435732](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011104078.png)



`新建消费者cloudalibaba-consumer-nacos-order84`

> pom：

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloudalibaba-consumer-nacos-order84</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>


        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>
        <dependency>
            <groupId>com.atgugui.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
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
        </dependency>
    </dependencies>
</project>
```

> yml

```java
server:
  port: 84


spring:
  application:
    name: nacos-order-consumer
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
    sentinel:
      transport:
        dashboard: localhost:8080
        port: 8719

service-url:
  nacos-user-service: http://nacos-payment-provider

#对Feign的支持
feign:
  sentinel:
    enabled: true
```

> 主启动类：

```java
package alibaba;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.openfeign.EnableFeignClients;


@EnableDiscoveryClient
@SpringBootApplication
@EnableFeignClients
public class OrderNacosMain84
{
    public static void main(String[] args) {
        SpringApplication.run(OrderNacosMain84.class, args);
    }
}
```

> 配置类

```java
package alibaba.config;

import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;


@Configuration
public class ApplicationContextConfig
{
    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate()
    {
        return new RestTemplate();
    }
}
```

> controller

```java
package com.atguigu.springcloud.alibaba.controller;

import com.alibaba.csp.sentinel.annotation.SentinelResource;
import com.alibaba.csp.sentinel.slots.block.BlockException;


import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.entities.Payment;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;


@RestController
@Slf4j
public class CircleBreakerController {

    public static final String SERVICE_URL = "http://nacos-payment-provider";

    @Resource
    private RestTemplate restTemplate;



    @RequestMapping("/consumer/fallback/{id}")
    @SentinelResource(value = "fallback") //没有配置
    //@SentinelResource(value = "fallback",fallback = "handlerFallback") //fallback只负责业务异常
    //@SentinelResource(value = "fallback",blockHandler = "blockHandler") //blockHandler只负责sentinel控制台配置违规
  
    public CommonResult<Payment> fallback(@PathVariable Long id) {
        CommonResult<Payment> result = restTemplate.getForObject(SERVICE_URL + "/paymentSQL/"+id, CommonResult.class,id);

        if (id == 4) {
            throw new IllegalArgumentException ("IllegalArgumentException,非法参数异常....");
        }else if (result.getData() == null) {
            throw new NullPointerException ("NullPointerException,该ID没有对应记录,空指针异常");
        }

        return result;
    }

    //fallback
    public CommonResult handlerFallback(@PathVariable  Long id,Throwable e) {
        Payment payment = new Payment(id,"null");
        return new CommonResult<>(444,"兜底异常handlerFallback,exception内容  "+e.getMessage(),payment);
    }

    //blockHandler
    public CommonResult blockHandler(@PathVariable  Long id,BlockException blockException) {
        Payment payment = new Payment(id,"null");
        return new CommonResult<>(445,"blockHandler-sentinel限流,无此流水: blockException  "+blockException.getMessage(),payment);
    }


}
 
 
```

> 测试84是否搭建成功   访问下面的uri 发现总是9003-9004直接轮询 OK!!!

![image-20220401113538876](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011135408.png)

==**热部署对java代码级生效及时，对@SentinelResource注解内属性，有时效果不好，因此修改了注解内属性我们就重启**==



#### Sentinel服务熔断无配置

![image-20220401114303219](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011143721.png)==**没有配置的话，就给客户error页面，十分不友好**==



#### Sentinel服务熔断只配置fallback

> 修改84的controller

```java
package alibaba.controller;
import alibaba.service.PaymentService;
import com.alibaba.csp.sentinel.annotation.SentinelResource;
import com.alibaba.csp.sentinel.slots.block.BlockException;

import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.entities.Payment;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;


@RestController
@Slf4j
public class CircleBreakerController {

    public static final String SERVICE_URL = "http://nacos-payment-provider";

    @Resource
    private RestTemplate restTemplate;



    @RequestMapping("/consumer/fallback/{id}")
    //@SentinelResource(value = "fallback") //没有配置
  @SentinelResource(value = "fallback",fallback = "handlerFallback") //fallback只负责业务异常
  //  @SentinelResource(value = "fallback",blockHandler = "blockHandler") //blockHandler只负责sentinel控制台配置违规
   /* @SentinelResource(value = "fallback",fallback = "handlerFallback",blockHandler = "blockHandler")*/
    public CommonResult<Payment> fallback(@PathVariable Long id) {
        CommonResult<Payment> result = restTemplate.getForObject(SERVICE_URL + "/paymentSQL/"+id, CommonResult.class,id);

        if (id == 4) {
            throw new IllegalArgumentException ("IllegalArgumentException,非法参数异常....");
        }else if (result.getData() == null) {
            throw new NullPointerException ("NullPointerException,该ID没有对应记录,空指针异常");
        }

        return result;
    }

    //fallback
    public CommonResult handlerFallback(@PathVariable  Long id,Throwable e) {
        Payment payment = new Payment(id,"null");
        return new CommonResult<>(444,"兜底异常handlerFallback,exception内容  "+e.getMessage(),payment);
    }

    //blockHandler
    public CommonResult blockHandler(@PathVariable  Long id,BlockException blockException) {
        Payment payment = new Payment(id,"null");
        return new CommonResult<>(445,"blockHandler-sentinel限流,无此流水: blockException  "+blockException.getMessage(),payment);
    }
    // OpenFeign
    @Resource
    private PaymentService paymentService;

    @GetMapping(value = "/consumer/paymentSQL/{id}")
    public CommonResult<Payment> paymentSQL(@PathVariable("id") Long id) {
        return paymentService.paymentSQL(id);
    }

}
```

> 测试

![image-20220401115435395](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011154594.png)



#### Sentinel服务熔断只配置blockHandler

> controller

```java
package alibaba.controller;
import alibaba.service.PaymentService;
import com.alibaba.csp.sentinel.annotation.SentinelResource;
import com.alibaba.csp.sentinel.slots.block.BlockException;

import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.entities.Payment;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;


@RestController
@Slf4j
public class CircleBreakerController {

    public static final String SERVICE_URL = "http://nacos-payment-provider";

    @Resource
    private RestTemplate restTemplate;



    @RequestMapping("/consumer/fallback/{id}")
    //@SentinelResource(value = "fallback") //没有配置
  //@SentinelResource(value = "fallback",fallback = "handlerFallback") //fallback只负责业务异常
    @SentinelResource(value = "fallback",blockHandler = "blockHandler") //blockHandler只负责sentinel控制台配置违规
   /* @SentinelResource(value = "fallback",fallback = "handlerFallback",blockHandler = "blockHandler")*/
    public CommonResult<Payment> fallback(@PathVariable Long id) {
        CommonResult<Payment> result = restTemplate.getForObject(SERVICE_URL + "/paymentSQL/"+id, CommonResult.class,id);

        if (id == 4) {
            throw new IllegalArgumentException ("IllegalArgumentException,非法参数异常....");
        }else if (result.getData() == null) {
            throw new NullPointerException ("NullPointerException,该ID没有对应记录,空指针异常");
        }

        return result;
    }

    //fallback
    public CommonResult handlerFallback(@PathVariable  Long id,Throwable e) {
        Payment payment = new Payment(id,"null");
        return new CommonResult<>(444,"兜底异常handlerFallback,exception内容  "+e.getMessage(),payment);
    }

    //blockHandler
    public CommonResult blockHandler(@PathVariable  Long id,BlockException blockException) {
        Payment payment = new Payment(id,"null");
        return new CommonResult<>(445,"blockHandler-sentinel限流,无此流水: blockException  "+blockException.getMessage(),payment);
    }
    // OpenFeign
    @Resource
    private PaymentService paymentService;

    @GetMapping(value = "/consumer/paymentSQL/{id}")
    public CommonResult<Payment> paymentSQL(@PathVariable("id") Long id) {
        return paymentService.paymentSQL(id);
    }


}
```

> 配置降级规则   配置流控规则也是可以的

![image-20220401143027404](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011430834.png)

> 输出：

==刚开始：==

![image-20220401143056138](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011430405.png)

==快速点击后：==

![image-20220401143125057](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011431300.png)



#### Sentinel服务熔断fallback和blockHandler都配置



> controller

```java
package alibaba.controller;
import alibaba.service.PaymentService;
import com.alibaba.csp.sentinel.annotation.SentinelResource;
import com.alibaba.csp.sentinel.slots.block.BlockException;

import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.entities.Payment;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;


@RestController
@Slf4j
public class CircleBreakerController {

    public static final String SERVICE_URL = "http://nacos-payment-provider";

    @Resource
    private RestTemplate restTemplate;



    @RequestMapping("/consumer/fallback/{id}")
    //@SentinelResource(value = "fallback") //没有配置
  //@SentinelResource(value = "fallback",fallback = "handlerFallback") //fallback只负责业务异常
   // @SentinelResource(value = "fallback",blockHandler = "blockHandler") //blockHandler只负责sentinel控制台配置违规
    @SentinelResource(value = "fallback",fallback = "handlerFallback",blockHandler = "blockHandler")
    public CommonResult<Payment> fallback(@PathVariable Long id) {
        CommonResult<Payment> result = restTemplate.getForObject(SERVICE_URL + "/paymentSQL/"+id, CommonResult.class,id);

        if (id == 4) {
            throw new IllegalArgumentException ("IllegalArgumentException,非法参数异常....");
        }else if (result.getData() == null) {
            throw new NullPointerException ("NullPointerException,该ID没有对应记录,空指针异常");
        }

        return result;
    }

    //fallback
    public CommonResult handlerFallback(@PathVariable  Long id,Throwable e) {
        Payment payment = new Payment(id,"null");
        return new CommonResult<>(444,"兜底异常handlerFallback,exception内容  "+e.getMessage(),payment);
    }

    //blockHandler
    public CommonResult blockHandler(@PathVariable  Long id,BlockException blockException) {
        Payment payment = new Payment(id,"null");
        return new CommonResult<>(445,"blockHandler-sentinel限流,无此流水: blockException  "+blockException.getMessage(),payment);
    }
    // OpenFeign
    @Resource
    private PaymentService paymentService;

    @GetMapping(value = "/consumer/paymentSQL/{id}")
    public CommonResult<Payment> paymentSQL(@PathVariable("id") Long id) {
        return paymentService.paymentSQL(id);
    }




}
```

> 配置流控规则  配置降级规则也OK的

![image-20220401143706807](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011437188.png)

> 测试：

`慢慢点：`

![image-20220401143751261](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011437507.png)`快快点：`

![image-20220401143819993](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011438281.png)

`慢慢点`

![image-20220401143932456](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011439794.png)`快快点`

![image-20220401143948514](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011439796.png)==**小总结：**==

![image-20220401144127880](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011441188.png)



#### Sentinel服务熔断 exceptionsTolgnore

==**exceptionsTolgnore可以将某个特定的异常排除在外，抛异常时不走fallback方法，正常报错在页面**==

> 图说：

![image-20220401152122737](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011521272.png)

> 修改84的controller

```java
package com.atguigu.springcloud.alibaba.controller;

import com.alibaba.csp.sentinel.annotation.SentinelResource;
import com.alibaba.csp.sentinel.slots.block.BlockException;


import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.entities.Payment;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;


@RestController
@Slf4j
public class CircleBreakerController {

    public static final String SERVICE_URL = "http://nacos-payment-provider";

    @Resource
    private RestTemplate restTemplate;



    @RequestMapping("/consumer/fallback/{id}")
    //@SentinelResource(value = "fallback") //没有配置
    //@SentinelResource(value = "fallback",fallback = "handlerFallback") //fallback只负责业务异常
    //@SentinelResource(value = "fallback",blockHandler = "blockHandler") //blockHandler只负责sentinel控制台配置违规
    @SentinelResource(value = "fallback",fallback = "handlerFallback",blockHandler = "blockHandler",
            exceptionsToIgnore = {IllegalArgumentException.class})
    public CommonResult<Payment> fallback(@PathVariable Long id) {
        CommonResult<Payment> result = restTemplate.getForObject(SERVICE_URL + "/paymentSQL/"+id, CommonResult.class,id);

        if (id == 4) {
            throw new IllegalArgumentException ("IllegalArgumentException,非法参数异常....");
        }else if (result.getData() == null) {
            throw new NullPointerException ("NullPointerException,该ID没有对应记录,空指针异常");
        }

        return result;
    }

    //fallback
    public CommonResult handlerFallback(@PathVariable  Long id,Throwable e) {
        Payment payment = new Payment(id,"null");
        return new CommonResult<>(444,"兜底异常handlerFallback,exception内容  "+e.getMessage(),payment);
    }

    //blockHandler
    public CommonResult blockHandler(@PathVariable  Long id,BlockException blockException) {
        Payment payment = new Payment(id,"null");
        return new CommonResult<>(445,"blockHandler-sentinel限流,无此流水: blockException  "+blockException.getMessage(),payment);
    }


}
```

> 启动访问：

![image-20220401152834953](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011528156.png)

![image-20220401152842883](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011528093.png)

==**当然也可以排错多个异常：**==

![image-20220401153326134](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011533376.png)

> 输出

![image-20220401153341003](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011533206.png)



#### Sentinel服务熔断OpenFeign

==前一节讲的是Sentinel+Rinbbon，这一节讲Sentinel+OpenFeign==

84模块新加依赖

```java
<dependencies>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-openfeign</artifactId>
    </dependency>
```

> yml新加

![image-20220401154958077](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011549319.png)

> service

```
package alibaba.service;



import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.entities.Payment;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;


@FeignClient(value = "nacos-payment-provider",fallback = PaymentFallbackService.class)
public interface PaymentService
{
    @GetMapping(value = "/paymentSQL/{id}")
    public CommonResult<Payment> paymentSQL(@PathVariable("id") Long id);
}
```

> 实现类

```java
package alibaba.service;


import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.entities.Payment;
import org.springframework.stereotype.Component;


@Component
public class PaymentFallbackService implements PaymentService
{
    @Override
    public CommonResult<Payment> paymentSQL(Long id)
    {
        return new CommonResult<>(44444,"服务降级返回,---PaymentFallbackService",new Payment(id,"errorSerial"));
    }
}
```

> controller 新加代码

```java
// OpenFeign
@Resource
private PaymentService paymentService;

@GetMapping(value = "/consumer/paymentSQL/{id}")
public CommonResult<Payment> paymentSQL(@PathVariable("id") Long id) {
    return paymentService.paymentSQL(id);
}
```

> 主启动类   添加@EnableFeignClients启动Feign功能

![image-20220401155513110](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011555357.png)

> 启动9003,9004,84    发现9003和9004轮询出现

![image-20220401155836640](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011558930.png)

> 此时故意关闭9003和9004微服务，看到84消费侧自动降级，不会被耗死

![image-20220401155953216](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011559420.png)



#### 熔断框架比较

![image-20220401183422601](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011834866.png)

![image-20220401183453276](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011834524.png)

#### Sentinel持久化规则

`一旦我们重启应用，sentinel规则（流控、降级.....）将消失，生产环境需要将配置规则进行持久化`

> 我们想要实现的效果：

将限流配置规则持久化进`Nacos`保存，只要刷新8401某个rest地址，sentinel控制台的流控规则就能看到，只要Nacos里面的配置不删除，针对8401上Sentinel上的流控规则持续有效



> 修改8401的pom

添加：

```
<!--sentienl持久化用的-->
        <dependency>
            <groupId>com.alibaba.csp</groupId>
            <artifactId>sentinel-datasource-nacos</artifactId>
        </dependency>
```

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloudablibaba-sentinel-service8401</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <dependency>
            <groupId>com.atguigu.cloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
<!--sentienl持久化用的-->
        <dependency>
            <groupId>com.alibaba.csp</groupId>
            <artifactId>sentinel-datasource-nacos</artifactId>
        </dependency>

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>4.6.3</version>
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
        </dependency>

    </dependencies>

</project>
```

> yml

==Nacos数据源配置：==

![image-20220401184043752](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011840060.png)

```
server:
  port: 8401

spring:
  application:
    name: cloudalibaba-sentinel-service
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #Nacos服务注册中心地址
    sentinel:
      transport:
        dashboard: localhost:8080 #配置Sentinel dashboard地址
        port: 8719
      datasource:
        ds1:
          nacos:
            server-addr: localhost:8848
            dataId: cloudalibaba-sentinel-service
            groupId: DEFAULT_GROUP
            data-type: json
            rule-type: flow

management:
  endpoints:
    web:
      exposure:
        include: '*'

```

> controller

```
package alibaba.controller;

import alibaba.myHander.CustomerBlockHandler;
import com.alibaba.csp.sentinel.annotation.SentinelResource;
import com.alibaba.csp.sentinel.slots.block.BlockException;
import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.entities.Payment;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;


@RestController
public class RateLimitController {

    @GetMapping("/rateLimit/byUrl")
    @SentinelResource(value = "byUrl")
    public CommonResult byUrl()
    {
        return new CommonResult(200,"按url限流测试OK",new Payment(2020L,"serial002"));
    }

}
```

> 添加Nacos业务规则配置

![image-20220401185614355](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011856715.png)

`内容解析：`

![image-20220401184617568](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011846844.png)

![image-20220401184629818](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011846166.png)

==注意点：此时还需要我们手动配置一条流控规则（和上面的业务规则要保持一致喔）==

![image-20220401190229801](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011902118.png)

> 测试

启动8401后访问uri(`必须要访问到违反流控规则`)，刷新sentinel就能发现业务规则有了（`不访问uri出不来`）

![image-20220401190312977](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011903464.png)

![image-20220401185814097](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011858393.png)

> 此时关闭我们的微服务8401 查看sentinel控制台

![image-20220401185847199](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011858444.png)

> 启动8401控制台访问uri（`必须要访问到违法流控规则`），又出现了

![image-20220401185946628](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204011859858.png)



## Seata

### 分布式事务问题的由来

> 分布式之前：

单机单库没这个问题

> 演变：

从1:1 ->1:N ->N:N

![image-20220402163820892](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204021638199.png)

> 分布式之后：

![image-20220402163355897](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204021633580.png)

![image-20220402163420826](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204021634096.png)

==**一句话：一次业务操作需要跨多个数据源或需要跨多个系统进行远程调用，就会产生分布式事务**==

### Seata术语

官网：[Seata](http://seata.io/zh-cn/)

> 是什么？

![image-20220402164224981](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204021642252.png)

> 一个典型的分布式事务过程

`1 ID+3 组件`

![image-20220402165314352](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204021653581.png)

![image-20220402165346179](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204021653423.png)

![image-20220402165429005](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204021654229.png)

![image-20220402191928351](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204021919643.png)

### Seata-Server安装

去哪下？  [Releases · seata/seata (github.com)](https://github.com/seata/seata/releases/)

本次学习下载的是seata-server-0.9版本的。下载安装完成压缩包后，解压到指定目录并修改conf目录下的file.conf配置文件

==已经成为共识了，改配置文件前先备份一个，先备份原始的file.conf文件==

主要修改`file.conf`：自定义事务组名称+事务日志存储模式为db+数据库连接信息

> service模块

![image-20220402193030112](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204021930313.png)

> store模块

![image-20220402193600941](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204021936203.png)

![image-20220402193649539](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204021936729.png)



> Mysql中的操作

新建一个库seata

在seata安装目录下找到conf目录，找到db_store.sql,导入seata中。



> 修改registry.conf文件

![image-20220402194343539](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204021943708.png)

> 最后一步就是启动seata

启动seata前先启动`Nacos`。启动好Nacos后就进入seata安装目录下的bin文件夹，双击`seata-server.bat`



### 业务数据库准备

> 分布式事务业务说明：

![image-20220402200106454](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204022001821.png)

==下订单--->扣库存--->减账户（余额）==

> 建数据库

seata_order:存储订单的数据库

seata_storage:存储库存的数据库

seata_account:存储账户信息的数据库

`建库SQL`

```
CREATE DATABASE seata_order；

CREATE DATABASE seata_storage；

CREATE DATABASE seata_account；
```

> 按照上述3库分别建对应业务表和回滚日志表

![image-20220402200634381](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204022006633.png)

```
CREATE TABLE t_order(
    `id` BIGINT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
    `user_id` BIGINT(11) DEFAULT NULL COMMENT '用户id',
    `product_id` BIGINT(11) DEFAULT NULL COMMENT '产品id',
    `count` INT(11) DEFAULT NULL COMMENT '数量',
    `money` DECIMAL(11,0) DEFAULT NULL COMMENT '金额',
    `status` INT(1) DEFAULT NULL COMMENT '订单状态：0：创建中; 1：已完结'
) ENGINE=INNODB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8;
 
SELECT * FROM t_order;

```

```
CREATE TABLE t_storage(
    `id` BIGINT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
    `product_id` BIGINT(11) DEFAULT NULL COMMENT '产品id',
   `'total` INT(11) DEFAULT NULL COMMENT '总库存',
    `used` INT(11) DEFAULT NULL COMMENT '已用库存',
    `residue` INT(11) DEFAULT NULL COMMENT '剩余库存'
) ENGINE=INNODB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;
 
INSERT INTO seata_storage.t_storage(`id`,`product_id`,`total`,`used`,`residue`)
VALUES('1','1','100','0','100');
 
 
SELECT * FROM t_storage;

```

```java
CREATE TABLE t_account(
    `id` BIGINT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY COMMENT 'id',
    `user_id` BIGINT(11) DEFAULT NULL COMMENT '用户id',
    `total` DECIMAL(10,0) DEFAULT NULL COMMENT '总额度',
    `used` DECIMAL(10,0) DEFAULT NULL COMMENT '已用余额',
    `residue` DECIMAL(10,0) DEFAULT '0' COMMENT '剩余可用额度'
) ENGINE=INNODB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;
 
INSERT INTO seata_account.t_account(`id`,`user_id`,`total`,`used`,`residue`) VALUES('1','1','1000','0','1000')
 
 
 
SELECT * FROM t_account;

```

==三个库中各自建回滚日志表==

在seata安装目录的conf目录下找到db_undo_log_sql

```
drop table `undo_log`;
CREATE TABLE `undo_log` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `branch_id` bigint(20) NOT NULL,
  `xid` varchar(100) NOT NULL,
  `context` varchar(128) NOT NULL,
  `rollback_info` longblob NOT NULL,
  `log_status` int(11) NOT NULL,
  `log_created` datetime NOT NULL,
  `log_modified` datetime NOT NULL,
  `ext` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `ux_undo_log` (`xid`,`branch_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

```

> 最终效果：

![image-20220402200936971](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204022009172.png)



### 订单/库存/账户业务微服务准备

==业务需求：下订单->减库存->扣余额->改（订单）状态==

#### 搭建Order-Mondule

模块名：seata-order-service2001

> pom

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>seata-order-service2001</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <!--nacos-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!--seata-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
            <exclusions>
                <exclusion>
                    <artifactId>seata-all</artifactId>
                    <groupId>io.seata</groupId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>io.seata</groupId>
            <artifactId>seata-all</artifactId>
            <version>0.9.0</version>
        </dependency>
        <!--feign-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <!--web-actuator-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--mysql-druid-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.37</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.0.0</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>
</project>
```

> yml

```java
server:
  port: 2001

spring:
  application:
    name: seata-order-service
  cloud:
    alibaba:
      seata:
        #自定义事务组名称需要与seata-server中的对应
        tx-service-group: fsp_tx_group
    nacos:
      discovery:
        server-addr: localhost:8848
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/seata_order
    username: root
    password: 123123

feign:
  hystrix:
    enabled: false

logging:
  level:
    io:
      seata: info

mybatis:
  mapperLocations: classpath:mapper/*.xml
```

> file.conf   直接从seata的conf目录下拷贝

```java
transport {
  # tcp udt unix-domain-socket
  type = "TCP"
  #NIO NATIVE
  server = "NIO"
  #enable heartbeat
  heartbeat = true
  #thread factory for netty
  thread-factory {
    boss-thread-prefix = "NettyBoss"
    worker-thread-prefix = "NettyServerNIOWorker"
    server-executor-thread-prefix = "NettyServerBizHandler"
    share-boss-worker = false
    client-selector-thread-prefix = "NettyClientSelector"
    client-selector-thread-size = 1
    client-worker-thread-prefix = "NettyClientWorkerThread"
    # netty boss thread size,will not be used for UDT
    boss-thread-size = 1
    #auto default pin or 8
    worker-thread-size = 8
  }
  shutdown {
    # when destroy server, wait seconds
    wait = 3
  }
  serialization = "seata"
  compressor = "none"
}

service {

  vgroup_mapping.fsp_tx_group = "default"

  default.grouplist = "127.0.0.1:8091"
  enableDegrade = false
  disable = false
  max.commit.retry.timeout = "-1"
  max.rollback.retry.timeout = "-1"
  disableGlobalTransaction = false
}


client {
  async.commit.buffer.limit = 10000
  lock {
    retry.internal = 10
    retry.times = 30
  }
  report.retry.count = 5
  tm.commit.retry.count = 1
  tm.rollback.retry.count = 1
}

## transaction log store
store {
  ## store mode: file、db
  mode = "db"

  ## file store
  file {
    dir = "sessionStore"

    # branch session size , if exceeded first try compress lockkey, still exceeded throws exceptions
    max-branch-session-size = 16384
    # globe session size , if exceeded throws exceptions
    max-global-session-size = 512
    # file buffer size , if exceeded allocate new buffer
    file-write-buffer-cache-size = 16384
    # when recover batch read size
    session.reload.read_size = 100
    # async, sync
    flush-disk-mode = async
  }

  ## database store
  db {
    ## the implement of javax.sql.DataSource, such as DruidDataSource(druid)/BasicDataSource(dbcp) etc.
    datasource = "dbcp"
    ## mysql/oracle/h2/oceanbase etc.
    db-type = "mysql"
    driver-class-name = "com.mysql.jdbc.Driver"
    url = "jdbc:mysql://127.0.0.1:3306/seata"
    user = "root"
    password = "123123"
    min-conn = 1
    max-conn = 3
    global.table = "global_table"
    branch.table = "branch_table"
    lock-table = "lock_table"
    query-limit = 100
  }
}
lock {
  ## the lock store mode: local、remote
  mode = "remote"

  local {
    ## store locks in user's database
  }

  remote {
    ## store locks in the seata's server
  }
}
recovery {
  #schedule committing retry period in milliseconds
  committing-retry-period = 1000
  #schedule asyn committing retry period in milliseconds
  asyn-committing-retry-period = 1000
  #schedule rollbacking retry period in milliseconds
  rollbacking-retry-period = 1000
  #schedule timeout retry period in milliseconds
  timeout-retry-period = 1000
}

transaction {
  undo.data.validation = true
  undo.log.serialization = "jackson"
  undo.log.save.days = 7
  #schedule delete expired undo_log in milliseconds
  undo.log.delete.period = 86400000
  undo.log.table = "undo_log"
}

## metrics settings
metrics {
  enabled = false
  registry-type = "compact"
  # multi exporters use comma divided
  exporter-list = "prometheus"
  exporter-prometheus-port = 9898
}

support {
  ## spring
  spring {
    # auto proxy the DataSource bean
    datasource.autoproxy = false
  }
}



```

> register.conf      直接从seata的conf目录下拷贝

```java
registry {
  # file 、nacos 、eureka、redis、zk、consul、etcd3、sofa
  type = "nacos"

  nacos {
    serverAddr = "localhost:8848"
    namespace = ""
    cluster = "default"
  }
  eureka {
    serviceUrl = "http://localhost:8761/eureka"
    application = "default"
    weight = "1"
  }
  redis {
    serverAddr = "localhost:6379"
    db = "0"
  }
  zk {
    cluster = "default"
    serverAddr = "127.0.0.1:2181"
    session.timeout = 6000
    connect.timeout = 2000
  }
  consul {
    cluster = "default"
    serverAddr = "127.0.0.1:8500"
  }
  etcd3 {
    cluster = "default"
    serverAddr = "http://localhost:2379"
  }
  sofa {
    serverAddr = "127.0.0.1:9603"
    application = "default"
    region = "DEFAULT_ZONE"
    datacenter = "DefaultDataCenter"
    cluster = "default"
    group = "SEATA_GROUP"
    addressWaitTime = "3000"
  }
  file {
    name = "file.conf"
  }
}

config {
  # file、nacos 、apollo、zk、consul、etcd3
  type = "file"

  nacos {
    serverAddr = "localhost"
    namespace = ""
  }
  consul {
    serverAddr = "127.0.0.1:8500"
  }
  apollo {
    app.id = "seata-server"
    apollo.meta = "http://192.168.1.204:8801"
  }
  zk {
    serverAddr = "127.0.0.1:2181"
    session.timeout = 6000
    connect.timeout = 2000
  }
  etcd3 {
    serverAddr = "http://localhost:2379"
  }
  file {
    name = "file.conf"
  }
}
```

![image-20220402202655846](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204022026279.png)

> domain

```java
package alibaba.domain;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;


@Data
@AllArgsConstructor
@NoArgsConstructor
public class CommonResult<T>
{
    private Integer code;
    private String  message;
    private T       data;

    public CommonResult(Integer code, String message)
    {
        this(code,message,null);
    }
}
```

```java
package alibaba.domain;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.math.BigDecimal;


@Data
@AllArgsConstructor
@NoArgsConstructor
public class Order
{
    private Long id;

    private Long userId;

    private Long productId;

    private Integer count;

    private BigDecimal money;

    private Integer status; //订单状态：0：创建中；1：已完结
}
```

> dao

```java
package alibaba.dao;


import alibaba.domain.Order;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;

@Mapper
public interface OrderDao
{
    //新建订单
    void create(Order order);

    //修改订单状态，从零改为1
    void update(@Param("userId") Long userId,@Param("status") Integer status);
}
```

> resource文件夹下新建mapper文件夹后添加xml

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >

<mapper namespace="alibaba.dao.OrderDao">

    <resultMap id="BaseResultMap" type="alibaba.domain.Order">
        <id column="id" property="id" jdbcType="BIGINT"/>
        <result column="user_id" property="userId" jdbcType="BIGINT"/>
        <result column="product_id" property="productId" jdbcType="BIGINT"/>
        <result column="count" property="count" jdbcType="INTEGER"/>
        <result column="money" property="money" jdbcType="DECIMAL"/>
        <result column="status" property="status" jdbcType="INTEGER"/>
    </resultMap>

    <insert id="create">
        insert into t_order (id,user_id,product_id,count,money,status)
        values (null,#{userId},#{productId},#{count},#{money},0);
    </insert>


    <update id="update">
        update t_order set status = 1
        where user_id=#{userId} and status = #{status};
    </update>

</mapper>
```

> service接口及其实现类

```java
package alibaba.service;


import alibaba.domain.Order;

public interface OrderService {

    public void create(Order order);
}
```

```java
package alibaba.service.impl;

import alibaba.dao.OrderDao;
import alibaba.domain.Order;
import alibaba.service.AccountService;
import alibaba.service.OrderService;
import alibaba.service.StorageService;
import io.seata.spring.annotation.GlobalTransactional;
import lombok.extern.slf4j.Slf4j;
import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;


@Service
@Slf4j
public class OrderServiceImpl implements OrderService
{
    @Resource
    private OrderDao orderDao;
    @Resource
    private StorageService storageService;
    @Resource
    private AccountService accountService;

    /**
     * 创建订单->调用库存服务扣减库存->调用账户服务扣减账户余额->修改订单状态
     */

    @Override

    public void create(Order order){
        log.info("----->开始新建订单");
        //新建订单
        orderDao.create(order);

        //扣减库存
        log.info("----->订单微服务开始调用库存，做扣减Count");
        storageService.decrease(order.getProductId(),order.getCount());
        log.info("----->订单微服务开始调用库存，做扣减end");

        //扣减账户
        log.info("----->订单微服务开始调用账户，做扣减Money");
        accountService.decrease(order.getUserId(),order.getMoney());
        log.info("----->订单微服务开始调用账户，做扣减end");


        //修改订单状态，从零到1代表已经完成
        log.info("----->修改订单状态开始");
        orderDao.update(order.getUserId(),0);
        log.info("----->修改订单状态结束");

        log.info("----->下订单结束了");

    }
}
 

```

```java
package alibaba.service;


import alibaba.domain.CommonResult;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

@FeignClient(value = "seata-storage-service")
public interface StorageService{
    @PostMapping(value = "/storage/decrease")
    CommonResult decrease(@RequestParam("productId") Long productId, @RequestParam("count") Integer count);
}
```

```
package alibaba.service;


import alibaba.domain.CommonResult;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

import java.math.BigDecimal;

@FeignClient(value = "seata-account-service")
public interface AccountService{
    @PostMapping(value = "/account/decrease")
    CommonResult decrease(@RequestParam("userId") Long userId, @RequestParam("money") BigDecimal money);
}
```

> 配置类：

```
package alibaba.config;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Configuration;


@Configuration
@MapperScan({"alibaba.dao"})
public class MyBatisConfig {

}
```

```
package alibaba.config;

import com.alibaba.druid.pool.DruidDataSource;
import io.seata.rm.datasource.DataSourceProxy;
import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.transaction.SpringManagedTransactionFactory;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.io.support.PathMatchingResourcePatternResolver;

import javax.sql.DataSource;


@Configuration
public class DataSourceProxyConfig {


    @Value("${mybatis.mapperLocations}")
    private String mapperLocations;


    @Bean
    @ConfigurationProperties(prefix = "spring.datasource")
    public DataSource druidDataSource(){
        return new DruidDataSource();
    }


    @Bean
    public DataSourceProxy dataSourceProxy(DataSource dataSource) {
        return new DataSourceProxy(dataSource);
    }


    @Bean
    public SqlSessionFactory sqlSessionFactoryBean(DataSourceProxy dataSourceProxy) throws Exception {
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSourceProxy);
        sqlSessionFactoryBean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources(mapperLocations));
        sqlSessionFactoryBean.setTransactionFactory(new SpringManagedTransactionFactory());
        return sqlSessionFactoryBean.getObject();
    }

}
```

> 主启动类

```java
package alibaba;



import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.openfeign.EnableFeignClients;

@EnableDiscoveryClient
@MapperScan({"alibaba.dao"})
@EnableFeignClients
@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)//取消数据源自动创建的配置
public class SeataOrderMainApp2001{

    public static void main(String[] args)
    {
        SpringApplication.run(SeataOrderMainApp2001.class, args);
    }
}


```

> 启动一下，看看有无搭建成功。发现启动不报错。OKK



#### 搭建Storage-Module

新建模块：seata-storaga-service2002

> pom：

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>seata-storaga-service2002</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <!--nacos-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!--seata-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
            <exclusions>
                <exclusion>
                    <artifactId>seata-all</artifactId>
                    <groupId>io.seata</groupId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>io.seata</groupId>
            <artifactId>seata-all</artifactId>
            <version>0.9.0</version>
        </dependency>
        <!--feign-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.0.0</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.37</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>
</project>
```

> yml

```java
server:
  port: 2002

spring:
  application:
    name: seata-storage-service
  cloud:
    alibaba:
      seata:
        tx-service-group: fsp_tx_group
    nacos:
      discovery:
        server-addr: localhost:8848
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/seata_storage
    username: root
    password: 123123

logging:
  level:
    io:
      seata: info

mybatis:
  mapperLocations: classpath:mapper/*.xml
```

> file.conf

```java
transport {
  # tcp udt unix-domain-socket
  type = "TCP"
  #NIO NATIVE
  server = "NIO"
  #enable heartbeat
  heartbeat = true
  #thread factory for netty
  thread-factory {
    boss-thread-prefix = "NettyBoss"
    worker-thread-prefix = "NettyServerNIOWorker"
    server-executor-thread-prefix = "NettyServerBizHandler"
    share-boss-worker = false
    client-selector-thread-prefix = "NettyClientSelector"
    client-selector-thread-size = 1
    client-worker-thread-prefix = "NettyClientWorkerThread"
    # netty boss thread size,will not be used for UDT
    boss-thread-size = 1
    #auto default pin or 8
    worker-thread-size = 8
  }
  shutdown {
    # when destroy server, wait seconds
    wait = 3
  }
  serialization = "seata"
  compressor = "none"
}

service {

  vgroup_mapping.fsp_tx_group = "default"

  default.grouplist = "127.0.0.1:8091"
  enableDegrade = false
  disable = false
  max.commit.retry.timeout = "-1"
  max.rollback.retry.timeout = "-1"
  disableGlobalTransaction = false
}


client {
  async.commit.buffer.limit = 10000
  lock {
    retry.internal = 10
    retry.times = 30
  }
  report.retry.count = 5
  tm.commit.retry.count = 1
  tm.rollback.retry.count = 1
}

## transaction log store
store {
  ## store mode: file、db
  mode = "db"

  ## file store
  file {
    dir = "sessionStore"

    # branch session size , if exceeded first try compress lockkey, still exceeded throws exceptions
    max-branch-session-size = 16384
    # globe session size , if exceeded throws exceptions
    max-global-session-size = 512
    # file buffer size , if exceeded allocate new buffer
    file-write-buffer-cache-size = 16384
    # when recover batch read size
    session.reload.read_size = 100
    # async, sync
    flush-disk-mode = async
  }

  ## database store
  db {
    ## the implement of javax.sql.DataSource, such as DruidDataSource(druid)/BasicDataSource(dbcp) etc.
    datasource = "dbcp"
    ## mysql/oracle/h2/oceanbase etc.
    db-type = "mysql"
    driver-class-name = "com.mysql.jdbc.Driver"
    url = "jdbc:mysql://127.0.0.1:3306/seata"
    user = "root"
    password = "123123"
    min-conn = 1
    max-conn = 3
    global.table = "global_table"
    branch.table = "branch_table"
    lock-table = "lock_table"
    query-limit = 100
  }
}
lock {
  ## the lock store mode: local、remote
  mode = "remote"

  local {
    ## store locks in user's database
  }

  remote {
    ## store locks in the seata's server
  }
}
recovery {
  #schedule committing retry period in milliseconds
  committing-retry-period = 1000
  #schedule asyn committing retry period in milliseconds
  asyn-committing-retry-period = 1000
  #schedule rollbacking retry period in milliseconds
  rollbacking-retry-period = 1000
  #schedule timeout retry period in milliseconds
  timeout-retry-period = 1000
}

transaction {
  undo.data.validation = true
  undo.log.serialization = "jackson"
  undo.log.save.days = 7
  #schedule delete expired undo_log in milliseconds
  undo.log.delete.period = 86400000
  undo.log.table = "undo_log"
}

## metrics settings
metrics {
  enabled = false
  registry-type = "compact"
  # multi exporters use comma divided
  exporter-list = "prometheus"
  exporter-prometheus-port = 9898
}

support {
  ## spring
  spring {
    # auto proxy the DataSource bean
    datasource.autoproxy = false
  }
}
```

> register.conf

```java
registry {
  # file 、nacos 、eureka、redis、zk、consul、etcd3、sofa
  type = "nacos"

  nacos {
    serverAddr = "localhost:8848"
    namespace = ""
    cluster = "default"
  }
  eureka {
    serviceUrl = "http://localhost:8761/eureka"
    application = "default"
    weight = "1"
  }
  redis {
    serverAddr = "localhost:6379"
    db = "0"
  }
  zk {
    cluster = "default"
    serverAddr = "127.0.0.1:2181"
    session.timeout = 6000
    connect.timeout = 2000
  }
  consul {
    cluster = "default"
    serverAddr = "127.0.0.1:8500"
  }
  etcd3 {
    cluster = "default"
    serverAddr = "http://localhost:2379"
  }
  sofa {
    serverAddr = "127.0.0.1:9603"
    application = "default"
    region = "DEFAULT_ZONE"
    datacenter = "DefaultDataCenter"
    cluster = "default"
    group = "SEATA_GROUP"
    addressWaitTime = "3000"
  }
  file {
    name = "file.conf"
  }
}

config {
  # file、nacos 、apollo、zk、consul、etcd3
  type = "file"

  nacos {
    serverAddr = "localhost"
    namespace = ""
  }
  consul {
    serverAddr = "127.0.0.1:8500"
  }
  apollo {
    app.id = "seata-server"
    apollo.meta = "http://192.168.1.204:8801"
  }
  zk {
    serverAddr = "127.0.0.1:2181"
    session.timeout = 6000
    connect.timeout = 2000
  }
  etcd3 {
    serverAddr = "http://localhost:2379"
  }
  file {
    name = "file.conf"
  }
}
```

> domain

```java
package alibaba.domain;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class CommonResult<T>
{
    private Integer code;
    private String  message;
    private T       data;

    public CommonResult(Integer code, String message)
    {
        this(code,message,null);
    }
}
```

```java
package alibaba.domain;

import lombok.Data;

@Data
public class Storage {

    private Long id;

    // 产品id
    private Long productId;

    //总库存
    private Integer total;


    //已用库存
    private Integer used;


    //剩余库存
    private Integer residue;
}
```

> dao

```java
package alibaba.dao;

import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;

@Mapper
public interface StorageDao {


    //扣减库存信息
    void decrease(@Param("productId") Long productId, @Param("count") Integer count);
}
```

> service

```java
package alibaba.service;


public interface StorageService {

     // 扣减库存
    void decrease(Long productId, Integer count);
}
```

> service实现类

```java
package alibaba.service.impl;


import alibaba.dao.StorageDao;
import alibaba.service.StorageService;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;


@Service
public class StorageServiceImpl implements StorageService {

    private static final Logger LOGGER = LoggerFactory.getLogger(StorageServiceImpl.class);

    @Resource
    private StorageDao storageDao;

     // 扣减库存
    @Override
    public void decrease(Long productId, Integer count) {
        LOGGER.info("------->storage-service中扣减库存开始");
        storageDao.decrease(productId,count);
        LOGGER.info("------->storage-service中扣减库存结束");
    }
}
```

> mapper

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >


<mapper namespace="alibaba.dao.StorageDao">

    <resultMap id="BaseResultMap" type="alibaba.domain.Storage">
        <id column="id" property="id" jdbcType="BIGINT"/>
        <result column="product_id" property="productId" jdbcType="BIGINT"/>
        <result column="total" property="total" jdbcType="INTEGER"/>
        <result column="used" property="used" jdbcType="INTEGER"/>
        <result column="residue" property="residue" jdbcType="INTEGER"/>
    </resultMap>

    <update id="decrease">
        UPDATE
            t_storage
        SET
            used = used + #{count},residue = residue - #{count}
        WHERE
            product_id = #{productId}
    </update>

</mapper>
```

> config

```java
package alibaba.config;

import com.alibaba.druid.pool.DruidDataSource;
import io.seata.rm.datasource.DataSourceProxy;
import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.transaction.SpringManagedTransactionFactory;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.io.support.PathMatchingResourcePatternResolver;

import javax.sql.DataSource;


@Configuration
public class DataSourceProxyConfig {

    @Value("${mybatis.mapperLocations}")
    private String mapperLocations;

    @Bean
    @ConfigurationProperties(prefix = "spring.datasource")
    public DataSource druidDataSource(){
        return new DruidDataSource();
    }

    @Bean
    public DataSourceProxy dataSourceProxy(DataSource dataSource) {
        return new DataSourceProxy(dataSource);
    }

    @Bean
    public SqlSessionFactory sqlSessionFactoryBean(DataSourceProxy dataSourceProxy) throws Exception {
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSourceProxy);
        sqlSessionFactoryBean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources(mapperLocations));
        sqlSessionFactoryBean.setTransactionFactory(new SpringManagedTransactionFactory());
        return sqlSessionFactoryBean.getObject();
    }

}
```

> controller

```
package alibaba.controller;



import alibaba.domain.CommonResult;
import alibaba.service.StorageService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class StorageController {

    @Autowired
    private StorageService storageService;


    //扣减库存
    @RequestMapping("/storage/decrease")
    public CommonResult decrease(Long productId, Integer count) {
        storageService.decrease(productId, count);
        return new CommonResult(200,"扣减库存成功！");
    }
}
```

```java
package alibaba.config;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Configuration;


@Configuration
@MapperScan({"alibaba.dao"})
public class MyBatisConfig {
}
```

> resource目录结构

![image-20220402213243406](../AppData/Roaming/Typora/typora-user-images/image-20220402213243406.png)

> 启动一下，不报错说明就搭建没问题

#### 搭建Account-Module

新建模块：seata-account-service2003

> pom

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>studyCloud</artifactId>
        <groupId>com.atguigu.cloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>seata-account-service2003</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <!--nacos-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!--seata-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
            <exclusions>
                <exclusion>
                    <artifactId>seata-all</artifactId>
                    <groupId>io.seata</groupId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>io.seata</groupId>
            <artifactId>seata-all</artifactId>
            <version>0.9.0</version>
        </dependency>
        <!--feign-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.0.0</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.37</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>

</project>
```

> yml

```java
server:
  port: 2003

spring:
  application:
    name: seata-account-service
  cloud:
    alibaba:
      seata:
        tx-service-group: fsp_tx_group
    nacos:
      discovery:
        server-addr: localhost:8848
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/seata_account
    username: root
    password: 123123

feign:
  hystrix:
    enabled: false

logging:
  level:
    io:
      seata: info

mybatis:
  mapperLocations: classpath:mapper/*.xml
```

> file.conf

```java
transport {
  # tcp udt unix-domain-socket
  type = "TCP"
  #NIO NATIVE
  server = "NIO"
  #enable heartbeat
  heartbeat = true
  #thread factory for netty
  thread-factory {
    boss-thread-prefix = "NettyBoss"
    worker-thread-prefix = "NettyServerNIOWorker"
    server-executor-thread-prefix = "NettyServerBizHandler"
    share-boss-worker = false
    client-selector-thread-prefix = "NettyClientSelector"
    client-selector-thread-size = 1
    client-worker-thread-prefix = "NettyClientWorkerThread"
    # netty boss thread size,will not be used for UDT
    boss-thread-size = 1
    #auto default pin or 8
    worker-thread-size = 8
  }
  shutdown {
    # when destroy server, wait seconds
    wait = 3
  }
  serialization = "seata"
  compressor = "none"
}

service {

  vgroup_mapping.fsp_tx_group = "default"

  default.grouplist = "127.0.0.1:8091"
  enableDegrade = false
  disable = false
  max.commit.retry.timeout = "-1"
  max.rollback.retry.timeout = "-1"
  disableGlobalTransaction = false
}


client {
  async.commit.buffer.limit = 10000
  lock {
    retry.internal = 10
    retry.times = 30
  }
  report.retry.count = 5
  tm.commit.retry.count = 1
  tm.rollback.retry.count = 1
}

## transaction log store
store {
  ## store mode: file、db
  mode = "db"

  ## file store
  file {
    dir = "sessionStore"

    # branch session size , if exceeded first try compress lockkey, still exceeded throws exceptions
    max-branch-session-size = 16384
    # globe session size , if exceeded throws exceptions
    max-global-session-size = 512
    # file buffer size , if exceeded allocate new buffer
    file-write-buffer-cache-size = 16384
    # when recover batch read size
    session.reload.read_size = 100
    # async, sync
    flush-disk-mode = async
  }

  ## database store
  db {
    ## the implement of javax.sql.DataSource, such as DruidDataSource(druid)/BasicDataSource(dbcp) etc.
    datasource = "dbcp"
    ## mysql/oracle/h2/oceanbase etc.
    db-type = "mysql"
    driver-class-name = "com.mysql.jdbc.Driver"
    url = "jdbc:mysql://127.0.0.1:3306/seata"
    user = "root"
    password = "123123"
    min-conn = 1
    max-conn = 3
    global.table = "global_table"
    branch.table = "branch_table"
    lock-table = "lock_table"
    query-limit = 100
  }
}
lock {
  ## the lock store mode: local、remote
  mode = "remote"

  local {
    ## store locks in user's database
  }

  remote {
    ## store locks in the seata's server
  }
}
recovery {
  #schedule committing retry period in milliseconds
  committing-retry-period = 1000
  #schedule asyn committing retry period in milliseconds
  asyn-committing-retry-period = 1000
  #schedule rollbacking retry period in milliseconds
  rollbacking-retry-period = 1000
  #schedule timeout retry period in milliseconds
  timeout-retry-period = 1000
}

transaction {
  undo.data.validation = true
  undo.log.serialization = "jackson"
  undo.log.save.days = 7
  #schedule delete expired undo_log in milliseconds
  undo.log.delete.period = 86400000
  undo.log.table = "undo_log"
}

## metrics settings
metrics {
  enabled = false
  registry-type = "compact"
  # multi exporters use comma divided
  exporter-list = "prometheus"
  exporter-prometheus-port = 9898
}

support {
  ## spring
  spring {
    # auto proxy the DataSource bean
    datasource.autoproxy = false
  }
}
```

> register.conf

```java
registry {
  # file 、nacos 、eureka、redis、zk、consul、etcd3、sofa
  type = "nacos"

  nacos {
    serverAddr = "localhost:8848"
    namespace = ""
    cluster = "default"
  }
  eureka {
    serviceUrl = "http://localhost:8761/eureka"
    application = "default"
    weight = "1"
  }
  redis {
    serverAddr = "localhost:6379"
    db = "0"
  }
  zk {
    cluster = "default"
    serverAddr = "127.0.0.1:2181"
    session.timeout = 6000
    connect.timeout = 2000
  }
  consul {
    cluster = "default"
    serverAddr = "127.0.0.1:8500"
  }
  etcd3 {
    cluster = "default"
    serverAddr = "http://localhost:2379"
  }
  sofa {
    serverAddr = "127.0.0.1:9603"
    application = "default"
    region = "DEFAULT_ZONE"
    datacenter = "DefaultDataCenter"
    cluster = "default"
    group = "SEATA_GROUP"
    addressWaitTime = "3000"
  }
  file {
    name = "file.conf"
  }
}

config {
  # file、nacos 、apollo、zk、consul、etcd3
  type = "file"

  nacos {
    serverAddr = "localhost"
    namespace = ""
  }
  consul {
    serverAddr = "127.0.0.1:8500"
  }
  apollo {
    app.id = "seata-server"
    apollo.meta = "http://192.168.1.204:8801"
  }
  zk {
    serverAddr = "127.0.0.1:2181"
    session.timeout = 6000
    connect.timeout = 2000
  }
  etcd3 {
    serverAddr = "http://localhost:2379"
  }
  file {
    name = "file.conf"
  }
}
```

> domain

```java
package com.atguigu.springcloud.alibaba.domain;
 
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
 
 
@Data
@AllArgsConstructor
@NoArgsConstructor
public class CommonResult<T>
{
    private Integer code;
    private String  message;
    private T       data;
 
    public CommonResult(Integer code, String message)
    {
        this(code,message,null);
    }
}
```

```
package com.atguigu.springcloud.alibaba.domain;
 
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
 
import java.math.BigDecimal;
 
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Account {
 
    private Long id;
 
    /**
     * 用户id
     */
    private Long userId;
 
    /**
     * 总额度
     */
    private BigDecimal total;
 
    /**
     * 已用额度
     */
    private BigDecimal used;
 
    /**
     * 剩余额度
     */
    private BigDecimal residue;
}
 
```

> dao

```java
package com.atguigu.springcloud.alibaba.dao;
 
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Repository;
 
import java.math.BigDecimal;
 
@Mapper
public interface AccountDao {
 
    /**
     * 扣减账户余额
     */
    void decrease(@Param("userId") Long userId, @Param("money") BigDecimal money);
}
 
```

> xml

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >

<mapper namespace="com.atguigu.springcloud.alibaba.dao.AccountDao">

    <resultMap id="BaseResultMap" type="com.atguigu.springcloud.alibaba.domain.Account">
        <id column="id" property="id" jdbcType="BIGINT"/>
        <result column="user_id" property="userId" jdbcType="BIGINT"/>
        <result column="total" property="total" jdbcType="DECIMAL"/>
        <result column="used" property="used" jdbcType="DECIMAL"/>
        <result column="residue" property="residue" jdbcType="DECIMAL"/>
    </resultMap>

    <update id="decrease">
        UPDATE t_account
        SET
            residue = residue - #{money},used = used + #{money}
        WHERE
            user_id = #{userId};
    </update>

</mapper>
```

> service

```java
package com.atguigu.springcloud.alibaba.service;
 
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;
 
import java.math.BigDecimal;
 
 
public interface AccountService {
 
    /**
     * 扣减账户余额
     */
    void decrease(@RequestParam("userId") Long userId, @RequestParam("money") BigDecimal money);
}
 
```

> service实现类

```java
package com.atguigu.springcloud.alibaba.service.impl;
 
 
import com.atguigu.springcloud.alibaba.dao.AccountDao;
import com.atguigu.springcloud.alibaba.service.AccountService ;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
 
import javax.annotation.Resource;
import java.math.BigDecimal;
import java.util.concurrent.TimeUnit;
 
/**
 * 账户业务实现类
 */
@Service
public class AccountServiceImpl implements AccountService {
 
    private static final Logger LOGGER = LoggerFactory.getLogger(AccountServiceImpl.class);
 
 
    @Resource
    AccountDao accountDao;
 
    /**
     * 扣减账户余额
     */
    @Override
    public void decrease(Long userId, BigDecimal money) {
        
         LOGGER.info("------->account-service中扣减账户余额开始");
       
        accountDao.decrease(userId,money);
        LOGGER.info("------->account-service中扣减账户余额结束");
    }
}
 
```

> controller

```
package com.atguigu.springcloud.alibaba.controller;
 
import com.atguigu.springcloud.alibaba.domain.CommonResult ;
import com.atguigu.springcloud.alibaba.service.AccountService ;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
 
import javax.annotation.Resource;
import java.math.BigDecimal;
 
@RestController
public class AccountController {
 
    @Resource
    AccountService accountService;
 
    /**
     * 扣减账户余额
     */
    @RequestMapping("/account/decrease")
    public CommonResult decrease(@RequestParam("userId") Long userId, @RequestParam("money") BigDecimal money){
        accountService.decrease(userId,money);
        return new CommonResult(200,"扣减账户余额成功！");
    }
}
 
```

> config

```java
package com.atguigu.springcloud.alibaba.config;
 
import com.alibaba.druid.pool.DruidDataSource;
import io.seata.rm.datasource.DataSourceProxy;
import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.transaction.SpringManagedTransactionFactory;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.io.support.PathMatchingResourcePatternResolver;
 
import javax.sql.DataSource;
 
 
@Configuration
public class DataSourceProxyConfig {
 
    @Value("${mybatis.mapperLocations}")
    private String mapperLocations;
 
    @Bean
    @ConfigurationProperties(prefix = "spring.datasource")
    public DataSource druidDataSource(){
        return new DruidDataSource();
    }
 
    @Bean
    public DataSourceProxy dataSourceProxy(DataSource dataSource) {
        return new DataSourceProxy(dataSource);
    }
 
    @Bean
    public SqlSessionFactory sqlSessionFactoryBean(DataSourceProxy dataSourceProxy) throws Exception {
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSourceProxy);
        sqlSessionFactoryBean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources(mapperLocations));
        sqlSessionFactoryBean.setTransactionFactory(new SpringManagedTransactionFactory());
        return sqlSessionFactoryBean.getObject();
    }
 
}
 
```

```java
package com.atguigu.springcloud.alibaba.config;
 
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Configuration;
 
@Configuration
@MapperScan({"com.atguigu.springcloud.alibaba.dao"})
public class MyBatisConfig {
 
}
 
```

> 主启动类

```java
package com.atguigu.springcloud.alibaba;
 
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.openfeign.EnableFeignClients;
 
 
@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)
@EnableDiscoveryClient
@EnableFeignClients
public class SeataAccountMainApp2003
{
    public static void main(String[] args)
    {
        SpringApplication.run(SeataAccountMainApp2003.class, args);
    }
}
 
```

==启动不报错说明搭建ok==



#### Seata的@GlobalTransactional验证

> 数据库初始情况

![image-20220403091454172](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204030914442.png)

![image-20220403091528040](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204030915400.png)

> 测试

![image-20220403092705942](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204030927328.png)==数据库情况：==

`订单库`:

![image-20220403093233783](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204030932053.png)

`账户库`：

![image-20220403093152253](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204030931576.png)

`库存库`：

![image-20220403093110358](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204030931780.png)

> 修改账户模块的service实现类 添加超时方法或者运行时异常比如10/0

```java
package com.atguigu.springcloud.alibaba.service.impl;


import com.atguigu.springcloud.alibaba.dao.AccountDao;
import com.atguigu.springcloud.alibaba.service.AccountService ;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;
import java.math.BigDecimal;
import java.util.concurrent.TimeUnit;

/**
 * 账户业务实现类
 */
@Service
public class AccountServiceImpl implements AccountService {

    private static final Logger LOGGER = LoggerFactory.getLogger(AccountServiceImpl.class);


    @Resource
    AccountDao accountDao;

    /**
     * 扣减账户余额
     */
    @Override
    public void decrease(Long userId, BigDecimal money) {

         LOGGER.info("------->account-service中扣减账户余额开始");
         //模拟超时异常，全局事务回滚   openfeign默认等待时间为1s中
        //暂停线程几秒钟
        try { TimeUnit.SECONDS.sleep(20); } catch (InterruptedException e) { e.printStackTrace(); }
        accountDao.decrease(userId,money);
        LOGGER.info("------->account-service中扣减账户余额结束");
    }
}
```

> 因为openfeign调用的等待时长是1s，暂停了20s，因此报错超时

![image-20220403093557280](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204030935642.png)

> 数据库情况：

`订单表`：

![image-20220403094244905](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204030942279.png)

`库存表`：

![image-20220403094320659](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204030943965.png)

`账户表`：

![image-20220403094352348](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204030943621.png)

==问题：==`余额扣了，库存减了，订单状态却还是0  ----->产生了数据不一致问题,由于openfeign的重试机制，账户余额还有可能被多次扣减` 



> 如何解决？？？ 在2001的业务service实现类加上下面这个注解即可  name的value可以任意

![image-20220403094806734](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204030948076.png)

==测试发现，三个库都没有新增记录==



#### Seata之原理简介

==2019年1月份蚂蚁金服和阿里巴巴共同开源的分布式事务解决方案，简单可扩展的自制事务框架==

工作中要用1.0以后的版本，因为0.9无法支持集群，无法商用 

> 对三个组件的理解加强

![image-20220403104202693](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031042151.png)

==分布式事务执行流程：==

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031043897.png)

TC：可以简单说就是seata服务器    我们启动seata-server.bat就是它

TM（事务的发起方）:头上带着@GlobalTransactional的微服务         

RM（ 事务的参与方）:一个数据库就是一个RM     

> AT模式如何做到对业务的无侵入             

![image-20220403104530852](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031045295.png)

![image-20220403104546188](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031045530.png)

`un_do表 ：用来做反向补偿`

> 一阶段加载:

![image-20220403104640742](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031046990.png)

![image-20220403104656684](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031046076.png)

> 二阶段提交

![image-20220403104724271](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031047683.png)

> 二阶段回滚

![image-20220403104747627](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031047933.png)

![image-20220403104803419](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031048810.png)

> debug分析过程

==打个断点，卡在这个地方，看看到这个地方的时候，数据库的状态==    这个调试法要会用，好用的

![image-20220403111157731](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031111149.png)

debug模式启动这三个微服务

![image-20220403111116882](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031111157.png)

访问：[localhost:2001/order/create?userId=1&productId=1&count=10&money=100](http://localhost:2001/order/create?userId=1&productId=1&count=10&money=100)

==此时查看seata库==

![image-20220403111452208](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031114560.png)

全局表：

![image-20220403111659676](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031117025.png)

行锁表

![image-20220403111730336](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031117671.png)

account的un_do表 其他两个业务表类似

![image-20220403111840619](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031118183.png)

用json解析工具解析rollback_info里面的内容

![image-20220403112108002](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031121254.png)

==最后跑完程序，发现un_do和seata库里的记录都没有了，完全符合阳哥讲的理论知识==

> 补充： 代理数据源就是我们自己写的配置类  ==DataSourceProxyConfig==

![image-20220403104835541](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031048788.png)

![image-20220403104850042](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204031048424.png)





