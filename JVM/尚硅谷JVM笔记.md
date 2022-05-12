# 尚硅谷JVM笔记

## 类的加载器以及加载过程

### 类加载器子系统的作用

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639638347062-af6e0d00-d833-4ba4-ac36-4135ce4b7b69.png)

### 类加载器ClassLoader角色

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639638615447-7b149619-57f7-4b1e-ac87-febf83a4ab6b.png)

### 类的加载过程

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639638840001-ed8adf37-fc0a-48f3-9412-bcf0f936b25d.png)

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639638883542-08fd7504-f615-4635-b78c-1f2f8ae8d54d.png)

### 类的加载过程一：loading

`加载`：

1．通过一个类的全限定名获取定义此类的二进制字节流

2．将这个字节流所代表的`静态存储结构转化为方法区的运行时数据结构`

3.`在内存中生成一个代表这个类的java .lang.Class对象，作为方法区这个类的各种数据的访问入口`



补充：加载.class文件的方式

- 从本地系统中直接加载
- 通过网络获取，典型场景：Web Applet

- 从zip压缩包中读取，，成为日后jar、war格式的基础
- `运行时计算生成，使用最多的是：动态代理技术`

- 由其他文件生成，典型场景：：JSP应用
- 从专有数据库中提取.class文件，比较少见

- 从加密文件中获收,典型的防Class文件被反编译的保护措施



### 类的加载过程二：Linking

**验证(Verify):**

- `目的在于确保class文件的字节流中包含信息符合当前虚拟机要求，保证被加载类的正确性，不会危害虚拟机自身安全。`
- 主要包括四种验证，文件格式验证，元数据验证，字节码验证，符号引用验证。

- 下面是两个字节码文件，发现开头规范都是符合虚拟机的规范

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639642925009-81492ca6-5721-4406-936e-b41f3691b119.png)

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639642999247-48262108-b9ad-46c7-8325-b7e32d26bb03.png)

**准备(Prepare):**

- 为类变量分配内存并且设置该类变量的`默认初始值，即零值`。
- 这里不包含用`final修饰的static，因为final在编译的时候就会分配了`，准备阶段会显式初始化；

- `这里不会为实例变量分配初始化，类变量会分配在方法区中，而实例变量是会随着对象一起分配到Java堆中`。

**解析(Resolve)：**

- 将常量池内的符号引用转换为直接引用的过程。
- 事实上，解析操作往往会伴随着JVM在执行完初始化之后再执行。

- `符号引用就是一组符号来描述所引用的目标。符号引用的字面量形式明确定义在《java虚拟机规范》的Class文件格式中。直接引用就是直接指向目标的指针、相对偏移量或一个间接定位到目标的句柄。`

- 解析动作主要针对类或接口、字段、类方法、接口方法、方法类型等。对应常量池中的CONSTANT Class info、CONSTANT Fieldref info、CONSTANT Methodref info等



进行反编译

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639643235952-cd2d7b0b-d39b-4068-ba00-6e9d6430c16d.png)

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639643264149-9afd51fd-e1ab-46d7-9f49-e141eda6c65a.png)

`上图左边是符号引用。会去加载这些类`



### 类的加载过程三:Initialization

**初始化：**

- 初始化阶段就是执行类构造器方法<clinit>()的过程。
- 此方法不需定义，`是javac编译器自动收集类中的所有类变量的赋值动作和静态代码块中的语句合并而来。`

- `构造器方法中指令按语句在源文件中出现的顺序执行。`

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639654661902-9ed5ecd7-ddfd-4abf-a86f-1408e479c8df.png)

==PS：因为存在类变量（静态方法也会）所以会生成clinit方法。==

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639654769351-fb04dd52-3eec-4379-bbee-31026d4955db.png)

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639654778235-72f2219f-717d-40f5-8cfe-4caa3fad51b0.png)

若没有静态代码块或者类变量，则不会生成clinit方法，如下图所示

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639654825675-60cc3ded-0770-4d30-84f5-88a9ce30a187.png)

- <clinit>()不同于类的构造器。(关联：构造器是虚拟机视角下的<init>())若该类具有父类，JVM会保证子类的<clinit>()执行前，父类的<clinit>()已经执行完毕。

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639655049051-b83d9980-1ca6-4c2f-9489-7e35fa2bdd0a.png)

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639655070495-5857a6be-dbb8-4b81-8de9-829adbdfab0f.png)

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639655482064-78d0c2b2-0b4b-434f-ad9e-fb36a7fd0f48.png)



- `虚拟机必须保证一个类的<clinit>()方法在多线程下被同步加锁。`

```java
package com.kwq.java;

public class DeadThreadTest {
    public static void main(String[] args) {
        Runnable r=()->{
            System.out.println(Thread.currentThread().getName()+"开始");
            DeadThread deadThread = new DeadThread();
            System.out.println(Thread.currentThread().getName()+"结束");
        };

        Thread t1 = new Thread(r, "线程1");
        Thread t2 = new Thread(r, "线程2");
        t1.start();
        t2.start();
    }
}

class DeadThread{
    static {
        if (true){
            System.out.println(Thread.currentThread().getName()+"初始化当前类");
          while (true){

          }
        }
    }
}
```

输出

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639655902116-9ef16fd9-8f15-4a3c-a825-9596c62bed2d.png)

## 类加载器分类

### 类加载器分类

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639658677626-78290e8f-fe44-4aa7-a5ab-c0b9ee6aa3f1.png)



- JVM支持两种类型的类加载器，`分别为引导类加载器（BooClassLoader）和自定义类加载器（User-Defined ClassLoader).`



- 从概念上来讲，自定义类加载器一般指的是程序中由开发人员自定义的一类类加载器，但是Java虚拟机规范却没有这么定义，而是将所有派生于抽象类ClassLoader的类加载器都划分为自定义类加载器。



- 无论类加载器的类型如何划分，在程序中我们最常见的类加载器始终只有3个，如下所示：

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639659037113-2bf7ca64-2c99-465b-a031-10b731f5aee8.png)

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639659105133-b0b59097-337c-49c3-90a9-1be34e94e13f.png)

关系图

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639659139064-dbb667af-0c5a-4531-a7a3-a3b8429b69fd.png)

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639659224068-13846a51-56ac-43cb-8398-0fd9831a5293.png)

关系图

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639659270387-a6e62066-e92f-468b-8faf-e8bc792236d1.png)

代码实现：

```java
package com.kwq.java1;

public class ClassLoaderTest {
    public static void main(String[] args) {
        //获取系统类加载器
        ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
        System.out.println(systemClassLoader);//sun.misc.Launcher$AppClassLoader@18b4aac2
        //获取其上层：扩展类加载器
        ClassLoader extClassLoader = systemClassLoader.getParent();
        System.out.println(extClassLoader);//sun.misc.Launcher$ExtClassLoader@1b6d3586
        //获取其上层 获取不到引导类加载器
        ClassLoader bootstarpClassLoader = extClassLoader.getParent();
        System.out.println(bootstarpClassLoader);//null
        //对于用户自定义类来说:默认使用系统类加载器进行加载
        ClassLoader classLoader = ClassLoaderTest.class.getClassLoader();
        System.out.println(classLoader);//sun.misc.Launcher$AppClassLoader@18b4aac2

        //String类使用引导类加载器进行加载的 java的核心类库都是使用引导类加载器进行加载的
        ClassLoader classLoader1 = String.class.getClassLoader();
        System.out.println(classLoader1);//null
    }
}
```

### 引导类、扩展类、系统类加载器的使用及演示

`虚拟机自带的加载器`

启动类加载器（引导类加载器，Bootstrap ClassLoader）

1. 这个类加载使用c/C++语言实现的，，嵌套在JVM内部。
2. 它用来加载Java的核心库（JAVA_HOME/jre/lib/rt.jar、resources.jar或sun.boot.class.path路径下的内容）,用于提供JVM自身需要的类

1. 并不继承自java .lang.ClassLoader，没有父加载器。
2. 加载扩展类和应用程序类加载器，并指定为他们的父类加载器。

1. 出于安全考虑，Bootstrap启动类加载器只加载包名为java、javax、sun等开头的类

```java
package com.kwq.java1;

import sun.misc.Launcher;

import java.net.URL;

public class ClassLoaderTest1 {
    public static void main(String[] args) {
        System.out.println("*********启动器加载类*********");
        //获取BootstrapClassLoader能够加载的api路径
        URL[] urLs = Launcher.getBootstrapClassPath().getURLs();
        for (URL element : urLs) {
            System.out.println(element.toExternalForm());
        }
    }
}
```

输出

```java
*********启动器加载类*********
file:/C:/Program%20Files/Java/jdk1.8.0_301/jre/lib/resources.jar
file:/C:/Program%20Files/Java/jdk1.8.0_301/jre/lib/rt.jar
file:/C:/Program%20Files/Java/jdk1.8.0_301/jre/lib/sunrsasign.jar
file:/C:/Program%20Files/Java/jdk1.8.0_301/jre/lib/jsse.jar
file:/C:/Program%20Files/Java/jdk1.8.0_301/jre/lib/jce.jar
file:/C:/Program%20Files/Java/jdk1.8.0_301/jre/lib/charsets.jar
file:/C:/Program%20Files/Java/jdk1.8.0_301/jre/lib/jfr.jar
file:/C:/Program%20Files/Java/jdk1.8.0_301/jre/classes
```

对应磁盘中的位置

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639660255976-b7978d0e-fc34-4294-b7b0-d40306ced47d.png)

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639660563110-c273ad1f-d069-482d-bda9-a2d08fb60208.png)

修改代码

```java
public class ClassLoaderTest1 {
    public static void main(String[] args) {
        System.out.println("*********启动器加载类*********");
        //获取BootstrapClassLoader能够加载的api路径
        URL[] urLs = Launcher.getBootstrapClassPath().getURLs();
        for (URL element : urLs) {
            System.out.println(element.toExternalForm());
        }
        //从上面的路径中随意选择一个类，来看看他的加载器是什么
        ClassLoader classLoader = Provider.class.getClassLoader();
        System.out.println(classLoader);//null
    }
}
```

`扩展类加载器（Extension ClassLoader)`

1. Java语言编写，由sun.misc.Launcher$ExtClassLoader实现。
2. 派生于ClassLoader类

1.  父类加载器为启动类加载器
2. `从系统属性所指定目录中加载类库，或从JDK的安装目录的jre/lib/ext子目录（扩展目录）下加载类库。如果用户创建的JAR放在此目录下，也会自动由扩展类加载器加载。`



增加代码用来测试扩展类加载器

```java
 System.out.println("******扩展类加载器******");
        String extDirs = System.getProperty("java.ext.dirs");
        for (String path : extDirs.split(";")) {
            System.out.println(path);
        }
```

输出

```java
******扩展类加载器******
C:\Program Files\Java\jdk1.8.0_301\jre\lib\ext
C:\WINDOWS\Sun\Java\lib\ext
```

测试

```java
package com.kwq.java1;

import sun.misc.Launcher;
import sun.security.ec.ECKeyFactory;


import java.net.URL;
import java.security.Provider;

public class ClassLoaderTest1 {
    public static void main(String[] args) {
        System.out.println("*********启动器加载类*********");
        //获取BootstrapClassLoader能够加载的api路径
        URL[] urLs = Launcher.getBootstrapClassPath().getURLs();
        for (URL element : urLs) {
            System.out.println(element.toExternalForm());
        }
        //从上面的路径中随意选择一个类，来看看他的加载器是什么
        ClassLoader classLoader = Provider.class.getClassLoader();
        System.out.println(classLoader);//null

        System.out.println("******扩展类加载器******");
        String extDirs = System.getProperty("java.ext.dirs");
        for (String path : extDirs.split(";")) {
            System.out.println(path);
        //    C:\Program Files\Java\jdk1.8.0_301\jre\lib\ext
		//	C:\WINDOWS\Sun\Java\lib\ext  系统所指定的目录
        }
        //从上面的路径随意选择一个类，来看看她的泪加载器是什么：扩展类加载器
        ClassLoader classLoader1 = ECKeyFactory.class.getClassLoader();
        System.out.println(classLoader1);//sun.misc.Launcher$ExtClassLoader@1b6d3586
    }
}
```

### 为什么需要用户自定义类加载器及具体实现

`用户自定义类加载器`

- 在Java的日常应用程序开发中，类的加载几乎是由上述3种类加载器相互
  配合执行的，在必要时，我们还可以自定义类加载器，来定制类的加载方式。
- 为什么要自定义类加载器？

```java
隔离加载类
修改类加载的方式
扩展加载源
防止源码泄漏
```

用户自定义类加载器实现步骤：

1．开发人员可以通过继承抽象类java.lang.ClassLoader类的方式，实现自己的类加载器，以满足一些特殊的需求

2．`在JDK1.2之前，在自定义类加载器时，总会去继承ClassLoader类并重写loadClass()方法，从而实现自定义的类加载类，但是在JDK1.2之后已不再建议用户去覆盖loadClass()方法，而是建议把自定义的类加载逻辑写在findClass()方法中`

3.在编写自定义类加载器时，如果没有太过于复杂的需求，可以直接继承URLClassLoader类，这样就可以避免自己去编写findClass()方法及其获取字节码流的方式，使自定义类加载器编写更加简洁。



具体代码

```java
package com.kwq.java1;

import java.io.FileNotFoundException;

//用户自定义类加载器
public class CustomClassLoader extends ClassLoader{
    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
            try {
                byte[] result = getClassFromCustomerPath(name);
                if (result == null) {
                    throw new FileNotFoundException();
                } else {
                    return defineClass(name,result,0,result.length);
                }
            }catch (FileNotFoundException e) {
                e.printStackTrace();
            }
            throw new ClassNotFoundException();
        }


    private byte[] getClassFromCustomerPath(String name){
        //从自定义路径加载指定类：细节略
        //如果指定路径的字节码文件进行了加密，则需要在此方法中进行解密操作
        return null;
    }

    public static void main(String[] args) {
        CustomClassLoader customClassLoader = new CustomClassLoader();

        try {
            Class<?> clazz = Class.forName("One",true,customClassLoader);
            Object obj = clazz.newInstance();
            System.out.println(obj.getClass().getClassLoader());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    }
```

### ClassLoader的常用方法及获取方法

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639664902576-4cb8acac-6d7f-4524-a17c-778c97c04bfb.png)

上面两个圈起来的方法是配套使用的

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639665210014-72ba9113-e883-4b54-be4a-a586a64af970.png)

获取ClassLoader的方法

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639665265616-50084c8b-4bd7-49c1-b6ca-26739d4c1dae.png)

```java
package com.kwq.java1;

public class ClassLoaderTest2 {
    public static void main(String[] args) {
        try {
            //1.
            ClassLoader classLoader = Class.forName("java.lang.String").getClassLoader();
            System.out.println(classLoader);//null
            //2.
            ClassLoader classLoader1 = Thread.currentThread().getContextClassLoader();
            System.out.println(classLoader1);//sun.misc.Launcher$AppClassLoader@18b4aac2

            //3.
            ClassLoader classLoader2 = ClassLoader.getSystemClassLoader().getParent();
            System.out.println(classLoader2);//sun.misc.Launcher$ExtClassLoader@1b6d3586

        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

## JVM机制

### 双亲委派机制

**Java虚拟机对class文件采用的是按需加载的方式，也就是说当需要使用该类时才会将它的class文件加载到内存生成class对象。而且加载某个类的class文件时，Java虚拟机采用的是双亲委派模式，即把请求交由父类处理，它是一种任务委派模式。**



**双亲委派机制工作原理**

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639666960970-ebbab779-b273-4baf-af6e-76f752960963.png)



编写测试代码

在java.lang包下新建String类

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639666839297-1306aef5-a778-4ded-899e-c5264df25e9b.png)

> 解释说明：

Sring里面有main方法，因此会去加载String类，通过双亲委派机制，让引导类加载器去加载类，发现java核心类库的java.lang有String这个类，就直接加载了java核心类库里的类，然而核心类库中的String没有main方法，因此上图注释写的错误！！！

```java
package com.kwq.java1;

public class StringTest {

    public static void main(String[] args) {
       java.lang.String str = new java.lang.String();
        System.out.println("hello,kwq.com");

        StringTest test = new StringTest();
        System.out.println(test.getClass().getClassLoader());
    }
}
```

输出

```java
hello,kwq.com
sun.misc.Launcher$AppClassLoader@18b4aac2
```

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639703321691-f6260fb5-00dc-44c4-b5a0-17a40b1a1a48.png)

双亲委派机制优势：

1.  避免类的重复加载（向上委托的时候，只要父类加载了，子类不再加载）
2. 保护程序安全，防止核心API被随意篡改（怕自定义类有恶意代码）

`自定义类：java.lang.String`

`自定义类：java.lang.kwq`

java开头的包都是引用加载器去加载的

下图直接不给运行，就算运行也会报错：Prohibited package name:java.lang

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639703728757-13a13695-ea78-441c-b2b2-4b91a63eb64d.png)

### 沙箱安全机制

自定义String类，但是在加载自定义String类的时候会率先使用引导类加载器加载，而引导类加载器在加载的过程中会先加载jdk自带的文件(rt.jar包中java\lang\String.class)，报错信息说没有main方法,就是因为加载的是rt.jar包中的String类。这样可以保证对java核心源代码的保护，这就是沙箱安全机制。

## 类的主动使用与被动使用

`在JVM中表示两个Class对象是否为同一个类存在两个必要条件：`

- - - 类的完整类名必须一致，包括包名。
    -  加载这个类的ClassLoader(指ClassLoader实例对象)必须相同。

`换句话说，在JVM中，即使这两个类对象(class对象)来源同一个Class文件，被同一个虚拟机所加载，但只要加载它们的ClassLoader实例对象不同，那么这两个类对象也是不相等的。`

==**对类加载器的引用**==

JVM必须知道一个类型是由启动加载器加载的还是由用户类加载器加载的。如果一个类型是由用户类加载器加载的，那么JVM会将这这个类加载器的一个引用作为类型信息的一部分保存在方法区中。当解析一个类型到另一个类型的引用的时候，JVM需要保证这两个类型的类加载器是相同的。

`类的主动使用与被动使用`

Java程序对类的使用方式分为：：`主动使用和被动使用。`

主动使用，又分为七种情况：

-  创建类的实例
- 访问某个类或接口的静态变量，或者对该静态变量赋值

- 调用类的静态方法
- 反射（比如：Class.forName(":com.atguigu.Test"））

-  初始化一个类的子类
- Java虚拟机启动时被标明为启动类的类

- java.lang.invoke.MethodHandle实例的解析结果， REF_getStatic、REF_putStatic、REF_invokeStatic句柄对应的类没有初始化，则初始化

==**除了以上七种情况，其他使用Java类的方式都被看作是对类的被动使用，都不会导致类的初始化。**==



## 运行时数据区及线程

==**运行时数据区内部结构**==

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639707529642-c54622ef-7f97-43af-8ad8-249a3e394e2c.png)

食材相当于运行时数据区，厨师相当于执行引擎

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639707606698-d3f54f4d-2f35-47df-9f25-3e9e4c1a7298.png)

==**内存的介绍**==

内存是非常重要的系统资源，是硬盘和CPU的中间仓库及桥梁，承载着操作系统和应用程序的实时运行。`JVM内存布局规定了Java在运行过程中内存申请、分配、管理的策略，，保证了JVM的高效稳定运行。不同的JVM对于内存的划分方式和管理机制存在着部分差异。结合JVM虚拟机规范，来探讨一下经典的JVM内存布局。`

![img](https://cdn.nlark.com/yuque/0/2021/jpeg/25623364/1639708014989-61d5cb9a-5f7f-4aeb-adfd-16acd6ee89ca.jpeg)

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639708108481-65bcb6a1-60f1-4b3a-9325-d6e3fbea0470.png)

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639708197126-b370c3ce-6bc0-4dd1-8f6a-e0efedc73812.png)

关于线程间共享的说明

![img](https://cdn.nlark.com/yuque/0/2021/png/25623364/1639708250037-281d92fe-9c24-4fda-ba63-79e79cac9b6c.png)

JVM中线程的说明：

- 线程是一个程序里的运行单元。JVM运行一个应用有多个线程并行的执行。
- 在Hotspot JVM里，每个线程都与操作系统的本地线程直接映射。

- - -  当一个Java线程准备好执行以后，此时一个操作系统的本地线程也同时创建。Java线程执行终止后，本地线程也会回收。

- ==**操作系统负责所有线程的安排调度到任何一个可用的CPU上。一旦本地线程初始化成功，，它就会调用Java线程中的run()方法。**==



JVM系统线程：

- 如果你使用jconsole或者是任何一个调试工具，都能看到在后台有许多线程在运行。这些后台线程不包括调用public static void main(String[])的main线程以及所有这个main线程自己创建的线程。
- 这些主要的后台系统线程在Hotspot JVM里主要是以下几个：

**虚拟机线程**：`这种线程的操作是需要JVM达到安全点才会出现。这些操作必须在不同的线程中发生的原因是他们都需要JVM达到安全点，这样堆才不会变化。这种线程的执行类型包括"stop-the-world"的垃圾收集，线程栈收集，线程挂起以及
偏向锁撤销。`

**周期任务线程**：这种线程是时间周期事件的体现（比如中断），他们一般用于周期性
操作的调度执行。

**GC线程**：这种线程对在JVM里不同种类的垃圾收集行为提供了支持。

**编译线程**：这种线程在运行时会将字节码编译成到本地代码。

**信号调度线程**：这种线程接收信号并发送给JVM，在它内部通过调用适当的方法进
行处理。



## 程序计数器-PC寄存器

`官网对PC寄存器的介绍`

> JDK8的jvm规范  [The Java® Virtual Machine Specification (oracle.com)](https://docs.oracle.com/javase/specs/jvms/se8/html/)

![image-20220221231433688](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220221231433688.png)`点开上图所圈`

![image-20220221231508454](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220221231508454.png)

- 它是一块很小的内存空间，==几乎可以忽略不记==,也是运行速度最快的存
  储区域。
- 在JVM规范中，每个线程都有它自己的程序计数器，==是线程私有的，生命
  周期与线程的生命周期保持一致。==
- 任何时间一个线程都只有一个方法在执行，也就是所谓的==当前方法==。程序计数器会存储当前线程正在执行的Java方法的==JVM指令地址==；或者，==如果是在执行native方法，则是未指定值（undefned）==

![image-20220221232655443](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220221232655443.png)

​	JVM中的程序计数寄存器（Program Couhter Rfgister）中，
Register 的命名源于
CPU的寄存器，寄存器存储指令相关的现场信息。 CPU只有把数据装载到寄存器才能够运行。

​	这里，并非是广义上所指的物理寄存器，或许将其翻译为PC计数器（或指令计数器）会更加贴
切（也称为程序钩子)，并且也不容易引起一些不必要的误会。==JVM中的PC寄存器是对物理PC
寄存器的一种抽象模拟。==

![image-20220221233108598](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220221233108598.png)

`PC寄存器的作用`

寄存器用来存储指向下一
条指令的地址，也即将要执行的指令代码。由执行引擎读取下条指令。



> PC寄存器的使用案例：

```
package com.sgg;

