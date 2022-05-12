# SpringBoot集成Redis之阿里云短信发送

## 1.了解阿里云用户权限操作

个人账户需要去调用阿里云的服务，需要授权码 就是AccessKeyID 和 AccessKeySercret

![截图](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220114230006078.png)

1：进入阿里云 进入控制台 鼠标移动到个人头像那里 选择AccessKey管理

![image-20220114230706079](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220114230706079.png)

2：点击使用子用户AccessKey

![image-20220114230808232](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220114230808232.png)

3：新建一个用户组（设置添加权限 sms）

点击创建用户组

![image-20220114232503483](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220114232503483.png)

填写信息

![image-20220114232645882](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220114232645882.png)

创建好用户组后，点击用户组列表右侧的【添加权限】，然后搜索 `sms`，选择 **AliyunDysmsFullAccess：管理短信服务（SMS）的权限**，点击【确认】为用户组添加 sms 权限。

![image-20220114232759742](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220114232759742.png)

4：创建一个用户（用来具体操作的账号）

点击左侧菜单栏【用户】，然后点击【创建用户】，填写相关信息，勾选 `Open API调用访问`，点击【确认】完成用户的创建。

![image-20220114233042499](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220114233042499.png)

![image-20220114233010311](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220114233010311.png)

5：得到AccessKey(id,密码)   用来操作阿里云权限的（不要泄露，泄露的话就删除或者禁用）

![image-20220115100043551](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115100043551.png)

LTAI5tArVUsdLsQQgH2qNjKh

PkKoEqORwl48PPyngIvOVVhb3wsgdX

==注意：要将这个账号密码保存下来==

6：点击【添加到用户组】，将创建好的用户添加到创建好的用户组里。

![image-20220114233428278](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220114233428278.png)

可以发现用户会自动继承所属用户组的权限：

![image-20220114233522436](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220114233522436.png)



## 2.开通阿里云短信服务

一、在控制台搜索款搜索 `短信服务`，点击【短信服务】，进入短信服务控制台

短信服务帮助文档 https://help.aliyun.com/product/44282.html?spm=5176.12207334/0/4ee71cbecWxKqm

![image-20220115123705079](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115123705079.png)

二、点击左侧菜单栏的【快速学习】，开通短信服务并快速了解短信服务相关信息。

![image-20220115121620051](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115121620051.png)



可以测试短信发送，了解短信计费规则等。

![image-20220115121722192](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115121722192.png)

可以设置发送频率（企业用户才行）

![image-20220115122301992](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115122301992.png)

## 3.添加短信模板

一、选择左侧菜单【国内消息】，选择【模板管理】，点击【添加模板】。

![image-20220115122823138](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115122823138.png)

二、填写相关内容（申请说明，需要正当理由），填写完后等待审核。

![image-20220115122933913](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115122933913.png)

**模板内容是短信的具体内容，而name则是我们通过程序传入进去的**

申请后：（一定要审核通过）

![image-20220115123051454](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115123051454.png)





## 4.添加签名

**![image-20220115123146402](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115123146402.png)**

场景说明需要正当理由

![image-20220115123235229](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115123235229.png)



## 4.编写测试代码

查看APIdemo 在快速学习中选择控制台发送测试 然后查看API Demo

![image-20220115124037724](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115124037724.png)

SDK选择原版本

![image-20220115124254958](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115124254958.png)

打开SDK的参考文档（上面的那个帮助文档连接）可以知道，需要jdk1.6以上的开发环境,还需要导入阿里云jdk依赖

![image-20220115124821038](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115124821038.png)

![image-20220115124907181](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115124907181.png)

接着还有发送短信的实例代码

![image-20220115124948802](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115124948802.png)

代码如下：

![image-20220115125105119](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115125105119.png)



打开API参考 点击发送短信

![image-20220115125351981](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115125351981.png)

![image-20220115125518459](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115125518459.png)



请求参数：

![image-20220115125535631](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115125535631.png)

新建一个boot项目 

导入三个依赖

```
  <!--导入依赖-->
        <!-- 阿里云SDK 发短信用-->
        <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-core</artifactId>
            <version>4.5.3</version>
        </dependency>
        <!-- json工具类  构建数据用-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.62</version>
        </dependency>
        <!-- Redis  缓存用户信息-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

```



> 测试一下：将实例代码复制过来
>
> 在SpringBoot的test中测试
>

