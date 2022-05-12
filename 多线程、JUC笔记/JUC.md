# JUC

==面试高频点==

![image-20220204000354969](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220204000354969.png)

java.util 工具包、包、分类

**业务：普通的线程代码 new Thread(Runnable runnable), new Thread(Callable callable)**

![image-20220204000650254](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220204000650254.png)

![image-20220204000813995](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220204000813995.png)



## 线程和进程

> 线程、进程，如果不能使用一句话说出来的技术，不扎实！

==进程==：一个程序，QQ.==exe== Music.==exe== 程序的集合；

一个进程往往可以包含多个线程，==至少包含一个==！

==Java默认有几个线程==？ 2 个 mian、GC

==线程==：开了一个进程 Typora(==typora.exe==)，写字，自动保存（线程负责的）

对于Java而言：Thread、Runnable、Callable

**Java 真的可以开启线程吗？ 开不了**

```
public static void main(String[] args) {
    new Thread().start();
}
```

```
  public synchronized void start() {     
        if (threadStatus != 0)
            throw new IllegalThreadStateException();

         //将线程对象本身放入一个组中
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
// 本地方法，调用底层的C++ ，Java是运行在jvm上的 无法直接操作硬件 因此开不了线程
    private native void start0();
```

> 并发、并行

并发（多个线程操作同一个资源）

- CPU 一核 ，模拟出来多条线程，天下武功，唯快不破，快速交替

并行（多个人一起行走）

- CPU 多核 ，多个线程可以同时执行； ==可以用线程池来提高效率==

> 如何获取当前电脑的CPU核数

==方法一：==![image-20220204003116546](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220204003116546.png)

`逻辑处理器数目就是CPU核数，有多少个CPU就可以同时执行多少个线程`



==方法二==

![image-20220204003305449](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220204003305449.png)

`处理器下面有多少行CPU就是几核的`



> 方法三：

```java
public class Test1 {
    public static void main(String[] args) {
        //获取CPU核数
        System.out.println(Runtime.getRuntime().availableProcessors());
    }
}
```

> 输出：

![image-20220204003440684](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220204003440684.png)

==并发编程的本质：==`充分利用CPU的资源 `



## 回顾多线程

> 线程有几个状态

```java
public enum State {
   // 新生
    NEW,

   // 运行
    RUNNABLE,

  // 阻塞
    BLOCKED,

 // 等待，死死地等
    WAITING,

  // 等待 超时就不等了
    TIMED_WAITING,

    // 终止
    TERMINATED;
}
```



> wait和sleep的区别

**1、来自不同的类**

wait => Object

sleep => Thread

**2、关于锁的释放**

wait 会释放锁，sleep 不会释放锁 形象记忆（睡觉也要抱着锁睡）

**3、使用的范围是不同的**

wait ==只能在同步块和同步方法中使用==

sleep ==可以再任何地方睡 因此可以在任何地方用==

**4、是否需要捕获异常**

wait 不需要捕获异常sleep 必须要捕获异常

在企业开发中一般不会用sleep一般用下面这个

```java
public static void main(String[] args) throws InterruptedException {
    //休眠一天
    TimeUnit.DAYS.sleep(1);  //底层封装了Thread.Sleep();
    //休眠两秒
    TimeUnit.SECONDS.sleep(2);
}
```

![image-20220204225514819](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220204225514819.png)

## Lock锁（重点）

> 传统 Synchronized

==来个经典的卖票例子：==

> 真正的多线程开发，公司中的开发，为了降低耦合性，会尽量去实现下面原则：
>
> 线程就是一个单独的资源类，没有任何附属的操作(不继承，不实现接口，就单纯是一个类 ==OOP思想==)！
>
>  * 1、 属性、方法

```java
package com.juc.demo1;

public class SaleTicketDemo01 {
    public static void main(String[] args) {
        // 并发：多线程操作同一个资源类, 把资源类丢入线程
        Ticket ticket = new Ticket();

        // @FunctionalInterface 函数式接口，jdk1.8  lambda表达式 (参数)->{ 代码 }
        new Thread(()->{
            for (int i = 1; i < 10 ; i++) {
                ticket.sale();
            }
        },"A").start();

        new Thread(()->{
            for (int i = 1; i < 10 ; i++) {
                ticket.sale();
            }
        },"B").start();

        new Thread(()->{
            for (int i = 1; i < 10 ; i++) {
                ticket.sale();
            }
        },"C").start();


    }
}

// 资源类 OOP
class Ticket {
    // 属性、方法
    private int number = 10;

    // 卖票的方式
    // synchronized 本质: 队列，锁
    public synchronized void sale(){
        if (number>0){
            System.out.println(Thread.currentThread().getName()+"卖出了第"+(number--)+"票,剩余："+number);
        }
    }

}

```

> 输出：

![image-20220204232402467](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220204232402467.png)



> Lock接口

==Lock接口的实现类：==

![image-20220205092940216](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205092940216.png)



==先对Lock的实现类ReentrantLock用法有个了解吧！，查看JDK8帮助文档：==

![image-20220205092358857](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205092358857.png)



![image-20220205093132582](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205093132582.png)

公平锁：十分公平：先来后到   

==非公平锁：十分不公平：可以插队 （默认）==

==**非公平锁更高效的原因：**==

在计算机世界中，对于Java而言，用户线程和系统线程是属于1:1的对应关系，线程切换存在【执行现场保存和恢复】，这个过程是非常耗费时间（在许多情况下，线程切换的时间远远大于代码执行的时间）。因此，如果一个线程在running期间直接抢占到锁资源，就不需要进行【恢复现场】操作，也就可以更快地执行业务代码。相反地，如果是一个就绪态的线程想要获得锁资源，首先需要恢复现场，之后争抢锁（可能成功也可能失败），此时就已经浪费了大量的CPU，只有在获取锁成功后才能继续执行业务代码。
总的来说，主要高效的原因就在于需不需要进行现场恢复和不同态之间切换。



==**为什么Lock锁比Synchronized 高效？**==

> 测试：

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.atomic.AtomicLong;
import java.util.concurrent.locks.ReentrantLock;

public class Tester {
    private static SynRunner synRunner=new SynRunner();
    private static LockRunner lockRunner=new LockRunner();
    public static void main(String[] args) {
        long start = System.currentTimeMillis();
        for (long i=0;i<10000l;i++){
            new Thread(synRunner).start();
        }
        long end = System.currentTimeMillis();
        System.out.println(end-start);

    }

   static class  SynRunner implements Runnable {
        private long v = 0;

        public synchronized void run() {
            v = v + 1;
        }
    }

    static class LockRunner implements Runnable {
        private ReentrantLock lock = new ReentrantLock();
        private long v = 0;

        public void run() {
            lock.lock();
            try {
                v = v + 1;
            } finally {
                lock.unlock();
            }
        }

    }
}

```



详解请参考博客：

[synchronized与lock，哪个效率更高 - ㄓㄤㄑㄧㄤ - 博客园 (cnblogs.com)](https://www.cnblogs.com/azhqiang/p/3945402.html)

> ReentrantLock的使用三部曲：

1. new ReentrantLock()
2. lock.lock(); // 加锁
3. finally=> lock.unlock(); // 解锁

```java
package com.juc.demo1;

// 基本的卖票例子


import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class SaleTicketDemo02 {
    public static void main(String[] args) {
        // 并发：多线程操作同一个资源类, 把资源类丢入线程
        Ticket2 ticket = new Ticket2();

        // @FunctionalInterface 函数式接口，jdk1.8  lambda表达式 (参数)->{ 代码 }
        new Thread(()->{ for (int i = 1; i < 10 ; i++) ticket.sale(); },"A").start();
        new Thread(()->{ for (int i = 1; i < 10 ; i++) ticket.sale(); },"B").start();
        new Thread(()->{ for (int i = 1; i < 10 ; i++) ticket.sale(); },"C").start();
        new Thread(()->{ for (int i = 1; i < 10 ; i++) ticket.sale(); },"D").start();


    }
}

// 资源类 OOP
class Ticket2 {
    // 属性、方法
    private int number = 10;
   //声明锁（可重入锁）
    Lock lock=new ReentrantLock();

    // 卖票的方式

    public  void sale(){
        lock.lock();
        try {
            if (number>0){
                System.out.println(Thread.currentThread().getName()+"卖出了第"+(number--)+"票,剩余："+number);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

}
```

> 输出：

![image-20220205093446311](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205093446311.png)

==可以发现线程是安全的==

> synchronized 和 lock的区别

1. Synchronized 内置的Java关键字， Lock 是一个Java类
2. ==Synchronized 无法判断获取锁的状态，Lock 可以判断是否获取到了锁==
3. Synchronized 会自动释放锁，lock 必须要手动释放锁！如果不释放锁，**死锁**
4. Synchronized 线程 1（获得锁，阻塞）、线程2（等待，傻傻的等）；==Lock锁就不一定会等待下去；== **用lock.tryLock()**
5. Synchronized 可重入锁，不可以中断的，==非公平==；Lock ，可重入锁，可以 判断锁，非公平（可以自己设置）；
6. ==Synchronized 适合锁少量的代码同步问题，Lock 适合锁大量的同步代码！==

2，4详解请看此博客：[synchronized与Lock的区别 _sychronizedlock - 然妈网 (rmssf.com)](https://www.rmssf.com/news/sychronizedlock.html)

5详解参考博客[(27条消息) 【Java并发】synchronized不可以被中断？_runhua的专栏-CSDN博客_synchronized可以被中断吗](https://blog.csdn.net/runhua/article/details/98595867?ops_request_misc=%7B%22request%5Fid%22%3A%22164482468816780271534104%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=164482468816780271534104&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~hot_rank-2-98595867.pc_search_result_cache&utm_term=为什么说Synchronized+不可中断&spm=1018.2226.3001.4187)

Lock锁住的是谁？ 参考博客 https://www.cnblogs.com/fordreamxin/p/5382171.html



## 生产者和消费者问题 

==面试重点==：单例模式、排序算法、生产者和消费者、死锁

==线程通信资源类三板斧：==

- 判断等待
- 具体业务
- 通知唤醒

> 生产者和消费者问题 Synchronized 版

==演示代码:==

```java
package com.juc.pc;

/**
 * 线程之间的通信问题：生产者和消费者问题！  等待唤醒，通知唤醒
 * 线程交替执行  A   B 操作同一个变量   num = 0
 * A num+1
 * B num-1
 */
public class A {
    public static void main(String[] args) {
        Data data = new Data();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }, "A").start();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }, "B").start();

    }
}

// 判断等待，业务，通知
class Data{ // 数字 资源类

    private int number = 0;

    //+1
    public synchronized void increment() throws InterruptedException {
        if (number!=0){  //0
            // 等待
            this.wait();
        }
        number++;
        System.out.println(Thread.currentThread().getName()+"=>"+number);
        // 通知其他线程，我+1完毕了
        this.notifyAll();
    }

    //-1
    public synchronized void decrement() throws InterruptedException {
        if (number==0){ // 1
            // 等待
            this.wait();
        }
        number--;
        System.out.println(Thread.currentThread().getName()+"=>"+number);
        // 通知其他线程，我-1完毕了
        this.notifyAll();
    }

}
```

> 输出：

![image-20220205101346465](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205101346465.png)

> 如果多加两个线程，就会出现问题：==虚假唤醒==

==代码如下：== 

```java
package com.juc.pc;

