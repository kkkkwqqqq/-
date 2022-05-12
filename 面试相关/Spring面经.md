# Spring面经

## 说下你理解的 Spring

可以从两个层面来理解 Spring。

第一个层面指的是 Spring Framework，是一个开源的应用框架，提供 IOC 和 AOP（==比如项目用过的统一日志打印，就是用了aop，商城项目==） 降低了应用开发的复杂度。

第二个层面指的是 Spring 全家桶，Spring 发展到今天可以说几乎是统领了 Java，有关 Java 应用开发所需的全部功能， Spring 都提供了解决方案，包括对 web 的支持、对微服务的支持等。

这种题大致说一下就行了，不需要太多细节，面试官会接着追问的。



## 什么是 IOC？

IOC是控制反转的意思，它是一种思想。程序由主动的编程变成被动的接收。控制指的是对象的创建完全由ioc来帮我们创建。我们只需要编写好配置文件即可。反转指的是，创建对象且注入依赖对象的这个动作，本来这个动作是由我们程序员在代码里面指定的，例如对象 A 依赖对象 B，在创建对象 A 代码里，我们需要写好如何创建对象 B，这样才能构造出一个完整的 A。

而反转之后，这个动作就由 IOC 容器触发，IOC 容器在创建对象 A 的时候，发现依赖对象 B ，就会将对象 B 注入 A 中。

这里要注意，注入的不一定非得是一个对象，也可以注入配置文件里面的一个值给对象 A 等等。



## IOC 有什么好处？

对象的创建都由 IOC 容器来控制之后，对象之间就不会有很明确的依赖关系，使得非常容易设计出松耦合的程序。

例如，对象 A 需要依赖一个B实现，但是对象都由 IOC 控制之后，我们不需要明确地在对象 A 的代码里写死依赖的实现 B，只需要写明依赖一个接口，这样我们的代码就能顺序的编写下去。

然后，我们可以在配置文件里定义 A 依赖的具体的实现 B，根据配置文件，在创建 A 的时候，IOC 容器就知晓 A 依赖的 B，这时候注入这个依赖即可。

如果之后你有新的实现需要替换，那 A 的代码不需要任何改动，你只需要将配置文件 A 依赖 B 改成 B1，这样重启之后，IOC 容器会为 A 注入 B1。

这样就使得类 A 和类 B 解耦了， very nice！

==并且也因为创建对象由 IOC 全权把控，那么我们就能很方便的让 IOC 基于扩展点来“加工”对象，例如我们要代理一个对象，IOC 在对象创建完毕，直接判断下这个对象是否需要代理，如果要代理，则直接包装返回代理对象。==

这等于我们只要告诉 IOC 我们要什么，IOC 就能基于我们提供的配置文件，创建符合我们需求的对象。

**正是这个控制反转的思想，解放了我们的双手**。



## 什么是 DI

DI，Dependency Injection，依赖注入。

普遍的答案是 DI 是 IOC 的一种实现。

大致理解为容器在运行的时候，可以找到被依赖的对象，然后将其注入，通过这样的方式，使得各对象之间的关系可由运行期决定，而不用在编码时候明确。




## 什么是 Bean

可以认为，由 Spring 创建的就是 Bean。



## BeanFactory 是什么？

BeanFactory 其实就是 **IOC 的底层容器**。

我们都说 Spring 是 IOC 容器，说的再直白点，其实就是 Bean 的工厂，它帮我们生产 Bean，如果我们需要 Bean 就从工厂拿到 bean，所以再来理解下 BeanFactory 这个名字，就知晓它就是 Spring 的核心。

例如我们调用 getBean ，这就是 BeanFactory 定义的方法，通过它得到 Bean。

不过一般我们所述的 BeanFactory 指的是它实现类，例如 DefaultListableBeanFactory。

BeanFactory 本身只是一个接口。



## FactoryBean 又是啥

从命名角度来看，我们可以得知它就是一个 Bean，而不是一个工厂。

那为什么名字如此奇怪，它其实是一个接口，并且有以下几个方法

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204202351542.png)

如果一个对象实现了这接口，那它就成为一种特殊的 Bean，注册到 IOC 容器之后，如果调用 getBean 获取得到的其实是 FactoryBean.getObject() 方法返回的结果。

==为什么要这样做？==

假设你依赖一个第三方的类 A，而我们又不能修改第三方的类，并且这个对象创建比较复杂，那么你就可以创建一个 bean 来封装它：