public class PCRegisterTest {
    public static void main(String[] args) {
        int i=10;
        int j=20;
        int k=i+j;
    }
}
```

反编译：`javap -v 类名.class`

![image-20220221234834176](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220221234834176.png)

==**具体说明：**==

![image-20220221234536610](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220221234536610.png)

![image-20220221234718190](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220221234718190.png)



![image-20220221235503436](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220221235503436.png)

==往上翻：==

![image-20220222000043355](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222000043355.png)

![image-20220222000124259](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222000124259.png)

`完整操作:`

![image-20220222000404474](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222000404474.png)

`说明：`

==**main方法压栈，pc寄存器存储某一条指令地址，执行引擎拿到PC寄存器存储的指令地址，做出相对应的操作指令（先翻译成机器指令，然后让CPU帮我们去计算操作），从而操作局部变量、操作数栈。**==



### 两个常见的PC寄存器问题

**问题一：使用PC寄存器存储字节码指令地址有什么用呢？**等价于问题：**为什么使用PC寄存器记录当前线程的执行地址呢？**

- 因为CPU需要不停的切换各个线程，这时候切换回来以后，
  就得知道接着从哪开始继续执行。
- JVM的字节码解释器就需要通过改变PC寄存器的值来明确下一条应该执行什么样的字节码指令。

![image-20220222002202557](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222002202557.png)**问题二：PC寄存器为什么会被设定为线程私有?**

 	我们都知道所谓的多线程在一个特定的时间段内只会执行其中某一个线程的方法，CPU
会不停地做任务切换，这样必然导致经常中断或恢复，如何保证分毫无差呢?==**为了能够准确地记录各个线程正在执行的当前码指令地址,设好的办然都分配一个PC寄存器**==，这样一来各个线程之间便可以进行独立计算,从而不会出现相互干扰的情况。

​	由于CPU时间片限制，众多线程在并发执行过程中，任何一个确定的时刻,一个处理器或者多核处理器中的一个内核，只会执行某个线程中的一条指令。

​	这样必然导致经常中断或恢复，如何保证分毫无差呢？每个线程在创建后，都会产生自己的程序计数器和栈帧，程序计数器在各个线程之间互不影响。

> CPU时间片

![image-20220222002438030](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222002438030.png)



> 并发和并行

并发：只有一个CPU，线程轮流执行，切换速度和执行速度很快，宏观上感觉是并行的

并行：多个CPU，使多个线程同时运行



## 虚拟机栈-java栈

`虚拟机栈也叫作java栈`

虚拟机规范的官方概述：

![image-20220222150321380](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222150321380.png)

`由于跨平台性的设计，Java的指令都是根据栈来设计的。不同平台CPU架构不同，所以不能设计为基于寄存器的。优点栈是容易实现跨平台，指令集小，编译器容易实现，缺点是性能下降
，实现同样的功能需要更多的指令。`

![image-20220222152228697](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222152228697.png)

- **Java虚拟机栈是什么？**

   Java虚拟机栈（Java Virtual Machine Stack），早期也叫Java栈。
  每个线程在创建时都会创建一个虚拟机栈，其内部保存一个个的栈帧
  （Stack Frame），对应着一次次的Java方法调用。

  - 是线程私有的

- **生命周期：**

​		生命周期和线程一致。

- **作用：**

​		主管Java程序的运行，它保存方法的局部变量（基本数据类型直接存在栈中，引用类型保留其真实对象地址）、部分结果，并参与方法的调用和返回。

​		局部变量 VS 成员变量（或属性）

​		基本数据变量 VS 引用类型变量（类、数组、接口）

```
package com.sgg.VMStack;

public class StackTest {
    public static void main(String[] args) {
        StackTest test = new StackTest();
        test.methodA();
    }

    private void methodA() {
        int i=10;
        int j=20;
        methodB();
    }

    private void methodB() {
        int k=30;
        int m=40;
    }

}
```

![image-20220222153411561](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222153411561.png)

![image-20220222153615845](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222153615845.png)

`栈不存在GC，但存在OOM和栈溢出`



### 虚拟机栈的常见异常

> 栈溢出：报错java.lang.StackOverflowError

```
package com.sgg.VMStack;

public class StackErrorTest {
    public static void main(String[] args) {
        main(args);
    }
}
```

### 如何设置栈的大小

==**Java
虚拟机规范允许Java栈的大小是`动态`的或者是`固定不变`的。**==

- 如果采用固定大小的Java虚拟机栈，那每一个线程的Java虚拟机栈容量
  可以在线程创建的时候独立选定。如果线程请求分配的栈容量超过Java
  虚拟机栈允许的最大容量，Java虚拟机将会抛出一个
  ==stackOverflowError== 异常。
- 如果Java虚拟机栈可以动态扩展，并且在尝试扩展的时候无法申请到足
  够的内存，或者在创建新的线程时没有足够的内存去创建对应的虚拟机栈，
  那Java虚拟机将会抛出一个
  ==OutOfMemoryError== 异常。

> 测试：

```
package com.sgg.VMStack;

public class StackErrorTest {
    private static int count=1;
    public static void main(String[] args) {
        System.out.println(count);
        count++;
        main(args);
    }
}
```

> 输出：

![image-20220222155650996](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222155650996.png)

==**我们可以使用参数-XSS 选项来设置线程的最大栈空间，栈的大小决定了函数调用的最大可达深度。**==

设置栈的大小为：256k 指令`-Xss 256`

> 官方文档：

![image-20220222161501596](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222161501596.png)

![image-20220222155809714](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222155809714.png)

> 输出:

![image-20220222155848317](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222155848317.png)



### 栈的存储单位---栈帧

==**栈中存储什么?**==

- 每个线程都有自己的栈，栈中的数据都是以==栈帧（Stack Frame）的格式存在==。
- 在这个线程上正在执行的每个方法都各自对应一个栈帧（Stack Frame）
- 栈帧是一个内存区块，是一个数据集，维系着方法执行过程中的各种数据信息。

==**栈的运行原理：**==

- JVM直接对Java栈的操作只有两个，就是对栈帧的==压栈和出栈==，==遵循“先进
  后出”/“后进先出”原则==。
- 在一条活动线程中，
  一个时间点上，只会有一个活动的栈帧。即只有当前正在执行的方法的栈帧（栈顶栈帧）是有效的，这个栈帧被称为==当前栈帧
  （Current Frame）==，与当前栈帧相对应的方法就是==当前方法
  (Current
  Method）==，定义这个方法的类就是==当前类（Current
  Class)== 。
- 执行引擎运行的字节码指令只针对当前栈帧进行操作。
- 如果在该方法中调用了其他方法，对应的新的栈帧会被创建出来，放在栈的顶端，成为新的当前帧。

![image-20220222174155185](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222174155185.png)

> 补充说明：

- 不同线程中所包含的栈帧是不允许存在相互引用的，即不可能在一个栈帧之中引用另外一个线程的栈帧。
- 如果当前方法调用了其他方法，方法返回之际，当前栈帧会传回此方法的执行结果给前一个栈帧，接着，虚拟机会丢弃当前栈帧，使得前一个栈帧重新成为当前栈帧。
- Java方法有两种返回函数的方式，==一种是正常的函数返回，使用return指令；外一种是抛出异常。不管使用哪种方式，都会导致栈帧被弹出==

```
package com.sgg.VMStack;

public class StackFrameTest {
    public static void main(String[] args) {
        StackFrameTest test = new StackFrameTest();
        test.method1();

    }

    public void method1(){
        System.out.println("method1()开始执行");
        method2();
        System.out.println("method1()执行结束");
    }

    public int method2() {
        System.out.println("method2()开始执行");
        int i=10;
        int v =(int) method3();
        System.out.println("method2()即将结束");
        return i+v;
    }

    public double method3() {
        System.out.println("method3()开始执行");
        double j=20.0;
        System.out.println("method3()即将执行结束");
        return j;
    }
}
```

![image-20220222172728840](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222172728840.png)

```
package com.sgg.VMStack;
/*
* 方法的结束方式分为两种：① 正常结束 以return为代表
* ② 方法执行中出现未捕获处理的异常，以抛出异常的方式结束
* */
public class StackFrameTest {
    public static void main(String[] args) {
        try {
            StackFrameTest test = new StackFrameTest();
            test.method1();
           
        } catch (Exception e) {
            e.printStackTrace();
        }
        System.out.println("main方法执行结束");
    }

    public void method1(){
        System.out.println("method1()开始执行");
        method2();
        System.out.println("method1()执行结束");
        System.out.println(10 / 0);
    }

    private int method2() {
        System.out.println("method2()开始执行");
        int i=10;
        int v =(int) method3();
        System.out.println("method2()即将结束");
        return i+v;
    }

    private double method3() {
        System.out.println("method3()开始执行");
        double j=20.0;
        System.out.println("method3()即将执行结束");
        return j;
    }
}
```

> 输出：

![image-20220222175454068](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222175454068.png)

```java
package com.sgg.VMStack;
/*
* 方法的结束方式分为两种：① 正常结束 以return为代表
* ② 方法执行中出现未捕获处理的异常，以抛出异常的方式结束
* */
public class StackFrameTest {
    public static void main(String[] args) {
        try {
            StackFrameTest test = new StackFrameTest();
            test.method1();

        } catch (Exception e) {
            e.printStackTrace();
        }
        System.out.println("main方法执行结束");
    }

    public void method1(){
        System.out.println("method1()开始执行");
        method2();
        System.out.println("method1()执行结束");

    }

    private int method2() {
        System.out.println("method2()开始执行");
        int i=10;
        int v =(int) method3();
        System.out.println("method2()即将结束");System.out.println(10 / 0);
        return i+v;
    }

    private double method3() {
        System.out.println("method3()开始执行");
        double j=20.0;
        System.out.println("method3()即将执行结束");
        return j;
    }
}
```

> 输出：

![image-20220222180253429](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222180253429.png)

#### 栈帧的内部结构

![image-20220222180909113](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222180909113.png)

##### 局部变量表

- 局部变量表也被称之为局部变量数组或本地变量表
- ==定义一个数字数组，主要用于存储方法参数和定义在方法体内的局部变量值==，
  这些数据类型包括各类基本数据类型、对象引用（reference），以及
  returnAddress类型。
- 由于局部变量表是建立在线程的栈上，是线程的私有数据，==因此不存在数据安全问题==
- ==局部变量表所需的容量大小是在编译期确定下来的==，并保存在方法的Code
  属性的maximum local variables数据项中。在方法运行期间是不会改变局部变量表的大小的。

> 说明：

- 参数值的存放总是在局部变量数组的index0开始，到数组长度-1的索引结束。
- 局部变量表，最基本的存储单元是Slot（变量槽）
- 局部变量表中存放编译期可知的各种基本数据类型（8种)，引用类型
  （reference），returnAddress类型。
  在局部变量表里，
  32位以内的类型只占用一个slot（包括
  returnAddress类型），64位的类型（long和double）占用两个slot。

`byte 、short 、char 在存储前被转换为int，boolean
也被转
换为int，O 表示false ，非0
表示true。`

> 案例：

```
public static void main(String[] args) {
    LocalVariablesTest test = new LocalVariablesTest();
    int num = 10;
    test.test1();
}
```

![image-20220222220654271](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222220654271.png)



![image-20220222220728161](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222220728161.png)

![image-20220222221012301](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222221012301.png)

- ==方法嵌套调用的次数由栈的大小决定。==一般来说，==栈越大，方法嵌套调用次数越多。对一个函数而言，它的参数和局部变量越多==，使得局部变量表膨胀，
  它的栈帧就越大，以满足方法调用所需传递的信息增大的需求。进而函数调
  用就会占用更多的栈空间，导致其嵌套调用次数就会减少。
- ==局部变量表中的变量只在当前方法调用中有效。==在方法执行时，虚拟机通过使用局部变量表完成参数值到参数变量列表的传递过程。
  ==当方法调用结束后,随着方法栈帧的销毁变量表也会随之销毁==

> ==字节码指令的行号与java代码行号的对应关系==

![image-20220222223312536](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222223312536.png)

![image-20220222222513467](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222222513467.png)

![image-20220222222541424](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222222541424.png)

![image-20220222223914817](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222223914817.png)

###### 局部变量表存储单元Slot

> 关于Slot的理解：

- 参数值的存放总是在局部变量数组的index0开始，到数组长度-1的索引结束。
- 局部变量表，最基本的存储单元是Slot（变量槽）
- 局部变量表中存放编译期可知的各种基本数据类型（8种)，引用类型
  （reference），returnAddress类型。
  在局部变量表里，
  32位以内的类型只占用一个slot（包括
  returnAddress类型），64位的类型（long和double）占用两个slot。

`byte 、short 、char 在存储前被转换为int，boolean
也被转
换为int，O 表示false ，非0
表示true。`

![image-20220222232014699](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222232014699.png)

==**为什么static方法中不能使用this？**==

因为this变量不存在当前方法的局部变量表中

> 案例：

```
public void test1() {
    Date date = new Date();
    String name1 = "atguigu.com";
    test2(date, name1);
    System.out.println(date + name1);
}
```

![image-20220222233000725](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222233000725.png)

```
public String test2(Date dateP, String name2) {
    dateP = null;
    name2 = "songhongkang";
    double weight = 130.5;//占据两个slot
    char gender = '男';
    return dateP + name2;
}
```

![image-20220222233054764](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222233054764.png)

==**构造方法:**==

```
public LocalVariablesTest(){
    this.count = 1;
}
```

![image-20220222233211165](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222233211165.png)

> Slot的重复利用:

`栈帧中的局部变量表中的槽位是可以重用的，如果一个局部变量过了其作用域，那么在其作用域之后申明的新的局部变量就很有可能会复用过期局部变量的槽位，从而达到节省资源的目的`

```
public void test4() {
    int a = 0;
    {
        int b = 0;
        b = a + 1;
    }
    //变量c使用之前已经销毁的变量b占据的slot的位置
    int c = a + 1;
}
```

==**复用了索引为2的Slot：**==

![image-20220222233523586](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220222233523586.png)

###### 静态变量与局部变量的对比

```java
/*
变量的分类：按照数据类型分：① 基本数据类型  ② 引用数据类型
            按照在类中声明的位置分：① 成员变量：在使用前，都经历过默认初始化赋值
                                            类变量： linking的prepare阶段：给类变量默认赋值  ---> initial阶段：给类变量显式赋值即静态代码块赋值
                                            实例变量：随着对象的创建，会在堆空间中分配实例变量空间，并进行默认赋值
                                   ② 局部变量：在使用前，必须要进行显式赋值的！否则，编译不通过
 */
public void test5Temp(){
    int num;
    //System.out.println(num);//错误信息：变量num未进行初始化
}
```

> 补充说明

- 在栈帧中，与性能调优关系最为密切的部分就是前面提到的局部变量表。
  在方法执行时，虚拟机使用局部变量表完成方法的传递。
- ==局部变量表中的变量也是重要的垃圾回收根节点，只要被局部变量表中直接或间接引用的对象都不会被回收。==



##### 操作数栈

- 每一个独立的栈帧中除了包含局部变量表以外，还包含一个==后进先出=== （Last-In-First-Out）的操作数栈，也可以称之为==表达式栈==
  (Expression Stack)
- ==操作数栈，在方法执行过程中，根据字节码指令，往栈中写入数据或提取
  数据，即入栈（push）/出栈(pop)。==

某些字节码指令将值压入操作数栈，其余的字节码指令将操作数取出栈。使用它们后再把结果压入栈。

比如：执行复制、交换、求和等操作

![image-20220223135044639](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220223135044639.png)

> 举例：

![image-20220223135121340](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220223135121340.png)



- 操作数栈，==主要用于保存计算过程的中间结果，同时作为计算过程中变量临时的存储空间。==
- ==操作数栈就是JVM执行引擎的一个工作区==，当一个方法刚开始执行的时候，
  一个新的栈帧也会随之被创建出来，==这个方法的操作数栈是空的==。
- 每一个操作数栈都会拥有一个明确的栈深度用于存储数值，
  ，其所需的最大深度在编译期就定义好了，保存在方法的Code属性中，为max_stack的
  值。
- 栈中的任何一个元素都是可以任意的Java数据类型。

`32bit的类型占用一个栈单位深度,64bit的类型占用两个栈单位深度`

操作数栈并非采用访问索引的方式来进行数据访问的，而是只能通过标准
的入栈（push）和出栈（pop）操作来完成一次数据访问。

![image-20220223135820065](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220223135820065.png)



- ==如果被调用的方法带有返回值的话，其返回值将会被压入当前栈帧的操作数栈中，==并更新PC寄存器中下一条需要执行的字节码指令。
- 操作数栈中元素的数据类型必须与字节码指令的序列严格匹配，这由编译器在编译器期间进行验证，同时在类加载过程中的类检验阶段的数据流分析阶段要再次验证。
- 另外，我们说Java虚拟机的==解释引擎是基于栈的执行引擎==，其中的栈指的就是操作数栈。

> 案例分析：

```java
public class OperandStackTest {
    public void testAddOperation() {
        //byte、short、char、boolean：都以int型来保存
        byte i = 15;
        int j = 8;
        int k = i + j;

       // int m = 800;

    }
```

![image-20220223142555710](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220223142555710.png)

![image-20220223142652710](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220223142652710.png)![image-20220223142705694](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220223142705694.png)

![image-20220223142717690](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220223142717690.png)

![image-20220223142730005](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220223142730005.png)

```java
public int getSum(){
    int m = 10;
    int n = 20;
    int k = m + n;
    return k;
}

public void testGetSum(){
    //获取上一个栈桢返回的结果，并保存在操作数栈中
    int i = getSum();
    int j = 10;
}
```

![image-20220223143420480](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220223143420480.png)

`aload`的含义是将getSum的返回值，存进操作数栈中



> 栈顶缓存技术

- 前面提过，基于栈式架构的虚拟机所使用的零地址指令更加紧凑，但完成一项操作的时候必然需要使用更多的入栈和出栈指令，（`我们自定义方法中明明只写了两三行，但是字节码指令却是其几倍`）这同时也就意
  味着将需要更多的指令分派（instruction dispatch）次数和内存
  读/写次数。

- 由于操作数是存储在内存中的，因此频繁地执行内存读/写操作必然会影
  响执行速度。为了解决这个问题，HotSpot JVM的设计者们提出了栈顶
  缓存（ToS，Top-of-Stack Cashing）技术，==将栈顶元素全部缓存在物理CPU的寄存器中，以此降低对内存的读/写次数，提升执行引擎的执行效率==

  ```
  public int getSum(){
      int m = 10;
      int n = 20;
      int k = m + n;
      return k;
  }
  ```

  将一个局部变量加载到操纵栈的指令包括：`iload`

  ![image-20220223145817675](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220223145817675.png)

`寄存器相对于栈的优点:`指令更少，执行速度快

##### 动态链接(指向运行时常量池的方法引用)

有些教辅会将`方法返回地址`，`动态链接`以及`一些附加信息`的`并集`称为`帧数据区`

- 每一个栈帧内部都包含一个指向`运行时常量池`中==该栈帧所属方法的引用==。
  包含这个引用的目的就是为了==支持当前方法的代码能够实现====动态链接
  (Dynamic Linking)== 比如：`invokedynamic`指令
- 在Java源文件被编译到字节码文件中时，所有的==变量==和==方法引用==都作为符号引用（Symbolic Reference）保存在class文件的常量池里。
  比如：描述一个方法调用了另外的其他方法时，就是通过常量池中指向方法的符号引用来表示的，那么==动态链接的作用就是为了将这些符号
  .
  用转换为调用方法的直接引用。==

> 示例代码：

```java
package com.sgg.VMStack;

public class DynamicLinkingTest {
    int num=10;

    public void methodA(){
        System.out.println("methodA()....");
    }
    public void methodB(){
        System.out.println("methodB()....");
        methodA();
        num++;
    }
}
```

![image-20220225232512122](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220225232512122.png)

![image-20220225232518389](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220225232518389.png)

![image-20220225232524836](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220225232524836.png)

![image-20220225232534978](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220225232534978.png)

![image-20220225232543933](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220225232543933.png)

![image-20220225232551280](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220225232551280.png)

![image-20220225232106103](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220225232106103.png)



==**为什么需要常量池呢？**==

`常量池的作用就是为了提供一些符号和常量，便于指令的识别。`



> 方法的调用

在JVM中，将符号引用转换为调用方法的直接引用与方法的绑定机制相关。

==**静态链接：**==
当一个字节码文件被装载进JVM内部时，如果==被调用的目标方法在编译期可知==，
且运行期保持不变时。这种情况下将调用方法的符号引用转换为直接引用的
过程称之为静态链接。

==**动态链接：**==
如果==被调用的方法在编译期无法被确定下来==，也就是说，只能够在程序运行期将调用方法的符号引用转换为直接引用，由于这种引用转换过程具备动态性，因此也就被称之为动态链接。



对应的方法的绑定机制为：==早期绑定（Early Binding）和晚期绑定
(Late Binding)==。==绑定是一个字段、方法或者类在符号引用被替换为
直接引用的过程，这仅仅发生一次==。

==**早期绑定：**==
早期绑定就是指被调用的==目标方法如果在编译期可知，且运行期保持不变时==，
即可将这个方法与所属的类型进行绑定，这样一来，由于明确了被调用的目
标方法究竟是哪一个，因此也就可以使用静态链接的方式将符号引用转换为
直接引用。

==**晚期绑定：**==
如果==被调用的方法在编译期无法被确定下来，只能够在程序运行期根据实际
的类型绑定相关的方法，==这种绑定方式也就被称之为晚期绑定。

> 代码演示：

```
package com.sgg.VMStack;

class Animal{