/**
 * 线程之间的通信问题：生产者和消费者问题！  等待唤醒，通知唤醒
 * 线程交替执行  A   B 操作同一个变量   num = 0
 * A num+1
 * B num-1
 */
public class A {
    public static void main(String[] args) {
        Data data = new Data();

        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"A").start();

        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"B").start();

        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"C").start();


        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"D").start();
    }
}

// 判断等待，业务，通知
class Data{ // 数字 资源类

    private int number = 0;

    //+1
    public synchronized void increment() throws InterruptedException {
        if (number!=0){  //0
            // 等待   这个等待会释放锁 所以会出现问题
            this.wait();
        }
        number++;
        System.out.println(Thread.currentThread().getName()+"=>"+number);
        // 通知其他线程，我+1完毕了
        this.notifyAll();
    }

    //-1
    public synchronized void decrement() throws InterruptedException {
        if (number==0){ // 1
            // 等待
            this.wait();
        }
        number--;
        System.out.println(Thread.currentThread().getName()+"=>"+number);
        // 通知其他线程，我-1完毕了
        this.notifyAll();
    }

}
```

> 输出：

![image-20220205101754057](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205101754057.png)



`解决方法：将if改为while即可1-0-1-0的输出`

JDK1.8官方文档也给我们写出了建议：

![image-20220205102051089](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205102051089.png)



> JUC版的生产者和消费者问题

![image-20220205145152035](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205145152035.png)

通过Lock 找到 Condition

![image-20220205150247501](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205150247501.png)

![image-20220205150332516](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205150332516.png)

> Condition的方法：

![image-20220205150516042](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205150516042.png)

![image-20220205145220644](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205145220644.png)

> 代码实现：

```java
package com.juc.pc;

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class B {

        public static void main (String[]args){
            Data2 data = new Data2();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }, "A").start();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }, "B").start();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }, "C").start();


        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }, "D").start();
    }
    }

    // 判断等待，业务，通知
    class Data2 { // 数字 资源类

        private int number = 0;
        Lock lock=new ReentrantLock();
        Condition condition = lock.newCondition();


        //+1
        public  void increment() throws InterruptedException {
            lock.lock();
            try {
                while (number != 0) {  //0
                    // 等待
                    condition.await();
                }
                number++;
                System.out.println(Thread.currentThread().getName() + "=>" + number);
                // 通知其他线程，我+1完毕了
                condition.signalAll();
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                lock.unlock();
            }
        }

        //-1
        public  void decrement() throws InterruptedException {
            lock.lock();
            try {
                while (number == 0) { // 1
                    // 等待
                    condition.await();
                }
                number--;
                System.out.println(Thread.currentThread().getName() + "=>" + number);
                // 通知其他线程，我-1完毕了
                condition.signalAll();
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                lock.unlock();
            }
        }

    }
```

`输出：和Synchronized 版的输出是一样的,既然一样为什么要出现conditon呢？？？`   

![image-20220205151152168](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205151152168.png)

`记住一句话:任何一个新的技术，绝对不是仅仅只是覆盖了原来的技术，在于优势和补充！`



> Condition 实现精准的通知和唤醒线程

==话不多说，代码实现:==

```java
package com.juc.pc;

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

/**实现：
 * A 执行完调用B，B执行完调用C，C执行完调用A
 */
public class C {

    public static void main(String[] args) {
        Data3 data = new Data3();

        new Thread(()->{
            for (int i = 0; i <10 ; i++) {
                data.printA();
            }
        },"A").start();

        new Thread(()->{
            for (int i = 0; i <10 ; i++) {
                data.printB();
            }
        },"B").start();

        new Thread(()->{
            for (int i = 0; i <10 ; i++) {
                data.printC();
            }
        },"C").start();
    }

}

class Data3{ // 资源类 Lock

    private Lock lock = new ReentrantLock();
    private Condition condition1 = lock.newCondition();
    private Condition condition2 = lock.newCondition();
    private Condition condition3 = lock.newCondition();
    private int number = 1; // 1A  2B  3C

