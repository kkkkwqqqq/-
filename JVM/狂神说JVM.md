# JVM

## 1.JVM的位置

==**jvm是运行在操作系统之上的**==

jvm是用c写的

![image-20220217021408007](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220217021408007.png)

==**对于笔记本而已硬件体系是最重要的，操作系统随便装，装完可以装jre（包含着jvm），然后java程序放上去运行**==

## 2.JVM体系结构

![image-20220220211111628](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220220211111628.png)

`java栈、本地方法栈、程序计数器不会有垃圾回收，否则程序会死掉`
百分之99的[JVM调优](https://so.csdn.net/so/search?q=JVM调优&spm=1001.2101.3001.7020)都是在方法区和堆（99%是堆）中调优，Java栈、本地方法栈、程序计数器是不会有垃圾存在的。



## 3.类加载器

作用：==加载.class文件==

![image-20220217142456631](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220217142456631.png)

==引用地址在栈里面  真实实例在堆里面 栈的引用指向堆中某个特定实例==

![image-20220217143447299](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220217143447299.png)

>  类是模板，对象是具体的

```
public class Car {
    public static void main(String[] args) {
        //类是模板，对象是具体的
        Car car1 = new Car();
        Car car2 = new Car();
        Car car3 = new Car();
        System.out.println(car1.hashCode());
        System.out.println(car2.hashCode());
        System.out.println(car3.hashCode());

        Class<? extends Car> aClass1 = car1.getClass();
        Class<? extends Car> aClass2 = car2.getClass();
        Class<? extends Car> aClass3 = car3.getClass();

        System.out.println(aClass1.hashCode());
        System.out.println(aClass2.hashCode());
        System.out.println(aClass3.hashCode());
    }
}
```

输出：

![image-20220217143725492](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220217143725492.png)

> 类加载器：

1.启动类(根)加载器

2.扩展类加载器

3应用程序(系统类)加载器

```
public class Car {
    public static void main(String[] args) {
        //类是模板，对象是具体的
        Car car1 = new Car();

        Class<? extends Car> aClass1 = car1.getClass();

        ClassLoader classLoader = aClass1.getClassLoader();
        System.out.println(classLoader);//AppClassLoader
        System.out.println(classLoader.getParent());//ExtClassLoader
        System.out.println(classLoader.getParent().getParent());//null 1. 不存在 2. java程序获取不到
    }
}
```

这个目录下的rt.jar是由系统类加载器加载的 （系统类加载器是c++写的）

![image-20220217144905130](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220217144905130.png)

==**可以改java环境 把类加载器对应的jar改了就行了 也就是调优  大公司都这样干**==

这个ext目录下的jar是由扩展类加载器加载的

![image-20220217145223499](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220217145223499.png)这些jar解压后其实得到的就是一堆.class文件

==**我们自定义的类是由应用加载器加载的**==

### 双亲委派机制：

```
package java.lang;

public class String {
    //双亲委派机制：（保证安全）
    // 1.APP--> EXC--> BOOT
    //BOOT
    //EXC
    //APP
    public String toString(){
        return "hello";
    }

    public static void main(String[] args) {
        String s = new String();
        s.toString();
    }
    //1.类加载器收到类加载的请求
    //2.将这个请求向上委托给父类加载器去完成, 一直向上委托, 直到启动类加载器(根加载器rt)
    //3.启动类加载器检查是否能够加载当前这个类, 能够加载就结束了, 使用当前加载器,否则抛出异常,通知子类加载器进行加载.
    //4.重复步骤3.
    
    //null ： java调用不到~ c，c++
    // java=c++：去掉繁琐的东西，指针，内存管理~
}

```

输出：

![image-20220217145919363](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220217145919363.png)



## 沙箱安全机制

了解~Java历史—沙箱安全机制

参考博客：https://blog.csdn.net/qq_30336433/article/details/83268945



`扩展:`

```java
package com.kuang;

public class Test {
    public static void main(String[] args) {
        new Test().a();
    }
    public void a(){
        b();
    }

    private void b() {
        a();
    }
}
```

![image-20220217153223312](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220217153223312.png)

==当我们将stack的内存设置成合我们的笔记本一样大 那么执行的话 我们的电脑内存瞬间被打满 危险==



## Native

```java
package com.kuang;

public class Demo {
    public static void main(String[] args) {
        new Thread(()->{
        },"my thread name").start();
    }
}

```

```java
public synchronized void start() {

    if (threadStatus != 0)
        throw new IllegalThreadStateException();

   
    group.add(this);

    boolean started = false;
    try {
        start0();
        started = true;
    } finally {
        try {
            if (!started) {
                group.threadStartFailed(this);
            }
        } catch (Throwable ignore) {
            /* do nothing. If start0 threw a Throwable then
              it will be passed up the call stack */
        }
    }
}

private native void start0();
```

`这个start方法调用了本地方法start0；`

`Thread是一个类，类里面的方法居然没有方法体 不报错 这是因为加了native关键字`

```
package com.kuang;

public class Demo {
    public static void main(String[] args) {
        new Thread(()->{
        },"my thread name").start();
    }
    //native：凡是带了native关键字的，说明Java的作用范围已经达不到了，会去调用底层C语言的库
    //会进入本地方法栈
    //调用本地方法通过本地接口JNI
    //JNI(本地方法接口)作用：扩展Java的使用，融合不同编程语言Java所用（最初：C、C++）
    //Java诞生的时候C、C++盛行，想要立足，必须要有调用C、C++的程序
    //它在内存区域中专门准备了一块标记区域:Native Method Stack,登记native 方法
    //在最终执行的时候，加载本地方法库中的方法通过JNI
    private native void hello();
}
```



## 程序计数器-PC寄存器

程序计数器：program counter register

​	每个线程都有一个程序计数器，**是线程私有的**，就是一个指针，指向方法区中的方法字节码（用来存储**指向一条指令的地址**，也即将要执行的指令代码），在执行引擎读取下一条指令，是一个非常小的内存空间，几乎可以忽略不计

`参考博客：`[(31条消息) JVM 程序计数器_leaf_0303的博客-CSDN博客_jvm程序计数器的作用](https://blog.csdn.net/leaf_0303/article/details/78953669)

## 方法区 Method Area

==方法区是被所有线程共享==，所有字段和方法字节码，以及一些特殊方法，如构造函数，接口代码也在此定义，简单来说，所有定义的方法的信息都存在该区域，**此区域属于共享区间**；

​	==**静态变量、常量、类信息（构造方法、接口定义）、运行时的常量池存在方法区中，但是实例存在堆内存中，和方法区无关。**==

> 案例：

```
package com.kuang;

public class Test {
    private int a;
    private String name="kwq";

    public static void main(String[] args) {
        Test test = new Test();
        test.a=1;
        test.name="kwq1";  // 如果没有这个操作 就直接拿常量池中的kwq
    }
}
```

![image-20220217171031608](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220217171031608.png)



## Java栈 Java stack

栈：先进后出、后进先出：类似桶

==栈是运行时的单位==，`Java 虚拟机栈，线程私有，生命周期和线程一致`。描述的是 Java 方法执行的内存模型：每个方法在执行时都会创建一个栈帧(Stack Frame)用于`存储局部变量表`、`操作数栈`、`动态链接`、`方法出口`等信息。每一个方法从调用直至执行结束，就对应着一个栈帧从虚拟机栈中入栈到出栈的过程。

局部变量表：存放了编译期可知的各种基本类型(boolean、byte、char、short、int、float、long、double)、对象引用(reference 类型)和 returnAddress 类型(指向了一条字节码指令的地址)



==**队列：先进先出（FIFO）**==

```java
package com.kuang;

public class Test {

    public static void main(String[] args) {
        Test test = new Test();
        test.test();
    }

    private void test() {
    }
}
```

`程序运行先将main压如java栈中`

![image-20220217172911807](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220217172911807.png)

==**递归导致栈溢出**==

```java
package com.kuang;

public class Test {

    public static void main(String[] args) {
        Test test = new Test();
        test.test();
    }

    private void test() {
        a();
    }

    private void a() {
        test();
    }
}

```

![image-20220217173126324](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220217173126324.png)

![image-20220217173200327](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220217173200327.png)

栈：栈内存，主管程序的运行，声明周期和线程同步

线程结束，栈内存也就释放，对于栈来说，==不存在垃圾回收问题，也就是栈里面不存在垃圾==

栈：8大基本类型，对象引用+实例的方法

栈运行原理：栈帧

![image-20220217193751500](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220217193751500.png)

栈满了：StackOverflowError

栈+堆+方法区：交互关系

![image-20220217193646388](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220217193646388.png)



## 三种JVM

- Sun公司HotSpot Java Hotspot™ 64-Bit Server VM (build 25.181-b13,mixed mode)

![image-20220217234111476](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220217234111476.png)

- BEA JRockit
- IBM J9 VM

`我们学习都是HotSpot`



## 堆

`Heap,一个JVM只有一个堆内存，堆内存的大小是可调节的`

![image-20220218123029087](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218123029087.png)

```java
package com.kuang;

public class Test {

    public static void main(String[] args) {
        Test test = new Test();
        System.out.println(args[0]);
    }


}
```



> 输出：

![image-20220218123053674](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218123053674.png)

类加载器读取了类文件后，一般会把什么东西放到堆中？保存所有引用类型的真实对象

==**栈中的变量，若只有一次，那么用完堆里面的真实对象就属于垃圾了**==

堆内存细分3个区域：

- 新生区（伊甸园区） Young/new
- 养老区 old
- 永久区 Perm

![image-20220218124209210](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218124209210.png)

GC垃圾回收，主要是在伊甸园区和养老区~

```java
package com.kuang;

import java.util.Random;

public class Test {

    public static void main(String[] args) {
        String str="kuangshensayjava";
        while (true){
            str+=str+new Random().nextInt(88888888)+new Random().nextInt(99999999);
        }
    }


}
```

假设内存满了，报错OOM，因为堆内存不够！报错：java.lang.OutOfMemoryError: Java heap space

在JDK8以后，永久存储区改了个名字(`元空间`) 本质并没改变就是换了个名字



### 新生区

- 类的实例：诞生 和成长的地方，甚至死亡；
- 伊甸园 ，所有的对象都是在伊甸园区new出来的  如果GC第一次没有回收掉 就会进入幸存者区
- 幸存者区（0,1）

`补充说明`

假设伊甸园只能存十个对象，幸存区（幸存区12加起来）也只能存十个，那么当伊甸园的对象达到10个就会触发一次轻GC，假如9个被回收了，留下了一个，这一个就会进入到幸存区，如十个都留下来了那么就会占满幸存区，再new十个对象的话就会占满伊甸园区，此时伊甸园和幸存区都满了，就会触发一次重GC，将幸存区和伊甸园区的对象尝试进行回收，若都没回收到则对象进入`养老区`，==**若新生区和养老区都满了**==，此时再有对象进来就会报错，java.lang.OutOfMemoryError: Java heap space;



`真理`

经过研究，99%的对象都是临时对象，用一次就会被回收掉~



### 永久区

这个区域常驻内存，用来存放JDK自身携带的Class对象，Interface元数据，存储的是java运行时的一些环境或类信息，**该区域不存在垃圾回收GC**。关闭虚拟机就会释放这片区域的内存。

一个启动类,加载了大量的第三方jar包。Tomcat部署了太多的应用，大量动态生成的反射类。不断的被加载。直
到内存满，就会出现OOM。

==**堆的结构图：**==

![image-20220218131956095](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218131956095.png)

`逻辑上存在，物理上不存在`

```
package com.kuang;

public class Demo02 {
    public static void main(String[] args) {
        //返回虚拟机试图使用的最大内存
        long max = Runtime.getRuntime().maxMemory();// 字节 1024*1024
        // 返回jvm的初始化总内存
        long total = Runtime.getRuntime().totalMemory();

        System.out.println("max="+max+"字节\t"+(max/(double)1024/1024)+"MB");
        System.out.println("total="+max+"字节\t"+(total/(double)1024/1024)+"MB");



    }
}
```

![image-20220221144950145](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220221144950145.png)

![image-20220218132926295](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218132926295.png)

`默认情况下：分配的总内存 是电脑内存的1/4,而初始化内存为分配总内存的1/64`



==**我们可以调整分配的总内存以及初始化内存**==

![image-20220218143452080](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218143452080.png)

```java
package com.kuang;

public class Demo02 {
    public static void main(String[] args) {
        //返回虚拟机试图使用的最大内存
        long max = Runtime.getRuntime().maxMemory();// 字节 1024*1024
        // 返回jvm的初始化总内存
        long total = Runtime.getRuntime().totalMemory();

        System.out.println("max="+max+"字节\t"+(max/(double)1024/1024)+"MB");
        System.out.println("total="+max+"字节\t"+(total/(double)1024/1024)+"MB");

  //OOM报错解决方案：
        //1.尝试扩大堆内存看结果  如果还报错就是业务代码逻辑的问题了
        //2.分析内存，看一下哪个地方出现了问题（专业工具）、
        // vm options：-Xms1024m -Xmx1024m -XX:+PrintGCDetails  调整分配的内存

    }
}
```

![image-20220218143852223](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218143852223.png)

==**元空间逻辑上存在，物理结构上不存在：**== `不加元空间占的总内存=新生区+老年区`

![image-20220218145308877](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218145308877.png)

```
package com.kuang;

import java.util.Random;
// 配置VM options：-Xms8m -Xmx8m -XX:+PrintGCDetails
public class Test {

    public static void main(String[] args) {
        String str="kuangshensayjava";
        while (true){
            str+=str+new Random().nextInt(88888888)+new Random().nextInt(99999999);
        }
    }


}
```

> 输出：

![image-20220218145007382](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218145007382.png)

![image-20220218145021018](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218145021018.png)

### 在一个项目中，突然出现了OOM故障，那么该如何排除~

扩大内存还报错那就是逻辑问题了

- 能够看到代码第几行出错：内存快照分析工具，MAT（EC集成的），Jprofiler(IDEA集成的)
- Debug，一行行分析代码! 线上项目无法debug

Jprofiler作用：

- 分析Dump内存文件，快速定位内存泄漏；
- 获得堆中的数据
- 获得大的对象（大厂面试）
- …

`安装Jprofiler`

![image-20220218150700877](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218150700877.png)

`百度搜索，JProfiler下载`:下载一个JProfiler监控软件 参考博客：

[(30条消息) JProfiler的安装和使用_vicky_pyh的博客-CSDN博客_jprofiler](https://blog.csdn.net/vicky_pyh/article/details/88797514?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_utm_term~default-0.pc_relevant_default&spm=1001.2101.3001.4242.1&utm_relevant_index=2)

```
package com.kuang;

import java.util.ArrayList;
import java.util.List;

//-Xms1m -Xmx8m -XX:+HeapDumpOnOutOfMemoryError
public class Demo03 {
    byte[] array=new byte[1*1024*1024];
    public static void main(String[] args) {
        List<Demo03> list = new ArrayList<>();
        int count=0;
        try {
            while (true){
                list.add(new Demo03());
                count=count+1;
            }
        } catch (Exception e) {
            System.out.println(count);
            e.printStackTrace();
        } finally {
        }
    }

}
```

![image-20220218155053785](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218155053785.png)

`双击打开 主要看的地方`

![image-20220218155157180](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218155157180.png)

`一般来说先打开Big Objects(大对象)  可以看到是那个对象占据了大部分内存`

![image-20220218155248261](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218155248261.png)

然后打开线程 看具体哪行出错了

![image-20220218155401962](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218155401962.png)

> 修改：

```java
package com.kuang;

import java.util.ArrayList;
import java.util.List;

//-Xms1m -Xmx8m -XX:+HeapDumpOnOutOfMemoryError
public class Demo03 {
    byte[] array=new byte[1*1024*1024];
    public static void main(String[] args) {
        List<Demo03> list = new ArrayList<>();
        int count=0;
        try {
            while (true){
                list.add(new Demo03());
                count=count+1;
            }
        } catch (Error e) {
            System.out.println(count);
            e.printStackTrace();
        } finally {
        }
    }

}
```

![image-20220218155728705](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218155728705.png)

> 说明：

- -Xms 设置初始化内存分配内存  默认笔分配最大内存内存的1/64
- -Xms 设置最大分配内存  默认笔记本内存的1/4
- -XX:+PrintGCDetail   打印GC垃圾回收信息
- -XX:+HeapDumpOnOutOfMemortError   //dump出报错信息文件1



## GC介绍

![image-20220218161204202](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218161204202.png)

JVM在进行GC时，并不是对这三个区域统一回收。大部分时候，回收都是新生代~

- 新生代
- 幸存区(form , to)  
- 老年区

GC两种类：轻GC(普通的GC)，重GC(全局GC)

![image-20220218161522975](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218161522975.png)



![image-20220218161948718](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218161948718.png)





## GC算法：

### 引用计数法：

![image-20220218162310088](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218162310088.png)

在对具体对象的操作时，每操作一次计数+1，垃圾回收机制先把次数少的回收掉。这种算法会占用大量的内存（因为计数），因此JVM不用了已经。

### 复制算法

==**很重要的一个结论**==:`to永远是干净的，空的。`

==**新生区主要是用复制算法**==

![image-20220218174213488](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218174213488.png)

当一个对象经历了15次GC，都还没有死就会进入养老区(默认)

我们可以通过设置参数改变对象进入养老区的GC次数

`-XX:MaxTenuringThreshold=xxx`

![image-20220218174606084](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218174606084.png)

第二次GC后

![image-20220218174246138](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218174246138.png)

- 好处：没有内存的碎片（两个幸存区只有一个有对象，另外一个是空的）
- 坏处：浪费了内存空间（一个幸存区的空间永远是空：to）。假设对象100%存活（极端情况)，那么复杂算法的成本就高了，要复制大量的对象去另外一个幸存区。所以

`复制算法最佳使用场景：对象存活度较低的时候；新生区~`



### 标记清除法

![image-20220218180655942](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218180655942.png)

- 优点：不需要额外的空间！
- 缺点：两次扫描，严重浪费时间，会产生内存碎片。

改良：标记清除再压缩

![image-20220218180909984](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220218180909984.png)

`再改进：先标记清除几次之后，再压缩1次。` 



## 总结：

- 内存效率：复制算法>标记清除算法>标记压缩算法(时间复杂度)
- 内存整齐度:复制算法=标记压缩算法>标记清除算法
- 内存利用率:标记压缩算法=标记清除算法>复制算法

思考一个问题：难道没有最优算法吗?
答案：没有，没有最好的算法，只有最合适的算法 —> GC：分代收集算法

**年轻代：**

- 存活率低
  复制算法!

**老年代:**

- 区域大：存活率
  标记清除 (内存碎片不是太多) + 标记压缩混合实现

==**一天时间学JVM，不现实，要深究，必须要下去花时间，和多看面试题，以及《深入理解JVM》
但是，我们可以掌握一个学习JVM的方法：先整体再局部**==



## JMM

JMM ：（java Memory Model 缩写）java内存模型

他是干什么的？

作用：缓存一致性协议，用于定义数据读写的规则；
JMM定义了线程工作内容与主内存之间的抽象关系：线程之间的共享变量存储在主内存（Main Memory）中，每个线程都有一个私有的本地内存（Local Memory）

![image-20220219134128410](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220219134128410.png)

volatile关键字
解决共享对象可见性问题：volilate
两点作用：
1.保证线程间变量的可见性。
2.禁止CPU进行指令重排序。
可见性
volatile修饰的变量，当一个线程改变了该变量的值，其他线程是立即可见的。普通变量则需要重新读取才能获得最新值。
volatile保证可见性的流程大概就是这个一个过程：

------------------------------------------------
![image-20220219134158930](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220219134158930.png)

关于主内存与工作内存之间的具体交互协议，即一个变量如何从主内存拷贝到工作内存、如何从工作内存同步到主内存之间的实现细节，Java内存模型定义了以下八种操作来完成：

- lock （锁定）：作用于主内存的变量，把一个变量标识为线程独占状态
- unlock （解锁）：作用于主内存的变量，它把一个处于锁定状态的变量释放出来，释放后的变量才可以被其他线程锁定
- read （读取）：作用于主内存变量，它把一个变量的值从主内存传输到线程的工作内存中，以便随后的load动作使用
- load （载入）：作用于工作内存的变量，它把read操作从主存中变量放入工作内存中
- use （使用）：作用于工作内存中的变量，它把工作内存中的变量传输给执行引擎，每当虚拟机遇到一个需要使用到变量的值，就会使用到这个指令
- assign （赋值）：作用于工作内存中的变量，它把一个从执行引擎中接受到的值放入工作内存的变量副本中
- store （存储）：作用于主内存中的变量，它把一个从工作内存中一个变量的值传送到主内存中，以便后续的write使用
- write （写入）：作用于主内存中的变量，它把store操作从工作内存中得到的变量的值放入主内存的变量中

**JMM对这八种指令的使用，制定了如下规则：**

- 不允许read和load、store和write操作之一单独出现。即使用了read必须load，使用了store必须write
- 不允许线程丢弃他最近的assign操作，即工作变量的数据改变了之后，必须告知主存
- 不允许一个线程将没有assign的数据从工作内存同步回主内存
- 一个新的变量必须在主内存中诞生，不允许工作内存直接使用一个未被初始化的变量。就是对变量实施use、store操作之前，必须经过assign和load操作
- 一个变量同一时间只有一个线程能对其进行lock。多次lock后，必须执行相同次数的unlock才能解锁
- 如果对一个变量进行lock操作，会清空所有工作内存中此变量的值，在执行引擎使用这个变量前，必须重新load或assign操作初始化变量的值
- 如果一个变量没有被lock，就不能对其进行unlock操作。也不能unlock一个被其他线程锁住的变量对一个变量进行unlock操作之前，必须把此变量同步回主内存 



> 学习三部曲

学习新东西是常态：

==**如何学？**==

1.什么是JMM（`概念，定义，一般看官网或者百度百科`）

2.它是干嘛的？  了解途径：`官方`，`其他人的博客(看看大部分人的理解)，多看几篇，共同点即是重点`，对应的视频！

3.它该如何学习？   `从博客中了解应该学习这个知识点的哪些内容，也就是多篇博客的共同点，即重点`

面试：

3/10=pass，面经=10，分析这10个==》触类旁通：百度面试题

通过了大量的面试题总结，得出一套解题思路
