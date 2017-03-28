# 堆内内存vs堆外内存

简单的说，我们需要牢记三点：

1. 平时的read/write，都会在I/O设备与应用程序空间之间经历一个“内核缓冲区”。

2. Direct Buffer(直接内存，也是堆外内存)就好比是“内核缓冲区”上的缓存，不直接受GC管理；而Heap Buffer(堆内内存)就仅仅是byte[]字节数组的包装形式。因此把一个Direct Buffer写入一个Channel的速度要比把一个Heap Buffer写入一个Channel的速度要快

3. Direct Buffer创建和销毁的代价很高，所以要用在尽可能重用的地方

##堆内内存(InDirectBuffer || HeapBuffer)

 Java程序磁盘进行读操作：

1. JVM只能读取OS中的Buffer

2. OS可以读取磁盘中的数据，并存入本层的Buffer中

3. 操作系统将自己的Buffer拷贝给JVM堆里的Buffer

##堆外内存(DirectBuffer)

Java程序磁盘进行读操作：

1. OS从磁盘读取数据后，直接存到了堆外内存中

2. 接缓冲区在使用Socket传递数据时性能很好，因为若使用间接缓冲区，JVM会先将数据复制到直接缓冲区再进行传递；但是直接缓冲区的缺点是在分配内存空间和释放内存时比堆缓冲区更复杂

3. jdk7后的nio使用了堆外内存

###优点

1. 节约了时间

2. 节约了堆内内存，频繁的IO将会占用大量堆内内存，将buffer放到堆外内存进行调优，即ByteBuffer.allocateDirect(int)——例如Hadoop中

3. 不受young GC的限制


