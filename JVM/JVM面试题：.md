# JVM面试题：

==**面试常见**==：

​	==1.请你谈谈你对JVM的理解?==

Java虚拟机（JVM）是运行 Java 字节码的虚拟机。JVM有针对不同系统的特定实现（Windows，Linux，macOS），目的是使用相同的字节码，它们都会给出相同的结果。`字节码和不同系统的 JVM 实现`是 Java 语言“一次编译，随处可以运行”的关键所在。

​	==2.java8虚拟机和之前的变化更新?==

`java7`：堆和方法区连在了一起，但这并不能说堆和方法区是一起的，它们在逻辑上依旧是分开的。但在物理上来说，它们又是连续的一块内存。方法区是JVM的一种规范，而hotspot的具体实现是永久代。也就是说永久代就是方法区。永久代的垃圾收集是和老年代捆绑在一起的，因此无论谁满了，都会触发永久代和老年代的垃圾收集。

![image-20220220213936981](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220220213936981.png)

然后，在Java8中，时代变了，Hotspot取消了永久代。永久代真的成了永久的记忆。永久代的参数-XX:PermSize和-XX：MaxPermSize也随之失效。

在Java8中，元空间(Metaspace)登上舞台，方法区存在于元空间(Metaspace)。同时，==元空间不再与堆连续，而且是存在于本地内存（Native memory）。==

![image-20220220213635977](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220220213635977.png)

本地内存（Native memory），也称为C-Heap，是供JVM自身进程使用的。当Java Heap空间不足时会触发GC，但Native memory空间不够却不会触发GC。

==**针对Java8的调整，我们再次对内存结构图进行调整。**==

![image-20220220213734262](C:\Users\19395\AppData\Roaming\Typora\typora-user-images\image-20220220213734262.png)

元空间存在于本地内存，意味着只要本地内存足够，它不会出现像永久代中“java.lang.OutOfMemoryError: PermGen space”这种错误。看上图中的方法区，是不是“膨胀”了。

默认情况下元空间是可以无限使用本地内存的，但为了不让它如此膨胀，JVM同样提供了参数来限制它使用的使用。

- -XX:MetaspaceSize，class metadata的初始空间配额，以bytes为单位，达到该值就会触发垃圾收集进行类型卸载，同时GC会对该值进行调整：如果释放了大量的空间，就适当的降低该值；如果释放了很少的空间，那么在不超过MaxMetaspaceSize（如果设置了的话），适当的提高该值。
- -XX：MaxMetaspaceSize，可以为class metadata分配的最大空间。默认是没有限制的。
- -XX：MinMetaspaceFreeRatio,在GC之后，最小的Metaspace剩余空间容量的百分比，减少为class metadata分配空间导致的垃圾收集。
- -XX:MaxMetaspaceFreeRatio,在GC之后，最大的Metaspace剩余空间容量的百分比，减少为class metadata释放空间导致的垃圾收集。

==**思考一下，为什么使用元空间替换永久代？**==

表面上看是为了避免OOM异常。因为通常使用PermSize和MaxPermSize设置永久代的大小就决定了永久代的上限，但是不是总能知道应该设置为多大合适, 如果使用默认值很容易遇到OOM错误。

当使用元空间时，可以加载多少类的元数据就不再由MaxPermSize控制, 而由系统的实际可用空间来控制。



​	==**3.什么是栈溢出StackOverFlowError? 怎么分析?**==

==**StackOverflowError原因：**==

 Java 里的 `StackOverflowError`。`抛出这个错误表明应用程序因为深递归导致栈被耗尽了。`每当java程序启动一个新的线程时，java虚拟机会为他分配一个栈，java栈以==帧==为单位保持线程运行状态；当线程调用一个方法是，jvm压入一个==新的栈帧==到这个线程的栈中，只要这个方法还没返回，这个栈帧就存在。 如果方法的嵌套调用层次太多(如递归调用),随着java栈中的帧的增多，最终导致这个线程的栈中的所有栈帧的大小的总和大于-Xss设置的值，而产生生`StackOverflowError`溢出异常。