```java
public class AFactoryBean implements FactoryBean<A> {
    public A getObject() throws Exception {
        A a = new A();
        a.setXXX
        ....
        ...
        return A
    }
    ....省略一些实现
}
```

这样，我们 getBean("A") 会得到 AFactoryBean.getObject 的结果，如果单纯只想要 AFactoryBean， 那么加个 “&” 即可，即 getBean("&A")   ==具体可以看我的博客有写==



## ObjectFactory 又是啥（==看循环依赖的时候一起过掉==）

从命名角度来看，它是一个工厂。

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204210003887.png)

好吧，摊牌了，没啥特殊含义，就是一个工厂。

Spring 的循环依赖（后面会细说）里就用到它了，三级缓存的 map 里面存储的就是 ObjectFactory，用于延迟代理对象的创建。

其实**就是封装对象的创建过程**，像三级缓存里的 ObjectFactory 逻辑就是判断这个 Bean 是否有代理，如果有则返回代理对象，没有则返回原来传入的对象。

## ApplicationContext 呢

ApplicationContext 对我们来说应该很熟悉，我们基本上都是基于 ApplicationContext 操作的。

它也继承了 BeanFactory 的实现类，也属于一个 BeanFactory ，但是它内部还包装了一个 BeanFactory 的实现，这属于**组合**。

简单理解就是我 ApplicationContext 虽然是个 BeanFactory ，但是关于 Bean 的操作我还是委托内部组合的 BeanFactory 。

即，**关于 Spring Beans 的一些操作都是委托内部的 BeanFactory 来操作的**。

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204210003189.png)

所以它有 BeanFactory 的所有功能。看到这肯定有同学发出疑问，那这玩意又啥用？

别急嘛，因为它还扩展了很多其他功能：

- AOP
- 国际化
- 资源管理
- 事件
- 注解
- 等等

因此，我们开发直接用的肯定是 ApplicationContext 而不是 BeanFactory。



## Bean 一共有几种作用域

从官网，我们很容易可以得知，最新版本一共有六种作用域：

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204210015240.png)

- singleton：默认是单例，含义不用解释了吧，一个 IOC 容器内部仅此一个
- prototype：原型，多实例
- request：每个请求都会新建一个属于自己的 Bean 实例，这种作用域仅存在 Spring Web 应用中
- session：一个 http session 中有一个 bean 的实例，这种作用域仅存在 Spring Web 应用中
- application：整个 ServletContext 生命周期里，只有一个 bean，这种作用域仅存在 Spring Web 应用中
- websocket：一个 WebSocket 生命周期内一个 bean 实例，这种作用域仅存在 Spring Web 应用中

别背网上那些多年前的答案了，以上才是最新的~

参考文章：

[(14条消息) Spring Bean的作用域_kongmin_123的博客-CSDN博客_springbean的作用域](https://blog.csdn.net/kongmin_123/article/details/82048392?ops_request_misc=%7B%22request%5Fid%22%3A%22165051724116782184618390%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=165051724116782184618390&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-82048392.142^v9^control,157^v4^control&utm_term=Bean的作用域&spm=1018.2226.3001.4187)

## Spring 一共有几种注入方式

- 构造器注入，**Spring 倡导构造函数注入**，因为构造器注入返回给客户端使用的时候一定是完整的。
- setter 注入，可选的注入方式，好处是在有变更的情况下，可以重新注入。
- 字段注入，就是平日我们用 @Autowired **标记字段**
- 方法注入，就是平日我们用 @Autowired **标记方法**
- 接口回调注入，就是实现 Spring 定义的一些内建接口，例如 BeanFactoryAware，会进行 BeanFactory 的注入

其实官网上关于注入就写了构造器和setter ：


![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204210016695.png)

## 说下 AOP

AOP，Aspect Oriented Programming，面向切面编程。

将一些通用的逻辑集中实现，然后通过 AOP 进行逻辑的切入，减少了零散的碎片化代码，提高了系统的可维护性。

具体是含义可以理解为：通过代理的方式，在调用想要的对象方法时候，进行拦截处理，执行切入的逻辑，然后再调用真正的方法实现。

例如，你实现了一个 A 对象，里面有  addUser 方法，此时你需要记录该方法的调用次数。

那么你就可以搞个代理对象，这个代理对象也提供了 addUser 方法，最终你调用的是代理对象的 addUser ，在这个代理对象内部填充记录调用次数的逻辑，最终的效果就类似下面代码：

```java
class A代理 {
    A a;// 被代理的 A
   void addUser(User user) {
     count();// 计数
     a.addUser(user);
   }
}
最终使用的是：
A代理.addUser(user);
```

这就叫做**面向切面编程**，当然具体的代理的代码不是像上面这样写死的，**而是动态切入**。

实现上代理大体上可以分为：**动态代理**和**静态代理**。

- 动态代理，即**在运行时**将切面的逻辑进去，按照上面的逻辑就是你实现 A 类，然后定义要代理的切入点和切面的实现，程序会自动在运行时生成类似上面的代理类。
- 静态代理，**在编译时或者类加载时**进行切面的织入，典型的 AspectJ 就是静态代理。



## JDK 动态代理

**JDK 动态代理是基于接口的**，==也就是被代理的类一定要实现了某个接口，否则无法被代理。==

主要实现原理就是：

1. 首先通过实现一个 InvocationHandler 接口得到一个切面类。
2. 然后利用 Proxy 糅合目标类的类加载器、接口和切面类得到一个代理类。
3. 代理类的逻辑就是执行切入逻辑，把所有接口方法的调用转发到 InvocationHandler 的 invoke() 方法上，然后根据反射调用目标类的方法。

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204211945730.png)

