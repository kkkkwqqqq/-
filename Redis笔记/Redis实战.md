# Redis

## Redis模拟验证码的实现

> 导入依赖

```
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.2.0</version>
</dependency>
```

> 示例代码：

```
package com.atguigu.jedis;

import redis.clients.jedis.Jedis;

import java.util.Random;

public class PhoneCode {
    public static void main(String[] args) {
        //模拟验证码发送
        verifyCode("15819475746");

        //模拟验证码校验
   getRedisCode("15819475746","848764");
    }

    //3 验证码校验
    public static void getRedisCode(String phone,String code) {
        //从redis获取验证码
        Jedis jedis = new Jedis("192.168.229.128",6379);
        //验证码key
        String codeKey = "VerifyCode"+phone+":code";
        String redisCode = jedis.get(codeKey);
        //判断
        if(redisCode.equals(code)) {
            System.out.println("成功");
        }else {
            System.out.println("失败");
        }
        jedis.close();
    }

    //2 每个手机每天只能发送三次，验证码放到redis中，设置过期时间120
    public static void verifyCode(String phone) {
        //连接redis
        Jedis jedis = new Jedis("192.168.229.128",6379);

        //拼接key
        //手机发送次数key
        String countKey = "VerifyCode"+phone+":count";
        //验证码key
        String codeKey = "VerifyCode"+phone+":code";

        //每个手机每天只能发送三次
        String count = jedis.get(countKey);
        if(count == null) {
            //没有发送次数，第一次发送
            //设置发送次数是1
            jedis.setex(countKey,24*60*60,"1");
        } else if(Integer.parseInt(count)<=2) {
            //发送次数+1
            jedis.incr(countKey);
        } else if(Integer.parseInt(count)>2) {
            //发送三次，不能再发送
            System.out.println("今天发送次数已经超过三次");
            jedis.close();
            return;
        }

        //发送验证码放到redis里面
        String vcode = getCode();
        jedis.setex(codeKey,120,vcode);
        jedis.close();
    }

    //1 生成6位数字验证码
    public static String getCode() {
        Random random = new Random();
        String code = "";
        for(int i=0;i<6;i++) {
            int rand = random.nextInt(10);
            code += rand;
        }
        return code;
    }
}
```

> 上面的代码可以加以修改形成如下图的实现
>
> ![image-20220116105055188](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116105055188.png)



## SpringBoot和Redis的整合

> 导入依赖

```
<!-- redis -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>

    <!-- spring2.X集成redis所需common-pool2-->
    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-pool2</artifactId>
        <version>2.6.0</version>
    </dependency>
</dependencies>
```



> 配置文件

```
#Redis服务器地址
spring.redis.host=192.168.229.128
#Redis服务器连接端口
spring.redis.port=6379
#Redis数据库索引（默认为0）
spring.redis.database= 0
#连接超时时间（毫秒）
spring.redis.timeout=1800000
#连接池最大连接数（使用负值表示没有限制）
spring.redis.lettuce.pool.max-active=20
#最大阻塞等待时间(负数表示没限制)
spring.redis.lettuce.pool.max-wait=-1
#连接池中的最大空闲连接
spring.redis.lettuce.pool.max-idle=5
#连接池中的最小空闲连接
spring.redis.lettuce.pool.min-idle=0

```

### RedisTemplate的使用

> 示例代码：编写Controller

```
package com.atguigu.redis_springboot.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/redisTest")
public class RedisTestController {
    @Autowired
    private RedisTemplate redisTemplate;

    @GetMapping
    public String testRedis() {
        //设置值到redis
        redisTemplate.opsForValue().set("name","lucy");
        //从redis获取值
        String name = (String)redisTemplate.opsForValue().get("name");
        return name;
    }
}
```

> 若报错
>
> ![image-20220116111656306](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116111656306.png)

> 解决方案：增加配置类