```
package com.sms;

@SpringBootTest
class SmsApplicationTests {

    @Test
    void contextLoads() {
        //连接阿里云 账号LTAI5tArVUsdLsQQgH2qNjKh 密码PkKoEqORwl48PPyngIvOVVhb3wsgdX（就是创建用户时生成的AccessKey）
                DefaultProfile profile = DefaultProfile.getProfile("cn-hangzhou", "自己的账号", "自己的密码");
                
                IAcsClient client = new DefaultAcsClient(profile);
            //构建请求
                CommonRequest request = new CommonRequest();
                request.setSysMethod(MethodType.POST);
                request.setSysDomain("dysmsapi.aliyuncs.com"); //不动 官方说的
                request.setSysVersion("2017-05-25");//不动 官方说的
                request.setSysAction("SendSms");
        // 自定义的参数（手机号，验证码，签名，模板）
        request.putQueryParameter("phoneNumbers","自己的手机号");
        request.putQueryParameter("SignName","阿里云签名名称");  //柯维科技
        request.putQueryParameter("TemplateCode","阿里云模板Code");
        //构建一个短信验证码 写死
        Map<String,Object> param = new HashMap<>();
        param.put("code",2233);//发送给客户的验证码
        request.putQueryParameter("TemplateParam", JSONObject.toJSONString(param)); //短信参数  json格式化

                try {

                    CommonResponse response = client.getCommonResponse(request);
                    System.out.println(response.getData());
                } catch (ServerException e) {
                    e.printStackTrace();
                } catch (ClientException e) {
                    e.printStackTrace();
                }
            }
        }




```

发送测试：

短信收到：

【柯维科技】您正在申请注册，验证码维：2233，五分钟内有效

## 5.编写可服用的微服务接口，实现验证码的发送！

> 架构：
>

![image-20220115140623916](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220115140623916.png)



```java
Service接口
public interface SendSms {
    boolean send(String PhoneNumbers, String TemplateCode, Map<String,Object> code);
}
实现类


@Service
public class SendSmsImpl implements SendSms {
    @Override
    public boolean send(String phoneNum, String templateCode, Map<String, Object> code) {
        //连接阿里云
        DefaultProfile profile = DefaultProfile.getProfile("cn-hangzhou", "自己的账号", "自己的密码");
        IAcsClient client = new DefaultAcsClient(profile);
        //构建请求
        CommonRequest request = new CommonRequest();
        request.setSysMethod(MethodType.POST);
        request.setSysDomain("dysmsapi.aliyuncs.com"); //不懂
        request.setSysVersion("2017-05-25");//不懂
        request.setSysAction("SendSms");
        //自定义的参数（手机号，验证码，签名，模板）
        request.putQueryParameter("phoneNumbers", phoneNum);
        request.putQueryParameter("SignName", "阿里云签名名称");
        request.putQueryParameter("TemplateCode", templateCode);

        request.putQueryParameter("TemplateParam", JSONObject.toJSONString(code)); //短信参数

        try {

            CommonResponse response = client.getCommonResponse(request);
            System.out.println(response.getData());
            return response.getHttpResponse().isSuccess();
        } catch (ServerException e) {
            e.printStackTrace();
        } catch (ClientException e) {
            e.printStackTrace();
        }

    return false; }

}
Controller


@RestController
@CrossOrigin //跨域支持  不同微服务直接需要处理跨域问题
public class SmsApiController {

    @Autowired
    private SendSms sendSms;

    @Autowired
    private RedisTemplate<String,String> redisTemplate; //redis模板

    @GetMapping("/send/{phone}")//restful风格
    public String code(@PathVariable("phone") String phone){
        String code = redisTemplate.opsForValue().get(phone);
        if (!StringUtils.isEmpty(code)){
            return phone + ":" + code + "已存在，还没有过期";
        }
        code = (int)((Math.random()*9+1)*100000) + ""; // 生成6位随机数
        //生成验证码并且存储到redis中
        // code = UUID.randomUUID().toString().substring(0, 4);
        Map<String,Object> param = new HashMap<>();
        param.put("code",code);
        boolean result = sendSms.send(phone, "SMS_199770xxx", param);
        if (result){
            //设置五分钟过期
            redisTemplate.opsForValue().set(phone,code,5, TimeUnit.MINUTES);
            return phone + ":" + code + "已发送成功";
        }
        return "发送失败";
    }
}
配置文件
    # 应用名称
spring.application.name=sms

server.port=9090

spring.redis.host=127.0.0.1
spring.redis.port=6379



```