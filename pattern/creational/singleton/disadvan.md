# 双检索的缺点

参考博文：http://ifeve.com/double-checked-locking-with-delay-initialization/

```java
public class DoubleCheckedLocking {                 	 //1
    private static Instance instance;                    //2

    public static Instance getInstance() {               //3
        if (instance == null) {                          //4:第一次检查
            synchronized (DoubleCheckedLocking.class) {  //5:加锁
                if (instance == null)                    //6:第二次检查
                    instance = new Instance();           //7:问题的根源出在这里
            }                                            //8
        }                                                //9
        return instance;                                 //10
    }                                                    //11
}                                                        //12
```

在线程执行到第4行代码读取到instance不为null时，instance引用的对象有可能还没有完成初始化

对于第7行创创建对象会被分解为如下3行伪代码：
```java
memory = allocate();	//1:分配对象的内存空间
ctorInstance(memory);	//2:初始化对象
instance = memory;		//3:设置instance指向刚分配的内存地址
```

但是，上诉的2、3之间可能会被重排序。经常重排序后的时序图可能如下，此时B线程判定Instance不为空（但是此时的instance并未被初始化），所以B线程将会访问一个还未被初始化的对象

![重排序后B线程可能访问未被初始化的对象]()

##解决方法：volatile
使用关键字volatile禁止指令重排序，关于volatile关键字使用参见前文

这个需要在jdk5.0过后使用