    public void eat(){
        System.out.println("动物进食");
    }
}
interface Huntable{
    void hunt();
}
class Dog extends Animal implements Huntable{
    @Override
    public void eat() {
        System.out.println("狗吃骨头");
    }

    @Override
    public void hunt() {
        System.out.println("捕食耗子，多管闲事");
    }
}

class Cat extends Animal implements Huntable{

    public Cat(){
        super();//表现为：早期绑定
    }

    public Cat(String name){
        this();//表现为：早期绑定
    }

    @Override
    public void eat() {
        super.eat();//表现为：早期绑定
        System.out.println("猫吃鱼");
    }

    @Override
    public void hunt() {
        System.out.println("捕食耗子，天经地义");
    }
}
public class AnimalTest {
    public void showAnimal(Animal animal){
        animal.eat();//表现为：晚期绑定
    }
    public void showHunt(Huntable h){
        h.hunt();//表现为：晚期绑定
    }
}
```

随着高级语言的横空出世，类似于Java一样的基于面向对象的编程语言如今
越来越多，尽管这类编程语言在语法风格上存在一定的差别，但是它们彼此
之间始终保持着一个共性，那就是都支持`封装`、`继承`和`多态`等面向对象特性，
既然==这一类的编程语言具备多态特性，那么自然也就具备早期绑定和晚期绑
定两种绑定方式。==

Java中任何一个普通的方法其实都具备虚函数的特征，它们相当于C++语言
中的虚函数（C++中则需要使用关键字virtual来显式定义）
。如果在Java
程序中不希望某个方法拥有虚函数的特征时，则可以使用关键字final来标
记这个方法。



> 虚方法和非虚方法：

- 非虚方法：
  如果方法在编译期就确定了具体的调用版本，这个版本在运行时是不可变的
  这样的方法称为==非虚方法==。
- 静态方法(静态方法不能被重写)、私有方法、final方法、实例构造器、父类方法（`super.`）都是非虚方法。
- 其他方法称为虚方法。

虚拟机中提供了以下几条方法调用指令：

==**普通调用指令：**==

`invokestatic`：
`调用静态方法，解析阶段确定唯一方法版本`

`invokespecial`：`调用<init>方法、私有及父类方法，解析阶段确定唯一方法版本`

invokevirtual：调用所有虚方法

invokeinterface：调用接口方法 

==**动态调用指令：**==

invokedynamic：动态解析出需要调用的方法，然后执行

前四条指令固化在虚拟机内部，方法的调用执行不可人为干预，而invokedynamic指令则支
持由用户确定方法版本。其中==**invokestatic指令和invokespecial指令调用称为非虚方法，其余的（final修饰的除外）称为虚方法。**==

> 案例：

```
package com.sgg.VMStack;

/**
 * 解析调用中非虚方法、虚方法的测试
 *
 * invokestatic指令和invokespecial指令调用的方法称为非虚方法
 * @author shkstart
 * @create 2020 下午 12:07
 */
class Father {
    public Father() {
        System.out.println("father的构造器");
    }

    public static void showStatic(String str) {
        System.out.println("father " + str);
    }

    public final void showFinal() {
        System.out.println("father show final");
    }

    public void showCommon() {
        System.out.println("father 普通方法");
    }
}

public class Son extends Father {
    public Son() {
        //invokespecial
        super();
    }
    public Son(int age) {
        //invokespecial
        this();
    }
    //不是重写的父类的静态方法，因为静态方法不能被重写！
    public static void showStatic(String str) {
        System.out.println("son " + str);
    }
    private void showPrivate(String str) {
        System.out.println("son private" + str);
    }

    public void show() {
        //invokestatic
        showStatic("atguigu.com");
        //invokestatic
        super.showStatic("good!");
        //invokespecial
        showPrivate("hello!");
        //invokespecial
        super.showCommon();

        //invokevirtual
        showFinal();//因为此方法声明有final，不能被子类重写，所以也认为此方法是非虚方法。
        //虚方法如下：
        //invokevirtual   没有重写但是也表现是虚方法 因为编译期间无法判断子类是否重写
        showCommon();
        //这个和上面的同理
        info();

        MethodInterface in = null;
        //invokeinterface
        in.methodA();
    }

    public void info(){

    }

    public void display(Father f){
        f.showCommon();
    }

    public static void main(String[] args) {
        Son so = new Son();
        so.show();
    }
}

interface MethodInterface{
    void methodA();
}

```



![image-20220226010245677](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220226010245677.png)



> invokedynamic指令

- JVM字节码指令集一直比较稳定，
  ，一直到Java7中才增加了一个
  invokedynamic指令，这是==Java为了实现『动态类型语言」支持而做的一种改进==。
- 但是在Java7中并没有提供直接生成invokedynamic指令的方法，需要借助
  ASM这种底层字节码工具来产生invokedynamic指令。==直到Java8的Lambda
  表达式的出现，invokedynamic指令的生成，在Java中才有了直接的生成
  方式。==
- Java7中增加的动态语言类型支持的本质是对Java虚拟机规范的修改，而不
  是对Java语言规则的修改，这一块相对来讲比较复杂，增加了虚拟机中的方
  法调用，最直接的受益者就是运行在Java平台的动态语言的编译器。

==**动态类型语言和动态语言:**==

动态类型语言和静态类型语言两者的区别就在于对类型的检查是在编译期还
是在运行期，满足前者就是静态类型语言，反之是动态类型语言。

说的再直白一点就是，==静态类型语言是判断变量自身的类型信息；动态类型
语言是判断变量值的类型信息，变量没有类型信息，变量值才有类型信息，==
这是动态语言的一个重要特征。

`例子：`

静态语言：==java：== String info=“atguigu”;  info=atguigu;

动态语言：==JS:== var name=“kwq”; var name=10;

 				==Python:== info=130.5

> 具体代码：

```
package com.sgg.VMStack;

/**
 * 体会invokedynamic指令
 * @author shkstart
 * @create 2020 下午 3:09
 */
@FunctionalInterface
interface Func {
    public boolean func(String str);
}

public class Lambda {
    public void lambda(Func func) {
        return;
    }

    public static void main(String[] args) {
        Lambda lambda = new Lambda();

        Func func = s -> {
            return true;
        };

        lambda.lambda(func);

        lambda.lambda(s -> {
            return true;
        });
    }
}
```

![image-20220226230158441](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220226230158441.png)



> 方法重写和虚方法表

==**Java 语言中方法重写的本质：**==

1.找到操作数栈顶的第一个元素所执行的对象的实际类型，记作 C。

2.如果在类型 C 中找到与常量中的描述符合简单名称都相符的方法，则进行访问权限校验，如果通过则返回这个方法的直接引用，查找过程结束；如果不通过，则返回
`java.lang.IllegalAccessError `异常。

3.否则，按照继承关系从下往上依次对 C 的各个父类进行第 2 步的搜索和验证过程。

4.如果始终没有找到合适的方法，则抛出
java.lang.AbstractMethodError异常。



==**IllegalAccessError介绍：**==
程序试图访问或修改一个属性或调用一个方法，这个属性或方法，你没有权限访问。一般的，这个会引起编译器异常。这个错误如果发生在运行时，就说明一个类发生了不兼容的改变。



- 在面向对象的编程中，会很频繁的使用到`动态分派`，如果在每次动态分派
  的过程中都要重新在类的方法元数据中搜索合适的目标的话就可能影响到
  执行效率。因此，==为了提高性能==，JVM采用在类的方法区建立一个虚方法表==(virtual method table)
  (非虚方法不会出现在表中)来实现。
  使用
  索引表来代替查找==。
- 每个类中都有一个虚方法表，表中存放着各个方法的实际入口。
- 那么虚方法表什么时候被创建？
  虚方法表会在类加载的==链接阶段被创建并开始初始化==，类的变量初始值准备完
  成之后，JVM会把该类的方法表也初始化完毕。

![image-20220227205513109](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220227205513109.png)



> 案例一：

![image-20220227210142756](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220227210142756.png)

> 案例二：

![image-20220227210209096](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220227210209096.png)![image-20220227210246169](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220227210246169.png)



##### 方法返回地址

- 存放调用该方法的pc寄存器的值。
- 方法的结束，有两种方式：

1.正常执行完成

2.出现未处理的异常，非正常退出

- 无论通过哪种方式退出，在方法退出后都返回到该方法被调用的位置。方
  法正常退出时，
  ==**调用者的pc计数器的值作为返回地址，即调用该方法的指令的下一条指令的地址**==。而通过异常退出的，返回地址是要通过异常表来确定，栈帧中一般不会保存这部分信息。



本质上，方法的退出就是当前栈帧出栈的过程。此时，需要恢复上层方法
的局部变量表、操作数栈、将返回值压入调用者栈帧的操作数栈、设置PC
寄存器值等，让调用者方法继续执行下去。

`正常完成出口和异常完成出口的区别在于：通过异常完成出口退出不会给他的上层调用者产生任何的返回值。`



当一个方法开始执行后，只有两种方式可以退出这个方法：
1、执行引擎遇到任意一个方法返回的字节码指令（return），会有返回值
传递给上层的方法调用者，简称==正常完成出口；==

- 一个方法在正常调用完成之后究竟需要使用哪一个返回指令还需要根据方法返回值的实际数据类型而定。
- 在字节码指令中，返回指令包含ireturn（当返回值是boolean、byte、char、
  short和int类型时使用）
  、lreturn、freturn、dreturn以及areturn，
  另
  外还有一个return指令供声明为void的方法、实例初始化方法、类和接口的初始
  化方法使用。

![image-20220227213415440](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220227213415440.png)



> 具体代码演示：


    package com.sgg.VMStack.java3;
    
    import java.io.FileReader;
    import java.io.IOException;
    import java.util.Date;
    
    /**
     *
     * 返回指令包含ireturn（当返回值是boolean、byte、char、short和int类型时使用）、
     * lreturn、freturn、dreturn以及areturn，另外还有一个return指令供声明为void的方法、
     * 实例初始化方法、类和接口的初始化方法使用。
     *
     * @author shkstart
     * @create 2020 下午 4:05
     */
    public class ReturnAddressTest {
        public boolean methodBoolean() {
            return false;
        }
    
        public byte methodByte() {
            return 0;
        }
    
        public short methodShort() {
            return 0;
        }
    
        public char methodChar() {
            return 'a';
        }
    
        public int methodInt() {
            return 0;
        }
    
        public long methodLong() {
            return 0L;
        }
    
        public float methodFloat() {
            return 0.0f;
        }
    
        public double methodDouble() {
            return 0.0;
        }
    
        public String methodString() {
            return null;
        }
    
        public Date methodDate() {
            return null;
        }
    
        public void methodVoid() {
    
        }
    
        static {
            int i = 10;
        }


​    
​        //
​        public void method2() {
​    
​            methodVoid();
​    
​            try {
​                method1();
​            } catch (IOException e) {
​                e.printStackTrace();
​            }
​        }
​    
​        public void method1() throws IOException {
​            FileReader fis = new FileReader("atguigu.txt");
​            char[] cBuffer = new char[1024];
​            int len;
​            while ((len = fis.read(cBuffer)) != -1) {
​                String str = new String(cBuffer, 0, len);
​                System.out.println(str);
​            }
​            fis.close();
​        }


​    
​    }



![image-20220227213204078](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220227213204078.png)

![image-20220227213248368](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220227213248368.png)

![image-20220227213317845](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220227213317845.png)![image-20220227215228942](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220227215228942.png)

![image-20220227215500279](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220227215500279.png)

![image-20220227215536845](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220227215536845.png)

##### 附加信息

==**栈帧中还运行携带与java虚拟机实现相关的一些附加信息，例如：对程序调试提供支持的信息**==



## 栈相关面试题

==**分配的栈内存越大越好吗？**== 不是 会抢占其他空间（堆等）的内存

==**方法中定义的局部变量是否线程安全？具体情况具体分析，看下面具体案例**==

> 案例代码：

```java
package com.sgg.VMStack.java3;

/**
 * 面试题：
 * 方法中定义的局部变量是否线程安全？具体情况具体分析
 *
 *   何为线程安全？
 *      如果只有一个线程才可以操作此数据，则必是线程安全的。
 *      如果有多个线程操作此数据，则此数据是共享数据。如果不考虑同步机制的话，会存在线程安全问题。
 * @author shkstart
 * @create 2020 下午 7:48
 */
public class StringBuilderTest {

    int num = 10;

    //s1的声明方式是线程安全的
    public static void method1(){
        //StringBuilder:线程不安全
        StringBuilder s1 = new StringBuilder();
        s1.append("a");
        s1.append("b");
        //...
    }
    //sBuilder的操作过程：是线程不安全的 多个线程同时传一个sBuilder
    public static void method2(StringBuilder sBuilder){
        sBuilder.append("a");
        sBuilder.append("b");
        //...
    }
    //s1的操作：是线程不安全的  传回的s1可以被多个线程操作
    public static StringBuilder method3(){
        StringBuilder s1 = new StringBuilder();
        s1.append("a");
        s1.append("b");
        return s1;
    }
    //s1的操作：是线程安全的
    public static String method4(){
        StringBuilder s1 = new StringBuilder();
        s1.append("a");
        s1.append("b");
        return s1.toString();
    }

    public static void main(String[] args) {
        StringBuilder s = new StringBuilder();


        new Thread(() -> {
            s.append("a");
            s.append("b");
        }).start();

        method2(s);

    }

}
```

## 本地方法接口

![image-20220228134153862](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228134153862.png)

> 什么是本地方法：

简单地讲，==一个Native Method就是一个Java调用非Java代码的接口==。一个
Native Method是这样一个Java方法：该方法的实现由非Java语言实现，比如
C。这个特征并非Java所特有，很多其它的编程语言都有这一机制，比如在C++中，
你可以用extern
"C"告知c++编译器去调用一个c的函数。

"A native method is a Java method whose
implementation is
provided by non-java code."

在定义一个native method时，并不提供实现体（有些像定义一个Java
interface），因为其实现体是由非java语言在外面实现的。

本地接口的作用是融合不同的编程语言为Java所用，它的初衷是融合 C/C++程序。

> 举例：

```java
package com.sgg.nativeMethod;

/**
 * @author shkstart
 * @create 2020 下午 8:53
 */
public class IHaveNatives {
    public native void Native1(int x);  //native 表明调用c实现的 

    public native static long Native2();

    private native synchronized float Native3(Object o);

    native void Native4(int[] ary) throws Exception;

}
```

`标识符native可以与所有其他的java标识符连用，但是abstract除外`



==**为什么要使用Native Method？**==

Java使用起来非常方便，然而有些层次的任务用Java实现起来不容易，或者
我们对程序的效率很在意时，问题就来了。

- 与Java环境外交互：=== 有时Java应用需要与Java外面的环境交互，这是本地方法存在的主要原因==。
  你可以想想Java需要与一些底层系统，如操作系统或某些硬件交换信息时的
  情况。本地方法正是这样一种交流机制：它为我们提供了一个非常简洁的接口，
  而且我们无需去了解Java应用之外的繁琐的细节。
- 与操作系统交互：
  JVM支持着Java语言本身和运行时库，它是Java程序赖以生存的平台，它由一个解释
  器（解释字节码）和一些连接到本地代码的库组成。然而不管怎样，它毕竟不是一个完整的系统，它经常依赖于一些底层系统的支持。这些底层系统常常是强大的操作系统。==通过使用本地方法，我们使得java实现了jre的与地产系统的交互，甚至jvm的一部分是用c写的。==还有，如果我们要使用一些Java语言本身没有提供封装的
  操作系统的特性时,我们也需要使用本地方法。
- ==Sun's Java
  Sun的解释器是用c实现的，这使得它能像一些普通的C一样与外部交互。==jre大部分是
  用Java实现的，它也通过一些本地方法与外界交互。例如：类java.lang.Thread
  的 setPriority()方法是用Java实现的，但是它实现调用的是该类里的本地方法
  setPriority0()。这个本地方法是用c实现的，并被植入JVM内部，在Windows 95
  的平台上，这个本地方法最终将调用Win32 SetPriority() API。这是一个本地方
  法的具体实现由JVM直接提供，更多的情况是本地方法由外部的动态链接库
  （external dynamic link library）提供，然后被JVM调用。

![image-20220228134125256](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228134125256.png)



==目前该方法使用的越来越少了，除非是与硬件有关的应用==，比如通过
Java程序驱动打印机或者Java系统管理生产设备，在企业级应用中已经
比较少见。因为现在的异构领域间的通信很发达，比如可以使用Socket
通信，也可以使用Web Servide等等，不多做介绍。



## 本地方法栈

- ==Java虚拟机栈用于管理java方法的调用，而本地方法栈用于管理本地方法的调用。==
- 本地方法栈，也是线程私有的。
  允许被实现成固定或者是可动态扩展的内存大小。（在内存溢出方面是相同的）

1.如果线程请求分配的栈容量超过本地方法栈允许的最大容量，Java虚拟机将会
抛出一个 StackoverrlowError 异常。

2.如果本地方法栈可以动态扩展，并且在尝试扩展的时候无法申请到足够的内存，
或者在创建新的线程时没有足够的内存去创建对应的本地方法栈，那么Java虚
拟机将会抛出一个 OutOfMemoryError 异常。

- 本地方法是使用c语言实现的。
- ==它的具体做法是Native Method Stack中登记native方法，在
  Execution Engine 执行时加载本地方法库。==

![image-20220228150252206](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228150252206.png)

==**当某个线程调用一个本地方法时，它就进入一个全新的并且不再受虚拟
机限制的世界。它和虚拟机拥有同样的权限。**==

- 本地方法可以通过本地方法接口来访问虚拟机内部的运行时数据区。

- 它甚至可以直接使用本地处理器中的寄存器

- 直接从本地内存的堆中分配任意数量的内存。

  ==并不是所有的JVM都支持本地方法。因为Java虚拟机规范并没有明确要求
  本地方法栈的使用语言、具体实现方式、数据结构等。==如果JVM产品不打
  算支持native方法，也可以无需实现本地方法栈。

  在Hotspot JVM中，直接将本地方法栈和虚拟机栈合二为一。



## 堆

`一个进程对应一个jvm实例，一个进程里面包含多个线程,一个实例对应一份方法区和堆 进程里的多个线程共用一份方法区和堆` 

![image-20220228155915309](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228155915309.png)

> 堆的核心内容概述：

- 一个JVM实例只存在一个堆内存，堆也是Java内存管理的核心区域。

- Java 堆区在JVM 启动的时候即被创建，其空间大小也就确定了。是JVM
  管理的最大一块内存空间。

- 堆内存的大小是可以调节的。

- 《Java虚拟机规范》规定，堆可以处于==物理上不连续的内存空间中，但在
  逻辑上它应该被视为连续的。==`解释下:逻辑上连续是指堆内存映射出的虚拟内存是连续的`

- 所有的线程共享Java堆，在这里还可以划分线程私有的缓冲区（Thread
  Local Allocation Buffer, TLAB)。

  ![image-20220228184730984](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228184730984.png)

  > 案例：

  ```
  package com.sgg.Heap;
  
  /**
   * -Xms10m -Xmx10m
   *
   * @author shkstart  shkstart@126.com
   * @create 2020  16:41
   */
  public class HeapDemo {
      public static void main(String[] args) {
          System.out.println("start...");
          try {
              Thread.sleep(1000000);
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
  
          System.out.println("end...");
      }
  
  }
  ```

  ```
  package com.sgg.Heap;
  
  /**
   * -Xms20m -Xmx20m
   * @author shkstart  shkstart@126.com
   * @create 2020  16:42
   */
  public class HeapDemo1 {
      public static void main(String[] args) {
          System.out.println("start...");
          try {
              Thread.sleep(1000000);
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
  
          System.out.println("end...");
      }
  }
  ```

  `在JDK8的lib下打开这个，内存分析工具`

![image-20220228160443831](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228160443831.png)

![image-20220228160513522](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228160513522.png)

安装插件

![image-20220228161235643](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228161235643.png)![image-20220228161356835](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228161356835.png)

![image-20220228161409568](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228161409568.png)

> 堆的核心概述：

1.《Java虚拟机规范》中对Java堆的描述是：所有的对象实例以及数组都应
当在运行时分配在堆上。
(The heap is the run-time data area from
which memory for all class instances and arrays is allocated )

`我要说的是:“几乎”所有的对象实例都在这里分配内存。
—从实际
使用角度看的。`

2.数组和对象可能永远不会存储在栈上，因为栈帧中保存引用，这个引用指向
对象或者数组在堆中的位置。

3.在方法结束后，`栈帧中的引用也就消失了`，此时，堆中的对象不会马上被移除，仅仅在垃圾收集的时候才会被移除。

4.堆，是GC ( Garbage Collection，垃圾收集器）执行垃圾回收的重点区域。

![image-20220228184624585](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228184624585.png)

### 堆的细分内存结构

==**现代垃圾收集器大部分都基于分代收集理论设计，堆空间细分为：**==

![image-20220228194806475](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228194806475.png)

`永久代和元空间是方法区的具体实现`

![image-20220228194952098](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228194952098.png)

> 案例：

```java
package com.sgg.Heap;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  17:28
 */
public class SimpleHeap {
    private int id;//属性、成员变量

    public SimpleHeap(int id) {
        this.id = id;
    }

