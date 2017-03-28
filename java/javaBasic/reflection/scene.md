# 应用场景
反射可以用于：
1. Spring的IOC(控制注入)
2. Hibernater的JDBC封装
3. 白盒测试

###反射的例子1
如果想用一个list，但是在不同的场景下需要用ArrayList或者LinkedList，编译时无法确定是哪一个

这时可以利用反射，在运行时候通过配置文件之类的载入具体实现类的名字， 然后通过反射机制得到自己想要的list，如下：
```java
List<Integer> list = (List<Integer>)Class.forName("ArrayList").newInstance();
```

类似于`switch(类名)`,然后通过`Class.forName().nesInstance()`得到想要引用的类型
###反射的例子2
![反射的例子2](http://askingwindy-gitcafe.qiniudn.com/反射的例子.png)