```
package com.atguigu.redis_springboot.config;

import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.cache.CacheManager;
import org.springframework.cache.annotation.CachingConfigurerSupport;
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.cache.RedisCacheConfiguration;
import org.springframework.data.redis.cache.RedisCacheManager;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.RedisSerializationContext;
import org.springframework.data.redis.serializer.RedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

import java.time.Duration;

@EnableCaching
@Configuration
    public class RedisConfig extends CachingConfigurerSupport {

        @Bean
        public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
            RedisTemplate<String, Object> template = new RedisTemplate<>();
            RedisSerializer<String> redisSerializer = new StringRedisSerializer();
            Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
            ObjectMapper om = new ObjectMapper();
            om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
            om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
            jackson2JsonRedisSerializer.setObjectMapper(om);
            template.setConnectionFactory(factory);
//key序列化方式
            template.setKeySerializer(redisSerializer);
//value序列化
            template.setValueSerializer(jackson2JsonRedisSerializer);
//value hashmap序列化
            template.setHashValueSerializer(jackson2JsonRedisSerializer);
            return template;
        }

        @Bean
        public CacheManager cacheManager(RedisConnectionFactory factory) {
            RedisSerializer<String> redisSerializer = new StringRedisSerializer();
            Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
//解决查询缓存转换异常的问题
            ObjectMapper om = new ObjectMapper();
            om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
            om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
            jackson2JsonRedisSerializer.setObjectMapper(om);
// 配置序列化（解决乱码的问题）,过期时间600秒
            RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                    .entryTtl(Duration.ofSeconds(600))
                    .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer))
                    .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(jackson2JsonRedisSerializer))
                    .disableCachingNullValues();
            RedisCacheManager cacheManager = RedisCacheManager.builder(factory)
                    .cacheDefaults(config)
                    .build();
            return cacheManager;}
    }
```

> 测试效果：

![image-20220116105631750](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116105631750.png)

> redis内部存储：

![image-20220116111007487](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116111007487.png)

## Redis事务------秒杀并发模拟

> 核心业务就是实现：
>
> 商品库存对应的value-1同时将秒杀成功者放入秒杀成功者的value中

![image-20220116135422197](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116135422197.png)

> 准备了一个Web项目

![image-20220116140054328](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116140054328.png)

> index.jsp页面 代码

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
<h1>iPhone 13 Pro !!!  1元秒杀！！！
</h1>


<form id="msform" action="${pageContext.request.contextPath}/doseckill" enctype="application/x-www-form-urlencoded">
	<input type="hidden" id="prodid" name="prodid" value="0101">
	<input type="button"  id="miaosha_btn" name="seckill_btn" value="秒杀点我"/>
</form>

</body>
<script  type="text/javascript" src="${pageContext.request.contextPath}/script/jquery/jquery-3.1.0.js"></script>
<script  type="text/javascript">
$(function(){
	$("#miaosha_btn").click(function(){	 
		var url=$("#msform").attr("action");
	     $.post(url,$("#msform").serialize(),function(data){
     		if(data=="false"){
    			alert("抢光了" );
    			$("#miaosha_btn").attr("disabled",true);
    		}
		} );    
	})
})
</script>
</html>
```

> web.xml配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
        http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
           version="2.5">

    <servlet>
        <description></description>
        <display-name>doseckill</display-name>
        <servlet-name>doseckill</servlet-name>
        <servlet-class>com.atguigu.SecKillServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>doseckill</servlet-name>
        <url-pattern>/doseckill</url-pattern>
    </servlet-mapping>
</web-app>

```

> SecKillServlet

```
package com.atguigu;

import java.io.IOException;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Random;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.xml.ws.soap.AddressingFeature.Responses;

/**
 * 秒杀案例
 */
public class SecKillServlet extends HttpServlet {
   private static final long serialVersionUID = 1L;

    public SecKillServlet() {
        super();
    }

   protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

      String userid = new Random().nextInt(50000) +"" ;
      String prodid =request.getParameter("prodid"); //写死的0101

      boolean isSuccess=SecKill_redis.doSecKill(userid,prodid);
      response.getWriter().print(isSuccess);
   }

}
```