    public void show() {
        System.out.println("My ID is " + id);
    }
    public static void main(String[] args) {
        SimpleHeap sl = new SimpleHeap(1);
        SimpleHeap s2 = new SimpleHeap(2);

        int[] arr = new int[10];

        Object[] arr1 = new Object[10];
    }
}

```

`添加虚拟机选项:`

![image-20220228195510177](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228195510177.png)

==**打印堆空间的信息:**==

> 输出：

![image-20220228195357413](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228195357413.png)

### 堆空间大小的设置和查看

- Java堆区用于存储Java对象实例，那么堆的大小在JVM启动时就已经设定好了，大
  家可以通过选项”-Xmx"和"-Xms"来进行设置。

​	“-Xms"用于表示堆区的起始内存，等价于-XX:InitialHeapSize

​	-Xmx”则用于表示堆区的最大内存，等价于-XX：MaxHeapSize

- 一旦堆区中的内存大小超过“-Xmx”所指定的最大内存时，将会抛出
  OutOfMemoryError异常。
- 通常会将 -Xms 和 -Xmx两个参数配置相同的值，==其目的是为了能够在java垃圾回收收机制清理完堆区后不需要重新分隔计算堆区的大小，从而提高性能。==  `如果初始值设置太小，那么就会频繁发生扩容，GC后又频繁释放，加大了系统的压力，造成效率十分低下`
- 默认情况下，初始内存大小：物理电脑内存大小 1/ 64
  最大内存大小：物理电脑内存大小 1 / 4

```java
package com.sgg.Heap;

/**
 * 1. 设置堆空间大小的参数
 * -Xms 用来设置堆空间（年轻代+老年代）的初始内存大小
 *      -X 是jvm的运行参数
 *      ms 是memory start
 * -Xmx 用来设置堆空间（年轻代+老年代）的最大内存大小
 *
 * 2. 默认堆空间的大小
 *    初始内存大小：物理电脑内存大小 1/ 64
 *             最大内存大小：物理电脑内存大小 1/ 4
 * 3. 手动设置：-Xms600m -Xmx600m
 *     开发中建议将初始堆内存和最大的堆内存设置成相同的值。
 *
 * 4. 查看设置的参数：方式一： jps   /  jstat -gc 进程id
 *                  方式二：-XX:+PrintGCDetails
 * @author shkstart  shkstart@126.com
 * @create 2020  20:15
 */
public class HeapSpaceInitial {
    public static void main(String[] args) {

        //返回Java虚拟机中的堆内存总量
        long initialMemory = Runtime.getRuntime().totalMemory() / 1024 / 1024;
        //返回Java虚拟机试图使用的最大堆内存量
        long maxMemory = Runtime.getRuntime().maxMemory() / 1024 / 1024;

        System.out.println("-Xms : " + initialMemory + "M");
        System.out.println("-Xmx : " + maxMemory + "M");

       System.out.println("系统内存大小为：" + initialMemory * 64.0 / 1024 + "G");
       System.out.println("系统内存大小为：" + maxMemory * 4.0 / 1024 + "G");

    
}
```

> 输出：

```java
  /*-Xms : 121M
-Xmx : 1783M
系统内存大小为：7.5625G
系统内存大小为：6.96484375G*/
```

> 手动设置

![image-20220228202738512](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228202738512.png)

> 运行输出:

![image-20220228202653445](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228202653445.png)



![image-20220228203754331](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228203754331.png)

![image-20220228203953477](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228203953477.png)

25600+25600+153600+409600的和除以1024=600

为什么打印出来的是575，少了25 

因为只能算一个幸存区的内存 

![image-20220228204453014](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228204453014.png)

### OutOfMemoryError举例

```java
package com.sgg.Heap;

import java.util.ArrayList;
import java.util.Random;

/**
 * -Xms600m -Xmx600m
 * @author shkstart  shkstart@126.com
 * @create 2020  21:12
 */
public class OOMTest {
    public static void main(String[] args) {
        ArrayList<Picture> list = new ArrayList<>();
        while(true){
            try {
                Thread.sleep(20);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            list.add(new Picture(new Random().nextInt(1024 * 1024)));
        }
    }
}

class Picture{
    private byte[] pixels;

    public Picture(int length) {
        this.pixels = new byte[length];
    }
}
```

> 输出：

![image-20220228205726434](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228205726434.png)



打开jdk自带的工具**Java VisualVM**

![image-20220228210408131](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228210408131.png)

![image-20220228210444483](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228210444483.png)

`可以查看大对象`

![image-20220228210512212](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220228210512212.png)

### 新生代与老年代中相关参数的设置：

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203011310206.png)
![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203011310207.png)
![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203011310208.png)

> 案例代码：




 * -XX:NewRatio ： 设置新生代与老年代的比例。默认值是2.
 * -XX:SurvivorRatio ：设置新生代中Eden区与Survivor区的比例。默认值是8
 * -XX:-UseAdaptiveSizePolicy ：关闭自适应的内存分配策略  （暂时用不到） 如果不关闭这个的话 就不是8:1:1 了 -代表关闭 +代表添加
 * -Xmn:设置新生代的空间的大小。 （一般不设置）
    *

```java
package com.sgg.Heap.java2;

/**
 * -Xms600m -Xmx600m
 *
 * -XX:NewRatio ： 设置新生代与老年代的比例。默认值是2.
 * -XX:SurvivorRatio ：设置新生代中Eden区与Survivor区的比例。默认值是8
 * -XX:-UseAdaptiveSizePolicy ：关闭自适应的内存分配策略  （暂时用不到） 如果不关闭这个的话 就不是8:1:1 了 -代表关闭 +代表添加
 * -Xmn:设置新生代的空间的大小。 （一般不设置）
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  17:23
 */
public class EdenSurvivorTest {
    public static void main(String[] args) {
        System.out.println("我只是来打个酱油~");
        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

`上面代码运行起来后可以通过jdk自带java viusal 打开所属线程的gc查看新生代内存分配。`

当然也可以通过命令行的方式：

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203011323907.png)

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203011310209.png)

### 新生代对象分配与回收过程

> 概述：

`伊甸园区满的时候触发YGC(这个gc会针对伊甸园区和幸存者区进行垃圾回收，若伊甸园区没满，而幸存区满了不会触发gc，幸存区是一个被动处于回收的状态，幸存区满了一些对象直接跨级到老年代，不用等到15次gc)，此时用户线程停止（因此频繁触发gc，会造成响应速度慢，因为gc和用户线程不能并行）`

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203011348672.png)

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203011348670.png)
![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203011348671.png)
![](h

`每次触发一次gc，留下来的对象的某个属性都+1，若此属性达到了15，下次gc就会根据机制放入老年代`

==**总结：**==

==针对幸存者s0,s1区的总结：复制之后有交换，谁空谁是to==

==回收：
对于垃块
频繁在新生区收集，很少在养老区收集，几乎不在永久区/元空间收集。==

#### 特殊情况:

![image-20220301140212859](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301140212859.png)

> 代码举例:

```java
package com.sgg.Heap.java2;

import java.util.ArrayList;
import java.util.Random;

/**
 * -Xms600m -Xmx600m
 * @author shkstart  shkstart@126.com
 * @create 2020  17:51
 */
public class HeapInstanceTest {
    byte[] buffer = new byte[new Random().nextInt(1024 * 200)];

    public static void main(String[] args) {
        ArrayList<HeapInstanceTest> list = new ArrayList<HeapInstanceTest>();
        while (true) {
            list.add(new HeapInstanceTest());
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

> 输出：

```java
package com.sgg.Heap.java2;

import java.util.ArrayList;
import java.util.Random;

/**
 * -Xms600m -Xmx600m
 * @author shkstart  shkstart@126.com
 * @create 2020  17:51
 */
public class HeapInstanceTest {
    byte[] buffer = new byte[new Random().nextInt(1024 * 200)];

    public static void main(String[] args) {
        ArrayList<HeapInstanceTest> list = new ArrayList<HeapInstanceTest>();
        while (true) {
            list.add(new HeapInstanceTest());
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

![image-20220301153628002](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301153628002.png)

`通过java viusal来分析GC,主要看下图所圈的内容`

![image-20220301142332586](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301142332586.png)

==**老年代报的栈溢出**==

### 性能调优工具JProfiler

> 常见的调优工具：

![image-20220301154914737](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301154914737.png)

这里我们讲下`JProfiler`

具体安装教程，看狂神说jvm的笔记

==运行起来后可以看到每个空间（伊甸园等）的内存占用情况。（实时变化的）==

![image-20220301155208138](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301155208138.png)



### Minor GC、Major GC与Full GC

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203011848472.png)



![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203011848201.png)

PS：`用户线程和GC线程不能同时执行！STW指的是GC启用到结束的时间，这个时间段里面用户线程是等待的，因此STW越短越好 而Minor GC的STW相对来说是比较短的`

==Minor GC 频率高但是执行速度快==

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203011850414.png)

PS: `threshold默认是15次`

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203011850018.png)



![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203011850295.png)



### GC举例与日志分析

`给程序添加VM 选项 ：-Xms9m -Xmx9m -XX:+PrintGCDetails`

```java
package com.sgg.Heap.java2;

import java.util.ArrayList;
import java.util.List;

/**
 * 测试MinorGC 、 MajorGC、FullGC
 * -Xms9m -Xmx9m -XX:+PrintGCDetails
 * @author shkstart  shkstart@126.com
 * @create 2020  14:19
 */
public class GCTest {
    public static void main(String[] args) {
        int i = 0;
        try {
            List<String> list = new ArrayList<>();
            String a = "atguigu.com";
            while (true) {
                list.add(a);
                a = a + a;
                i++;
            }

        } catch (Throwable t) {
            t.printStackTrace();
            System.out.println("遍历次数为：" + i);
        }
    }
}
```

> 输出

`参数解释：`

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203011931125.png)





### 堆空间的分代思想

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203011941964.png)

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203011940940.png)



### 内存分配策略

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203012011818.png)

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203012012154.png)



> 测试案例：大对象直接进入老年代

```java
package com.sgg.Heap.java2;

/** 测试：大对象直接进入老年代
 * -Xms60m -Xmx60m -XX:NewRatio=2 -XX:SurvivorRatio=8 -XX:+PrintGCDetails
 * @author shkstart  shkstart@126.com
 * @create 2020  21:48
 */
public class YoungOldAreaTest {
    public static void main(String[] args) {
        byte[] buffer = new byte[1024 * 1024 * 20];//20m

    }
}
NewRatio=2  老年代内存大小/新生代内存大小
    伊甸园区 16
    幸存区1   2
    幸存区2   2
    老年期    40
```

> 输出：

![image-20220301201328505](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301201328505.png)



### 堆空间为每个线程分配的TLAB

==**为什么要用TLAB？**==

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203012119585.png)

因为上述第一点可以实现`线间的通信`



==**什么是TLAB?**==

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203012121067.png)

`基于TLAB的设计下图多线程环境下就不会造成并发`

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203012122017.png)

==**TLAB再说明：**==

![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203012123889.png)



