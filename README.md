# JvmLearning
笔记就托管在这了，大部分内容摘自周志明老师的《深入理解java虚拟机》，其中的代码是以jdk1.7版本为基础的。（ps:第一次接触这个编译器，排版方面可能会比较丑
我会努力修正的）

  第一章 java内存模型
----------------------------------- 

java虚拟机所管理的内存将会包括以下几个运行是数据区域：

![](https://github.com/KuiLoveLOL/JvmLearning/blob/master/image/abc.jpg)

###   线程隔离数据区

#### 程序计数器(Program Counter Register)：
一小块内存空间，单前线程所执行的字节码行号指示器。字节码解释器工作时，通过改变这个计数器的值来选取下一条需要执行的字节码指令，分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖这个计数器来完成。

#### JVM虚拟机栈(Java Virtual Machine Stacks)：
Java方法执行内存模型，用于存储局部变量，操作数栈，动态链接，方法出口等信息。是线程私有的。

#### 本地方法栈(Native Method Stacks)：
为JVM用到的Native方法服务，Sun HotSpot 虚拟机把本地方法栈和JVM虚拟机栈合二为一。是线程私有的。

###  线程共享的数据区

#### 方法区（Method Area）：
用于存储JVM加载的类信息、常量、静态变量、即使编译器编译后的代码等数据。
#### 运行时常量池（Runtime Constant Pool）：
是方法区的一部分，用于存放编译器生成的各种字面量和符号引用，这部分内容将在类加载后存放到方法取得运行时常量池中。具备动态性，用的比较多的就是String类的intern()方法。
#### JVM堆（ Java Virtual Machine Heap）：
存放所有对象实例的地方。<br> 
新生代，由Eden Space 和大小相同的两块Survivor组成<br> 
旧生代，存放经过多次垃圾回收仍然存活的对象<br> 
如图：<br> 

![](https://github.com/KuiLoveLOL/JvmLearning/blob/master/image/bcd.jpg)

#### 直接内存（Direct Memory）：
它并不是虚拟机运行时数据区的一部分，也不是JAVA虚拟机规范中定义的内存区域。在JDK1.4中加入了NIO类，引入了一种基于通道(Channel)于缓冲区(Buffer)的I/O方式，他可以使用Native函数库直接分配堆外内存，然后通过一个存储在JAVA堆里面的DirectByteBuffer对象作为这块内存的引用进行操作。这样能在一些场景中显著提高性能，因为避免了在JAVA堆中和Native堆中来回复制数据。