> SecKill_redis类（写核心代码）

```
public class SecKill_redis {


   //秒杀过程
   public static boolean doSecKill(String uid,String prodid) throws IOException {
      //1 uid和prodid非空判断
      if(uid == null || prodid == null) {
         return false;
      }

      //2 连接redis
      Jedis jedis = new Jedis("192.168.229.128",6379);

      //3 拼接key
      // 3.1 库存key
      String kcKey = "sk:"+prodid+":qt";
      // 3.2 秒杀成功用户key
      String userKey = "sk:"+prodid+":user";

      //4 获取库存，如果库存null，秒杀还没有开始
      String kc = jedis.get(kcKey);
      if(kc == null) {
         System.out.println("秒杀还没有开始，请等待");
         jedis.close();
         return false;
      }

      // 5 判断用户是否重复秒杀操作  jedis.sismember 判断uid在userKey这个集合中是否存在
      if(jedis.sismember(userKey, uid)) {
         System.out.println("已经秒杀成功了，不能重复秒杀");
         jedis.close();
         return false;
      }

      //6 判断如果商品数量，库存数量小于1，秒杀结束
      if(Integer.parseInt(kc)<=0) {
         System.out.println("秒杀已经结束了");
         jedis.close();
         return false;
      }

      //7 秒杀过程
      //7.1 库存-1
      jedis.decr(kcKey);
      //7.2 把秒杀成功用户添加清单里面
      jedis.sadd(userKey,uid);

      System.out.println("秒杀成功了..");
      jedis.close();
      return true;
   }
}
```

### **没有进行并发操作秒杀时**

 **一切都很正常** 库存减少了，用户名单如预期被保存且没有重复

![image-20220116155421072](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116155421072.png)

![image-20220116155430610](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116155430610.png)

![image-20220116155512559](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116155512559.png)

![image-20220116155713847](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116155713847.png)

### 进行并发操作秒杀时

> 1. 在虚拟机 联网下载工具ab来模拟并发测试，联网条件下下载使用命令 yum install httpd-tools下载安装即可
>
> 2. 查看win的ip地址 cmd命令行输入 ipconfig查看
>
>    ![image-20220116160329107](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116160329107.png)
>
> 使用指令模拟高并发操作  `指令说明`
>
> - n是请求次数
> - c是并发次数
> - p是提交参数（需要建立一个文件）在根目录下开一个文件（路径可变，ab测试的时候把p后面的路径改了就行了） 使用命令 vi `postfile` 里面内容是prodid=0101& `也就是参数的内容`
> - T后面的值来自表单+请求地址
> - ![image-20220116160849448](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116160849448.png)
>
> 用指令 ab -n 1000 -c 100 -p ~/`postfile` -T application/x-www-form-urlencoded http://192.168.124.1:8080/Seckill/doseckill
>
> 发送测试

**执行操作图如下**：

![image-20220116161317600](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116161317600.png)

ab测试：

![image-20220116161345491](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116161345491.png)

ide控制台：

![image-20220116161416099](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116161416099.png)

 这是比较理想的情况：

![image-20220116161549278](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116161549278.png)

上诉ab测试，多进行几次可能会出现问题：

### 可能出现的问题：

#### **问题一：商品超卖**

（sk:0101:qt可能为负数，比如-3等）

原因如下图：![image-20220116162410852](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116162410852.png)

##### **利用乐观锁可以解决超卖的问题**

![image-20220116162454392](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116162454392.png)

> 修改SecKill_redis类 解决超卖问题