    public void printA(){
        lock.lock();
        try {
            // 业务，判断-> 执行-> 通知
            while (number!=1){
                // 等待
                condition1.await();
            }
            System.out.println(Thread.currentThread().getName()+"=>AAAAAAA");
            // 唤醒，唤醒指定的人，B
            number = 2;
            condition2.signal();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public void printB(){
        lock.lock();
        try {
            // 业务，判断-> 执行-> 通知
            while (number!=2){
                condition2.await();
            }
            System.out.println(Thread.currentThread().getName()+"=>BBBBBBBBB");
            // 唤醒，唤醒指定的人，c
            number = 3;
            condition3.signal();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
    public void printC(){
        lock.lock();
        try {
            // 业务，判断-> 执行-> 通知
            // 业务，判断-> 执行-> 通知
            while (number!=3){
                condition3.await();
            }
            System.out.println(Thread.currentThread().getName()+"=>BBBBBBBBB");
            // 唤醒，唤醒指定的人，c
            number = 1;
            condition1.signal();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

}
```

> 输出：

![image-20220205152559130](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205152559130.png)

`可以发现实现了精准唤醒`

## 8锁现象

`8锁，就是关于锁的8个问题`

**深刻理解我们的锁:**

> Question one and two：

```java
import java.util.concurrent.TimeUnit;

/**
 * 8锁，就是关于锁的8个问题
 * 1、标准情况下，两个线程先打印 发短信还是 打电话？ 1/发短信  2/打电话
 * 2、sendSms延迟4秒，两个线程先打印 发短信还是 打电话？ 1/发短信  2/打电话
 */
public class Test1 {
    public static void main(String[] args) {
        Phone phone = new Phone();

        //锁的存在
        new Thread(()->{
            phone.sendSms();
        },"A").start();

        // 捕获
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(()->{
            phone.call();
        },"B").start();
    }
}

class Phone{

    // synchronized 锁的对象是方法的调用者 也就是phone！
    // 两个方法用的是同一个锁，谁先拿到谁执行！
    public synchronized void sendSms(){
     /*   try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } */   //问题2打开这个注释
        System.out.println("发短信");
    }
    
    public synchronized void call(){
        System.out.println("打电话");
    }
}
```

`无论如何输出顺序都是先 发短信 后 打电话`



> Question Three：

```java
package com.juc.lock8;

import java.util.concurrent.TimeUnit;
/**
 * 3、 增加了一个普通方法后！先执行发短信还是Hello？ 普通方法
 */
public class Test2{
    public static void main(String[] args) {
        Phone phone = new Phone();

        //锁的存在
        new Thread(()->{
            phone.sendSms();
        },"A").start();

        // 捕获
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(()->{
            phone.hello();
        },"B").start();
    }
}

class Phone{

    // synchronized 锁的对象是方法的调用者！、
    public synchronized void sendSms(){
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("发短信");
    }

    public synchronized void call(){
        System.out.println("打电话");
    }
    //这里没有锁！不是同步方法，不受锁的影响
    public void hello(){
        System.out.println("hello");
    }

}
```

> 输出：

![image-20220205163927488](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205163927488.png)



> Question Four：

```java
import java.util.concurrent.TimeUnit;

/**
 * 4、 两个对象，两个同步方法， 发短信还是 打电话？ // 打电话
 */
public class Test2  {
    public static void main(String[] args) {
        // 两个对象，两个调用者，两把锁！
        Phone2 phone1 = new Phone2();
        Phone2 phone2 = new Phone2();

        //锁的存在
        new Thread(()->{
            phone1.sendSms();
        },"A").start();

        // 捕获
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(()->{
            phone2.call();
        },"B").start();
    }
}

class Phone2{

    // synchronized 锁的对象是方法的调用者！
    public synchronized void sendSms(){
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("发短信");
    }

    public synchronized void call(){
        System.out.println("打电话");
    }

    // 这里没有锁！不是同步方法，不受锁的影响
    public void hello(){
        System.out.println("hello");
    }

}
```

> 输出：先输出打电话然后才是发短信

![image-20220205164304018](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205164304018.png)



> Question Five：

```java
package com.juc.lock8;

import java.util.concurrent.TimeUnit;
/**
 * 5、增加两个静态的同步方法，只有一个对象，先打印 发短信？打电话？
 */
public class Test3 {
    public static void main(String[] args) {
        Phone3 phone3 = new Phone3();

        //锁的存在
        new Thread(()->{
            phone3.sendSms();
        },"A").start();

        // 捕获
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(()->{
            phone3.call();
        },"B").start();
    }
}

class Phone3{

    // synchronized 锁的对象是方法的调用者！
    public static synchronized void sendSms(){
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("发短信");
    }

    public static synchronized void call(){
        System.out.println("打电话");
    }



}
```

> 输出:

![image-20220205165324681](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205165324681.png)



> Question Six:

```java
package com.juc.lock8;

import java.util.concurrent.TimeUnit;
/*
 *  6、两个对象！增加两个静态的同步方法， 先打印 发短信？打电话？
 */
public class Test3  {
    public static void main(String[] args) {
        // 两个对象的Class类模板只有一个，static，锁的是Class
        Phone3 phone1 = new Phone3();
        Phone3 phone2 = new Phone3();

        //锁的存在
        new Thread(()->{
            phone1.sendSms();
        },"A").start();

        // 捕获
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(()->{
            phone2.call();
        },"B").start();
    }
}

// Phone3唯一的一个 Class 对象
class Phone3{

    // synchronized 锁的对象是方法的调用者！
    // static 静态方法
    // 类一加载就有了！锁的是Class
    public static synchronized void sendSms(){
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("发短信");
    }

    public static synchronized void call(){
        System.out.println("打电话");
    }


}
```

> 输出：

![image-20220205165445725](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205165445725.png)



> Question Seven：

```java
package com.juc.lock8;

import java.util.concurrent.TimeUnit;
/**
 * 6.1个静态的同步方法，1个普通的同步方法 ，一个对象，先打印 发短信？打电话？
 */
public class Test4 {
    public static void main(String[] args) {
        Phone4 phone = new Phone4();

        //锁的存在
        new Thread(()->{
            phone.sendSms();
        },"A").start();

        // 捕获
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(()->{
            phone.call();
        },"B").start();
    }
}

// Phone3唯一的一个 Class 对象
class Phone4{

    // 静态同步方法   锁的是Class 类模板
    public static synchronized void sendSms(){
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("发短信");
    }
        //普通的同步方法  锁的是调用者
    public  synchronized void call(){
        System.out.println("打电话");
    }


}


```

> 输出：

![image-20220205170257859](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205170257859.png)

`两个方法用的不是同一把锁`



> Question Eight:

```java
import java.util.concurrent.TimeUnit;

/**
 *8. 1个静态的同步方法，1个普通的同步方法 ，两个对象，先打印 发短信？打电话？
 */
public class Test4  {
    public static void main(String[] args) {
        // 两个对象的Class类模板只有一个，static，锁的是Class
        Phone4 phone1 = new Phone4();
        Phone4 phone2 = new Phone4();
        //锁的存在
        new Thread(()->{
            phone1.sendSms();
        },"A").start();

        // 捕获
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(()->{
            phone2.call();
        },"B").start();
    }
}

// Phone3唯一的一个 Class 对象
class Phone4{

    // 静态的同步方法 锁的是 Class 类模板
    public static synchronized void sendSms(){
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("发短信");
    }

    // 普通的同步方法  锁的调用者
    public synchronized void call(){
        System.out.println("打电话");
    }

}
```

> 输出：

![image-20220205170644380](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205170644380.png)



> 小结：

`明确锁到底锁的是谁即可！`



## 集合类不安全

> List在并发下不安全

```java
package com.juc.unsafe;

import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class ListTest {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            new Thread(()->{
                list.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(list);
            }).start();
        }
    }
}
```

> 输出：java.util.ConcurrentModificationException ==并发修改异常==

![image-20220205214104243](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205214104243.png)

> 解决方案：

```java
package com.juc.unsafe;

import java.util.ArrayList;
import java.util.List;
import java.util.UUID;
import java.util.Vector;

public class ListTest {
    public static void main(String[] args) {
        //并发ArrayList 不安全的
        /*
        * 解决方案：
        * 1.List<String> list = new Vector<>();
        * */
        List<String> list = new Vector<>();
        for (int i = 0; i <=10; i++) {
            new Thread(()->{
                list.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(list);
            }).start();
        }
    }
}
```

> 正常输出：

![image-20220205215242695](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205215242695.png)

并发下ArrayList不安全，那JDK的设计者不会在底层用`Synchronized`吗？

==查看ArrayList看add方法是什么时候出现的：==

![image-20220205215645576](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205215645576.png)

==查看Vector是什么时候出现的add这个方法的API：==

![image-20220205215757111](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220205215757111.png)

==Vector的add方法加了锁：==

![image-20220206151741797](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220206151741797.png)

`看一手源码吧`

```
private void ensureCapacityHelper(int minCapacity) {
    // overflow-conscious code
    if (minCapacity - elementData.length > 0)
        grow(minCapacity);
}
```

```
private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                     capacityIncrement : oldCapacity);
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    elementData = Arrays.copyOf(elementData, newCapacity);
}
```

==总结下就是用了锁 然后判断是否要扩容 赋值==

`Vector出现的明显比ArrayList早，Vector设置成线程安全的，那为什么不把ArrayList设置成线程安全的呢？肯定是存在ArrayList的应用场景` 

> 方案二：使用工具类Collections

```
package com.juc.unsafe;

import java.util.*;

public class ListTest {
    public static void main(String[] args) {
        //并发ArrayList 不安全的
        /*
        * 解决方案：
        * 1.List<String> list = new Vector<>();
        * 2、List<String> list = Collections.synchronizedList(new ArrayList<>());
        * */
       // List<String> list = new Vector<>();
      List<String> list = Collections.synchronizedList(new ArrayList<>());
        for (int i = 0; i <=10; i++) {
            new Thread(()->{
                list.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(list);
            }).start();
        }
    }
}
```

> 正常输出，线程安全不报错！



> 解决方案三：

==查看JUC包下的类（大部分里面的类都能保证线程安全）：==

![image-20220207011412014](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207011412014.png)

![image-20220207011424993](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207011424993.png)

![image-20220207011437331](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207011437331.png)

```java
package com.juc.unsafe;

import java.util.*;
import java.util.concurrent.CopyOnWriteArrayList;

public class ListTest {
    public static void main(String[] args) {
        //并发ArrayList 不安全的
        /*
        * 解决方案：
        * 1.List<String> list = new Vector<>();
        * 2、List<String> list = Collections.synchronizedList(new ArrayList<>());
        * 3、List<String> list = new CopyOnWriteArrayList<>()；
        * */
       // List<String> list = new Vector<>();
     // List<String> list = Collections.synchronizedList(new ArrayList<>());
        List<String> list = new CopyOnWriteArrayList<>();
        for (int i = 0; i <=10; i++) {
            new Thread(()->{
                list.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(list);
            }).start();
        }
    }
}
```

> 正常输出，线程安全不报错~！

==底层==：

```java
/**
 * Creates an empty list.
 */
public CopyOnWriteArrayList() {
    setArray(new Object[0]);
}
```

```java
/**
 * Sets the array.
 */
final void setArray(Object[] a) {
    array = a;
}
```

![image-20220206152027322](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220206152027322.png)

`CopyOnWriteArrayList 比 Vector Nb 在哪里？`

Vector的add方法：

![image-20220206152629043](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220206152629043.png)

`CopyOnWriteArrayList 用的是Lock锁，效率较高，来看一手源码吧！`

```
public boolean add(E e) {
    final ReentrantLock lock = this.lock;
    lock.lock();
    try {
        Object[] elements = getArray();
        int len = elements.length;
        Object[] newElements = Arrays.copyOf(elements, len + 1);
        newElements[len] = e;
        setArray(newElements);
        return true;
    } finally {
        lock.unlock();
    }
}
```

==从下面源码可以看出写的时候先复制一个数组出来：==

```java
public static <T> T[] copyOf(T[] original, int newLength) {
    return (T[]) copyOf(original, newLength, original.getClass());
}
```

`CopyOnWrite 写入时复制 COW思想 计算机程序设计领域的一种优化策略；`    

``多个线程调用的时候，（使用的是list），读取的时候，固定的，写入（覆盖）` 

`在写入的时候避免覆盖抛出的异常，造成数据问题就设计出了写入时复制思想 ` 

参考博客：[(28条消息) 多线程进阶（三）-- JUC中CopyOnWriteArrayList()解析及为什么要复制_leo_messi94的博客-CSDN博客](https://blog.csdn.net/weixin_39724194/article/details/107413655?ops_request_misc=%7B%22request%5Fid%22%3A%22164483169316780265450038%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=164483169316780265450038&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-18-107413655.pc_search_result_cache&utm_term=CopyOnWrite为什么要写时复制那么麻烦&spm=1018.2226.3001.4187)

==具体：==

CopyOnWrite的add方法先复制一个新数组出来，写完在把新数组插入回去 保证线程安全（==上面源码可以得出来这个结论==）

==学习方法推荐：1、会用 2、货比三家（寻找其他解决方案） 3、分析源码==



> Set不安全

查看一幅图

![image-20220207013836955](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207013836955.png)

==显示实现类==

![image-20220207013959636](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207013959636.png)

可以看到`List`，`Set`和`BlockingQueue（阻塞队列）`都是Collection的实现类

![image-20220207014116925](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207014116925.png)

实例代码：

```java
package com.juc.unsafe;

import java.util.Collections;
import java.util.HashSet;
import java.util.Set;
import java.util.UUID;
import java.util.concurrent.CopyOnWriteArraySet;

/**
 * 同理可证 ： ConcurrentModificationException
 * //1、Set<String> set = Collections.synchronizedSet(new HashSet<>());   工具类
 * //2、 Set<String> set = new CopyOnWriteArraySet<>();           
 */
public class SetTest {
    public static void main(String[] args) {
        //Set<String> set = new HashSet<>();
        //1 Collections.synchronizedSet(new HashSet<>());
        //Set<String> set = Collections.synchronizedSet(new HashSet<>());
        Set<String> set = new CopyOnWriteArraySet<>();
        for (int i = 0; i <= 30; i++) {
            new Thread(()->{
              set.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(set);
            }).start();
        }
    }
}
```

==JUC为我们提供的类:==

![image-20220207020051612](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207020051612.png)

> 底层  

```
public CopyOnWriteArraySet() {
    al = new CopyOnWriteArrayList<E>();
}

public CopyOnWriteArrayList() {
        setArray(new Object[0]);
    }

final void setArray(Object[] a) {
        array = a;
    }
private transient volatile Object[] array;

//add方法
  public boolean add(E e) {
        return al.addIfAbsent(e);
    }
    
  public boolean addIfAbsent(E e) {
        Object[] snapshot = getArray();
        return indexOf(e, snapshot, 0, snapshot.length) >= 0 ? false :
            addIfAbsent(e, snapshot);
    }
  private static int indexOf(Object o, Object[] elements,
                               int index, int fence) {
        if (o == null) {
            for (int i = index; i < fence; i++)
                if (elements[i] == null)
                    return i;
        } else {
            for (int i = index; i < fence; i++)
                if (o.equals(elements[i]))
                    return i;
        }
        return -1;
    }
    //可以再看看
     private boolean addIfAbsent(E e, Object[] snapshot) {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            Object[] current = getArray();
            int len = current.length;
            if (snapshot != current) {
                // Optimize for lost race to another addXXX operation
                int common = Math.min(snapshot.length, len);
                for (int i = 0; i < common; i++)
                    if (current[i] != snapshot[i] && eq(e, current[i]))
                        return false;
                if (indexOf(e, current, common, len) >= 0)
                        return false;
            }
            Object[] newElements = Arrays.copyOf(current, len + 1);
            newElements[len] = e;
            setArray(newElements);
            return true;
        } finally {
            lock.unlock();
        }
    }

```

> HashSet的底层是什么？

查看源码：

```java
public HashSet() {
    map = new HashMap<>();
}
//add方法   set 本质就是map key是无法重复的
public boolean add(E e) {
   return map.put(e, PRESENT)==null;
    }

private static final Object PRESENT = new Object(); //不变的常量 不影响
```

==**得闲把狂神HashMap的文章看了（两篇 讲的是底层）**==

> Map 不安全

回顾Map的基本操作：

![image-20220207100116657](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207100116657.png)

> JUC为我们提供的线程安全的集合

![image-20220207100335625](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207100335625.png)

![image-20220207100403154](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207100403154.png)

![image-20220207100433976](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207100433976.png)



```java
package com.juc.unsafe;

import java.util.Collections;
import java.util.HashMap;
import java.util.Map;
import java.util.UUID;
import java.util.concurrent.ConcurrentHashMap;

// ConcurrentModificationException
public class MapTest {

    public static void main(String[] args) {
        // map 是这样用的吗？ 不是，工作中不用 HashMap 因为线程不安全
        // 默认等价于什么？  new HashMap<>(16,0.75);
        // Map<String, String> map = new HashMap<>();


        //方法一： 集合类
     //   Map<String, String> map = Collections.synchronizedMap(new HashMap<>());

        //方法二： 使用JUC为我们提供的Map集合实现类
      Map<String, String> map = new ConcurrentHashMap<>();
        for (int i = 1; i <=30; i++) {
            new Thread(()->{
                map.put(Thread.currentThread().getName(),UUID.randomUUID().toString().substring(0,5));
                System.out.println(map);
            },String.valueOf(i)).start();
        }

    }
}
```



## Callable（简单）

JDK官方文档介绍：

![image-20220207102127989](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207102127989.png)

1. ==可以有返回值==
2. ==可以抛出异常==
3. ==方法不同，run()/ call()==



> 代码测试

![image-20220207102353909](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207102353909.png)

==如何让Callbale和Thread有联系呢？==  利用FutureTask作桥梁

![image-20220207102519516](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207102519516.png)

==构造函数里面可以放Callable实现类==

![image-20220207102618276](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207102618276.png)

```java
package com.juc.callable;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;


public class CallableTest {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // new Thread(new Runnable()).start();
        // new Thread(new FutureTask<V>()).start();
        // new Thread(new FutureTask<V>( Callable )).start();
        new Thread().start(); // 怎么启动Callable

        MyThread thread = new MyThread();
        FutureTask futureTask = new FutureTask(thread); // 适配类

        new Thread(futureTask,"A").start();
        new Thread(futureTask,"B").start(); // 结果会被缓存，效率高

        Integer o = (Integer) futureTask.get(); //这个get 方法可能会产生阻塞！把他放到最后
        // 或者使用异步通信来处理！
        System.out.println(o);

    }
}

class MyThread implements Callable<Integer> {

    @Override
    public Integer call() {
        System.out.println("call()"); // 会打印几个call  1个 结果会被缓存
        // 耗时的操作 导致get()方法阻塞 所以将get方法一般放最后
        return 1024;
    }

}

```

> 输出：

![image-20220207103210664](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207103210664.png)

**细节**：

1、有缓存

2、结果可能需要等待，会阻塞！



## 常用辅助类（必会）

==三个常用的辅助类 都是JUC下的==



### 1.CountDownLatch

==JDK文档说明：==

![image-20220207110320001](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207110320001.png)

> 演示代码：

```java
package com.juc.add;

import java.util.concurrent.CountDownLatch;

//计数器
public class CountDownLatchDemo {
    public static void main(String[] args) throws InterruptedException {
        //总数是6，必须要执行任务的时候用
        CountDownLatch countDownLatch = new CountDownLatch(6);
        for (int i = 0; i < 6; i++) {
            new Thread(()->{
                System.out.println(Thread.currentThread().getName()+"Go out");
                countDownLatch.countDown(); //数量-1
            }).start();
        }
     //   countDownLatch.await(); //等待计数器归零 然后再向下执行
        System.out.println("Close Door");
    }
}
```

输出：

![image-20220207110020390](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207110020390.png)

 打开countDownLatch.await()的注释，输出：

![image-20220207111017246](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207111017246.png)

> 原理：

`countDownLatch.countDown();` // 数量-1

`countDownLatch.await();` // 等待计数器归零，然后再向下执行

每次有线程调用 countDown() 数量-1，加入计数器变为0，countDownLatch.await() 就会被唤醒，主线程继续执行！



### 2.CyclicBarrier

JDK文档说明：

![image-20220207150736851](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207150736851.png)

```java
package com.juc.add;

import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class CyclicBarrierDemo {
    public static void main(String[] args) {
        /**
         * 集齐7颗龙珠召唤神龙
         */
        // 召唤龙珠的线程
        // public CyclicBarrier(int parties, Runnable barrierAction) {       }
        CyclicBarrier cyclicBarrier = new CyclicBarrier(8,()->{
            System.out.println("召唤神龙成功！");
        });

        for (int i = 1; i <=7 ; i++) {
            final int temp = i;
            // lambda能操作到 i 吗  不能 只能操作fina 常量
            new Thread(()->{
                System.out.println(Thread.currentThread().getName()+"收集"+temp+"个龙珠");
                try {
                    cyclicBarrier.await(); // 等待
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } catch (BrokenBarrierException e) {
                    e.printStackTrace();
                }
            }).start();
        }



    }
}
```

> 输出：

![image-20220207111931738](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207111931738.png)

 上面代码如果CyclicBarrier改为8 则永远等待 因为循环是7 永远到达不了8



### 3.Semaphore

Semaphore：翻译过来就是信号量

JDK文档说明：

![image-20220207152156812](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207152156812.png)

> 演示代码：

抢车位！6车---3个停车位置

```java
package com.juc.add;

import java.util.concurrent.Semaphore;
import java.util.concurrent.TimeUnit;

public class SemaphoreDemo {
    public static void main(String[] args) {
        // 线程数量：停车位! 限流！
        // 初始化线程数量为3
        Semaphore semaphore = new Semaphore(3);

        for (int i = 1; i <=6 ; i++) {
            new Thread(()->{
                // acquire() 得到
                try {
                    semaphore.acquire();
                    System.out.println(Thread.currentThread().getName()+"抢到车位");
                    TimeUnit.SECONDS.sleep(2);
                    System.out.println(Thread.currentThread().getName()+"离开车位");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    semaphore.release(); // release() 释放
                }

            },String.valueOf(i)).start();
        }

    }
}
```

> 输出：

![image-20220207152251863](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207152251863.png)

**原理：**

`semaphore.acquire() `获得，假设如果已经满了，等待，等待被释放为止！

`semaphore.release();`当前的信号量释放 + 1，然后唤醒等待的线程！

作用： 多个==共享资源互斥==的使用！==并发限流，控制最大的线程数==！



## 读写锁

`ReadWriteLock`

JDK文档说明：

![image-20220207162652516](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207162652516.png)



> 测试代码：

```java
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

/**
 * 独占锁（写锁） 一次只能被一个线程占有
 * 共享锁（读锁） 多个线程可以同时占有
 * ReadWriteLock
 * 读-读  可以共存！
 * 读-写  不能共存！
 * 写-写  不能共存！
 */
public class ReadWriteLockDemo {
    public static void main(String[] args) {
        MyCache myCache = new MyCache();

        // 写入
        for (int i = 1; i <= 5 ; i++) {
            final int temp = i;
            new Thread(()->{
                myCache.put(temp+"",temp+"");
            },String.valueOf(i)).start();
        }

        // 读取
        for (int i = 1; i <= 5 ; i++) {
            final int temp = i;
            new Thread(()->{
                myCache.get(temp+"");
            },String.valueOf(i)).start();
        }

    }
}

/**
 * 自定义缓存
 */
class MyCache{

    private volatile Map<String,Object> map = new HashMap<>();

    // 存，写
    public void put(String key,Object value){
        System.out.println(Thread.currentThread().getName()+"写入"+key);
        map.put(key,value);
        System.out.println(Thread.currentThread().getName()+"写入OK");
    }

    // 取，读
    public void get(String key){
        System.out.println(Thread.currentThread().getName()+"读取"+key);
        Object o = map.get(key);
        System.out.println(Thread.currentThread().getName()+"读取OK");
    }

}
```

> 输出： 1还没写完 5就开始写了 写入的时候被其他线程插队了

![image-20220207154321183](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207154321183.png)

> 修改：缓存中加个读写锁

```java
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

/**
 * 独占锁（写锁） 一次只能被一个线程占有
 * 共享锁（读锁） 多个线程可以同时占有
 * ReadWriteLock
 * 读-读  可以共存！
 * 读-写  不能共存！
 * 写-写  不能共存！
 */
public class ReadWriteLockDemo {
    public static void main(String[] args) {
        //MyCache myCache = new MyCache();
        MyCacheLock myCacheLock = new MyCacheLock();
        // 写入
        for (int i = 1; i <= 5 ; i++) {
            final int temp = i;
            new Thread(()->{
                myCacheLock.put(temp+"",temp+"");
            },String.valueOf(i)).start();
        }

        // 读取
        for (int i = 1; i <= 5 ; i++) {
            final int temp = i;
            new Thread(()->{
                myCacheLock.get(temp+"");
            },String.valueOf(i)).start();
        }

    }
}

// 加锁的
class MyCacheLock{

    private volatile Map<String,Object> map = new HashMap<>();
    // 读写锁： 更加细粒度的控制（读锁和写锁）
    private ReadWriteLock readWriteLock = new ReentrantReadWriteLock();

    // 存，写入的时候，只希望同时只有一个线程写
    public void put(String key,Object value){
        readWriteLock.writeLock().lock();
        try {
            System.out.println(Thread.currentThread().getName()+"写入"+key);
            map.put(key,value);
            System.out.println(Thread.currentThread().getName()+"写入OK");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            readWriteLock.writeLock().unlock();
        }
    }

    // 取，读，所有人都可以读！
    public void get(String key){
        readWriteLock.readLock().lock();
        try {
            System.out.println(Thread.currentThread().getName()+"读取"+key);
            Object o = map.get(key);
            System.out.println(Thread.currentThread().getName()+"读取OK");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            readWriteLock.readLock().unlock();
        }
    }

}

/**
 * 自定义缓存
 */
class MyCache{

    private volatile Map<String,Object> map = new HashMap<>();

    // 存，写
    public void put(String key,Object value){
        System.out.println(Thread.currentThread().getName()+"写入"+key);
        map.put(key,value);
        System.out.println(Thread.currentThread().getName()+"写入OK");
    }

    // 取，读
    public void get(String key){
        System.out.println(Thread.currentThread().getName()+"读取"+key);
        Object o = map.get(key);
        System.out.println(Thread.currentThread().getName()+"读取OK");
    }

}
```

> 输出：

![image-20220207162913642](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207162913642.png)

## 阻塞队列(需要熟练掌握且使用)

![image-20220207164934398](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207164934398.png)

阻塞队列：

文档说明：

![image-20220207182910669](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207182910669.png)

![image-20220207183218485](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207183218485.png)

**BlockingQueue** BlockingQueue 不是新的东西

![image-20220207183308459](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207183308459.png)

什么情况下我们会使用 阻塞队列：==多线程并发处理，线程池！==

**学会使用队列**

添加 、 移除

> 四组API

| 方式         | 抛出异常  | 有返回值 | 阻塞 等待 | 超时等待  |
| ------------ | --------- | -------- | --------- | --------- |
| 添加         | add()     | offer()  | put()     | offer(,,) |
| 移除         | remove()  | poll()   | take()    | poll(,)   |
| 检测队首元素 | element() | peek()   | -         | -         |

第一组API：

```java
  /*
     *抛出异常
     * */
    public static void test1(){
        //参数是 队列的大小
        Queue blockingQueue = new ArrayBlockingQueue<>(3);
        System.out.println(blockingQueue.add("a"));
        System.out.println(blockingQueue.add("b"));
        System.out.println(blockingQueue.add("c"));

        System.out.println(blockingQueue.element());//查看队首元素是啥 a

        //IllegalStateException: Queue full 抛出异常！
       // System.out.println(blockingQueue.add("d"));
        System.out.println("=====================");
        System.out.println(blockingQueue.remove());
        System.out.println(blockingQueue.remove());
        System.out.println(blockingQueue.remove());

        //java.util.NoSuchElementException 抛出异常
        //System.out.println(blockingQueue.element());

        //java.util.NoSuchElementException 抛出异常！！！
      //  System.out.println(blockingQueue.remove());

```

第二组API：

```java
  /*
     *有返回值，没有异常
     * */
    public static void test2(){
        //参数是 队列的大小
        Queue blockingQueue = new ArrayBlockingQueue<>(3);
        System.out.println(blockingQueue.offer("a"));
        System.out.println(blockingQueue.offer("b"));
        System.out.println(blockingQueue.offer("c"));

        System.out.println(blockingQueue.peek());//查看队首元素是啥 a

       // System.out.println(blockingQueue.offer("d"));// false 不抛出异常
       System.out.println("=====================");
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());

        System.out.println(blockingQueue.peek());//查看队首元素是啥 null

        System.out.println(blockingQueue.poll());// null 不抛出异常
    }
```

第三种API:

```
 /*
     *等待阻塞 （一直阻塞）
     * */
    public static void test3() throws InterruptedException {
        //参数是 队列的大小
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);
        //一直阻塞
        blockingQueue.put("a");
        blockingQueue.put("b");
        blockingQueue.put("c");

       //// blockingQueue.put("d"); // 队列没有位置了，一直阻塞 main线程永远结束不了

        System.out.println(blockingQueue.take());
        System.out.println(blockingQueue.take());
        System.out.println(blockingQueue.take());
        System.out.println(blockingQueue.take()); // 没有这个元素，一直阻塞
    }
}
```

![image-20220207192214979](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207192214979.png)



第四组API：

```java
   /*
     *等待，阻塞（等待超时）
     * */
    public static void test4() throws InterruptedException {
        //参数是 队列的大小
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);
        blockingQueue.offer("a");
        blockingQueue.offer("b");
        blockingQueue.offer("c");

        blockingQueue.offer("d",2, TimeUnit.SECONDS);//等待超过2s就退出

        System.out.println("=====================");
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());

        blockingQueue.poll(2,TimeUnit.SECONDS);//等待超过两s就退出
    }
```



## 同步队列SynchronousQueue

==从JDK文档可知:SynchronousQueue实现类BlockingQueue接口==

进去一个元素，必须等待取出来之后，才能再往里面放一个元素！put、take

> 测试代码：

```java
import java.sql.Time;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.SynchronousQueue;
import java.util.concurrent.TimeUnit;

/**
 * 同步队列
 * 和其他的BlockingQueue 不一样， SynchronousQueue 只能存储一个元素
 * put了一个元素，必须从里面先take取出来，否则不能在put进去值！
 */
public class SynchronousQueueDemo {
    public static void main(String[] args) {
        BlockingQueue<String> blockingQueue = new SynchronousQueue<>(); // 同步队列

        new Thread(()->{
            try {
                System.out.println(Thread.currentThread().getName()+" put 1");
                blockingQueue.put("1");
                System.out.println(Thread.currentThread().getName()+" put 2");
                blockingQueue.put("2");
                System.out.println(Thread.currentThread().getName()+" put 3");
                blockingQueue.put("3");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        },"T1").start();


        new Thread(()->{
            try {
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName()+"=>"+blockingQueue.take());
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName()+"=>"+blockingQueue.take());
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName()+"=>"+blockingQueue.take());
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        },"T2").start();
    }
}
```

> 输出：

![image-20220207203545516](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220207203545516.png)

**学了技术，不会用！ 是因为看的少，用的少！**



## 线程池(重点)

==**线程池：三大方法、7大参数、4种拒绝策略**==

> 池化技术

程序的运行，本质：占用系统的资源！ 优化资源的使用！=>池化技术

线程池、JDBC连接池、内存池、对象池///..... 创建、销毁。十分浪费资源

池化技术：事先准备好一些资源，有人要用，就来我这里拿，用完之后还给我。



**线程池的好处**:

1、降低资源的消耗

2、提高响应的速度

3、方便管理。 线程是稀缺资源 频繁创建和销毁 会导致系统防护下降

==线程复用、可以控制最大并发数、管理线程==



> 三大方法：

==阿里规范手册提出：==

![image-20220208195011238](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220208195011238.png)

```java
package com.juc.pool;

import java.util.concurrent.Executor;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

// Executors 工具类 三大方法
public class Demo1 {
    public static void main(String[] args) {
       // 1 ExecutorService threadPool = Executors.newSingleThreadExecutor();//单个线程

        // 2 ExecutorService threadPool = Executors.newFixedThreadPool(5);//创建一个固定的线程池大小
        // 3
     ExecutorService threadPool = Executors.newCachedThreadPool();//可伸缩的，遇强则强 遇弱则弱

        try {
            for (int i = 0; i < 100; i++) {
                //使用了线程池之后 使用线程来创建线程
                threadPool.execute(()->{
                    System.out.println(Thread.currentThread().getName()+" ok");
                });
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //线程池用完，程序结束，关闭线程池
            threadPool.shutdown();
        }
    }
}
```

> 对应的输出：

![image-20220208200234694](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220208200234694.png)

![image-20220208200153583](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220208200153583.png)

![image-20220208200300637](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220208200300637.png)



> 七大参数

源码分析：

```java
public static ExecutorService newSingleThreadExecutor() {
    return new FinalizableDelegatedExecutorService
        (new ThreadPoolExecutor(1, 1,
                                0L, TimeUnit.MILLISECONDS,
                                new LinkedBlockingQueue<Runnable>()));
}

   public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
    }
    
       public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
    }
    // 本质 ThreadPoolExecutor()
      public ThreadPoolExecutor(int corePoolSize,   
                              int maximumPoolSize,   
                              long keepAliveTime,   
                              TimeUnit unit,       
                              BlockingQueue<Runnable> workQueue) {   
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
             Executors.defaultThreadFactory(), defaultHandler);
    }
    
    public ThreadPoolExecutor(int corePoolSize,    // 核心线程池大小
                              int maximumPoolSize,  // 最大线程池大小
                              long keepAliveTime,  // 超时了没有人调用 就会释放
                              TimeUnit unit,      // 超时单位
                              BlockingQueue<Runnable> workQueue,  // 阻塞队列
                              ThreadFactory threadFactory, // 线程工厂:创建线程的 一般不用动
                              RejectedExecutionHandler handler)  // 拒绝策略  { 
        if (corePoolSize < 0 ||
            maximumPoolSize <= 0 ||
            maximumPoolSize < corePoolSize ||
            keepAliveTime < 0)
            throw new IllegalArgumentException();
        if (workQueue == null || threadFactory == null || handler == null)
            throw new NullPointerException();
        this.acc = System.getSecurityManager() == null ?
                null :
                AccessController.getContext();
        this.corePoolSize = corePoolSize;
        this.maximumPoolSize = maximumPoolSize;
        this.workQueue = workQueue;
        this.keepAliveTime = unit.toNanos(keepAliveTime);
        this.threadFactory = threadFactory;
        this.handler = handler;
    }

```

![image-20220208211557235](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220208211557235.png)

![image-20220208211608592](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220208211608592.png)

==上图补充说明下：==

刚开始1 2窗口是开着的 3 4 5关闭 

后面来了 陆陆续续来了人 当达到第五个人 3 4 5 窗口就开始打开

若与此同时又来人 当第四个出现的时候（窗口的人都还没办理完毕）就会采取拒绝策略



> 手动创建一个线程

```java
package com.juc.pool;

import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.*;

// Executors 工具类、3大方法

/**
 * new ThreadPoolExecutor.AbortPolicy() // 银行满了，还有人进来，不处理这个人的，抛出异常
 * new ThreadPoolExecutor.CallerRunsPolicy() // 哪来的去哪里！
 * new ThreadPoolExecutor.DiscardPolicy() //队列满了，丢掉任务，不会抛出异常！
 * new ThreadPoolExecutor.DiscardOldestPolicy() //队列满了，尝试去和最早的竞争，也不会抛出异常！
 */
public class Demo01 {
    public static void main(String[] args) {
        // 自定义线程池！工作 ThreadPoolExecutor
        ExecutorService threadPool = new ThreadPoolExecutor(
                2,
                5,  
                3,  // 超时了没有人调用 就会释放
                TimeUnit.SECONDS,
                new LinkedBlockingDeque<>(3), //类似等候区
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.AbortPolicy()); 
        try {
            // 最大承载：Deque + max
            // 超过 RejectedExecutionException
            for (int i = 1; i <= 9; i++) { // i达到6之后就会触发第三个线程出来工作 i达到9就会拒绝第九次循环
                // 使用了线程池之后，使用线程池来创建线程
                threadPool.execute(()->{
                    System.out.println(Thread.currentThread().getName()+" ok");
                });
            }

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // 线程池用完，程序结束，关闭线程池
            threadPool.shutdown(); 
        }

    }
}

```

> 输出:只有八个语句 最后一个被拒绝掉了 并且抛异常

![image-20220208213948367](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220208213948367.png)



> 四种拒绝策略

![image-20220208220906088](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220208220906088.png)

```java
/**
 * new ThreadPoolExecutor.AbortPolicy() // 银行满了，还有人进来，不处理这个人的，抛出异常
 * new ThreadPoolExecutor.CallerRunsPolicy() // 哪来的去哪里！
 * new ThreadPoolExecutor.DiscardPolicy() //队列满了，丢掉任务，不会抛出异常！
 * new ThreadPoolExecutor.DiscardOldestPolicy() //队列满了，尝试去和最早的竞争，也不会抛出异常！
 */
```

> 如果用第二个:输出：

![image-20220208221012913](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220208221012913.png)

==哪来的去哪里！ 主线程这边来的 主线程来执行！==



> 小结与展望

==池的最大的大小如何去设置！==

了解：IO密集型，CPU密集型：（调优）

```java
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.*;

// Executors 工具类、3大方法


public class Demo01 {
    public static void main(String[] args) {
        // 自定义线程池！工作 ThreadPoolExecutor

        // 最大线程到底该如何定义
        // 1、CPU 密集型，几核，就是几，可以保持CPu的效率最高！
        // 2、IO  密集型   > 判断你程序中十分耗IO的线程，
        // 程序   15个大型任务  io十分占用资源！ 那就定义30个线程（两倍） 这个还是要具体情况具体分析

        // 获取CPU的核数
        System.out.println(Runtime.getRuntime().availableProcessors());

        ExecutorService threadPool = new ThreadPoolExecutor(
                2,
                Runtime.getRuntime().availableProcessors(),
                3,
                TimeUnit.SECONDS,
                new LinkedBlockingDeque<>(3),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.DiscardOldestPolicy());  //队列满了，尝试去和最早的竞争，也不会抛出异常！
        try {
            // 最大承载：Deque + max
            // 超过 RejectedExecutionException
            for (int i = 1; i <= 9; i++) {
                // 使用了线程池之后，使用线程池来创建线程
                threadPool.execute(()->{
                    System.out.println(Thread.currentThread().getName()+" ok");
                });
            }

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // 线程池用完，程序结束，关闭线程池
            threadPool.shutdown();
        }

    }
}
```



## 四大函数接口（必须掌握）

新时代的程序员：lambda表达式、链式编程、函数式接口、Stream流式计算

`四大函数接口位于java.util.function包下`

> 函数式接口： 只有一个方法的接口

```java
@FunctionalInterface
public interface Runnable {
    public abstract void run();
}

// 泛型、枚举、反射
// lambda表达式、链式编程、函数式接口、Stream流式计算
// 超级多FunctionalInterface
// 简化编程模型，在新版本的框架底层大量应用！ 因此需要学习函数式接口和lambda表达式
// foreach(参数是:消费者类的函数式接口)

```

![image-20220209154121311](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209154121311.png)

![image-20220209154133000](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209154133000.png)



> Function函数式接口：

![image-20220209154459738](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209154459738.png)

```java
package com.juc.function;

import java.util.ArrayList;
import java.util.function.Function;

/**
 * Function 函数型接口, 有一个输入参数，有一个输出
 * 只要是 函数型接口 可以 用 lambda表达式简化
 */
public class Demo01 {
    public static void main(String[] args) {
        //匿名内部类
//        Function<String,String> function = new Function<String,String>() {
//            @Override
//            public String apply(String str) {
//                return str;
//            }
//        };
      
        Function<String,String> function = str->{return str;};

        System.out.println(function.apply("asd"));
    }
}
```

> 断定型接口：有一个输入参数，返回值只能是 布尔值！

![image-20220209160037584](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209160037584.png)

```
package com.juc.function;

import java.util.function.Predicate;

/**
 * 断定型接口：有一个输入参数，返回值只能是 布尔值！
 */
public class Demo02 {
    public static void main(String[] args) {
        // 判断字符串是否为空
//        Predicate<String> predicate = new Predicate<String>(){
////            @Override
////            public boolean test(String str) {
////                return str.isEmpty();
////            }
////        };

        Predicate<String> predicate = (str)->{return str.isEmpty(); };
        System.out.println(predicate.test(""));

    }
}
```



> Consumer 消费型接口

![image-20220209160852658](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209160852658.png)

```java
package com.juc.function;

import java.util.function.Consumer;

/**
 * Consumer 消费型接口: 只有输入，没有返回值
 */
public class Demo03 {
    public static void main(String[] args) {
//        Consumer<String> consumer = new Consumer<String>() {
//            @Override
//            public void accept(String str) {
//                System.out.println(str);
//            }
//        };
        Consumer<String> consumer = (str)->{System.out.println(str);};
        consumer.accept("sdadasd");
    }
}
```

> Supplier 供给型接口

![image-20220209161247098](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209161247098.png)

```java
package com.juc.function;

import java.util.function.Supplier;

/**
 * Supplier 供给型接口 没有参数，只有返回值
 */
public class Demo04 {
    public static void main(String[] args) {
//        Supplier supplier = new Supplier<Integer>() {
//            @Override
//            public Integer get() {
//                System.out.println("get()");
//                return 1024;
//            }
//        };

        Supplier supplier = ()->{ return 1024; };
        System.out.println(supplier.get());
    }
}
```



## Stream流式计算

大数据：存储 + 计算

集合、MySQL 本质就是存储东西的；

计算都应该交给流来操作！

==查看java.util.stream相关的API可以发现参数清一色是函数式接口:==

![image-20220209162848745](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209162848745.png)

> 实操：

```java
package com.juc.Stream;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

// 有参，无参构造，get、set、toString方法！
@Data
@NoArgsConstructor
@AllArgsConstructor
public class User {

    private int id;
    private String name;
    private int age;

}


```

```java
package com.juc.Stream;

import java.util.Arrays;
import java.util.List;

/**
 * 题目要求：一分钟内完成此题，只能用一行代码实现！
 * 现在有5个用户！筛选：
 * 1、ID 必须是偶数
 * 2、年龄必须大于23岁
 * 3、用户名转为大写字母
 * 4、用户名字母倒着排序
 * 5、只输出一个用户！
 */
public class Test {
    public static void main(String[] args) {
        User u1 = new User(1,"a",21);
        User u2 = new User(2,"b",22);
        User u3 = new User(3,"c",23);
        User u4 = new User(4,"d",24);
        User u5 = new User(6,"e",25);
        // 集合就是存储
        List<User> list = Arrays.asList(u1, u2, u3, u4, u5);

        // 计算交给Stream流
        // lambda表达式、链式编程、函数式接口、Stream流式计算
        list.stream()
                .filter(u->{return u.getId()%2==0;})
                .filter(u->{return u.getAge()>23;})
                .map(u->{return u.getName().toUpperCase();})
                .sorted((uu1,uu2)->{return uu2.compareTo(uu1);})
                .limit(1)
                .forEach(System.out::println);  //这就是链式编程
    }
}
```

> 输出：

![image-20220209163026579](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209163026579.png)



## ForkJoin

> 什么是 ForkJoin

ForkJoin 在 JDK 1.7就出现了， 并行执行任务！提高效率。`大数据量`！

![image-20220209194751579](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209194751579.png)

> ForkJoin 特点：工作窃取

这个里面维护的都是`双端队列`

![image-20220209194828726](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209194828726.png)

> ForkJoin

forkjoinPool里面的方法 `execute`

![image-20220209200413507](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209200413507.png)

![image-20220209195008923](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209195008923.png)

![image-20220209200638791](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209200638791.png)

> 具体编码：

```java
package com.juc.forkjoin;

import java.util.concurrent.RecursiveTask;

/**
 * 求和计算的任务！
 * 3000   6000（ForkJoin）  9000（Stream并行流）
 * // 如何使用 forkjoin
 * // 1、forkjoinPool 通过它来执行
 * // 2、计算任务 forkjoinPool.execute(ForkJoinTask task)
 * // 3. 计算类要继承 ForkJoinTask
 */
public class ForkJoinDemo extends RecursiveTask<Long> {

    private Long start;  // 1
    private Long end;    // 1990900000

    // 临界值
    private Long temp = 10000L;

    public ForkJoinDemo(Long start, Long end) {
        this.start = start;
        this.end = end;
    }

    // 计算方法
    @Override
    protected Long compute() {
        if ((end-start)<temp){
            Long sum = 0L;
            for (Long i = start; i <= end; i++) {
                sum += i;
            }
            return sum;
        }else { // forkjoin 递归
            long middle = (start + end) / 2; // 中间值
            ForkJoinDemo task1 = new ForkJoinDemo(start, middle);
            task1.fork(); // 拆分任务，把任务压入线程队列
            ForkJoinDemo task2 = new ForkJoinDemo(middle+1, end);
            task2.fork(); // 拆分任务，把任务压入线程队列

            return task1.join() + task2.join();
        }
    }
}
```

```java
package com.juc.forkjoin;

import java.util.concurrent.ExecutionException;
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.ForkJoinTask;
import java.util.stream.DoubleStream;
import java.util.stream.IntStream;
import java.util.stream.LongStream;

/**
 * 同一个任务，别人效率高你几十倍！
 */
public class Test {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // test1(); // 12224
        // test2(); // 10038
        // test3(); // 153
    }

    // 普通程序员
    public static void test1(){
        Long sum = 0L;
        long start = System.currentTimeMillis();
        for (Long i = 1L; i <= 10_0000_0000; i++) {
            sum += i;
        }
        long end = System.currentTimeMillis();
        System.out.println("sum="+sum+" 时间："+(end-start));
    }

    // 会使用ForkJoin
    public static void test2() throws ExecutionException, InterruptedException {
        long start = System.currentTimeMillis();

        ForkJoinPool forkJoinPool = new ForkJoinPool();
        ForkJoinTask<Long> task = new ForkJoinDemo(0L, 10_0000_0000L);
        ForkJoinTask<Long> submit = forkJoinPool.submit(task);// 提交任务
        Long sum = submit.get();

        long end = System.currentTimeMillis();

        System.out.println("sum="+sum+" 时间："+(end-start));
    }

    public static void test3(){
        long start = System.currentTimeMillis();
        // Stream并行流 range() rangeClosed (]
        long sum = LongStream.rangeClosed(0L, 10_0000_0000L).parallel().reduce(0, Long::sum);
        long end = System.currentTimeMillis();
        System.out.println("sum="+"时间："+(end-start));
    }

}

```



## 异步回调

> Future 设计的初衷： 对将来的某个事件的结果进行建模

![image-20220209205616678](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209205616678.png)

![image-20220209211104058](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209211104058.png)

```java
package com.juc.future;

import java.util.concurrent.*;

/**
 * 异步调用： CompletableFuture  类似AJAX
 * // 异步执行
 * // 成功回调
 * // 失败回调
 */
public class Demo01 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // 没有返回值的 runAsync 异步回调
      /* CompletableFuture<Void> completableFuture = CompletableFuture.runAsync(()->{
           try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
              e.printStackTrace();
           }
            System.out.println(Thread.currentThread().getName()+"runAsync=>Void");
        });

        System.out.println("1111");

        completableFuture.get();*/ // 获取阻塞执行结果
       /* 输出：1111
        ForkJoinPool.commonPool-worker-1runAsync=>Void*/

        // 有返回值的 supplyAsync 异步回调
        // ajax，成功和失败的回调
        // 返回的是错误信息；
        CompletableFuture<Integer> completableFuture = CompletableFuture.supplyAsync(()->{
            System.out.println(Thread.currentThread().getName()+"supplyAsync=>Integer");
           // int i = 10/0;
            return 1024;
        });

        System.out.println(completableFuture.whenComplete((t, u) -> {
            System.out.println("t=>" + t); // 正常的返回结果 出异常的话就是null
            System.out.println("u=>" + u); //没有异常的话就返回null 否则返回错误信息：java.util.concurrent.CompletionException: java.lang.ArithmeticException: / by zero
        }).exceptionally((e) -> {
            System.out.println(e.getMessage());
            return 233; // 可以获取到错误的返回结果
        }).get()); 
        /**
         * succee Code 200
         * error Code 404 500
         */
    }
}

```

> 输出：

![image-20220213201157762](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220213201157762.png)

![image-20220209210728655](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209210728655.png)



## JMM

> 请你谈谈你对 Volatile 的理解

`java的一个关键字`

`Volatile `是 Java 虚拟机提供`轻量级的同步机制`

1、保证可见性

==2、不保证原子性==

3、禁止指令重排



> 什么是JMM

JMM ： Java内存模型，不存在的东西，概念！约定！

**关于JMM的一些同步的约定：**

1、线程解锁前，必须把共享变量立刻刷回主存。

2、线程加锁前，必须读取主存中的最新值到工作内存中！

3、加锁和解锁是同一把锁 线程 工作内存 、主内存

==线程操作的时候：先从主内存中拷贝一份到自己的工作内存中，更新完自己工作内存中的数据后的，再写入并且存储进主内存==

**8种操作：**

![image-20220209234259937](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209234259937.png)

![image-20220209234333575](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209234333575.png)

> 问题： 程序不知道主内存的值已经被修改过了



> 示例代码：

```java
package com.juc.tvolatile;

import java.util.concurrent.TimeUnit;

public class JMMDemo {
    private  static int num = 0;

    public static void main(String[] args) { // main

        new Thread(()->{ // 线程 1 对主内存的变化不知道的
            while (num==0){

            }
        }).start();
       // 这段阻塞代码是为了让上面的线程先启动 因为就绪状态不一定马上进入运行状态
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        num = 1;
        System.out.println(num);

    }
}
```

> 输出：

![image-20220209235133840](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220209235133840.png)

![image-20220210003916656](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210003916656.png)

**内存交互操作有8种，虚拟机实现必须保证每一个操作都是原子的，不可在分的（对于double和long类型的变量来说，load、store、read和write操作在某些平台上允许例外)**

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



## Volatile

> 1、保证可见性

```java
package com.juc.tvolatile;

import java.util.concurrent.TimeUnit;

public class JMMDemo {
    // 不加volitile 程序就会死循环
    // 加volatile 就可以保证可见性
    private  static int num = 0;

    public static void main(String[] args) { // main

        new Thread(()->{ // 线程 1 对主内存的变化不知道的
            while (num==0){

            }
        }).start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        num = 1;
        System.out.println(num);

    }
}
```

==这样 就不会死循环了==



> 2、不保证原子性

原子性 : 不可分割线程

A在执行任务的时候，不能被打扰的(`不允许其他线程也进来操作同一个资源 下面的例子就是不能同时操作num++`)，也不能被分割。要么同时成功，要么同时失败。

```java
package com.juc.tvolatile;

import java.util.concurrent.atomic.AtomicInteger;

// volatile 不保证原子性
public class VDemo02 {

    // volatile 不保证原子性
    public static int num=0;

    public  static void add(){
       num++; // 不是一个原子性操作

    }

    public static void main(String[] args) {

        //理论上num结果应该为 2 万    但是结果总是小于2万
        for (int i = 1; i <= 20; i++) {
            new Thread(()->{
                for (int j = 0; j < 1000 ; j++) {
                    add();
                }
            }).start();
        }

        while (Thread.activeCount()>2){ // main  gc
            Thread.yield();
        }

        System.out.println(Thread.currentThread().getName() + " " + num);


    }
}
```

==将public  static void add(){}加个synchronized或者用lock锁即可解决==



`num++ 不是一个原子性操作，查看字节码文件`

![image-20220210012037559](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210012037559.png)



> 如果不加 lock 和 synchronized ，怎么样保证原子性

==使用原子类，解决 原子性问题==

![image-20220210012630062](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210012630062.png)

> 具体代码：

```java

import java.util.concurrent.atomic.AtomicInteger;

// volatile 不保证原子性
public class VDemo02 {

    // volatile 不保证原子性
    // 原子类的 Integer
    private volatile static AtomicInteger num = new AtomicInteger();

    public static void add(){
        // num++; // 不是一个原子性操作
        num.getAndIncrement(); // AtomicInteger + 1 方法， 底层用的是CAS 效率远比lock锁和synchronize高
    }

    public static void main(String[] args) {

        //理论上num结果应该为 2 万
        for (int i = 1; i <= 20; i++) {
            new Thread(()->{
                for (int j = 0; j < 1000 ; j++) {
                    add();
                }
            }).start();
        }

        while (Thread.activeCount()>2){ // main  gc
            Thread.yield();
        }

        System.out.println(Thread.currentThread().getName() + " " + num);


    }
}

```

这些类（`unsafe`）的底层都直接和==操作系统挂钩==！在内存中修改值！Unsafe类是一个很特殊的存在！

![image-20220210013334034](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210013334034.png)

![image-20220210013444686](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210013444686.png)



> 指令重排

什么是 指令重排：**你写的程序，计算机并不是按照你写的那样去执行的**。

源代码-->编译器优化的重排--> 指令并行也可能会重排--> 内存系统也会重排---> 执行

==**处理器在进行指令重排的时候，考虑：数据之间的依赖性！**==

```
int x = 1; // 1
int y = 2; // 2
x = x + 5; // 3
y = x * x; // 4
我们所期望的：1234 但是可能执行的时候回变成 2134 1324 可不可能是 4123！
```



可能造成影响的结果： a b x y 这四个值默认都是 0；

| 线程A | 线程B |
| :---- | :---- |
| x=a   | y=b   |
| b=1   | a=2   |

正常的结果： x = 0；y = 0；但是可能由于指令重排

| 线程A | 线程B |
| ----- | ----- |
| b=1   | a=2   |
| x=a   | y=b   |

指令重排导致的诡异结果： x = 2；y = 1；



> volatile可以避免指令重排：

内存屏障。作用：

1、保证特定的操作的执行顺序！

==2、可以保证某些变量的内存可见性 （利用这些特性volatile实现了可见性）==

![image-20220210125006574](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210125006574.png)

==**Volatile 是可以保持 可见性。不能保证原子性，由于内存屏障，可以保证避免指令重排的现象产生！**==



## 彻底玩转单例模式

饿汉式 DCL懒汉式，深究！

> 饿汉式：

```java
package com.juc.single;

// 饿汉式单例
public class Hungry {

    // 可能会浪费空间
    private byte[] data1 = new byte[1024*1024];
    private byte[] data2 = new byte[1024*1024];
    private byte[] data3 = new byte[1024*1024];
    private byte[] data4 = new byte[1024*1024];

    private Hungry(){

    }

    private final static Hungry HUNGRY = new Hungry();

    public static Hungry getInstance(){
        return HUNGRY;
    }

}
```

> DCL 懒汉式:

懒汉式：在多线程下是不安全的

```
package com.juc.single;

import com.sun.corba.se.impl.orbutil.CorbaResourceUtil;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;

// 懒汉式单例
// 道高一尺，魔高一丈！
public class LazyMan {
    private static LazyMan lazyMan;

    private LazyMan(){
        System.out.println(Thread.currentThread().getName()+"ok");
    }


    public static LazyMan getInstance() {
        if (lazyMan == null) {
            lazyMan = new LazyMan();
        }
        return lazyMan;
    }

    public static void main(String[] args) {
        for (int i = 0; i < 10; i++) {
            new Thread(()->{
                lazyMan.getInstance();
            }).start();
        }
    }
}
```

![image-20220210134846180](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210134846180.png)

修改成这样就好了

```java
//双重检测锁模式 懒汉式单例 DCL懒汉式
 public static LazyMan getInstance() {
     if (lazyMan==null){
         synchronized (LazyMan.class)
         { 
             if (lazyMan == null) {
               lazyMan = new LazyMan();
             }
         }
     }
     return lazyMan;
 }
```

> 还是存在问题....

```java
  lazyMan = new LazyMan(); 不是一个原子性操作
/**
 * 1. 分配内存空间
 * 2、执行构造方法，初始化对象
 * 3、把这个对象指向这个空间
 *
 * 123   正常
 * 132 A  指令重排序
 *     B // 此时lazyMan还没有完成构造   返回的结果就有可能是一个还没有完成属性赋值的对象
 */
 
 解决方法：加个volatile即可
   private volatile static LazyMan lazyMan;

```

> 反射破坏私有构造：

```java
package com.juc.single;

import com.sun.corba.se.impl.orbutil.CorbaResourceUtil;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;

// 懒汉式单例
// 道高一尺，魔高一丈！
public class LazyMan {
    private volatile static LazyMan lazyMan;

    private LazyMan(){
        System.out.println(Thread.currentThread().getName()+"ok");
    }

   //双重检测锁模式 懒汉式单例 DCL懒汉式
    public static LazyMan getInstance() {
        if (lazyMan==null){
            synchronized (LazyMan.class)
            {
                if (lazyMan == null) {
                  lazyMan = new LazyMan();//不是一个原子性操作

                }
            }
        }
        return lazyMan;
    }
  // 只要有反射任何的私有构造都没有用 反射都能破解
    public static void main(String[] args) throws Exception {
        LazyMan instance1 = LazyMan.getInstance();
        //拿到构造器    因为是无参构造 因此参数为null
        Constructor<LazyMan> declaredConstructor = LazyMan.class.getDeclaredConstructor(null);
        //打开私有
         declaredConstructor.setAccessible(true);
        LazyMan instance2 = declaredConstructor.newInstance();
     
        System.out.println(instance1);
        System.out.println(instance2);
    }
}
解决方案：
构造器来个判断就好了
  private LazyMan(){
      synchronized (LazyMan.class){
            if (lazyMan!=null){
                throw new RuntimeException("不要试图使用反射破坏异常");
            }
        }

    }
    
 又来一个问题：
   // 只要有反射任何的私有构造都没有用 反射都能破解
    public static void main(String[] args) throws Exception {
      //  LazyMan instance1 = LazyMan.getInstance();
        //拿到构造器
        Constructor<LazyMan> declaredConstructor = LazyMan.class.getDeclaredConstructor(null);
         declaredConstructor.setAccessible(true);
        LazyMan instance1 = declaredConstructor.newInstance();
        LazyMan instance2 = declaredConstructor.newInstance();

        System.out.println(instance1);
        System.out.println(instance2);
    }
}

来个标志位解决问题：
     private static boolean keweiqin=false;

    private LazyMan(){
        synchronized (LazyMan.class){
        if (keweiqin==false){
            keweiqin=!keweiqin;
        }else {
            throw new RuntimeException("不要试图使用反射破坏异常");
        }
    }
}
道高一尺魔高一丈：标志位能通过反射被改
     public static void main(String[] args) throws Exception {
      //  LazyMan instance1 = LazyMan.getInstance();
        //拿到构造器
        Field keweiqin = LazyMan.class.getDeclaredField("keweiqin");
        keweiqin.setAccessible(true);
        Constructor<LazyMan> declaredConstructor = LazyMan.class.getDeclaredConstructor(null);
         declaredConstructor.setAccessible(true);
        LazyMan instance1 = declaredConstructor.newInstance();
        keweiqin.set(instance1,false);
        LazyMan instance2 = declaredConstructor.newInstance();

        System.out.println(instance1);
        System.out.println(instance2);
    }
}
```

> 静态内部类实现单例：

```
package com.juc.single;

// 静态内部类
public class Holder {
    private Holder(){

    }

    public static Holder getInstace(){
        return InnerClass.HOLDER;
    }

    public static class InnerClass{
        private static final Holder HOLDER = new Holder();
    }

}
```

> 反射无法破坏枚举：

源码：

![image-20220210144501379](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210144501379.png)

==**案例：**==

```java
package com.juc.single;

import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

// enum 是一个什么？ 本身也是一个Class类
public enum EnumSingle {

    INSTANCE;

    public EnumSingle getInstance(){
        return INSTANCE;
    }

}

```

==**查看IDE枚举的字节码文件：**==发现有个无参构造

![image-20220210145045694](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210145045694.png)

==测试发现报错：==NoSuchMethodException: com.kuang.single.EnumSingle.<init>() 并不是源码的那个异常 说明ide的字节码文件骗了我们

```
class Test{

    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        EnumSingle instance1 = EnumSingle.INSTANCE;
        Constructor<EnumSingle> declaredConstructor = EnumSingle.class.getDeclaredConstructor(null);
        declaredConstructor.setAccessible(true);
        EnumSingle instance2 = declaredConstructor.newInstance();
        // NoSuchMethodException: com.kuang.single.EnumSingle.<init>() 意思是没有空参的构造方法
        System.out.println(instance1);
        System.out.println(instance2);

    }

}
```

然后cmd反编译发现还是有一个空参构造 也欺骗了我们

![image-20220210145502370](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210145502370.png)



用更靠谱的jad工具反编译源码得到字节码文件：只有一个有参构造

![image-20220210145642602](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210145642602.png)

> 修改代码：

```java
class Test{

    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        EnumSingle instance1 = EnumSingle.INSTANCE;
        Constructor<EnumSingle> declaredConstructor = EnumSingle.class.getDeclaredConstructor(String.class,int.class);
        declaredConstructor.setAccessible(true);
        EnumSingle instance2 = declaredConstructor.newInstance();
        // 报错Cannot reflectively create enum objects 实现了一致
        System.out.println(instance1);
        System.out.println(instance2);

    }

}
```



## 深入理解CAS

> 什么是CAS

大厂你必须要深入研究底层！有所突破！ 修内功，操作系统，计算机网络原理

```java
package com.juc.cas;

import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.concurrent.atomic.AtomicStampedReference;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class CASDemo {
    // CAS  compareAndSet : 比较并交换！
    public static void main(String[] args) {
        AtomicInteger atomicInteger = new AtomicInteger(2020);//底层用的CAS
        //期望 更新
       // public final boolean compareAndSet(int expect, int update) {
        // 如果我期望的值达到了 那么就去更新 否则 不更新 。 CAS 是CPU的并发原语
        System.out.println(atomicInteger.compareAndSet(2020, 2021));
        System.out.println(atomicInteger.get());  //2021
        atomicInteger.getAndIncrement();
        System.out.println(atomicInteger.compareAndSet(2020, 2021));
        System.out.println(atomicInteger.get()); //2022
    }
}

```

> Unsafe 类

![image-20220210153352674](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210153352674.png)![image-20220210154534681](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210154534681.png)

![image-20220210155209013](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210155209013.png)

`atomicInteger.compareAndSet（)底层操作和这个+1操作大同小异`

==**CAS ： 比较当前工作内存中的值和主内存中的值，如果这个值是期望的，那么则执行操作！如果不是就一直循环！**==

缺点：

1、 循环会耗时

2、==一次性只能保证一个共享变量的原子性==

3、ABA问题



> CAS ： ABA 问题（狸猫换太子）

![image-20220210170015553](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210170015553.png)

```java
package com.juc.cas;

import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.concurrent.atomic.AtomicStampedReference;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class CASDemo {
    // CAS  compareAndSet : 比较并交换！
    public static void main(String[] args) {
        AtomicInteger atomicInteger = new AtomicInteger(2020);//底层用的CAS
        //期望 更新
       // public final boolean compareAndSet(int expect, int update) {
        // 如果我期望的值达到了 那么就去更新 否则 不更新 。 CAS 是CPU的并发原语
        //============捣乱的线程================
        System.out.println(atomicInteger.compareAndSet(2020, 2021));
        System.out.println(atomicInteger.get());  //2021 
        
       System.out.println(atomicInteger.compareAndSet(2021, 2020));
        System.out.println(atomicInteger.get());  //2020
        //============期望的线程================
        System.out.println(atomicInteger.compareAndSet(2020, 6666));
        System.out.println(atomicInteger.get()); //6666
    }
}
```



## 原子引用

> 解决ABA 问题，引入原子引用！ 对应的思想：乐观锁！

带版本号 的原子操作！

```java
package com.juc.cas;

import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicStampedReference;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
/* public boolean compareAndSet(V   expectedReference,
                                 V   newReference,
                                 int expectedStamp,
                                 int newStamp)*/ //第一个参数是期望值 第二个是符合预期得到的值 第三个是没有发生变化前的版本号 最后一个是变化后的版本号
public class CASDemo {

    //AtomicStampedReference 注意，如果泛型是一个包装类，注意对象的引用问题

    // 正常在业务操作，这里面比较的都是一个个对象    AtomicStampedReference<>(1,1); 第二个参数是版本号（一个标识而已）
    static AtomicStampedReference<Integer> atomicStampedReference = new AtomicStampedReference<>(1,1);

    // CAS  compareAndSet : 比较并交换！
    public static void main(String[] args) {

        new Thread(()->{
            int stamp = atomicStampedReference.getStamp(); // 获得版本号
            System.out.println("a1=>"+stamp);

            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }


            atomicStampedReference.compareAndSet(1, 2,
                    atomicStampedReference.getStamp(), atomicStampedReference.getStamp() + 1);

            System.out.println("a2=>"+atomicStampedReference.getStamp());


            System.out.println(atomicStampedReference.compareAndSet(2, 1,
                    atomicStampedReference.getStamp(), atomicStampedReference.getStamp() + 1));

            System.out.println("a3=>"+atomicStampedReference.getStamp());

        },"a").start();


        // 乐观锁的原理相同！
        new Thread(()->{
            int stamp = atomicStampedReference.getStamp(); // 获得版本号
            System.out.println("b1=>"+stamp);

            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println(atomicStampedReference.compareAndSet(1, 6,
                    stamp, stamp + 1));

            System.out.println("b2=>"+atomicStampedReference.getStamp());

        },"b").start();

    }
}
```

> 输出：

![image-20220210171802160](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210171802160.png)

**注意：Integer 使用了对象缓存机制，默认范围是 -128 ~ 127 ，推荐使用静态工厂方法 valueOf 获取对象实例，而不是 new，因为 valueOf 使用缓存，而 new 一定会创建新的对象分配新的内存空间；**

==**阿里巴巴规范手册明确：**==

![image-20220210171907822](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220210171907822.png)

`源码分析一波:`

```java
  public class AtomicStampedReference<V> {

    private static class Pair<T> {
        final T reference;
        final int stamp;
        private Pair(T reference, int stamp) {
            this.reference = reference;
            this.stamp = stamp;
        }
        static <T> Pair<T> of(T reference, int stamp) {
            return new Pair<T>(reference, stamp);
        }
    

    private volatile Pair<V> pair;
    
      public AtomicStampedReference(V initialRef, int initialStamp) {
        pair = Pair.of(initialRef, initialStamp);
    }
    
public boolean compareAndSet(V   expectedReference,
                             V   newReference,
                             int expectedStamp,
                             int newStamp) {
    Pair<V> current = pair;
    return
        expectedReference == current.reference &&   如果用2020 这里就直接返回false了 所以狂神的出了问题
        expectedStamp == current.stamp &&
        ((newReference == current.reference &&
          newStamp == current.stamp) ||
         casPair(current, Pair.of(newReference, newStamp)));
}
}

    private boolean casPair(Pair<V> cmp, Pair<V> val) {
    //第二个参数是对象的内存偏移量
        return UNSAFE.compareAndSwapObject(this, pairOffset, cmp, val);
    }
    
 public final native boolean compareAndSwapObject(Object var1, long var2, Object var4, Object var5);
```



## 各种锁的理解

### 1.公平锁、非公平锁

公平锁： 非常公平， 不能够插队，必须先来后到！

非公平锁：非常不公平，可以插队 （默认都是非公平 ）==有利于提高性能 参考博客：①公平锁的性能比非公平锁的性能低  ②非公平锁比公平锁高效的原因_changyuan-CSDN博客_非公平锁为什么比公平锁快== `Synchronized和Lock锁都是非公平锁`

```java
Lock lock = new ReentrantLock();
  public ReentrantLock() {
        sync = new NonfairSync();
    }
    
Lock lock = new ReentrantLock(true);
   public ReentrantLock(boolean fair) {
        sync = fair ? new FairSync() : new NonfairSync();
    }
```

### 2.可重入锁

可重入锁（递归锁）

![image-20220211000840130](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220211000840130.png)

> Synchronized

```java
package com.juc.lock;

import javax.sound.midi.Soundbank;

// Synchronized  非公平锁 可重入锁
public class Demo01 {
    public static void main(String[] args) {
        Phone phone = new Phone();

        new Thread(()->{
            phone.sms();
        },"A").start();


        new Thread(()->{
            phone.sms();
        },"B").start();
    }
}

class Phone{

    public synchronized void sms(){
        System.out.println(Thread.currentThread().getName() + "sms");
        call(); // 这里也有锁
    }

    public synchronized void call(){
        System.out.println(Thread.currentThread().getName() + "call");
    }
}
```

![image-20220211001122377](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220211001122377.png)



> Lock版

```java
package com.juc.lock;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Demo02 {
    public static void main(String[] args) {
        Phone2 phone = new Phone2();
        new Thread(()->{
            phone.sms();
        },"A").start();


        new Thread(()->{
            phone.sms();
        },"B").start();
    }
}

class Phone2{
    Lock lock = new ReentrantLock();

    public void sms(){
        lock.lock(); // 细节问题：lock.lock(); lock.unlock(); // lock 锁必须配对，否则就会死在里面
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + "sms");
            call(); // 这里也有锁
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
            lock.unlock();
        }

    }

    public void call(){

        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + "call");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
```

> 输出：

![image-20220211003408467](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220211003408467.png)

若注释sms()的一个解锁操作（lock不再满足配对），则输出： ==死锁了==

![image-20220211003530472](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220211003530472.png)

### 3.自旋锁

`英文翻译:spinlock`

![image-20220211004406271](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220211004406271.png)

> 我们来自定义一个锁测试

```java
package com.juc.lock;

import java.util.concurrent.atomic.AtomicReference;

/**
 * 自旋锁
 */
public class SpinlockDemo {

    // int   0
    // 泛型是引用类型 不加参数 默认是null  Integer的话
    AtomicReference<Thread> atomicReference = new AtomicReference<>();

    // 加锁
    public void myLock(){
        Thread thread = Thread.currentThread();
        System.out.println(Thread.currentThread().getName() + "==> mylock");

        // 自旋锁
        while (!atomicReference.compareAndSet(null,thread)){

        }
    }


    // 解锁
    // 加锁
    public void myUnLock(){
        Thread thread = Thread.currentThread();
        System.out.println(Thread.currentThread().getName() + "==> myUnlock");
        atomicReference.compareAndSet(thread,null);
    }
}
```

> 测试：

```
package com.juc.lock;

import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.ReentrantLock;

public class TestSpinLock {
    public static void main(String[] args) throws InterruptedException {
//        ReentrantLock reentrantLock = new ReentrantLock();
//        reentrantLock.lock();
//        reentrantLock.unlock();

        // 底层使用的自旋锁CAS
        SpinlockDemo lock = new SpinlockDemo();


        new Thread(()-> {
            lock.myLock();

            try {
                TimeUnit.SECONDS.sleep(5);
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                lock.myUnLock();
            }

        },"T1").start();

        TimeUnit.SECONDS.sleep(1);

        new Thread(()-> {
            lock.myLock();

            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                lock.myUnLock();
            }

        },"T2").start();

    }
}
```

![image-20220211010236542](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220211010236542.png)



### 4.死锁

> 死锁是什么？

![image-20220211011205642](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220211011205642.png)

> 死锁测试，怎么排除死锁：

```java
package com.juc.lock;

import com.sun.org.apache.xpath.internal.SourceTree;

import java.util.concurrent.TimeUnit;

public class DeadLockDemo {
    public static void main(String[] args) {

        String lockA = "lockA";
        String lockB = "lockB";

        new Thread(new MyThread(lockA, lockB), "T1").start();
        new Thread(new MyThread(lockB, lockA), "T2").start();

    }
}


class MyThread implements Runnable{

    private String lockA;
    private String lockB;

    public MyThread(String lockA, String lockB) {
        this.lockA = lockA;
        this.lockB = lockB;
    }

    @Override
    public void run() {
        synchronized (lockA){
            System.out.println(Thread.currentThread().getName() + "lock:"+lockA+"=>get"+lockB);

            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            synchronized (lockB){
                System.out.println(Thread.currentThread().getName() + "lock:"+lockB+"=>get"+lockA);
            }

        }
    }
}
```

> 输出：

![image-20220211011244681](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220211011244681.png)

> 解决问题：

1、使用 `jps -l` 定位进程号

![image-20220211012036403](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220211012036403.png)

2、使用 `jstack 进程号` 找到死锁问题 (一般直接翻到最后 问题都显示在最后)

![image-20220211012133923](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220211012133923.png)面试，工作中！ 排查问题：

1、日志 

2、堆栈 

