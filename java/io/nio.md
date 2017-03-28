# NIO分析

- Nio最核心的是**Reactor模式**

- Selector使用一个线程来管理多个通道，做轮询选择准备好的通道

 - 这种轮询方式处理多线程请求时不需要上下文切换


##为什么需要NIO

1. BIO读写速度慢

2. BIO的阻塞特性将一部分时间浪费在等待IO操作上，同时大量创建、管理、销毁以及上下文切换使得性能下降和消耗大量系统资源

###解决方式

1. 提高IO读写速度，同时采取**非阻塞**方式读取，减少等待时间

2. 提高线程的使用率，减少系统资源的消耗（线程池）

3. 对于频繁的IO操作系统，IO是瓶颈，NIO可以更好结余IO操作的时间

4. Stream/Reader的读写单位是byte/char, NIO的单位是buffer

##NIO概述

Java NIO由以下3个核心部分组成：

1. Channels 与 Buffers（通道和缓冲区）：标准的IO基于字节流和字符流进行操作的，而NIO是基于Channel和Buffer进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写入到通道中。

 - 这些通道覆盖了UDP和TCP网络IO，以及文件IO

2. Selectors（选择器）：Java NIO引入了选择器的概念，选择器用于监听多个通道的事件（比如：连接打开，数据到达）。因此，单个的线程可以监听多个数据通道

 - 在一个单线程中使用一个Selector处理多个Channel

 - 要使用Selector，得向Selector注册Channel，然后调用它的select()方法。这个方法会一直**阻塞**到某个注册的通道有事件就绪。一旦这个方法返回，线程就可以处理这些事件，事件的例子有如新连接进来，数据接收等。

##NIO实现代码分析

###Buffer

NIO读写的容器，从Buffer中读取数据或者将数据写入到Buffer中

对比上诉InputSteam/OutputStream， Stream用byte数组byte[]来做缓存，状态无法记录，不知道读/写了多少，还有多少是有效字节，下次从哪里开始读/写

####Buffer内部的3个控制位置字段

1. **position**：下一个要读/写的位置

 - 读：position指定了下一个字节将放到数组的哪一个元素中。因此，如果您从通道中读三个字节到缓冲区中，那么缓冲区的 position 将会设置为3，指向数组中第四个元素

 - 写：position 值跟踪从缓冲区中获取了多少数据。更准确地说，它指定下一个字节来自数组的哪一个元素。因此如果从缓冲区写了5个字节到通道中，那么缓冲区的 position 将被设置为5，指向数组的第六个元素。

2. **limit**：有效位置边界

 - 读：能读到的最大位置

 - 写：可以被写到的最大位置

3. **capacity**：容量

一般而言，position <= limit <= capacity

#####关于filp() 和 clear()

- filp() 用在 read 和 write 之间，使 ready to write

- clear() 用在write 和下一次 read 之间，使 ready to read

####Buffer的获取

#####1. allocate通过工厂构造：使用堆内内存

```java

public static ByteBuffer allocate(int capacity) {

 if(capacity < 0){

 throw new IlleagalArgumentException();

 }

 return new HeapByteBuffer(capacity, capacity);

}

```

#####2.wrap封装byte数组：使用堆内内存

```java

public static ByteBuffer wrap(byte[] array, int offset, int length){

 try {

 return new HeapByteBuffer(array, offset, length);

 }catch(IlleagalArgumentException x) {

 throw new IndexOutOfBoundsException();

 }

}

```

#####3.allocateDirect：使用堆外内存

```java

public static ByteBuffer allocateDirect(int capacity){

 return new DirectByteBuffer(capacity);

}

```

###Channel

NIO中Input/Oupt的管道

- 矿山（File)， 矿道（Channel）,矿车（Buffer）

- Channel 不同于 InputStream/OutputStream，Channel是双向的，即可作为读的管道也可作为写的管道

###Selector(选择器)

选择器是Java NIO中能够检测到一到多个NIO通道，并能够知道通道是否为事件（例如读写）做好准备的组件。这样，一个单独的线程可以管理多个channel，从而管理多个连接。

##参考资料

1. [Java NIO系列教程](http://www.iteye.com/magazines/132-Java-NIO)

2. [NIO入门](http://www.ibm.com/developerworks/cn/education/java/j-nio/j-nio.html)