```
package com.atguigu;

import java.io.IOException;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

import org.apache.commons.pool2.impl.GenericObjectPoolConfig;
import org.slf4j.LoggerFactory;

import ch.qos.logback.core.rolling.helper.IntegerTokenConverter;
import redis.clients.jedis.HostAndPort;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisCluster;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;
import redis.clients.jedis.ShardedJedisPool;
import redis.clients.jedis.Transaction;

/**
 *
 */
public class SecKill_redis {

   public static void main(String[] args) {
      Jedis jedis =new Jedis("192.168.229.128",6379);
      System.out.println(jedis.ping());
      jedis.close();
   }

   //秒杀过程
   public static boolean doSecKill(String uid,String prodid) throws IOException {
      //1 uid和prodid非空判断
      if(uid == null || prodid == null) {
         return false;
      }

      //2 连接redis
     Jedis jedis = new Jedis("192.168.229.128",6379);


      //3 拼接key
      // 3.1 库存key
      String kcKey = "sk:"+prodid+":qt";
      // 3.2 秒杀成功用户key
      String userKey = "sk:"+prodid+":user";

      //监视库存  底层是乐观锁
      jedis.watch(kcKey);

      //4 获取库存，如果库存null，秒杀还没有开始
      String kc = jedis.get(kcKey);
      if(kc == null) {
         System.out.println("秒杀还没有开始，请等待");
         jedis.close();
         return false;
      }

      // 5 判断用户是否重复秒杀操作
      if(jedis.sismember(userKey, uid)) {
         System.out.println("已经秒杀成功了，不能重复秒杀");
         jedis.close();
         return false;
      }

      //6 判断如果商品数量，库存数量小于1，秒杀结束
      if(Integer.parseInt(kc)<=0) {
         System.out.println("秒杀已经结束了");
         jedis.close();
         return false;
      }

      //7 秒杀过程
      //使用事务
      Transaction multi = jedis.multi();

      //组队操作
      multi.decr(kcKey);
      multi.sadd(userKey,uid);

      //执行
      List<Object> results = multi.exec();

      if(results == null || results.size()==0) {
         System.out.println("秒杀失败了....");
         jedis.close();
         return false;
      }

      //7.1 库存-1
      //jedis.decr(kcKey);
      //7.2 把秒杀成功用户添加清单里面
      //jedis.sadd(userKey,uid);

      System.out.println("秒杀成功了..");
      jedis.close();
      return true;
   }
}
```

> 测试效果：

![image-20220116191454605](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116191454605.png)

#### **问题二：连接超时**

原因:大量的请求访问，需要排队，排在后面的就会出现连接超时的问题可以通过连接池来解决

![image-20220116162044724](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116162044724.png)

解决方案：新建一个类，用来创建Redis连接池对象

```
//Redis工具类 防止高并发场景下连接超时
public class JedisPoolUtil {
   private static volatile JedisPool jedisPool = null;

   private JedisPoolUtil() {
   }

   public static JedisPool getJedisPoolInstance() {
      if (null == jedisPool) {
         synchronized (JedisPoolUtil.class) {
            if (null == jedisPool) {
               JedisPoolConfig poolConfig = new JedisPoolConfig();
               poolConfig.setMaxTotal(200);
               poolConfig.setMaxIdle(32);
               poolConfig.setMaxWaitMillis(100*1000);
               poolConfig.setBlockWhenExhausted(true);
               poolConfig.setTestOnBorrow(true);  // ping  PONG

               jedisPool = new JedisPool(poolConfig, "192.168.229.128", 6379, 60000 );
            }
         }
      }
      return jedisPool;
   }

   public static void release(JedisPool jedisPool, Jedis jedis) {
      if (null != jedis) {
         jedisPool.returnResource(jedis);
      }
   }

}
```

将步骤二换成下面代码即可解决！！！

```
//2 连接redis
//Jedis jedis = new Jedis("192.168.44.168",6379);
//通过连接池得到jedis对象
JedisPool jedisPoolInstance = JedisPoolUtil.getJedisPoolInstance();
Jedis jedis = jedisPoolInstance.getResource();
```

#### 用乐观锁解决超卖的问题：会出现库存遗留问题

