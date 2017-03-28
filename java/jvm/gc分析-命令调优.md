## GC分析 命令调优

主要关注点：

- GC日志分析

- 调优命令

- 调优工具

**GC日志分析**

摘录GC日志一部分（前部分为年轻代gc回收；后部分为full gc回收）：

``` xml

2016-07-05T10:43:18.093+0800: 25.395: [GC [PSYoungGen: 274931K->10738K(274944K)] 371093K->147186K(450048K), 0.0668480 secs] [Times: user=0.17 sys=0.08, real=0.07 secs]

2016-07-05T10:43:18.160+0800: 25.462: [Full GC [PSYoungGen: 10738K->0K(274944K)] [ParOldGen: 136447K->140379K(302592K)] 147186K->140379K(577536K) [PSPermGen: 85411K->85376K(171008K)], 0.6763541 secs] [Times: user=1.75 sys=0.02, real=0.68 secs]

```

通过上面日志分析得出，PSYoungGen、ParOldGen、PSPermGen属于Parallel收集器。其中PSYoungGen表示gc回收前后年轻代的内存变化；ParOldGen表示gc回收前后老年代的内存变化；PSPermGen表示gc回收前后永久区的内存变化。young gc 主要是针对年轻代进行内存回收比较频繁，耗时短；full gc 会对整个堆内存进行回城，耗时长，因此一般尽量减少full gc的次数

young gc 日志:

{:.center}

![](http://www.ityouknow.com/assets/images/2017/jvm/yong.jpg)

Full GC日志:

![](http://www.ityouknow.com/assets/images/2017/jvm/full.jpg)

**调优命令**

Sun JDK监控和故障处理命令有jps jstat jmap jhat jstack jinfo

- jps，JVM Process Status Tool,显示指定系统内所有的HotSpot虚拟机进程。

- jstat，JVM statistics Monitoring是用于监视虚拟机运行时状态信息的命令，它可以显示出虚拟机进程中的类装载、内存、垃圾收集、JIT编译等运行数据。

- jmap，JVM Memory Map命令用于生成heap dump文件

- jhat，JVM Heap Analysis Tool命令是与jmap搭配使用，用来分析jmap生成的dump，jhat内置了一个微型的HTTP/HTML服务器，生成dump的分析结果后，可以在浏览器中查看

- jstack，用于生成java虚拟机当前时刻的线程快照。

- jinfo，JVM Configuration info 这个命令作用是实时查看和调整虚拟机运行参数。

> *详细的命令使用参考这里[jvm系列(四):jvm调优-命令篇](http://www.ityouknow.com/java/2016/01/01/jvm%E8%B0%83%E4%BC%98-%E5%91%BD%E4%BB%A4%E7%AF%87.html)*

**调优工具**

常用调优工具分为两类,jdk自带监控工具：jconsole和jvisualvm，第三方有：MAT(Memory Analyzer Tool)、GChisto。

- jconsole，Java Monitoring and Management Console是从java5开始，在JDK中自带的java监控和管理控制台，用于对JVM中内存，线程和类等的监控

- jvisualvm，jdk自带全能工具，可以分析内存快照、线程快照；监控内存变化、GC变化等。

- MAT，Memory Analyzer Tool，一个基于Eclipse的内存分析工具，是一个快速、功能丰富的Java heap分析工具，它可以帮助我们查找内存泄漏和减少内存消耗

- GChisto，一款专业分析gc日志的工具

> *工具使用参考 [jvm系列(七):jvm调优-工具篇](http://www.ityouknow.com/java/2017/02/22/jvm-tool.html)*


