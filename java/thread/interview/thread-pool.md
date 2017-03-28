# 关于多线程的面试题
##使用Runnalbe还是Thread?
JAVA里面允许调用多个接口，但是不允许多继承，所以实现Runable接口更好

##Runnable和Callable有什么不同？
- Runnable和Callable都代表那些要在不同的线程中执行的任务

- Callable是在JDK1.5增加的。它们的主要区别是Callable的 call() 方法可以返回值和抛出异常（返回Future对象）

##Thread类中的start() 和 run() 方法有什么区别？
- start()方法被用来启动新创建的线程，而且start()内部调用了run()方法

- 当你调用run()方法的时候，只会是在**原来的线程**中调用，没有新的线程启动，start()方法才会启动新线程

##为什么要使用线程池

* 创建线程要花费昂贵的资源和时间，如果任务来了才创建线程那么响应时间会变长，而且一个进程能创建的线程数有限

* 在程序启动的时候就创建若干线程来响应处理，它们被称为线程池，里面的线程叫工作线程(JDK1.5开始提供Excutor)



##提交任务时，线程池队列已满。会时发会生什么？
* 一个任务不能被调度执行那么ThreadPoolExecutor’s submit()方法将会抛出一个RejectedExecutionException异常

* 不是会阻塞直到线程池队列有空位

##线程池中submit()和 execute()方法有什么区别？
- 两个方法都可以向线程池提交任务

* execute()方法的返回类型是void，它定义在Executor接口中

* submit()方法可以返回持有计算结果的Future对象，它定义在ExecutorService接口中，它扩展了Executor接口

##什么是FutureTask？
* FutureTask表示一个可以取消的异步运算

* 有启动和取消运算、查询运算是否完成和取回运算结果等方法。只有当运算完成的时候结果才能取回，如果运算尚未完成get方法将会阻塞。

* 一个FutureTask对象可以对调用了Callable和Runnable的对象进行包装，由于FutureTask也是调用了Runnable接口所以它可以提交给Executor来执行

##如何创建一个线程池？
```java
class Worker implements Runnable {
	int id;
    public Worker(int i){
    	this.id = i;
    }
    @Override
    public void run(){
    	//...
    }
}
public class Main(){
	public void create(){
        ExecutorService executor = Executors.newFixedThreadPool(2);

        for(int i=1; i<2; i++){
            Runnable worker = new Worker(i);
            executor.execute(worker);
    	}
	}
}

```

