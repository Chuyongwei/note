# [JVM_01 简介](https://www.cnblogs.com/yanl55555/p/12610952.html)

**本篇仅仅是JVM的简介，关于更多的JVM细节，请参见[本专题JVM](https://www.cnblogs.com/yanl55555/category/1686360.html)：**

# 计算机系统当中的JVM

-  JVM是运行在操作系统之上的，并没有和硬件有直接的交互
- Java代码一次编译,到处运行

 ![img](D:\baidu\MarkDown\Java\结构\JVM\images\1846149-20200401104340482-1757970137.png)

![img](D:\baidu\MarkDown\Java\结构\JVM\images\1846149-20200403091940968-1617936617.png)

# HotSpot虚拟机结构概览

![img](D:\baidu\MarkDown\Java\结构\JVM\images\1846149-20200401103726898-763616456.png)

- 方法区和堆区是所有线程共享的内存区域；
- 而java栈、本地方法栈和程序计数器是运行是线程私有的内存区域。
- Java栈又叫做jvm虚拟机栈。

- 方法区（永久代）在jdk8中又叫做

  元空间

  Metaspace

  - 方法区用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器（JIT编译器，英文写作Just-In-Time Compiler）编译后的代码等数据。
  - 虽然Java虚拟机规范把方法区描述为堆的一个逻辑部分，但是它却有一个别名叫做 Non-Heap（非堆），目的应该是与 Java 堆区分开来。

- **java代码大致执行流程：**java源程序--编译javac-->字节码文件.class-->类装载子系统生成反射类（存入方法区）--->运行时数据区（五大块儿）--->执行引擎-->解释执行+编译执行（JIT）-->操作系统（Win，Linux，Mac JVM）

## 栈的指令集架构和寄存器的指令集架构

```
由于跨平台的设计，java的指令都是根据栈来设计的，不同平台CPU架构不同，所以不能设计为基于寄存器的
```

二者区别：

```
栈：跨平台性、指令集小、指令多；执行性比寄存器差
寄存器：指令少
```

一些简单查看命令：

```
//查看指令集命令代码
cd out/production/类根目录

//反编译
javap -v StackStruTest.class

//打印程序执行的进程
jps
```

## Hotspot中方法区的变动：

关于方法区的结构，在过去的版本jdk1.6/1.7/1.8当中均有变动，故在此提前声明：

- jdk1.6及之前：有永久代（permanent generation） ，静态变量、字符串常量池存放在 永久代上。
- jdk1.7：有永久代，但已经逐步“去永久代”，字符串常量池、静态变量移除，保存在堆中。注意：
- jdk1.8及之后： 无永久代，类型信息、字段、方法、常量保存在本地内存的元空间，但字符串常量池、静态变量仍留在堆空间.

![img](D:\baidu\MarkDown\Java\结构\JVM\images\1846149-20200809213838042-161042513.png)

![img](D:\baidu\MarkDown\Java\结构\JVM\images\1846149-20200809213856394-1788531515.png)

注意：

jdk1.8及之后： 无永久代，类型信息、字段、方法、常量保存在本地内存的元空间。

但字符串常量池、静态变量仍留在堆空间。

除此之外，元空间（或称方法区），不再使用虚拟机内存，而是使用本地内存。

![img](D:\baidu\MarkDown\Java\结构\JVM\images\1846149-20200809213935140-298002178.png)

参见： [ 关于类加载子系统详述](https://www.cnblogs.com/yanl55555/p/12611133.html)

参见：  [关于程序计数器详述](https://www.cnblogs.com/yanl55555/p/12614541.html)

参见  [关于Java虚拟机栈的详述](https://www.cnblogs.com/yanl55555/p/12615658.html)

参见： [关于本地方法接口详述](https://www.cnblogs.com/yanl55555/p/12623447.html)

参见： [关于本地方法栈详述](https://www.cnblogs.com/yanl55555/p/12624519.html)

参见： [ 关于方法区的详述](https://www.cnblogs.com/yanl55555/p/13323128.html)

参见：  [关于堆区的详述](https://www.cnblogs.com/yanl55555/p/13304652.html)

参见： [关于执行引擎的详述](https://www.cnblogs.com/yanl55555/p/13334713.html)

关于更多JVM细节，请参见[本专题JVM](https://www.cnblogs.com/yanl55555/category/1686360.html)：

# jvm生命周期

## 1.启动

通过引导类加载器（bootstrap class loader）创建一个初始类（initial class）来完成的，这个类是由虚拟机的具体实现指定的.

## 2.执行

- 一个运行中的java虚拟机有着一个清晰的任务：执行Java程序；
- 程序开始执行的时候他才运行，程序结束时他就停止；
- 执行一个所谓的Java程序的时候，真真正正在执行的是一个叫做Java虚拟机的进程。

## 3.退出

- 程序正常执行结束
- 程序异常或错误而异常终止
- 操作系统错误导致终止
- 某线程调用Runtime类或System类的exit方法，或Runtime类的halt方法，并且java安全管理器也允许这次exit或halt操作
- 除此之外，JNI规范描述了用JNI Invocation API来加载或卸载Java虚拟机时，Java虚拟机的退出情况

# 补充：古今JVM

- SUN Classic
- Exact VM
- HotSpot VM ：HotSpot指热点代码探测技术
- BEA JRockit：(BEA 已被Oracle收购) 专注于服务端应用，世界最快的jvm之一
- IBM J9
- Taobao JVM: 目前已经在淘宝、天猫上线，替换了Oracle官方JVM；
- Graal VM: Oracle 2018年4月公开，口号 Run Programs Faster Anywhere.最可能替代HotSpot的产品

## Android虚拟机 DVM

- 谷歌开发，基于Android，在2.2中提供了JIT
- 只能称作虚拟机 不能称为java虚拟机，他没有遵循Java虚拟机规范
- 基于寄存器架构，效率高，但是跟硬件耦合度比较高
- 不能直接执行class文件，执行的是dex文件
- 5.0使用支持提前编译的ART VM替换Dalvik VM

# 计算机系统当中的JVM