![](https://gitee.com/kekekeke111/typora/raw/master/typoraImage/202203012123404.png)

==**new 对象的时候优先考虑在TLAB中分配内存**==



### 堆空间中常见的参数

```java
package com.sgg.Heap.java2;

/**
 * 测试堆空间常用的jvm参数：
 * -XX:+PrintFlagsInitial : 查看所有的参数的默认初始值
 * -XX:+PrintFlagsFinal  ：查看所有的参数的最终值（可能会存在修改，不再是初始值）
 *      具体查看某个参数的值的指令： jps：查看当前运行中的进程
 *                             jinfo -flag SurvivorRatio 进程id
 *
 * -Xms：初始堆空间内存 （默认为物理内存的1/64）
 * -Xmx：最大堆空间内存（默认为物理内存的1/4）
 * -Xmn：设置新生代的大小。(初始值及最大值)
 * -XX:NewRatio：配置新生代与老年代在堆结构的占比
 * -XX:SurvivorRatio：设置新生代中Eden和S0/S1空间的比例
 * -XX:MaxTenuringThreshold：设置新生代垃圾的最大年龄
 * -XX:+PrintGCDetails：输出详细的GC处理日志
 * 打印gc简要信息：① -XX:+PrintGC   ② -verbose:gc
 * -XX:HandlePromotionFailure：是否设置空间分配担保
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  17:18
 */
public class HeapArgsTest {
    public static void main(String[] args) {

    }
}
```

==**添加VM 选项：-XX:+PrintFlagsInitial -XX:SurvivorRatio=5**==

![image-20220301213706188](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301213706188.png)

添加VM 选项：-XX:+PrintFlagsFinal  -XX:SurvivorRatio=5 

输出就变5

==**JVM调优就是要尽量要降低gc频率**==

> 对于-XX:HandlePromotionFailure的详解：

![image-20220301215240672](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301215240672.png)



### 堆是分配对象的唯一选择吗？(涉及逃逸分析)

![image-20220301223846812](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301223846812.png)

> 逃逸分析：

![image-20220301223928614](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301223928614.png)



![image-20220301223940188](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301223940188.png)

> 案例：

```
package com.sgg.Heap.java3;

/**
 * 逃逸分析
 *
 *  如何快速的判断是否发生了逃逸分析，大家就看new的对象实体是否有可能在方法外被调用。
 * @author shkstart
 * @create 2020 下午 4:00
 */
public class EscapeAnalysis {

    public EscapeAnalysis obj;

    /*
    方法返回EscapeAnalysis对象，发生逃逸
     */
    public EscapeAnalysis getInstance(){
        return obj == null? new EscapeAnalysis() : obj;
    }
    /*
    为成员属性赋值，发生逃逸
     */
    public void setObj(){
        this.obj = new EscapeAnalysis();
    }
    //思考：如果当前的obj引用声明为static的？仍然会发生逃逸。

    /*
    对象的作用域仅在当前方法中有效，没有发生逃逸
     */
    public void useEscapeAnalysis(){
        EscapeAnalysis e = new EscapeAnalysis();
    }
    /*
    引用成员变量的值，发生逃逸
     */
    public void useEscapeAnalysis1(){
        EscapeAnalysis e = getInstance();
        //getInstance().xxx()同样会发生逃逸
    }
}
```



![image-20220301224006097](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301224006097.png)

> 结论：

`开发中能使用局部变量的，就不要使用在方法外定义。这样不逃逸能降低gc回收频率从而提高用户线程效率`

![image-20220301232841040](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301232841040.png)

`3的最后一句话是说存在栈中而不存在堆中`

`JIT可以分为两个阶段：在运行时生成机器码和在运行时执行机器码。`

### 代码优化值栈上分配：

> 案例代码：

```
package com.sgg.Heap.java3;

/**
 * 栈上分配测试
 * -Xmx1G -Xms1G -XX:-DoEscapeAnalysis -XX:+PrintGCDetails
 * @author shkstart  shkstart@126.com
 * @create 2020  10:31
 */
public class StackAllocation {
    public static void main(String[] args) {
        long start = System.currentTimeMillis();

        for (int i = 0; i < 10000000; i++) {
            alloc();
        }
        // 查看执行时间
        long end = System.currentTimeMillis();
        System.out.println("花费的时间为： " + (end - start) + " ms");
        // 为了方便查看堆内存中对象个数，线程sleep
        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e1) {
            e1.printStackTrace();
        }
    }

    private static void alloc() {
        User user = new User();//未发生逃逸
    }

    static class User {

    }
}
```

配置：`-Xmx1G -Xms1G -XX:-DoEscapeAnalysis -XX:+PrintGCDetails`

用java viusal查看

![image-20220301231501755](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301231501755.png)

输出：没有打印gc信息 说明没有进程gc

![image-20220301231524183](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301231524183.png)



修改配置`-Xmx1G -Xms1G -XX:+DoEscapeAnalysis -XX:+PrintGCDetails`

输出：

![image-20220301231631748](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301231631748.png)

![image-20220301231744337](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301231744337.png)

修改配置 `-Xmx256m -Xms256m -XX:-DoEscapeAnalysis -XX:+PrintGCDetails`

![image-20220301231903120](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301231903120.png)

修改配置 `-Xmx256m -Xms256m -XX:+DoEscapeAnalysis -XX:+PrintGCDetails`

![image-20220301231932732](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220301231932732.png)



### 代码优化之同步省略

![image-20220302083636084](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302083636084.png)

![image-20220302083647349](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302083647349.png)

`编译的时候有 运行的时候自动省略掉了`

> 代码演示：

```java
package com.sgg.Heap.java3;

/**
 * 同步省略说明
 * @author shkstart  shkstart@126.com
 * @create 2020  11:07
 */
public class SynchronizedTest {
    public void f() {
        Object hollis = new Object();
        synchronized(hollis) {
            System.out.println(hollis);
        }
    }
}
```

![image-20220302083947952](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302083947952.png)



### 代码优化之标量替换

![image-20220302084754863](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302084754863.png)

`也就是说不存在堆空间中，而是存在栈空间中`

![image-20220302090243773](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302090243773.png)

![image-20220302090258411](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302090258411.png)

> 聚合量、标量实例

```
class Customer{  //聚合量
    String name;  // 标量
    int id;
    Account acct;  //聚合量

}

class Account{
    double balance;
}
```

> 案例之标量替换与不替换效率对比

```java
package com.sgg.Heap.java3;

/**
 * 标量替换测试
 *  -Xmx100m -Xms100m -XX:+DoEscapeAnalysis -XX:+PrintGC -XX:-EliminateAllocations
 * @author shkstart  shkstart@126.com
 * @create 2020  12:01
 */
public class ScalarReplace {
    public static class User {
        public int id;
        public String name;
    }

    public static void alloc() {
        User u = new User();//未发生逃逸
        u.id = 5;
        u.name = "www.atguigu.com";
    }

    public static void main(String[] args) {
        long start = System.currentTimeMillis();
        for (int i = 0; i < 10000000; i++) {
            alloc();
        }
        long end = System.currentTimeMillis();
        System.out.println("花费的时间为： " + (end - start) + " ms");
    }
}
```

```java
 -Xmx100m -Xms100m -XX:+DoEscapeAnalysis -XX:+PrintGC -XX:-EliminateAllocations   不开启标量替换
```

> 输出：

![image-20220302085032655](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302085032655.png)

``` java
-Xmx100m -Xms100m -XX:+DoEscapeAnalysis -XX:+PrintGC -XX:+EliminateAllocations   开启标量替换
```

> 输出：

![image-20220302085125590](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302085125590.png)

![image-20220302090317096](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302090317096.png)

`我们的Hotspot虚拟机默认就是启动Server模式的`

![image-20220302090814856](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302090814856.png)

### 逃逸分析总结

![image-20220302090344123](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302090344123.png)

==**也就是说看JVM的实现者，想要设计成某些对象通过逃逸分析分配到栈中完全可以实现，但是我们的HotSpot虚拟机则没有实现基于逃逸分析机制将对象分配到栈中，而刚刚的案例开启了标量替换效率却明显提高，是因为标量替换HotSpot虚拟机应用了，所以对于HotSpot虚拟机来说，一切的对象都被分配到堆区**==

### 堆总结

![image-20220302090402242](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302090402242.png)

## 堆、栈、方法区的交互关系

![image-20220302094812597](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302094812597.png)

![image-20220302094829771](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302094829771.png)

`整个类的结构加载到方法区`

## 方法区

> 方法区官方文档说明：

![image-20220302095129133](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302095129133.png)

![image-20220302100316374](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302100316374.png)

==**把上面官方文档的英文翻译就得到下面结论**==

![image-20220302100331753](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302100331753.png)

> 代码演示：

![image-20220302101239742](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302101239742.png)

==**光是这一个运行这个类（那么少代码），就加载了1000多个类在方法区**==

![image-20220302101346875](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302101346875.png)



### HotSpot中方法区的演进

![image-20220302111715325](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302111715325.png)

`在HotSpot虚拟机中，方法区等价于永久代和元空间。 方法区是jvm的一个规范，类似一个接口的作用，永久代和元空间是hotspot对于这个接口的落地实现`

==**之所以用永久代不是一个好的idea是因为永久代用的是jvm的内存，一般没有本地内存那么大，而元空间用的是本地内存，不容易报OOM**==

![image-20220302111726586](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302111726586.png)



![image-20220302111739116](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220302111739116.png)



### 设置方法区大小以及OOM

![image-20220303110954954](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220303110954954.png)

> 案例：

```
package com.sgg.Method;

/**
 *  测试设置方法区大小参数的默认值
 *
 *  jdk7及以前：
 *  -XX:PermSize=100m -XX:MaxPermSize=100m
 *
 *  jdk8及以后：
 *  -XX:MetaspaceSize=100m  -XX:MaxMetaspaceSize=100m
 * @author shkstart  shkstart@126.com
 * @create 2020  12:16
 */
public class MethodAreaDemo {
    public static void main(String[] args) {
        System.out.println("start...");
       try {
           Thread.sleep(1000000);
       } catch (InterruptedException e) {
           e.printStackTrace();
        }

        System.out.println("end...");
    }
}
```

idea配置jdk1.8

![image-20220303111125831](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220303111125831.png)

改成7就可以显示方法区的大小啦  大小为20.75m

配置jdk8的话 要用这条指令查看   大小约等于21m

![image-20220303111416718](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220303111416718.png)

![image-20220303112020768](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220303112020768.png)



> OOM：

测试代码：

```java
package com.sgg.Method;

import com.sun.xml.internal.ws.org.objectweb.asm.ClassWriter;
import jdk.internal.org.objectweb.asm.Opcodes;

/**
 * jdk6/7中：
 * -XX:PermSize=10m -XX:MaxPermSize=10m
 *
 * jdk8中：
 * -XX:MetaspaceSize=10m -XX:MaxMetaspaceSize=10m
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  22:24
 */
public class OOMTest extends ClassLoader {
    public static void main(String[] args) {
        int j = 0;
        //动态代理
        try {
            OOMTest test = new OOMTest();
            for (int i = 0; i < 10000; i++) {
                //创建ClassWriter对象，用于生成类的二进制字节码
                ClassWriter classWriter = new ClassWriter(0);
                //指明jdk版本号，修饰符，类名，包名，父类，接口
                classWriter.visit(Opcodes.V1_8, Opcodes.ACC_PUBLIC, "Class" + i, null, "java/lang/Object", null);
                //返回byte[]
                byte[] code = classWriter.toByteArray();
                //类的加载
                test.defineClass("Class" + i, code, 0, code.length);//Class对象
                j++;
            }
        } finally {
            System.out.println(j);
        }
    }
}
```

不设置的情况下，默认使用动态扩容

![image-20220303162854817](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220303162854817.png)



设置一下：`-XX:MetaspaceSize=10m -XX:MaxMetaspaceSize=10m`

![image-20220303163422448](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220303163422448.png)

> 如何解决这些OOM

![image-20220303163046871](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220303163046871.png)

### 方法区的内部结构：

![image-20220305162741473](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220305162741473.png)

![image-20220305162752043](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220305162752043.png)

![image-20220305162801779](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220305162801779.png)

![image-20220305162812124](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220305162812124.png)

![image-20220305162830237](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220305162830237.png)

> 案例：

```
package com.sgg.Method;

import java.io.Serializable;

/**
 * 测试方法区的内部构成
 * @author shkstart  shkstart@126.com
 * @create 2020  23:39
 */
public class MethodInnerStrucTest extends Object implements Comparable<String>,Serializable {
    //属性
    public int num = 10;
    private static String str = "测试方法的内部结构";
    //构造器
    //方法
    public void test1(){
        int count = 20;
        System.out.println("count = " + count);
    }
    public static int test2(int cal){
        int result = 0;
        try {
            int value = 30;
            result = value / cal;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return result;
    }

    @Override
    public int compareTo(String o) {
        return 0;
    }
}
```

反编译：`javap -v -p MethodInnerStrucTest.class>test.txt`  : 将这个类反编译后的字节码文件内容复制到test.txt文件中

不加p 私有结构就看不到了

![image-20220307182215443](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307182215443.png)

![image-20220307183733896](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307183733896.png)

![image-20220307183953612](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307183953612.png)

![image-20220307184238793](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307184238793.png)

![image-20220307184319362](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307184319362.png)

![image-20220307185805280](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307185805280.png)

![image-20220307190157409](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307190157409.png)

在这.txt文件中看不到类加载器的身影（因为仅仅是源文件反编译的结果），字节码文件被类加载器来加载后方法区中才会存在类加载器的信息（加载这个类的类加载器） 这个类加载器也记录了自己是否加载过这个类   `彼此互相记录`



#### non-final的类变量

![image-20220307191351100](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307191351100.png)

```
package com.sgg.Method;

/**
 * non-final的类变量
 * @author shkstart  shkstart@126.com
 * @create 2020  20:37
 */
public class MethodAreaTest {
    public static void main(String[] args) {
        Order order = null;
        order.hello();
        System.out.println(order.count);
    }
}

class Order {
    public static int count = 1;
    public static final int number = 2;


    public static void hello() {
        System.out.println("hello!");
    }
}
```

![image-20220307191552043](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307191552043.png)

#### 全局变量：static final

![image-20220307191852268](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307191852268.png)

`查看反编译的Order字节码文件：`

![image-20220307191201176](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307191201176.png)

### class文件中常量池的理解

![image-20220307194909871](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307194909871.png)



![image-20220307194923586](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307194923586.png)



![image-20220307194937639](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307194937639.png)

> 为什么需要常量池：

![image-20220307194955832](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307194955832.png)



> 常量池中有什么：

![image-20220307195015069](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307195015069.png)![image-20220307195025005](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307195025005.png)



> 案例：

```
package com.sgg.Method;

import java.io.Serializable;

/**
 * 测试方法区的内部构成
 * @author shkstart  shkstart@126.com
 * @create 2020  23:39
 */
public class MethodInnerStrucTest extends Object implements Comparable<String>,Serializable {
    //属性
    public int num = 10;
    private static String str = "测试方法的内部结构";
    //构造器
    //方法
    public void test1(){
        int count = 20;
        System.out.println("count = " + count);
    }
    public static int test2(int cal){
        int result = 0;
        try {
            int value = 30;
            result = value / cal;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return result;
    }

    @Override
    public int compareTo(String o) {
        return 0;
    }
}
```

> test1()

![image-20220307200218431](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307200218431.png)

![image-20220307200227505](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307200227505.png)

![image-20220307200238500](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307200238500.png)

![image-20220307200250086](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307200250086.png)![image-20220307200309333](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307200309333.png)

![image-20220307200322658](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307200322658.png)

![image-20220307200331956](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307200331956.png)

![，](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307200341647.png)

![image-20220307201912065](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307201912065.png)

`符号引用就是，举个例子吧 String类的符号引用就是String字面量,运行时常量池String类对应的就不再是一个String字面量了 而是String这个类的真实地址`

### 方法区的使用案例

> 演示代码：

```
package com.sgg.Method;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  14:28
 */
public class MethodAreaDemo {
    public static void main(String[] args) {
        int x = 500;
        int y = 100;
        int a = x / y;
        int b = 50;
        System.out.println(a + b);
    }
}
```

![image-20220307210710663](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307210710663.png)

![image-20220307210722114](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307210722114.png)

![image-20220307210734520](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307210734520.png)

![image-20220307210746587](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307210746587.png)

![image-20220307210756618](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307210756618.png)

![image-20220307210809540](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307210809540.png)

![image-20220307210821265](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307210821265.png)

![image-20220307210830793](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307210830793.png)

![image-20220307210840166](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307210840166.png)

![image-20220307210848575](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307210848575.png)

![image-20220307210859698](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307210859698.png)

![image-20220307210920545](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307210920545.png)

![image-20220307210936670](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307210936670.png)

![image-20220307210946074](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307210946074.png)

![image-20220307210953850](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307210953850.png)



### 方法区在jdk6/7/8中的演进过程：

![image-20220307223519328](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307223519328.png)

![image-20220307223529823](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307223529823.png)

![image-20220307223540315](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307223540315.png)

> 永久代为什么被元空间替换：

![image-20220307223616939](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307223616939.png)

#### StringTable 为什么要调整位置

![image-20220307230451206](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220307230451206.png)



#### 证明静态变量存在哪？

> 案例：

```
package com.sgg.Method;

/**
 * 结论：
 * 静态引用对应的对象实体始终都存在堆空间
 *
 * jdk7：
 * -Xms200m -Xmx200m -XX:PermSize=300m -XX:MaxPermSize=300m -XX:+PrintGCDetails
 * jdk 8：
 * -Xms200m -Xmx200m -XX:MetaspaceSize=300m -XX:MaxMetaspaceSize=300m -XX:+PrintGCDetails
 * @author shkstart  shkstart@126.com
 * @create 2020  21:20
 */
public class StaticFieldTest {
    private static byte[] arr = new byte[1024 * 1024 * 100];//100MB

    public static void main(String[] args) {
        System.out.println(StaticFieldTest.arr);

//        try {
//            Thread.sleep(1000000);
//        } catch (InterruptedException e) {
//            e.printStackTrace();
//        }
    }
}

```

`JDK1.7`

![image-20220308082637920](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308082637920.png)

`JDK1.8:`

![image-20220308082714427](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308082714427.png)

> 案例二：

```
package com.sgg.Method;

/**
 * 《深入理解Java虚拟机》中的案例：
 * staticObj、instanceObj、localObj存放在哪里？
 * @author shkstart  shkstart@126.com
 * @create 2020  11:39
 */
public class StaticObjTest {
    static class Test {
        static ObjectHolder staticObj = new ObjectHolder();
        ObjectHolder instanceObj = new ObjectHolder();

        void foo() {
            ObjectHolder localObj = new ObjectHolder();
            System.out.println("done");
        }
    }

    private static class ObjectHolder {
    }

    public static void main(String[] args) {
        Test test = new Test();
        test.foo();
    }
}
```

![image-20220308081907948](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308081907948.png)

![image-20220308081925741](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308081925741.png)

![image-20220308081943007](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308081943007.png)

### 方法区的垃圾回收行为

![image-20220308084411595](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308084411595.png)

==**言下之意就是：方法区的类型很难回收**==

![image-20220308084425808](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308084425808.png)

==**只要常量池中的常量没有被任何地方引用，就可以被回收**==

![image-20220308084445758](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308084445758.png)

## 总结：

![image-20220308090047579](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308090047579.png)`Full GC会对整个堆和方法区进行回收`

## 对象的实例化

### 对象实例化的几种方式：

![image-20220308092414180](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308092414180.png)

`创建对象的步骤5：设置对象的对象头`

`创建对象的步骤6：执行init方法（构造方法）进行初始化`



### 字节码角度看对象的创建过程

> 案例代码：

```
package com.sgg.createObject;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  17:16
 */
public class ObjectTest {
    public static void main(String[] args) {
        Object obj = new Object();
    }
}
```

`反编译后的字节码文件`

![image-20220308095309616](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308095309616.png)

> 说明：

先执行`new`先判断是否对Object这个类进行了加载 没有加载就先去加载  然后再堆空间为对象开辟空间

`dup` 复制引用 （在操作数栈中存在两个对象的引用地址）

接着执行Object的构造方法 把实体放入开辟好的堆空间中

将Object实例的引用存储到局部变量表中



### 创建对象的步骤

上面的图已经对对象创建的六个步骤进行了描述

==这里在补充说明一下：==

![image-20220308102636723](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308102636723.png)

![image-20220308102819821](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308102819821.png)

![image-20220308103045733](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308103045733.png)

![image-20220308103117138](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308103117138.png)

![image-20220308103129735](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308103129735.png)

![image-20220308103150689](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308103150689.png)

![image-20220308103221379](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308103221379.png)

> 案例：

```
package com.sgg.createObject;

/**
 * 测试对象实例化的过程
 *  ① 加载类元信息 - ② 为对象分配内存 - ③ 处理并发问题  - ④ 属性的默认初始化（零值初始化）
 *  - ⑤ 设置对象头的信息 - ⑥ 属性的显式初始化、代码块中初始化、构造器中初始化
 *
 *
 *  给对象的属性赋值的操作：
 *  ① 属性的默认初始化 - ② 显式初始化 / ③ 代码块中初始化 - ④ 构造器中初始化
 * @author shkstart  shkstart@126.com
 * @create 2020  17:58
 */

public class Customer{
    int id = 1001;
    String name;
    Account acct;

    {
        name = "匿名客户";
    }
    public Customer(){
        acct = new Account();
    }

}
class Account{

}
```

![image-20220308104659196](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308104659196.png)

### 对象的内存布局

![image-20220308113824423](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308113824423.png)

`说明:哈希值的作用是使得栈中的引用能指向所对应的堆空间的地址`

`GC分代年龄的作用是用来标记已经被gc了多少次`

`锁状态标志用来解决线程同步的问题`

`对齐填充其实没什么用的(不用过多的去理解)`

> 堆空间对象的大致图：

![image-20220308114402861](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308114402861.png)

> 案例代码和图示：

```
package com.sgg.createObject;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  18:42
 */
public class CustomerTest {
    public static void main(String[] args) {
        Customer cust = new Customer();
    }
}
```

![image-20220308113925344](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308113925344.png)



### 对象的访问定位

![image-20220308122616906](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308122616906.png)

> 图示：

![image-20220308122636530](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308122636530.png)

> 句柄访问：

![image-20220308122700349](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308122700349.png)



> 直接指针：

![image-20220308122724296](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308122724296.png)



## 直接内存（了解即可）

### 直接内存的初体验：

`jdk8原空间使用的就是本地内存(直接内存)`

> 直接内存概述：

![image-20220308125523420](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308125523420.png)

> 测试案例：

```java
package com.sgg.memory;

import java.nio.ByteBuffer;
import java.util.Scanner;

/**
 *  IO                  NIO (New IO / Non-Blocking IO)
 *  byte[] / char[]     Buffer
 *  Stream              Channel
 *
 * 查看直接内存的占用与释放
 * @author shkstart  shkstart@126.com
 * @create 2020  0:22
 */
public class BufferTest {
    private static final int BUFFER = 1024 * 1024 * 1024;//1GB

    public static void main(String[] args){
        //直接分配本地内存空间
        ByteBuffer byteBuffer = ByteBuffer.allocateDirect(BUFFER); //分配一个G的大小
        System.out.println("直接内存分配完毕，请求指示！");

        Scanner scanner = new Scanner(System.in);
        scanner.next();

        System.out.println("直接内存开始释放！");
        byteBuffer = null;
        System.gc();
        scanner.next();
    }
}
```

```
public static ByteBuffer allocateDirect(int capacity) {
    return new DirectByteBuffer(capacity);
}
```

> 启动后查看占用本地内存情况：

![image-20220308125937827](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308125937827.png)

![image-20220308125859267](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308125859267.png)

> 释放内存后：

![image-20220308130217220](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308130217220.png)

> 为什么NIO更快：

![image-20220308163317056](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308163317056.png)

![image-20220308163345948](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308163345948.png)

> 测试代码：分别用io和nio将f盘的电影拷贝三份 查看所花费的时间

```java
package com.sgg.memory;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  0:04
 */
public class BufferTest1 {

    private static final String TO = "F:\\test\\异界BD中字.mp4";
    private static final int _100Mb = 1024 * 1024 * 100;

    public static void main(String[] args) {
        long sum = 0;
        String src = "F:\\test\\异界BD中字.mp4";
        for (int i = 0; i < 3; i++) {
            String dest = "F:\\test\\异界BD中字_" + i + ".mp4";
//            sum += io(src,dest);//54606
            sum += directBuffer(src,dest);//50244
        }

        System.out.println("总花费的时间为：" + sum );
    }

    private static long directBuffer(String src,String dest) {
        long start = System.currentTimeMillis();

        FileChannel inChannel = null;
        FileChannel outChannel = null;
        try {
            inChannel = new FileInputStream(src).getChannel();
            outChannel = new FileOutputStream(dest).getChannel();

            ByteBuffer byteBuffer = ByteBuffer.allocateDirect(_100Mb);
            while (inChannel.read(byteBuffer) != -1) {
                byteBuffer.flip();//修改为读数据模式
                outChannel.write(byteBuffer);
                byteBuffer.clear();//清空
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (inChannel != null) {
                try {
                    inChannel.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
            if (outChannel != null) {
                try {
                    outChannel.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
        }

        long end = System.currentTimeMillis();
        return end - start;

    }

    private static long io(String src,String dest) {
        long start = System.currentTimeMillis();

        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            fis = new FileInputStream(src);
            fos = new FileOutputStream(dest);
            byte[] buffer = new byte[_100Mb];
            while (true) {
                int len = fis.read(buffer);
                if (len == -1) {
                    break;
                }
                fos.write(buffer, 0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fis != null) {
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
            if (fos != null) {
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
        }


        long end = System.currentTimeMillis();

        return end - start;
    }
}
```

### 直接内存的OOM与内存大小的设置

> 直接内存的弊端：

![image-20220308201355539](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308201355539.png)

> 案例代码：

```
package com.sgg.memory;

import java.nio.ByteBuffer;
import java.util.ArrayList;

/**
 * 本地内存的OOM:  OutOfMemoryError: Direct buffer memory
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  0:09
 */
public class BufferTest2 {
    private static final int BUFFER = 1024 * 1024 * 20;//20MB

    public static void main(String[] args) {
        ArrayList<ByteBuffer> list = new ArrayList<>();

        int count = 0;
        try {
            while(true){
                ByteBuffer byteBuffer = ByteBuffer.allocateDirect(BUFFER);
                list.add(byteBuffer);
                count++;
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        } finally {
            System.out.println(count);
        }


    }
}
```

> 输出：

![image-20220308200046028](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308200046028.png)

![image-20220308201424204](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308201424204.png)

## 执行引擎

### 执行引擎概述：

![image-20220308203831184](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308203831184.png)

![image-20220308203842977](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308203842977.png)

![image-20220308203854057](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308203854057.png)

![image-20220308203907775](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308203907775.png)

==**总结就是**==

操作系统只能识别机器指令，执行引擎将字节码翻译成机器指令操作系统(cpu)来执行

`执行引擎分为两种行为 一种是解释执行 一种是编译（后端编译） 源文件编译成字节码文件属于前端编译`



### java程序的编译和解释运行的理解

![image-20220308205358662](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308205358662.png)

`上面的黄色是前端编译`

`抽象语法树就是字节码文件`

字节码解释器的方式是绿色

jit即时编译器的方式是蓝色

==**既有解释器又有编译器 所以称之为半编译半解释形语言**==

​                                                                                

![image-20220308205408060](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308205408060.png)

![image-20220308205418815](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308205418815.png)

![image-20220308205427898](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308205427898.png)

> 总结：

`目的代码就是机器指令`

==**jit缓存 是机器指令 缓存起来 执行效率就高了**==



### 机器码_汇编 _高级语言

> 机器码的解释：       

==**机器语言就是类似01010101**==                    

![image-20220308213314366](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308213314366.png)

> 指令和指令集：

![image-20220308213340135](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308213340135.png)

> 汇编语言：

![image-20220308213416098](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308213416098.png)

> 高级语言：

![image-20220308213522521](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308213522521.png)

![image-20220308213556809](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308213556809.png)

> C/C++源程序执行过程

![image-20220308213725031](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308213725031.png)

> 全流程：

![image-20220308211011524](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308211011524.png)

==**重点：**==

`任何符合jvm规范的程序（不一定是java程序）都能在jvm上面执行`



### 解释器

![image-20220308220625838](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308220625838.png)

==**为什么不java源文件直接被jvm识别，这样就不用多了编译成字节码这个环节（不同的操作系统上也做出一定的改变，不也能实现java程序的跨平台性吗？）  直接将java源程序在jvm上跑 这样的设计思路也是行得通的**==

==**一条龙要求太苛刻了     不同的人干不同的事情**== 可能实现起来jvm直接将java源文件翻译成机器指令需要20分钟 而上面只需要15分钟

![image-20220308220638567](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308220638567.png)

> 现状：

![image-20220308220732517](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308220732517.png)

### JIT编译器

> java代码的执行分类：

![image-20220308224136287](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308224136287.png)

![image-20220308224201637](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308224201637.png)

> JVM为什么是解释器和JIT编译器并存的架构：

![image-20220308224233730](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308224233730.png)

==**解释器响应速度快 逐条翻译**==

==**JIT先全部编译成机器语言先（在这期间不会去执行，因此响应慢，但是翻译完后，响应速度就快了）**==

> Hot Spot的执行方式：

![image-20220308224304865](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308224304865.png)

> 案例：

![image-20220308224317998](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308224317998.png)

> JIT编译器：

![image-20220308224351383](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308224351383.png)



#### 热点代码探测确定何时使用JIT

解释器和JIT合作 那如何合作呢 哪些字节码用jit哪些用解释器呢？

![image-20220308231925208](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308231925208.png)

![image-20220308231937599](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308231937599.png)

> 方法调用计数器：

![image-20220308232002474](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308232002474.png)

![image-20220308232011040](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308232011040.png)

![image-20220308232021694](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308232021694.png)

> 回边计数器：

![image-20220308232039732](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308232039732.png)

![image-20220308232100315](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220308232100315.png)

#### JIT编译器的解释器的效率对比:

![image-20220309105048980](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309105048980.png)

![image-20220309105100464](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309105100464.png)

![image-20220309105113083](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309105113083.png)

![image-20220309105123970](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309105123970.png)

![image-20220309105148005](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309105148005.png)

```
package com.sgg.engine;

/**
 * 测试解释器模式和JIT编译模式
 *  -Xint  : 6520ms
 *  -Xcomp : 950ms
 *  -Xmixed : 936ms
 * @author shkstart  shkstart@126.com
 * @create 2020  12:40
 */
public class IntCompTest {
    public static void main(String[] args) {

        long start = System.currentTimeMillis();

        testPrimeNumber(1000000);

        long end = System.currentTimeMillis();

        System.out.println("花费的时间为：" + (end - start));

    }

    public static void testPrimeNumber(int count){
        for (int i = 0; i < count; i++) {
            //计算100以内的质数
            label:for(int j = 2;j <= 100;j++){
                for(int k = 2;k <= Math.sqrt(j);k++){
                    if(j % k == 0){
                        continue label;
                    }
                }
                //System.out.println(j);
            }

        }
    }
}
```

我们的电脑都是64位因此只支持server的模式

我们也可以在命令行修改执行引擎的执行模式

![image-20220309105403447](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309105403447.png)



![image-20220309110035044](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309110035044.png)

![image-20220309110045523](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309110045523.png)

`.java-->.class --> .so`



## String Table

### String的基本特性：

![image-20220309114745562](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309114745562.png)

> JDK1.8：

![image-20220309115223822](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309115223822.png)

![image-20220309114804045](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309114804045.png)

![image-20220309114815100](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309114815100.png)

`实际数据分析 String是我们堆空间的主要对象 并且大部分是拉丁字符 用一个字节就能存储,而一个char占用两个字节那么一半的空间就被浪费了 但是中文一个字就是两个字节来存储的`  那我们还是用byte数组来存储 拉丁字符等就用一个字节存，中文用两个字节存储

类似String的结构比如buffer和builder也做出了修改



> String的不可变性：

![image-20220309114836819](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309114836819.png)

案例代码：

```java
package com.sgg.StringTable;

import org.junit.Test;

/**
 * String的基本使用:体现String的不可变性
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  23:42
 */
public class StringTest1 {
    @Test
    public void test1() {
        String s1 = "abc";//字面量定义的方式，"abc"存储在字符串常量池中
        String s2 = "abc";
        s1 = "hello";

        System.out.println(s1 == s2);//判断地址：true  --> false

        System.out.println(s1);//
        System.out.println(s2);//abc

    }

    @Test
    public void test2() {
        String s1 = "abc";
        String s2 = "abc";
        s2 += "def";
        System.out.println(s2);//abcdef
        System.out.println(s1);//abc
    }

    @Test
    public void test3() {
        String s1 = "abc";
        String s2 = s1.replace('a', 'm');
        System.out.println(s1);//abc
        System.out.println(s2);//mbc
    }
}
```

### String底层Hashtable结构的说明

`字符串常量池中是不会存储相同内容的字符串的`

![image-20220309124526545](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309124526545.png)

`hashtable本质就是一个hashmap`

> 案例测试：

```
package com.sgg.StringTable;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

/**
 *  -XX:StringTableSize=1009
 * @author shkstart  shkstart@126.com
 * @create 2020  23:53
 */
public class StringTest2 {
    public static void main(String[] args) {
        //测试StringTableSize参数
        System.out.println("我来打个酱油");
        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

     /*   BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader("words.txt"));
            long start = System.currentTimeMillis();
            String data;
            while((data = br.readLine()) != null){
                data.intern(); //如果字符串常量池中没有对应data的字符串的话，则在常量池中生成
            }

            long end = System.currentTimeMillis();

            System.out.println("花费的时间为：" + (end - start));//1009:143ms  100009:47ms
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(br != null){
                try {
                    br.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
        }
    }*/
    }
}
```

在JDK8中我们把长度设置为1000  指令：`-XX:StringTableSize=1000`

==**输出：**==

![image-20220309125018719](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309125018719.png)

`说明最小是1009`

jdk6想改成几都可以最小值没有限制

> 接下来看看hashtable对时间的影响：

先执行下面这个类：

```
package com.sgg.StringTable;

import java.io.FileWriter;
import java.io.IOException;

/**
 * 产生10万个长度不超过10的字符串，包含a-z,A-Z
 * @author shkstart  shkstart@126.com
 * @create 2020  23:58
 */
public class GenerateString {
    public static void main(String[] args) throws IOException {
        FileWriter fw =  new FileWriter("words.txt");

        for (int i = 0; i < 100000; i++) {
            //1 - 10
           int length = (int)(Math.random() * (10 - 1 + 1) + 1);
            fw.write(getString(length) + "\n");
        }

        fw.close();
    }

    public static String getString(int length){
        String str = "";
        for (int i = 0; i < length; i++) {
            //65 - 90, 97-122
            int num = (int)(Math.random() * (90 - 65 + 1) + 65) + (int)(Math.random() * 2) * 32;
            str += (char)num;
        }
        return str;
    }
}
```

接着执行：

```
package com.sgg.StringTable;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

/**
 *  -XX:StringTableSize=1009
 * @author shkstart  shkstart@126.com
 * @create 2020  23:53
 */
public class StringTest2 {
    public static void main(String[] args) {
 /*       //测试StringTableSize参数
        System.out.println("我来打个酱油");
        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }*/

        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader("words.txt"));
            long start = System.currentTimeMillis();
            String data;
            while((data = br.readLine()) != null){
                data.intern(); //如果字符串常量池中没有对应data的字符串的话，则在常量池中生成
            }

            long end = System.currentTimeMillis();

            System.out.println("花费的时间为：" + (end - start));//1009:143ms  100009:47ms
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(br != null){
                try {
                    br.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
        }
    }
    }
```

==**data.intern //如果字符串常量池中没有对应的data的字符串的话，则在常量池中生成  其实就是通过String table来判断**==

不同长度对应的时间：//1009:143ms  100009:47ms

> 案例代码：==常考面试题==

```
package com.sgg.StringTable;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  23:44
 */
public class StringExer {
    String str = new String("good");
    char[] ch = {'t', 'e', 's', 't'};

    public void change(String str, char ch[]) {
        str = "test ok";
        ch[0] = 'b';
    }

    public static void main(String[] args) {
        StringExer ex = new StringExer();
        ex.change(ex.str, ex.ch);
        System.out.println(ex.str);//good
        System.out.println(ex.ch);//best
    }

}
```

### String内存结构的分配位置

> 代码案例：

```java
package com.sgg.StringTable;

import java.util.HashSet;
import java.util.Set;

/**
 * jdk6中：
 * -XX:PermSize=6m -XX:MaxPermSize=6m -Xms6m -Xmx6m
 *
 * jdk8中：
 * -XX:MetaspaceSize=6m -XX:MaxMetaspaceSize=6m -Xms6m -Xmx6m
 * @author shkstart  shkstart@126.com
 * @create 2020  0:36
 */
public class StringTest3 {
    public static void main(String[] args) {
        //使用Set保持着常量池引用，避免full gc回收常量池行为
        Set<String> set = new HashSet<String>();
        //在short可以取值的范围内足以让6MB的PermSize或heap产生OOM了。
        short i = 0;
        while(true){
            set.add(String.valueOf(i++).intern());
        }
    }
}
```

> jdk6:

![image-20220309142214452](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309142214452.png)

> jdk8

![image-20220309142523545](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309142523545.png)

![image-20220309141813326](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309141813326.png)

![image-20220309141820329](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309141820329.png)

![image-20220309141830103](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309141830103.png)

![image-20220309141841580](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309141841580.png)

==**①：permSize默认值比较小 ②：永久代垃圾回收频率低**==



### 两个案例熟悉String的基本操作

> 案例一：

```
package com.sgg.StringTable.java2;

import org.junit.Test;

import javax.annotation.processing.SupportedSourceVersion;
import java.sql.SQLOutput;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  0:49
 */
public class StringTest4 {
    public static void main(String[] args) {
        System.out.println();//2154
        System.out.println("1");//2155
        System.out.println("2");
        System.out.println("3");
        System.out.println("4");
        System.out.println("5");
        System.out.println("6");
        System.out.println("7");
        System.out.println("8");
        System.out.println("9");
        System.out.println("10");//2164
        //如下的字符串"1" 到 "10"不会再次加载
        System.out.println("1");//2165
        System.out.println("2");//2165
        System.out.println("3");
        System.out.println("4");
        System.out.println("5");
        System.out.println("6");
        System.out.println("7");
        System.out.println("8");
        System.out.println("9");
        System.out.println("10");//2165
    }
}
```

==**查看内存中String的总量（打debug即可 点开内存一行即可看到）**==

![image-20220309144424327](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309144424327.png)

![image-20220309144934525](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309144934525.png)

> 案例二：

```
package com.sgg.StringTable.java2;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  0:51
 */
class Memory {
    public static void main(String[] args) {//line 1
        int i = 1;//line 2
        Object obj = new Object();//line 3
        Memory mem = new Memory();//line 4
        mem.foo(obj);//line 5
    }//line 9

    private void foo(Object param) {//line 6
        String str = param.toString();//line 7
        System.out.println(str);
    }//line 8
}
```

![image-20220309145000327](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309145000327.png)



### 字符串的拼接操作

![image-20220309151216340](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309151216340.png)

```
@Test
public void test1(){
    String s1 = "a" + "b" + "c";//编译期优化：等同于"abc"
    String s2 = "abc"; //"abc"一定是放在字符串常量池中，将此地址赋给s2
    /*
     * 最终.java编译成.class,再执行.class
     * String s1 = "abc";
     * String s2 = "abc"
     */
    System.out.println(s1 == s2); //true
    System.out.println(s1.equals(s2)); //true
}
```

![image-20220309151246112](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309151246112.png)

==将常量池的abc放到局部变量表中==

反编译后的文件 在idea中可以直接看到![image-20220309150141649](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309150141649.png)

> 案例二：

```ava
@Test
public void test2(){
    String s1 = "javaEE";
    String s2 = "hadoop";

    String s3 = "javaEEhadoop";
    String s4 = "javaEE" + "hadoop";//编译期优化
    //如果拼接符号的前后出现了变量，则相当于在堆空间中new String()，具体的内容为拼接的结果：javaEEhadoop
    String s5 = s1 + "hadoop";
    String s6 = "javaEE" + s2;
    String s7 = s1 + s2;

    System.out.println(s3 == s4);//true
    System.out.println(s3 == s5);//false
    System.out.println(s3 == s6);//false
    System.out.println(s3 == s7);//false
    System.out.println(s5 == s6);//false
    System.out.println(s5 == s7);//false
    System.out.println(s6 == s7);//false
    //intern():判断字符串常量池中是否存在javaEEhadoop值，如果存在，则返回常量池中javaEEhadoop的地址；
    //如果字符串常量池中不存在javaEEhadoop，则在常量池中加载一份javaEEhadoop，并返回次对象的地址。
    String s8 = s6.intern();
    System.out.println(s3 == s8);//true
}
```

### 字符串变量拼接操作的底层原理

> 代码案例：

```java
@Test
public void test3(){
    String s1 = "a";
    String s2 = "b";
    String s3 = "ab";
    /*
    如下的s1 + s2 的执行细节：(变量s是我临时定义的）
    ① StringBuilder s = new StringBuilder();
    ② s.append("a")
    ③ s.append("b")
    ④ s.toString()  --> 约等于 new String("ab")

    补充：在jdk5.0之后使用的是StringBuilder,在jdk5.0之前使用的是StringBuffer
     */
    String s4 = s1 + s2;//
    System.out.println(s3 == s4);//false
}
```

> 查看字节码指令：

![image-20220309154123370](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309154123370.png)

```java
/*
1. 字符串拼接操作不一定使用的是StringBuilder!
   如果拼接符号左右两边都是字符串常量或常量引用，则仍然使用编译期优化，即非StringBuilder的方式。
2. 针对于final修饰类、方法、基本数据类型、引用数据类型的量的结构时，能使用上final的时候建议使用上。
 */
@Test
public void test4(){
    final String s1 = "a";
    final String s2 = "b";
    String s3 = "ab";
    String s4 = s1 + s2;
    System.out.println(s3 == s4);//true
}
```

### 拼接操作与append操作的效率对比

```
 /*
    体会执行效率：通过StringBuilder的append()的方式添加字符串的效率要远高于使用String的字符串拼接方式！
    详情：① StringBuilder的append()的方式：自始至终中只创建过一个StringBuilder的对象
          使用String的字符串拼接方式：创建过多个StringBuilder和String的对象
         ② 使用String的字符串拼接方式：内存中由于创建了较多的StringBuilder和String的对象，内存占用更大；如果进行GC，需要花费额外的时间。

     改进的空间：在实际开发中，如果基本确定要前前后后添加的字符串长度不高于某个限定值highLevel的情况下,建议使用构造器实例化：
               StringBuilder s = new StringBuilder(highLevel);//new char[highLevel]
     */
    @Test
    public void test6(){

        long start = System.currentTimeMillis();

//        method1(100000);//4014
        method2(100000);//7

        long end = System.currentTimeMillis();

        System.out.println("花费的时间为：" + (end - start));
    }

    public void method1(int highLevel){
        String src = "";
        for(int i = 0;i < highLevel;i++){
            src = src + "a";//每次循环都会创建一个StringBuilder、String
        }
//        System.out.println(src);

    }

    public void method2(int highLevel){
        //只需要创建一个StringBuilder
        StringBuilder src = new StringBuilder();
        for (int i = 0; i < highLevel; i++) {
            src.append("a");
        }
//        System.out.println(src);
    }
}
```

![image-20220309193115961](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309193115961.png)

![image-20220309194010428](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309194010428.png)



### intern()的理解

![image-20220309225819309](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309225819309.png)

![image-20220309225838403](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309225838403.png)

> 代码案例：

```java
package com.sgg.StringTable.java3;

import org.junit.Test;

/**
 * 如何保证变量s指向的是字符串常量池中的数据呢？
 * 有两种方式：
 * 方式一： String s = "shkstart";//字面量定义的方式
 * 方式二： 调用intern()
 *         String s = new String("shkstart").intern();
 *         String s = new StringBuilder("shkstart").toString().intern();
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  18:49
 */

```

### new String到底创建了几个对象

```
package com.sgg.StringTable.java3;

/**
 * 题目：
 * new String("ab")会创建几个对象？看字节码，就知道是两个。
 *     一个对象是：new关键字在堆空间创建的
 *     另一个对象是：字符串常量池中的对象"ab"。 字节码指令：ldc
 *
 *
 * 思考：
 * new String("a") + new String("b")呢？
 *  对象1：new StringBuilder()
 *  对象2： new String("a")
 *  对象3： 常量池中的"a"
 *  对象4： new String("b")
 *  对象5： 常量池中的"b"
 *
 *  深入剖析： StringBuilder的toString():
 *      对象6 ：new String("ab")
 *       强调一下，toString()的调用，在字符串常量池中，没有生成"ab"
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  20:38
 */
public class StringNewTest {
    public static void main(String[] args) {
        String str = new String("ab");

  /*      String str = new String("a") + new String("b");*/
    }
}
```

![image-20220309232146994](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309232146994.png)

![image-20220309233310392](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220309233310392.png)==**StringBuilder的toString的字节码文件**==

![image-20220310081950765](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310081950765.png)

==**可以看到StringBuilder的toString方法，在字符串常量池中，没有生成"ab"**==



### 关于intern()的面试难题

> 案例代码：

```
package com.sgg.StringTable.java3;

import org.junit.Test;

/**
 * 如何保证变量s指向的是字符串常量池中的数据呢？
 * 有两种方式：
 * 方式一： String s = "shkstart";//字面量定义的方式
 * 方式二： 调用intern()
 *         String s = new String("shkstart").intern();
 *         String s = new StringBuilder("shkstart").toString().intern();
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  18:49
 */
public class StringIntern {
    public static void main(String[] args) {

        String s = new String("1");
        s.intern();//调用此方法之前，字符串常量池中已经存在了"1"
        String s2 = "1";
        System.out.println(s == s2);//jdk6：false   jdk7/8：false


        String s3 = new String("1") + new String("1");//s3变量记录的地址为：new String("11")
        //执行完上一行代码以后，字符串常量池中，是否存在"11"呢？答案：不存在！！
        s3.intern();//在字符串常量池中生成"11"。如何理解：jdk6:创建了一个新的对象"11",也就有新的地址。
                                            //         jdk7:此时常量中并没有创建"11",而是创建一个指向堆空间中new String("11")的地址   JDK7/8中字符串常量池没有new一个新的对象 而是保存堆空间相应实例的地址
        String s4 = "11";//s4变量记录的地址：使用的是上一行代码代码执行时，在常量池中生成的堆中的"11"的地址
        System.out.println(s3 == s4);//jdk6：false  jdk7/8：true
    }


}
```

```
package com.sgg.StringTable.java3;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  22:10
 */
public class StringIntern1 {
    public static void main(String[] args) {
        //StringIntern.java中练习的拓展：
        String s3 = new String("1") + new String("1");//new String("11")
        //执行完上一行代码以后，字符串常量池中，是否存在"11"呢？答案：不存在！！
        String s4 = "11";//在字符串常量池中生成对象"11"
        String s5 = s3.intern();  //返回s4的引用地址
        System.out.println(s3 == s4);//false
        System.out.println(s5 == s4);//true
    }
}
```

==**总结String的inter()的使用：**==

![image-20220310083313443](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310083313443.png)



> 练习题一：

```java
package com.sgg.StringTable.java3;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  20:17
 */
public class StringExer1 {
    public static void main(String[] args) {
     //   String x = "ab";
        String s = new String("a") + new String("b");//new String("ab")
        //在上一行代码执行完以后，字符串常量池中并没有"ab"

        String s2 = s.intern();//jdk6中：在串池中创建一个字符串"ab"
                               //jdk8中：串池中没有创建字符串"ab",而是创建一个引用，指向new String("ab")，将此引用返回

        System.out.println(s2 == "ab");//jdk6:true  jdk8:true
        System.out.println(s == "ab");//jdk6:false  jdk8:true
    }
}
```

![image-20220310090614453](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310090614453.png)![image-20220310090636037](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310090636037.png)

==加上x=“ab”;==

![image-20220310090705630](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310090705630.png)



> 练习二：

```
package com.sgg.StringTable.java3;

/**
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  20:26
 */
public class StringExer2 {
    public static void main(String[] args) {
        String s1 = new String("ab");//执行完以后，会在字符串常量池中会生成"ab"
//        String s1 = new String("a") + new String("b");////执行完以后，不会在字符串常量池中会生成"ab"
        s1.intern();
        String s2 = "ab";
        System.out.println(s1 == s2);
    }
}
```



### intern()空间效率测试

> 代码案例：

```java
package com.sgg.StringTable.java3;

import java.util.Random;

/**
 * 使用intern()测试执行效率：空间使用上
 *
 * 结论：对于程序中大量存在存在的字符串，尤其其中存在很多重复字符串时，使用intern()可以节省内存空间。
 *
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  21:17
 */
public class StringIntern2 {
    static final int MAX_COUNT = 1000 * 10000;
    static final String[] arr = new String[MAX_COUNT];

    public static void main(String[] args) {
        Integer[] data = new Integer[]{1,2,3,4,5,6,7,8,9,10};

        long start = System.currentTimeMillis();
        for (int i = 0; i < MAX_COUNT; i++) {
//            arr[i] = new String(String.valueOf(data[i % data.length]));
            arr[i] = new String(String.valueOf(data[i % data.length])).intern();

        }
        long end = System.currentTimeMillis();
        System.out.println("花费的时间为：" + (end - start));

        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.gc();
    }
}
```

 ` arr[i] = new String(String.valueOf(data[i % data.length]));`

![image-20220310093241185](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310093241185.png)



![image-20220310093354381](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310093354381.png)

`arr[i] = new String(String.valueOf(data[i % data.length])).intern();`

![image-20220310093450231](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310093450231.png)

![image-20220310093506826](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310093506826.png)

`因为垃圾回收策略(数组的引用不在堆空间而是在常量池)，所以intern()才可以节省空间`



### StringTable的垃圾回收测试

```
package com.sgg.StringTable;

/**
 * String的垃圾回收:
 * -Xms15m -Xmx15m -XX:+PrintStringTableStatistics -XX:+PrintGCDetails
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  21:27
 */
public class StringGCTest {
    public static void main(String[] args) {
        for (int j = 0; j < 100000; j++) {
            String.valueOf(j).intern();
        }
    }
    }
```

![image-20220310101853866](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310101853866.png)

![image-20220310102005133](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310102005133.png)

![image-20220310101848891](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310101848891.png)

==**PS：按道理来讲，循环了10000，number of entire应该也增加10000，事实上并没，说明发生了GC**==



### G1垃圾收集器的String去重操作（有个概念即可）

![image-20220310103139902](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310103139902.png)

![image-20220310103149530](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310103149530.png)

![image-20220310103159856](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310103159856.png)



## GC

### 什么是GC，为什么需要GC？

![image-20220310105737660](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310105737660.png)

> 什么是垃圾？

![image-20220310105829180](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310105829180.png)

> 为什么需要GC：

![image-20220310105848113](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310105848113.png)



### 了解早期垃圾回收行为

![image-20220310110229646](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310110229646.png)

![image-20220310110239951](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310110239951.png)

==内存泄漏：数据不用了，试图gc的时候 却无法回收，还有引用指向==

### 自动内存管理介绍

![image-20220310162216135](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310162216135.png)

> 担忧：

![image-20220310162259951](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310162259951.png)

> 应当关心那些区域的回收：

![image-20220310162323547](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310162323547.png)

![image-20220310162340467](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310162340467.png)

## 垃圾回收的相关算法

### 垃圾标记阶段

![image-20220310164750704](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310164750704.png)

### 引用计数算法（JVM不用）

![image-20220310164759606](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310164759606.png)

==GC=====垃圾回收   涉及两个阶段 第一个是标记垃圾 第二个是清除垃圾==

> 循环引用导致内存泄露

![image-20220310164820402](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310164820402.png)==不要举这个循环引用的内存泄露例子，不太好，因为不用这种方法就不会导致循环引用造成的内存泄漏==

### 证明java中没有采用引用计数算法案例

```
package com.sgg.GC;

/**
 * -XX:+PrintGCDetails
 * 证明：java使用的不是引用计数算法
 * @author shkstart
 * @create 2020 下午 2:38
 */
public class RefCountGC {
    //这个成员属性唯一的作用就是占用一点内存
    private byte[] bigSize = new byte[5 * 1024 * 1024];//5MB

    Object reference = null;

    public static void main(String[] args) {
        RefCountGC obj1 = new RefCountGC();
        RefCountGC obj2 = new RefCountGC();

        obj1.reference = obj2;
        obj2.reference = obj1;

        obj1 = null;
        obj2 = null;
        //显式的执行垃圾回收行为
        //这里发生GC，obj1和obj2能否被回收？
        System.gc();

     /*   try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }*/
    }
}
```

![image-20220310191635182](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310191635182.png)

![image-20220310191305110](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310191305110.png)

![image-20220310191319776](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310191319776.png)

`占用空间变少了 又存在引用 说明没有采用引用计数算法`

> 小结：

![image-20220310191741014](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220310191741014.png)

### 可达性分析算法（JVM使用）

==可达性分析算法==也叫做==根搜索算法==、==追踪性垃圾收集==

![image-20220311230543705](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220311230543705.png)

![image-20220311231244157](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220311231244157.png)

![image-20220312202315944](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312202315944.png)

> GC Roots

![image-20220311231303601](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220311231303601.png)

==上面所说的都是引用 比如虚拟机栈中引用的对象理解为虚拟机栈中的引用==

==前4个要说出来==

![image-20220311231312241](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220311231312241.png)

![image-20220312202357189](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312202357189.png)

==**只要不在堆空间中，本身又是一个指向堆空间某个对象的引用，那么他就是一个root**== 

`注意`

![image-20220311231332175](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220311231332175.png)



### 对象的finalization机制

> finalization的源码：

![image-20220312215157456](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312215157456.png)

![image-20220312215011660](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312215011660.png)

![image-20220312215026437](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312215026437.png)

![image-20220312215037395](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312215037395.png)

> 具体过程：

![image-20220312215055963](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312215055963.png)

==**第一次复活后 如果没有引用链第二次gc还是得死 因为finalize只会被调用一次**==

#### 代码演示可复活的对象

```java
package com.sgg.GC;

/**
 * 测试Object类中finalize()方法，即对象的finalization机制。
 *
 * @author shkstart
 * @create 2020 下午 2:57
 */
public class CanReliveObj {
    public static CanReliveObj obj;//类变量，属于 GC Root


    //此方法只能被调用一次
 /*   @Override
    protected void finalize() throws Throwable {
        super.finalize();
        System.out.println("调用当前类重写的finalize()方法");
        obj = this;//当前待回收的对象在finalize()方法中与引用链上的一个对象obj建立了联系
    }
*/

    public static void main(String[] args) {
        try {
            obj = new CanReliveObj();
            // 对象第一次成功拯救自己
            obj = null;
            System.gc();//调用垃圾回收器
            System.out.println("第1次 gc");
            // 因为Finalizer线程优先级很低，暂停2秒，以等待它
            Thread.sleep(2000);
            if (obj == null) {
                System.out.println("obj is dead");
            } else {
                System.out.println("obj is still alive");
            }
            System.out.println("第2次 gc");
            // 下面这段代码与上面的完全相同，但是这次自救却失败了
            obj = null;
            System.gc();
            // 因为Finalizer线程优先级很低，暂停2秒，以等待它
            Thread.sleep(2000);
            if (obj == null) {
                System.out.println("obj is dead");
            } else {
                System.out.println("obj is still alive");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

> 输出：

![image-20220312221639871](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312221639871.png)

`将finalize()方法的注释打开`

> 输出：

![image-20220312221843838](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312221843838.png)

### 使用MAT查看GC Roots

![image-20220312223151793](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312223151793.png)

`JProfiler，jvisualvm和MAT一样，都是一款java堆内存分析器` 只是在某些情况下，三者有一种可以达到理想情况(用来分析)



> 如何获取dump文件：

![image-20220312223644816](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312223644816.png)

![image-20220312223706039](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312223706039.png)

方式三：使用MAT导出并且分析

![image-20220313092611602](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313092611602.png)

![image-20220313092644105](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313092644105.png)



![image-20220313092855863](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313092855863.png)

==**输入1，使得第一个阻塞停止**==

![image-20220313092947183](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313092947183.png)

==**再次导出dump文件来分析：**==

![image-20220313093045421](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313093045421.png)

==**可以发现，dump文件没有list和date两个对象了，说明被回收了**==

方式四：使用JProfiler导出

> 案例代码：



### 使用JProfiler进行GC Roots溯源

==**为什么需要溯源？**==

某一个对象出现了内存泄露， 我们可以通过jprofiler进行gc roots溯源 从而手动断开引用

> 演示代码：

```
package com.sgg.GC;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;
import java.util.Scanner;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  16:28
 */
public class GCRootsTest {
    public static void main(String[] args) {
        List<Object> numList = new ArrayList<>();
        Date birth = new Date();

        for (int i = 0; i < 100; i++) {
            numList.add(String.valueOf(i));
            try {
                Thread.sleep(100000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        System.out.println("数据添加完毕，请操作：");
        new Scanner(System.in).next();
        numList = null;
        birth = null;

        System.out.println("numList、birth已置空，请操作：");
        new Scanner(System.in).next();

        System.out.println("结束");
    }
}
```

> 操作演示图：

![image-20220312233837335](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312233837335.png)

![image-20220312233047301](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312233047301.png)

==**变化大的就是我们需要关注的对象**==

点击run GC后

![image-20220312233137006](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312233137006.png)

![image-20220312233223463](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312233223463.png)

![image-20220312234047985](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220312234047985.png)

==**通过溯源，我们就可以解决内存泄露的问题啦！**==



### 使用Jprofiler分析OOM

> 案例代码：

```
package com.sgg.GC;

import java.util.ArrayList;

/**
 * -Xms8m -Xmx8m -XX:+HeapDumpOnOutOfMemoryError   最后一个的意思是当出现oom时候，就会生成dump文件
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  15:29
 */
public class HeapOOM {
    byte[] buffer = new byte[1 * 1024 * 1024];//1MB

    public static void main(String[] args) {
        ArrayList<HeapOOM> list = new ArrayList<>();

        int count = 0;
        try{
            while(true){
                list.add(new HeapOOM());
                count++;
            }
        }catch (Throwable e){
            System.out.println("count = " + count);
            e.printStackTrace();
        }
    }
}
```

`dump默认生成在当前模块下`



![image-20220313095133287](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313095133287.png)

`通过Thread Dump可以看到哪个线程出现了问题，以及哪一行出现了问题`

![image-20220313095229958](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313095229958.png)



### 标记-清除（Mark-Sweep）算法

> 垃圾清除阶段：

![image-20220313101452266](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313101452266.png)

![image-20220313101502718](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313101502718.png)

![image-20220313101532602](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313101532602.png)

![image-20220313101545200](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313101545200.png)

==**小柯总结:**==

`效率不算高是因为全局遍历两遍·`

`标记的是可达对象（非垃圾对象） 在对象头中记录为可达对象 header没有被标记 就清除`

`空闲列表需要占据内存空间`

`碎片导致大对象放不进去 容易报错oom`

`清除不是清除掉对象实例，而是覆盖`



### 复制算法

> 背景：

![image-20220313115449805](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313115449805.png)

![image-20220313115505865](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313115505865.png)

`复制是把实体复制过来`

![image-20220313115522754](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313115522754.png)

`缺点2的意思是 栈中的引用地址也要发生改变`

`复制算法非常适合垃圾对象很多，存活对象很少的场景，这样的话复制算法就会被用的淋漓尽致啦`

![image-20220313115535336](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313115535336.png)

==**小柯总结:**==

`A-B区交换  这其实就是新生代的幸存区的算法机制 s0 s1`

`用的是指针碰撞`

`幸存区朝生夕死 所以用复制算法是非常完美的`

`显然，老年代不具备朝生夕死的特性，因此不适合用复制算法`



### 标记-压缩算法

![image-20220313122117866](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313122117866.png)

![image-20220313122130336](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313122130336.png)

![image-20220313122143628](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313122143628.png)

![image-20220313122155349](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313122155349.png)

==**小柯总结**==

`优点一其实就是指针碰撞，不用维护一个空闲列表`

`效率最低 因为在标记清除的基础上还多家了一个压缩`

`SWT比较长`

### 小结对比三种算法（针对朝生夕死的情况）

![image-20220313122356541](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313122356541.png)

`总结:`

==**追求什么就对应的选什么算法（比如追求速度，就选择复制算法）**==

### 分代收集算法

==**难道就没有一种最好的算法吗？**== 

`没有最合适的代码，只有最适合的算法`

![image-20220313192039155](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313192039155.png)

![image-20220313192050021](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313192050021.png)

`Mark表示标记阶段`

`Sweep表示清除阶段`

`compact表示压缩阶段（解决内存碎片化的问题）`

![image-20220313192101708](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313192101708.png)

`Mark-Sweep指的是标记清除算法`

`Mark-Compact指的是标记压缩算法`



### 增量收集算法

![image-20220313193539671](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313193539671.png)

![image-20220313193552483](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313193552483.png)

==**小柯总结：**==

`切换会浪费资源和时间，使得垃圾回收的总成本上升`

### 分区算法

![image-20220313194123969](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313194123969.png)

和`增量收集算法`一样，都是为了降低延迟

`Humongous`指的是大对象

![image-20220313194132914](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313194132914.png)

![image-20220313194140669](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313194140669.png)



## 垃圾回收相关概念的概述

### System.gc()的理解：

![image-20220313201555233](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313201555233.png)

==**小柯总结：**==

`Full GC对方法区也会进行回收`

`System.gc()属于是手动触发gc`

> 案例代码：

```
package com.sgg.gcThread;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  14:49
 */
public class SystemGCTest {
    public static void main(String[] args) {
        new SystemGCTest();
        System.gc();//提醒jvm的垃圾回收器执行gc,但是不确定是否马上执行gc
        //与Runtime.getRuntime().gc();的作用一样。System.gc()底层就是 Runtime.getRuntime().gc()

        System.runFinalization();//强制调用失去引用的对象的finalize()方法
    }

    @Override
    protected void finalize() throws Throwable {
        super.finalize();
        System.out.println("SystemGCTest 重写了finalize()");
    }
}
```

> 源码：

![image-20220313201325131](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313201325131.png)

![image-20220313201336824](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313201336824.png)



 若将`System.runFinalization()`注释掉，未必能看到控制台输出：SystemGCTest 重写了finalize()，看情况的。

加上`System.runFinalization(); `强制调用失去引用的对象的finalize()方法 就会输出：SystemGCTest 重写了finalize()。



### 手动gc理解不可达对象的回收行为

```java
package com.sgg.gcThread;

/**
 -XX:+PrintGCDetails
 */
public class LocalVarGC {
    public void localvarGC1() {
        byte[] buffer = new byte[10 * 1024 * 1024];//10MB
        System.gc();
    }

    public void localvarGC2() {
        byte[] buffer = new byte[10 * 1024 * 1024];
        buffer = null;
        System.gc();
    }

    public void localvarGC3() {
        {
            byte[] buffer = new byte[10 * 1024 * 1024];
        }
        System.gc();
    }

    public void localvarGC4() {
        {
            byte[] buffer = new byte[10 * 1024 * 1024];
        }
        int value = 10;
        System.gc();
    }

    public void localvarGC5() {
        localvarGC1();
        System.gc();
    }

    public static void main(String[] args) {
        LocalVarGC local = new LocalVarGC();
        local.localvarGC1();
    }
}
```

> 方法一：



> 方法二：

<img src="C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313204228174.png" alt="image-20220313204228174" style="zoom:150%;" />



### 内存溢出的分析

![image-20220313205636592](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313205636592.png)

![image-20220313205645430](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313205645430.png)



### 内存泄漏的分析

![image-20220313214640677](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313214640677.png)

`static随着类的加载而加载，随着类的消亡而消亡,所以能使用局部变量，尽量使用局部变量，减小oom的可能`

![image-20220313214656814](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313214656814.png)

==**忘记断开引用导致内存泄漏**==

> 面试可供举例:

![image-20220313214724770](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313214724770.png)

> 对举例一的说明：

![image-20220313215154418](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313215154418.png)

### Stop The World

![image-20220313221804347](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313221804347.png)

![image-20220313221815307](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313221815307.png)

> 案例代码：

```java
package com.sgg.gcThread;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  15:50
 */
public class StopTheWorldDemo {
    public static class WorkThread extends Thread {
        List<byte[]> list = new ArrayList<byte[]>();

        public void run() {
            try {
                while (true) {
                    for(int i = 0;i < 1000;i++){
                        byte[] buffer = new byte[1024];
                        list.add(buffer);
                    }

                    if(list.size() > 1000){
                        list.clear();
                        System.out.println("触发gc");
                        System.gc();//会触发full gc，进而会出现STW事件
                    }
                }
            } catch (Exception ex) {
                ex.printStackTrace();
            }
        }
    }

    public static class PrintThread extends Thread {
        public final long startTime = System.currentTimeMillis();

        public void run() {
            try {
                while (true) {
                    // 每秒打印时间信息
                    long t = System.currentTimeMillis() - startTime;
                    System.out.println(t / 1000 + "." + t % 1000);
                    Thread.sleep(1000);
                }
            } catch (Exception ex) {
                ex.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        WorkThread w = new WorkThread();
        PrintThread p = new PrintThread();
        w.start();
        p.start();
    }
}

```

> 输出

![image-20220313221317104](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313221317104.png)

`可以发现每隔1s左右就输出一次`

> 将w.start()方法打开

![image-20220313223317096](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313223317096.png)

![image-20220313223333050](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313223333050.png)

### 程序的并发与并行

> 并发：

![image-20220313224106043](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313224106043.png)

`CPU快速切换任务 看起来像是并行（多个任务共同进行）`

> 并行：

![image-20220313224124664](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313224124664.png)



![image-20220313224135270](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313224135270.png)

`资源指的是cpu资源`



### 垃圾回收的并发与并行

![image-20220313224836090](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313224836090.png)

`绿色的是用户线程 `

`红色的是垃圾回收线程`

![image-20220313224904363](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220313224904363.png)

`这里的并发指的是一段时间内`  (但不一定是并行的，可能会交替执行)这句话不看 我感觉是错的



### 安全点与安全区域

![image-20220314101630343](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314101630343.png)

![image-20220314101639323](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314101639323.png)

![image-20220314104011439](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314104011439.png)

![image-20220314101650386](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314101650386.png)

### Java中几种不同的引用

#### 多种引用概述

![image-20220314114041691](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314114041691.png)

![image-20220314114058581](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314114058581.png)

==**小柯总结（比较接地气的说话）：**==

`强引用： 死都不回收`

`软引用：内存不够才回收`

`弱引用：垃圾回收对象必挂`

`虚引用：对对象的回收进行跟踪`



#### 强引用

![image-20220314120300795](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314120300795.png)

![image-20220314120311238](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314120311238.png)

![image-20220314120322715](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314120322715.png)

> 代码案例：

```java
package com.sgg.reference;

/**
 *  强引用的测试
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  16:05
 */
public class StrongReferenceTest {
    public static void main(String[] args) {
        StringBuffer str = new StringBuffer ("Hello,尚硅谷");
        StringBuffer str1 = str;   //str和str1都是强引用

        str = null;
        System.gc();//还有一个引用 所以不会回收

        try {
            Thread.sleep(3000);  //因为   System.gc()不一定马上执行，因为来个延迟 这样gc就会回收了
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println(str1);
    }
}
```

> 输出：

![image-20220314120217001](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314120217001.png)



#### 软引用

![image-20220314154016309](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314154016309.png)

![image-20220314154043377](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314154043377.png)

==**第一次回收目标是不可触及的对象**==

> 案例代码：

```java
package com.sgg.reference;

import java.lang.ref.SoftReference;

/**
 * 软引用的测试：内存不足即回收
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  16:06
 */
public class SoftReferenceTest {
    public static class User {
        public User(int id, String name) {
            this.id = id;
            this.name = name;
        }

        public int id;
        public String name;

        @Override
        public String toString() {
            return "[id=" + id + ", name=" + name + "] ";
        }
    }

    public static void main(String[] args) {
        //创建对象，建立软引用
//        SoftReference<User> userSoftRef = new SoftReference<User>(new User(1, "songhk"));
        //上面的一行代码，等价于如下的三行代码
        User u1 = new User(1,"songhk");
        SoftReference<User> userSoftRef = new SoftReference<User>(u1);
        u1 = null;//取消强引用


        //从软引用中重新获得强引用对象
        System.out.println(userSoftRef.get());

     /*   System.gc();
        System.out.println("After GC:");
//        //垃圾回收之后获得软引用中的对象
        System.out.println(userSoftRef.get());//由于堆空间内存足够，所有不会回收软引用的可达对象。
//
        try {
            //让系统认为内存资源紧张、不够
//            byte[] b = new byte[1024 * 1024 * 7];
            byte[] b = new byte[1024 * 7168 - 635 * 1024];
        } catch (Throwable e) {
            e.printStackTrace();
        } finally {
            //再次从软引用中获取数据
            System.out.println(userSoftRef.get());//在报OOM之前，垃圾回收器会回收软引用的可达对象。
        }*/
    }
}
```

> 输出：

![image-20220314121202640](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314121202640.png)



==**将这些代码的打开并且设置参数：`-Xms10m -Xmx10m`**==

```java
       System.gc();
        System.out.println("After GC:");
//        //垃圾回收之后获得软引用中的对象
        System.out.println(userSoftRef.get());//由于堆空间内存足够，所有不会回收软引用的可达对象。
```

> 输出：

![image-20220314121355336](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314121355336.png)

==**将这段代码打开**==

```java
     try {
            //让系统认为内存资源紧张、不够
           byte[] b = new byte[1024 * 1024 * 7];
        } catch (Throwable e) {
            e.printStackTrace();
        } finally {
            //再次从软引用中获取数据
            System.out.println(userSoftRef.get());//在报OOM之前，垃圾回收器会回收软引用的可达对象。
        }
```

> 输出

![image-20220314121749623](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314121749623.png)

==**堆内存设置了10M  新生区和老年去比例是1：2 都不可能存的下b，因此会回收软引用**==

将上面的代码修改成：

   ``` java
   byte[] b = new byte[1024 * 1024 * 1];
   再配置参数：-Xms10m -Xmx10m -XX:+PrintGCDetails
   ```

> 输出：

![image-20220314122811264](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314122811264.png)

> 将代码修改成：

```java
package com.sgg.reference;

import java.lang.ref.SoftReference;

/**
 * 软引用的测试：内存不足即回收
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  16:06
 */
public class SoftReferenceTest {
    public static class User {
        public User(int id, String name) {
            this.id = id;
            this.name = name;
        }

        public int id;
        public String name;

        @Override
        public String toString() {
            return "[id=" + id + ", name=" + name + "] ";
        }
    }

    public static void main(String[] args) {
        //创建对象，建立软引用
//        SoftReference<User> userSoftRef = new SoftReference<User>(new User(1, "songhk"));
        //上面的一行代码，等价于如下的三行代码
        User u1 = new User(1,"songhk");
        SoftReference<User> userSoftRef = new SoftReference<User>(u1);
        u1 = null;//取消强引用


        //从软引用中重新获得强引用对象
        System.out.println(userSoftRef.get());

        System.gc();
        System.out.println("After GC:");
//        //垃圾回收之后获得软引用中的对象
        System.out.println(userSoftRef.get());//由于堆空间内存足够，所有不会回收软引用的可达对象。
//
        try {
            //让系统认为内存资源紧张、不够
        /*  byte[] b = new byte[1024 * 1024 * 7];*/
            byte[] b = new byte[1024 * 7180 -635*1024];//将老年代撑起来
        } catch (Throwable e) {
            e.printStackTrace();
        } finally {
            //再次从软引用中获取数据
            System.out.println(userSoftRef.get());//在报OOM之前，垃圾回收器会回收软引用的可达对象。
        }
    }
}
```

> 输出：

![image-20220314123145050](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314123145050.png)

==**这样既回收了软引用又避免报OOM**==



#### 弱引用

![image-20220314153835348](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314153835348.png)

![image-20220314153952195](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314153952195.png)

`比起软引用回收更快，因为软引用需要根据算法判断内存空间够不够、`

> 案例代码：

```
package com.sgg.reference;

import java.lang.ref.WeakReference;

/**
 * 弱引用的测试
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  16:06
 */
public class WeakReferenceTest {
    public static class User {
        public User(int id, String name) {
            this.id = id;
            this.name = name;
        }

        public int id;
        public String name;

        @Override
        public String toString() {
            return "[id=" + id + ", name=" + name + "] ";
        }
    }

    public static void main(String[] args) {
        //构造了弱引用
        WeakReference<User> userWeakRef = new WeakReference<User>(new User(1, "songhk"));
        //从弱引用中重新获取对象
        System.out.println(userWeakRef.get());

        System.gc();
        // 不管当前内存空间足够与否，都会回收它的内存
        System.out.println("After GC:");
        //重新尝试从弱引用中获取对象
        System.out.println(userWeakRef.get());
    }
}
```

> 输出：

![image-20220314153737817](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314153737817.png)



#### 虚引用

> 代码案例：

```java
package com.sgg.reference;

import java.lang.ref.PhantomReference;
import java.lang.ref.ReferenceQueue;

/**
 * 虚引用的测试
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  16:07
 */
public class PhantomReferenceTest {
    public static PhantomReferenceTest obj;//当前类对象的声明
    static ReferenceQueue<PhantomReferenceTest> phantomQueue = null;//引用队列

    public static class CheckRefQueue extends Thread {
        @Override
        public void run() {
            while (true) {
                if (phantomQueue != null) {
                    PhantomReference<PhantomReferenceTest> objt = null;
                    try {
                        objt = (PhantomReference<PhantomReferenceTest>) phantomQueue.remove();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    if (objt != null) {
                        System.out.println("追踪垃圾回收过程：PhantomReferenceTest实例被GC了");
                    }
                }
            }
        }
    }

    @Override
    protected void finalize() throws Throwable { //finalize()方法只能被调用一次！
        super.finalize();
        System.out.println("调用当前类的finalize()方法");
        obj = this;
    }

    public static void main(String[] args) {
        //实例化
        phantomQueue = new ReferenceQueue<PhantomReferenceTest>();
        obj = new PhantomReferenceTest();
        //构造了 PhantomReferenceTest 对象的虚引用，并指定了引用队列
        PhantomReference<PhantomReferenceTest> phantomRef = new PhantomReference<PhantomReferenceTest>(obj, phantomQueue);
            //不可获取虚引用中的对象
            System.out.println(phantomRef.get());  
    }
}
```

> 输出：

![image-20220314160254656](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314160254656.png)

==**修改代码：**==

```
package com.sgg.reference;

import java.lang.ref.PhantomReference;
import java.lang.ref.ReferenceQueue;

/**
 * 虚引用的测试
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  16:07
 */
public class PhantomReferenceTest {
    public static PhantomReferenceTest obj;//当前类对象的声明
    static ReferenceQueue<PhantomReferenceTest> phantomQueue = null;//引用队列

    public static class CheckRefQueue extends Thread {
        @Override
        public void run() {
            while (true) {
                if (phantomQueue != null) {
                    PhantomReference<PhantomReferenceTest> objt = null;
                    try {
                        objt = (PhantomReference<PhantomReferenceTest>) phantomQueue.remove();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    if (objt != null) {
                        System.out.println("追踪垃圾回收过程：PhantomReferenceTest实例被GC了");
                    }
                }
            }
        }
    }

    @Override
    protected void finalize() throws Throwable { //finalize()方法只能被调用一次！
        super.finalize();
        System.out.println("调用当前类的finalize()方法");
        obj = this;
    }

    public static void main(String[] args) {
        //实例化
        phantomQueue = new ReferenceQueue<PhantomReferenceTest>();
        obj = new PhantomReferenceTest();
        //构造了 PhantomReferenceTest 对象的虚引用，并指定了引用队列
        PhantomReference<PhantomReferenceTest> phantomRef = new PhantomReference<PhantomReferenceTest>(obj, phantomQueue);

        try {
            //不可获取虚引用中的对象
            System.out.println(phantomRef.get());

            //将强引用去除
            obj = null;
            //第一次进行GC,由于对象可复活，GC无法回收该对象
            System.gc();
            Thread.sleep(1000);
            if (obj == null) {
                System.out.println("obj 是 null");
            } else {
                System.out.println("obj 可用");//finalize（）方法中将其复活了
            }
          /*  System.out.println("第 2 次 gc");
            obj = null;
            System.gc(); //一旦将obj对象回收，就会将此虚引用存放到引用队列中。
            Thread.sleep(1000);
            if (obj == null) {
                System.out.println("obj 是 null");
            } else {
                System.out.println("obj 可用");
            }*/
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

> 输出：

![image-20220314160809441](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314160809441.png)

==**再次修改代码**==

```
package com.sgg.reference;

import java.lang.ref.PhantomReference;
import java.lang.ref.ReferenceQueue;

/**
 * 虚引用的测试
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  16:07
 */
public class PhantomReferenceTest {
    public static PhantomReferenceTest obj;//当前类对象的声明
    static ReferenceQueue<PhantomReferenceTest> phantomQueue = null;//引用队列

    public static class CheckRefQueue extends Thread {
        @Override
        public void run() {
            while (true) {
                if (phantomQueue != null) {
                    PhantomReference<PhantomReferenceTest> objt = null;
                    try {
                        objt = (PhantomReference<PhantomReferenceTest>) phantomQueue.remove();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    if (objt != null) {
                        System.out.println("追踪垃圾回收过程：PhantomReferenceTest实例被GC了");
                    }
                }
            }
        }
    }

    @Override
    protected void finalize() throws Throwable { //finalize()方法只能被调用一次！
        super.finalize();
        System.out.println("调用当前类的finalize()方法");
        obj = this;
    }

    public static void main(String[] args) {
        //实例化
        phantomQueue = new ReferenceQueue<PhantomReferenceTest>();
        obj = new PhantomReferenceTest();
        //构造了 PhantomReferenceTest 对象的虚引用，并指定了引用队列
        PhantomReference<PhantomReferenceTest> phantomRef = new PhantomReference<PhantomReferenceTest>(obj, phantomQueue);

        try {
            //不可获取虚引用中的对象
            System.out.println(phantomRef.get());

            //将强引用去除
            obj = null;
            //第一次进行GC,由于对象可复活，GC无法回收该对象
            System.gc();
            Thread.sleep(1000);
            if (obj == null) {
                System.out.println("obj 是 null");
            } else {
                System.out.println("obj 可用");//finalize（）方法中将其复活了
            }
            System.out.println("第 2 次 gc");
            obj = null;
            System.gc(); 
            Thread.sleep(1000);
            if (obj == null) {
                System.out.println("obj 是 null");
            } else {
                System.out.println("obj 可用");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

![image-20220314160849633](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314160849633.png)

==**最后一次修改代码：**==

```java
package com.sgg.reference;

import java.lang.ref.PhantomReference;
import java.lang.ref.ReferenceQueue;

/**
 * 虚引用的测试
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  16:07
 */
public class PhantomReferenceTest {
    public static PhantomReferenceTest obj;//当前类对象的声明
    static ReferenceQueue<PhantomReferenceTest> phantomQueue = null;//引用队列
//操作引用队列
    public static class CheckRefQueue extends Thread {
        @Override
        public void run() {
            while (true) {
                if (phantomQueue != null) {
                    PhantomReference<PhantomReferenceTest> objt = null;
                    try {
                        objt = (PhantomReference<PhantomReferenceTest>) phantomQueue.remove();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    if (objt != null) {
                        System.out.println("追踪垃圾回收过程：PhantomReferenceTest实例被GC了");
                    }
                }
            }
        }
    }

    @Override
    protected void finalize() throws Throwable { //finalize()方法只能被调用一次！
        super.finalize();
        System.out.println("调用当前类的finalize()方法");
        obj = this;
    }

    public static void main(String[] args) {
     /*   Thread t = new CheckRefQueue();
        t.start();*/
        //实例化
        phantomQueue = new ReferenceQueue<PhantomReferenceTest>();
        obj = new PhantomReferenceTest();
        //构造了 PhantomReferenceTest 对象的虚引用，并指定了引用队列
        PhantomReference<PhantomReferenceTest> phantomRef = new PhantomReference<PhantomReferenceTest>(obj, phantomQueue);

        try {
            //不可获取虚引用中的对象
            System.out.println(phantomRef.get());

            //将强引用去除
            obj = null;
            //第一次进行GC,由于对象可复活，GC无法回收该对象
            System.gc();
            Thread.sleep(1000);
            if (obj == null) {
                System.out.println("obj 是 null");
            } else {
                System.out.println("obj 可用");//finalize（）方法中将其复活了
            }
            System.out.println("第 2 次 gc");
            obj = null;
            System.gc(); //一旦将obj对象回收，就会将此虚引用存放到引用队列中。
            Thread.sleep(1000);
            if (obj == null) {
                System.out.println("obj 是 null");
            } else {
                System.out.println("obj 可用");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

> 输出：

![image-20220314161637486](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314161637486.png)

==**将线程修改为守护线程：**==

![image-20220314161549296](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314161549296.png)



#### 终结器引用（了解即可）

![image-20220314193655293](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314193655293.png)

## 垃圾回收器

### 垃圾回收期的分类

> java不同版本的新特性

1.语言层面：Lambda表达式、switch、自动拆箱、enum、<>....

2.API层面：Stream API、新的日期时间、Optional、String、集合框架

3.底层优化：JVM的优化，GC的变化，元空间、`静态域`、字符串常量池等

![image-20220314215509772](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314215509772.png)

- 按线程数分 `这里的线程指的是垃圾回收的线程数`

![image-20220314215523225](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314215523225.png)

`绿色是用户现场，橙色是gc线程`  `下面超过并行回收器省略掉 直接并发回收期即可` `1的第二点并行值得是一段时间内的 相当于并发`

![image-20220314215542778](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314215542778.png)

- 按工作模式分

![image-20220314215552437](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314215552437.png)

- ![image-20220314215606828](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314215606828.png)



### 评估GC的性能指标

![image-20220314221711962](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314221711962.png)

==**小柯总结：**==

`红色的是重点关注的`

`吞吐量和垃圾收集开销加起来等于1`

`暂停时间指的就是STW`

`收集频率高了的话，吞吐量就低了，暂停时间就短了`

`收集频率低了的话，吞吐量就高了，暂停时间就长了`

![image-20220314221723282](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314221723282.png)



### 吞吐量与暂停时间的对比说明

`JVM调优重点关注的两大指标`

> 吞吐量：

![image-20220314222947124](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314222947124.png)

`蓝色是用户线程`

`红色是垃圾回收线程`

> 暂停时间：

![image-20220314223004432](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314223004432.png)

![image-20220314223027068](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314223027068.png)

![image-20220314223039120](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220314223039120.png)

`按住一个指标（停顿时间)比如每次停顿时间控制在10ms内，在此基准上， 另一个指标往上提！`

### 垃圾收集器的发展迭代使

![image-20220315110617310](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315110617310.png)

`不同的jvm可以搭配不同的垃圾收集器`

![image-20220315110630878](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315110630878.png)

> 经典的七款垃圾回收器

![image-20220315114813663](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315114813663.png)

![image-20220315110705743](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315110705743.png)

==**CMS蓝色与黄色是并发执行**==

==**G1的红色表示分区算法**==

### 垃圾回收器的组合关系

![image-20220315115102537](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315115102537.png)

`蓝色表示只能收集年轻代的垃圾`，`黄色表示收集老年代的垃圾`，`G1比较特别，既能回收年轻代又能回收老年代`

> 垃圾回收器的组合关系：

![image-20220315115224402](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315115224402.png)

==这幅图是更新到jdk14的==

![image-20220315120124192](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315120124192.png)

==**CMS GC和Serial Old GC之间有条线是因为Serial Old GC是CMS的后选方案。原因是CMS是并发的垃圾回收器，收集垃圾的时候，用户线程也在跑，此时CMS回收的比较晚或者垃圾制造速度比回收速度快的话，就会去使用Serial Old GC**==

`因为Parallel Scavenge GC和CMS GC的底层框架不一样，因此不兼容，所以不能搭配使用`

![image-20220315120136168](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315120136168.png)

==移动端更看重交互(低延迟)，服务器端注重吞吐量==

### 如何查看默认的垃圾收集器

![image-20220315121433030](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315121433030.png)

> 在jdk8的环境中：

`案例代码：`

```

public class GCUseTest {
    public static void main(String[] args) {
        ArrayList<byte[]> list = new ArrayList<>();

        while(true){
            byte[] arr = new byte[100];
            list.add(arr);
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

```

添加VM选项：`-XX:+PrintCommandLineFlags`

`可以看出用的默认用是Parallel Scavange GC，老年代默认就是搭配的那个也就是 Parallel Old GC`

![image-20220315122516114](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315122516114.png)

`在命令行中也可以查看`

![image-20220315123044335](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315123044335.png)

> 将环境修改为jdk9

`运行查看输出:`

![image-20220315123609070](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315123609070.png)

`命令行窗口也可以看到`

![image-20220315123643560](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315123643560.png)

### Serial和Serial Old回收器

![image-20220315124105133](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315124105133.png)

![image-20220315124119701](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315124119701.png)

![image-20220315124129021](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315124129021.png)



#### 如何设置使用Serial垃圾回收器

> 案例代码：

```
package com.sgg.GCCollegtion;

import java.util.ArrayList;

/**
 *  -XX:+PrintCommandLineFlags
 *
 *  -XX:+UseSerialGC:表明新生代使用Serial GC ，同时老年代使用Serial Old GC
 */
public class GCUseTest {
    public static void main(String[] args) {
        ArrayList<byte[]> list = new ArrayList<>();

        while(true){
            byte[] arr = new byte[100];
            list.add(arr);
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

添加VM选项`-XX:+PrintCommandLineFlags -XX:+UseSerialGC`

> 输出：可以看到切换到了Serial回收器

![image-20220315190922489](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315190922489.png)

![image-20220315134329649](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315134329649.png)

`总结：`

![image-20220315134349295](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315134349295.png)

==**java Web应用中（就是ssm，springboot等项目）是不会使用这种串行回收器的**== 。`因为一旦执行用户线程就停止了，体验极差`

`客户端(Client)一般是单CPU的场景，因而采用这种垃圾回收器`

### ParNew垃圾回收器（并行回收）的介绍

![image-20220315135910213](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315135910213.png)

`Serial和ParNew底层共享了很多一样的代码，具体实现上的区别其实就是线程这方面`

![image-20220315135954550](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315135954550.png)

![image-20220315140003855](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315140003855.png)

![image-20220315140011538](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315140011538.png)

`设置线程的数量的话不要超过cpu的核数，因为回去争夺cpu资源`

#### 如何设置使用ParNew垃圾回收器

> 案例代码：

```java
package com.sgg.GCCollegtion;

import java.util.ArrayList;

/**
 *  -XX:+PrintCommandLineFlags
 *
 *  -XX:+UseSerialGC:表明新生代使用Serial GC ，同时老年代使用Serial Old GC
 *
 *  -XX:+UseParNewGC：标明新生代使用ParNew GC
 *  -XX:+UseConcMarkSweepGC：表明老年代使用CMS GC。同时，年轻代会触发对ParNew 的使用
 */
public class GCUseTest {
    public static void main(String[] args) {
        ArrayList<byte[]> list = new ArrayList<>();

        while(true){
            byte[] arr = new byte[100];
            list.add(arr);
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

> 添加VM选项：

`-XX:+PrintCommandLineFlags -XX:+UseParNewGC`

![image-20220315140456803](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315140456803.png)

`第一行红色的是不建议使用的意思`

> 修改VM选项：-XX:+PrintCommandLineFlags -XX:+UseConcMarkSweepGC

![image-20220315140658179](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315140658179.png)

> 改成jdk9的环境输出：==jdk9中ConcMarkSweepGC已被删除了 ==    
>
> -XX:+PrintCommandLineFlags -XX:+UseConcMarkSweepGC -XX:+UseParNewGC   

![image-20220315190336854](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315190336854.png)

### Parallel回收器（主打吞吐量优先）

![image-20220315213636448](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315213636448.png)

==自适应调节策略：动态调整内存的分配情况(`例如加大新生区的内存区域或者缩小等`)以致于达到一个最优的状态（把吞吐量提上去，或者把延迟降低）==

![image-20220315213700484](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315213700484.png)

`之所以Parallel Old收集器替代Serial Old，是因为并行的肯定比并发的好呀`

![image-20220315213713729](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315213713729.png)

![image-20220315213738791](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315213738791.png)

==jdk8中默认的GC是Parallel Scavenge GC和Parallel Old GC==

#### Paralle回收器的相关参数

==jdk8中默认的GC是Parallel Scavenge GC和Parallel Old GC==，验证一下：

> 案例代码：

```java
package com.sgg.GCCollegtion;

import java.util.ArrayList;

/**
 *  -XX:+PrintCommandLineFlags
 *
 *  -XX:+UseSerialGC:表明新生代使用Serial GC ，同时老年代使用Serial Old GC
 *
 *  -XX:+UseParNewGC：标明新生代使用ParNew GC
 *
 *  -XX:+UseParallelGC:表明新生代使用Parallel GC
 *  -XX:+UseParallelOldGC : 表明老年代使用 Parallel Old GC
 *  说明：二者可以相互激活
 *
 *  -XX:+UseConcMarkSweepGC：表明老年代使用CMS GC。同时，年轻代会触发对ParNew 的使用
 * @author shkstart  shkstart@126.com
 * @create 2020  0:10
 */
public class GCUseTest {
    public static void main(String[] args) {
        ArrayList<byte[]> list = new ArrayList<>();

        while(true){
            byte[] arr = new byte[100];
            list.add(arr);
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

> 输出

![image-20220315215431236](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315215431236.png)

`这里看不到老年代的垃圾回收器，但是我们可以通过命令行来查看`

![image-20220315215613383](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315215613383.png)

==JDK9中，老年代和新生代使用的都是G1GC==

> 参数配置：

![image-20220315215927788](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315215927788.png)具体例子吧：==当前8个CPU，却有10个线程，那CPU就要执行切换任务（十分浪费时间)，效率降低==

![image-20220315221404968](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315221404968.png)![image-20220315221415859](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315221415859.png)

`垃圾收集器的最大停顿时间和垃圾收集时间占总时间的比例此消彼长`

`设置最大停顿时间越小，会将java堆的内存调小=》GC频繁=》吞吐量低`

`设置最大停顿时间越大，会将java堆的内存调大=》GC不频繁=》吞吐量高`

`先设置吞吐量和延迟，然后自适应调节策略==》会尽量去满足吞吐量和延迟时间`



### CMS垃圾回收器（主打低延迟）

![image-20220315225156014](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315225156014.png)![image-20220315225207720](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315225207720.png)

![image-20220315225240540](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315225240540.png)

> CMS的工作原理：

![image-20220315225307970](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315225307970.png)

![image-20220315225318951](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315225318951.png)

#### CMS的特点与弊端分析

![image-20220315231150817](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315231150817.png)

![image-20220315231201296](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315231201296.png)

![image-20220315231219930](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220315231219930.png)

`重新标记：修正的是并发标记中怀疑的对象，并发标记的时候无法确定他们是不是垃圾`

`因为CMS的有些弊端非常致命，所以jdk后面的版本将其废弃了`  比如：在高峰期来到之时，CMS产生了内存碎片，无法分配大对象，不能不提前触发Full GC ，接着CMS去回收的时候，如果出现了“Concurrent Mode Failure”就会去用后备方案Serial Old，在我们硬件都很顶的时候，居然去用一个性能最低的垃圾回收器，那用户的卡顿不得卡死。这对我们来说是无法接受的，因此CMS被废弃了



### CMS垃圾回收器的参数设置

![image-20220316084319946](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316084319946.png)

![image-20220316084823488](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316084823488.png)

> 案例代码：==添加VM选项==：-XX:+PrintCommandLineFlags -XX:+UseConcMarkSweepGC

```
package com.sgg.GCCollegtion;

import java.util.ArrayList;

/**
 *  -XX:+PrintCommandLineFlags
 *
 *  -XX:+UseSerialGC:表明新生代使用Serial GC ，同时老年代使用Serial Old GC
 *
 *  -XX:+UseParNewGC：标明新生代使用ParNew GC
 *
 *  -XX:+UseParallelGC:表明新生代使用Parallel GC
 *  -XX:+UseParallelOldGC : 表明老年代使用 Parallel Old GC
 *  说明：二者可以相互激活
 *
 *  -XX:+UseConcMarkSweepGC：表明老年代使用CMS GC。同时，年轻代会触发对ParNew 的使用
 * @author shkstart  shkstart@126.com
 * @create 2020  0:10
 */
public class GCUseTest {
    public static void main(String[] args) {
        ArrayList<byte[]> list = new ArrayList<>();

        while(true){
            byte[] arr = new byte[100];
            list.add(arr);
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

> 输出：

![image-20220316084227832](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316084227832.png)

### CMS小结

![image-20220316085043420](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316085043420.png)

> JDK后续版本中CMS的变化

![image-20220316085125491](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316085125491.png)



### G1垃圾回收器（区域分代化）

==G1垃圾回收器的特点是区域分代化==

![image-20220316091015385](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316091015385.png)

![image-20220316091111506](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316091111506.png)

#### ![image-20220316091121989](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316091121989.png)G1垃圾回收器的优势和不足

> 优点：

![image-20220316093102692](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316093102692.png)

![image-20220316093113659](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316093113659.png)

![image-20220316093123859](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316093123859.png)

`某个regison清理完成后之后是可以换的（比如第一次区域内是Eden，第二次可能就是Old了）`

![image-20220316093133706](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316093133706.png)

![image-20220316093157631](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316093157631.png)

> 缺点：

![image-20220316093220982](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316093220982.png)

==G1为了垃圾收集产生的内存占用有一部分是因为记忆集（`后面会将`）==

#### G1的参数设置

![image-20220316094608163](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316094608163.png)

#### G1回收器常见的操作步骤

#### ![image-20220316095440242](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316095440242.png)G1回收器的适用场景

![image-20220316100244081](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316100244081.png)

#### reginon的使用

![image-20220316130300310](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316130300310.png)

![image-20220316102632477](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316102632477.png)

`上面对于Humongous内存区域的解释不正确,应当是大对象超过了0.5个region，就会放到Humongous内存区域`

![image-20220316102411364](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316102411364.png)

==如果大对象是短期存在的话，那么就相当于是内存泄漏了==

![image-20220316102832265](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316102832265.png)

#### G1回收器垃圾回收过程

![image-20220316103539716](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316103539716.png)

==如果我们设置暂停时间为20s，默认为200，那么每次留给gc发挥的时间就很短，如果此时制造垃圾的效率很高，就有可能触发Full GC==

![image-20220316103607463](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316103607463.png)

![image-20220316103617601](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316103617601.png)

#### 记忆集与写屏障

![image-20220316133423421](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316133423421.png)

![image-20220316133441276](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316133441276.png)

#### G1垃圾回收过程的详细说明

> G1回收过程一：年轻代GC

![image-20220316134159840](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316134159840.png)

![image-20220316135059756](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316135059756.png)

![image-20220316135112297](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316135112297.png)

![image-20220316135122607](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316135122607.png)



> G1回收过程二：并发标记过程

![image-20220316135152281](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316135152281.png)

> G1回收过程三：混合回收

![image-20220316135736625](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316135736625.png)

![image-20220316135855206](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316135855206.png)

> G1回收可选的过程四：Full GC

#### ![image-20220316140132741](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316140132741.png)G1回收过程补充以及优化建议

> 补充：

![image-20220316141818008](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316141818008.png)

> 优化建议：

![image-20220316141841348](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316141841348.png)



#### 七种经典垃圾回收器总结

![image-20220316150948093](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316150948093.png)![image-20220316151332912](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316151332912.png)

> 如何区选择垃圾回收器呢:

![image-20220316151626136](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316151626136.png)

![image-20220316151635708](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316151635708.png)

> 面试：

![image-20220316151927131](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316151927131.png)



### GC日志分析

#### 常用的显示GC日志的参数：

![image-20220316153625263](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316153625263.png)

添加VM选项：`-Xms60m -Xmx60m -XX:+PrintGCDetails`

```java
package com.sgg.GCCollegtion;

import java.util.ArrayList;

/**
 * -Xms60m -Xmx60m -XX:SurvivorRatio=8 -XX:+PrintGCDetails -Xloggc:./logs/gc.log
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  18:12
 */
public class GCLogTest {
    public static void main(String[] args) {
        ArrayList<byte[]> list = new ArrayList<>();

       for(int i=0;i<500;i++){
           byte[] arr = new byte[100*1024];//100kb
           list.add(arr);
        /*    try {
                Thread.sleep(50);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }*/


        }

    }}


```

> 运行输出：

![image-20220316153231056](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316153231056.png)



修改Vm选项：`-Xms60m -Xmx60m -XX:+PrintGC`

> 输出：

![image-20220316153303199](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316153303199.png)



修改VM选项：`-Xms60m -Xmx60m -XX:+PrintGCDetails -XX:+PrintGCTimeStamps`

> 输出：

![image-20220316153905837](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316153905837.png)

虚拟器启动起来读的秒数

修改VM选项`-Xms60m -Xmx60m -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -XX:+PrintGCDateStamps`

> 输出：

![image-20220316154047768](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316154047768.png)



> 具体说明：

![image-20220316154736096](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316154736096.png)

![image-20220316154745840](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316154745840.png)

![image-20220316154756605](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316154756605.png)

#### GC日志中垃圾回收数据的分析

> 日志补充说明：

![image-20220316155122424](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316155122424.png)

![image-20220316155138774](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316155138774.png)

`消耗时间只看real即可` 多线程总时间没必要累加，就是一个线程（最耗时）的时间

> GC日志数据分析：

![image-20220316155201242](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316155201242.png)

> Minor GC日志分析：

![image-20220316155812558](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316155812558.png)

> Full GC日志分析：

![image-20220316155833968](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316155833968.png)

#### 举例说明日志中堆空间数据如何解读



```
package com.sgg.GCCollegtion;

/**
 * -verbose:gc -Xms20M -Xmx20M -Xmn10M -XX:+PrintGCDetails -XX:SurvivorRatio=8 -XX:+UseSerialGC
 * @author shkstart  shkstart@126.com
 * @create 2020  0:12
 */
public class GCLogTest1 {
    private static final int _1MB = 1024 * 1024;

    public static void testAllocation() {
        byte[] allocation1, allocation2, allocation3, allocation4;
        allocation1 = new byte[2 * _1MB];
        allocation2 = new byte[2 * _1MB];
        allocation3 = new byte[2 * _1MB];
        allocation4 = new byte[4 * _1MB];
    }

    public static void main(String[] agrs) {
        testAllocation();
    }
}
```

`-Xmn10M表明新生代的大小是10M`

![image-20220316191703431](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316191703431.png)

`53%后面的三个地址其中第一个表示eden的起始地址，最后一个表示结尾地址。两者之间的部分就是eden区`

![image-20220316191013470](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316191013470.png)

![image-20220316191038254](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316191038254.png)

### 垃圾回收器的新发展

![image-20220316195212501](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316195212501.png)![image-20220316195224284](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316195224284.png)

![image-20220316195236723](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316195236723.png)

![image-20220316195249933](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316195249933.png)

![image-20220316195302994](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316195302994.png)

#### ZGC垃圾回收器

![image-20220316200310624](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316200310624.png)

![image-20220316200318679](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316200318679.png)

![image-20220316200330799](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316200330799.png)

![image-20220316200339592](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316200339592.png)

![image-20220316200347485](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316200347485.png)

![image-20220316200403220](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316200403220.png)

![image-20220316200417519](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220316200417519.png)