我们再深入一点点了解下原理实现。

如果你反编译的话，你能看到生成的代理类是会先在静态块中通过反射把所有方法都拿到存在静态变量中，（我盲写了一下）大致长这样：

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204211945461.png)

上面就是把 getUserInfo 方法缓存了，然后在调用代理类的 getUserInfo 的时候，会调用你之前实现的 InvocationHandler 里面的 invoke，这样就执行到切入的逻辑了，且最终执行了被代理类的  getUserInfo 方法。

就是中间商拦了一道咯，道理就是这个道理。

## CGLIB

如果被代理的类没有实现接口，那么就用 CGLIB 来完成动态代理。

CGLIB ，它是**通过继承的方式来实现代理类**，所以要注意 final 方法，这种方法无法被继承。

简单理解下，就是生成代理类的子类，如何生成呢？

**通过字节码技术动态拼接成一个子类，在其中织入切面的逻辑**。

使用例子：

```java
Enhancer en = new Enhancer();
//2.设置父类，也就是代理目标类，上面提到了它是通过生成子类的方式
en.setSuperclass(target.getClass());
//3.设置回调函数，这个this其实就是代理逻辑实现类，也就是切面，可以理解为JDK 动态代理的handler
en.setCallback(this);
//4.创建代理对象，也就是目标类的子类了。
return en.create();
```

JDK 动态代理和 CGLIB 两者经常还可能被面试官问性能对比，所以咱们也列一下（以下内容取自：haiq的博客）：

> - jdk6 下，在运行次数较少的情况下，jdk动态代理与 cglib 差距不明显，甚至更快一些；而当调用次数增加之后， cglib 表现稍微更快一些 
> - jdk7 下，情况发生了逆转！在运行次数较少（1,000,000）的情况下，jdk动态代理比 cglib 快了差不多30%；而当调用次数增加之后(50,000,000)， 动态代理比 cglib 快了接近1倍
> - jdk8 表现和 jdk7 基本一致

我没试过，有兴趣的同学可以自己实验一下。



## Spring AOP 和 AspectJ 有什么区别

从上面的题目我们已经知道，两者分别是动态代理和静态代理的区别。

Spring AOP 是动态代理，AspectJ 是静态代理。

从一个是运行时织入，一个在编译时织入，我们稍微一想到就能知道，编译时就准备完毕，那么在调用时候没有额外的织入开销，性能更好些。

且 AspectJ 提供完整的 AOP 解决方案，像 Spring AOP 只支持方法级别的织入，而 AspectJ 支持字段、方法、构造函数等等，所以它更加强大，当然也更加复杂。



## 高难面试题

请你谈谈网站是如何进行访问的！

1. 输入一个域名；回车

2. 检查本机的 ==C:\Windows\System32\drivers\etc\hosts==配置文件下有没有这个域名映射；

   1. 有：直接返回对应的ip地址，这个地址中，有我们需要访问的web程序，可以直接访问

      ```java
      127.0.0.1       www.qinjiang.com
      ```

   2. 没有：去DNS服务器找，找到的话就返回，找不到就返回找不到；

   ![1567827057913](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204291007492.png)

   
