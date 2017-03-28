## 类的加载机制

主要关注点：

- 什么是类的加载

- 类的生命周期

- 类加载器

- 双亲委派模型

**什么是类的加载**

类的加载指的是将类的.class文件中的二进制数据读入到内存中，将其放在运行时数据区的方法区内，然后在堆区创建一个java.lang.Class对象，用来封装类在方法区内的数据结构。类的加载的最终产品是位于堆区中的Class对象，Class对象封装了类在方法区内的数据结构，并且向Java程序员提供了访问方法区内的数据结构的接口。

**类的生命周期**

类的生命周期包括这几个部分，加载、连接、初始化、使用和卸载，其中前三部是类的加载的过程,如下图；


![](http://www.ityouknow.com/assets/images/2017/jvm/class.png)

- 加载，查找并加载类的二进制数据，在Java堆中也创建一个java.lang.Class类的对象

- 连接，连接又包含三块内容：验证、准备、初始化。1）验证，文件格式、元数据、字节码、符号引用验证；2）准备，为类的静态变量分配内存，并将其初始化为默认值；3）解析，把类中的符号引用转换为直接引用

- 初始化，为类的静态变量赋予正确的初始值

- 使用，new出对象程序中使用

- 卸载，执行垃圾回收

> *几个小问题？*

> *1、JVM初始化步骤 ？ 2、类初始化时机 ？3、哪几种情况下，Java虚拟机将结束生命周期？*

> *答案参考这篇文章[jvm系列(一):java类的加载机制](http://www.cnblogs.com/ityouknow/p/5603287.html)*

**类加载器**


![](http://www.ityouknow.com/assets/images/2017/jvm/calssloader.png)

- 启动类加载器：Bootstrap ClassLoader，负责加载存放在JDK\jre\lib(JDK代表JDK的安装目录，下同)下，或被-Xbootclasspath参数指定的路径中的，并且能被虚拟机识别的类库

- 扩展类加载器：Extension ClassLoader，该加载器由sun.misc.Launcher$ExtClassLoader实现，它负责加载DK\jre\lib\ext目录中，或者由java.ext.dirs系统变量指定的路径中的所有类库（如javax.*开头的类），开发者可以直接使用扩展类加载器。

- 应用程序类加载器：Application ClassLoader，该类加载器由sun.misc.Launcher$AppClassLoader来实现，它负责加载用户类路径（ClassPath）所指定的类，开发者可以直接使用该类加载器

**类加载机制**

- 全盘负责，当一个类加载器负责加载某个Class时，该Class所依赖的和引用的其他Class也将由该类加载器负责载入，除非显示使用另外一个类加载器来载入

- 父类委托，先让父类加载器试图加载该类，只有在父类加载器无法加载该类时才尝试从自己的类路径中加载该类

- 缓存机制，缓存机制将会保证所有加载过的Class都会被缓存，当程序中需要使用某个Class时，类加载器先从缓存区寻找该Class，只有缓存区不存在，系统才会读取该类对应的二进制数据，并将其转换成Class对象，存入缓存区。这就是为什么修改了Class后，必须重启JVM，程序的修改才会生效


**类加载**

类只有被加载到JVM后才能运行，这个加载的过程是由类加载器完成的，由ClassLoader和它的子类来实现。

类加载的实质是把类文件从**硬盘**读取到内存中。

**类加载的分类**

1. 显式加载：直接调用class.forName()来创建对象

2. 隐式加载：使用new来创建对象，会隐式的调用类加载器

**类加载的特性**

1. 通过全限定名定义二进制字节流

2. 字节流的静态存储结构转化为方法区运行时的数据结构

3. 在内存中生产**java.lang.Class**， 作为方法区对这个类的访问入口

4. 动态的， 并不会一次性对所有类进行加载；除了基础类外，其他类仅在需要时才运行。


**Class文件格式**
  
| 类型 |名称|物理意义|数量|
|--|--|--|--|
|u4|magic|魔数|1|
|u2|minor_version|版本号|1|
|u2|major_version|版本号|1|
|u2|constant_pool_count|常量池入口|1|
|cp_info|constant_pool|常量池入口|1|
|u2|access_flags|访问标志|1|
|u2|this_class|类索引|1|
|u2|super_class|类索引|1|
|u2|interface_count|接口索引集合|1|
|u2|interface|接口索引集合|interface_count|
|u2|fields_count|字段表|1|
|u2|fields|字段表|fields_count|
|field_info|fields|字段表|fields_count|
|u2|methods_count|方法表集合|methods_count|
|method_info|methods|方法表集合|methods_count|
|u2|attribute_count|属性表|1|
|attribute_info|attributes|属性表|attributes_count|



> *参考 [jvm系列(一):java类的加载机制](http://www.cnblogs.com/ityouknow/p/5603287.html)*




