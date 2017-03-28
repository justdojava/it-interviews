# JVM

Java虚拟机（Java Virtual Machine，缩写为JVM）是一种能够运行Java bytecode的虚拟机，以堆栈结构机器来进行实做

JVM有自己完善的硬体架构，如处理器、堆栈、寄存器等，还具有相应的指令系统

- JVM屏蔽了与具体操作系统平台相关的信息，使得Java程序只需生成在Java虚拟机上运行的目标代码（字节码），就可以在多种平台上不加修改地运行（平台无关性）

- 作为一种编程语言的虚拟机，只要生成的编译文件符合JVM对载入编译文件格式要求，任何语言都可以由JVM编译运行

##JVM实例

当启动一个Java程序时，一个JVM实例就产生了，任何一个拥有`public static void main(String[]args)`函数的class都可以作为JVM实例运行的起点

- 需要显式的告诉JVM类名，也就是我们平时运行Java程序命令的由来，如`Java classA helloworld`,这里Java是告诉os运行SunJava2SDK的Java虚拟机，而classA则指出了运行JVM所需要的类名。

##JVM实例的生存周期

### JVM实例的运行

`main（）`作为该程序初始线程的起点，任何其他线程均由该线程启动

JVM内部有两种线程

- 守护线程

- 非守护线程

`main（）`属于**非**守护线程，守护线程通常由JVM自己使用，Java程序也可以标明自己创建的线程是守护线程

### JVM实例的消亡

当程序中的所有非守护线程都终止时，JVM才退出；若安全管理器允许，程序也可以使用Runtime类或者System.exit()来退出。