重写测试：

![image-20220116201436879](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116201436879.png)

![image-20220116201454579](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116201454579.png)

![image-20220116204130557](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220116204130557.png)

`发现库存遗留，因为用了乐观锁，改了版本号，其他人进去发现版本号不一样，不执行。因此可能造成库存遗留！！！`

redis默认不能使用悲观锁。解决方案：**Lua脚本语言**

#### Lua解决超卖问题不产生库存遗留问题

#### Lua介绍：

> Lua 是一个小巧的[脚本语言](http://baike.baidu.com/item/脚本语言)，Lua脚本可以很容易的被C/C++ 代码调用，也可以反过来调用C/C++的函数，Lua并没有提供强大的库，一个完整的Lua解释器不过200k，所以Lua不适合作为开发独立应用程序的语言，而是作为嵌入式脚本语言。
>
> 很多应用程序、游戏使用LUA作为自己的嵌入式脚本语言，以此来实现可配置性、可扩展性。
>
> 这其中包括魔兽争霸地图、魔兽世界、博德之门、愤怒的小鸟等众多游戏插件或外挂。
>
> 学习网址：https://www.w3cschool.cn/lua/

Lua脚本在Redis的优势：

> 将复杂的或者多步的redis操作，写为一个脚本，一次提交给redis执行，减少反复连接redis的次数。提升性能。
>
> LUA脚本是类似redis事务，`有一定的原子性`，`不会被其他命令插队`，可以完成一些`redis事务性的操作`。
>
> 但是注意redis的lua脚本功能，只有在`Redis 2.6`以上的版本才可以使用。
>
> 利用lua脚本淘汰用户，解决`超卖`问题。
>
> redis 2.6版本以后，通过lua脚本解决`争抢问题`，实际上是`redis 利用其单线程的特性，用任务队列的方式解决多任务并发问题`。



具体应用在程序中：

新建一个类代替SecKill_redis

```
public class SecKill_redisByScript {

   private static final  org.slf4j.Logger logger =LoggerFactory.getLogger(SecKill_redisByScript.class) ;

   static String secKillScript ="local userid=KEYS[1];\r\n" +
         "local prodid=KEYS[2];\r\n" +
         "local qtkey='sk:'..prodid..\":qt\";\r\n" +
         "local usersKey='sk:'..prodid..\":usr\";\r\n" +
         "local userExists=redis.call(\"sismember\",usersKey,userid);\r\n" +
         "if tonumber(userExists)==1 then \r\n" +
         "   return 2;\r\n" +
         "end\r\n" +
         "local num= redis.call(\"get\" ,qtkey);\r\n" +
         "if tonumber(num)<=0 then \r\n" +
         "   return 0;\r\n" +
         "else \r\n" +
         "   redis.call(\"decr\",qtkey);\r\n" +
         "   redis.call(\"sadd\",usersKey,userid);\r\n" +
         "end\r\n" +
         "return 1" ;

   static String secKillScript2 =
         "local userExists=redis.call(\"sismember\",\"{sk}:0101:usr\",userid);\r\n" +
         " return 1";

   public static boolean doSecKill(String uid,String prodid) throws IOException {

      JedisPool jedispool =  JedisPoolUtil.getJedisPoolInstance();
      Jedis jedis=jedispool.getResource();

       //加载脚本
      String sha1=  jedis.scriptLoad(secKillScript);
      Object result= jedis.evalsha(sha1, 2, uid,prodid);

        String reString=String.valueOf(result);
      if ("0".equals( reString )  ) {
         System.err.println("已抢空！！");
      }else if("1".equals( reString )  )  {
         System.out.println("抢购成功！！！！");
      }else if("2".equals( reString )  )  {
         System.err.println("该用户已抢过！！");
      }else{
         System.err.println("抢购异常！！");
      }
      jedis.close();
      return true;
   }
}
```

测试：问题完美解决，既没有超卖也没有遗留！！！
