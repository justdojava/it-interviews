# Java多线程同步(锁)的实现方法(synchronised 与reentrantlock)

##1. synchronized关键字

- synchronized可以在任意对象上加锁，而加锁的这段代码将成为互斥区或临界区

- 每个对象都可以做为锁，但一个对象做为锁时，应该被多个线程共享，这样显得有意义

- sleep()和CPU调度都不会放锁，而wait()会放锁

- 在什么对象上加锁 ？

 - 代码块 ： 指定的对象

 - 方法上 ： this引用

 - 静态方法上 ： class对象



- 另外：Collections类里有一组synchronized方法可以将Collection封装成同步的

##2. java.util.concurrent.locks.lock类

- 临界区边界灵活了

- 锁释放的顺序由用户决定

- 可以有多个Condition

###ReentrantLock

- 重入锁指的是在某一个线程中可以多次获得同一把锁，在线程中多次操作有锁的方法

- 需要使用除了内置锁以外的锁特性，比如可中断，可等待的锁，平等锁等

- 可轮询，可中断，定时，非块，公平队列等高级特性时候使用可重入锁

##Synchronized与Lock之间的区别？

| Synchronized | Lock |
|--------|--------|
| 使用Object本身的notify、wait、notify调度机制 | 可以使用condition进行线程间的调度 |
|必须显示声明加载在方法上或者指定代码块中|需要显示声明指定的起始位置与终止位置|
|托管给JVM执行，不会因为异常、没有释放而发生死锁|手动释放锁（最好再finally中释放）|

##synchronized与ReentrantLock之间区别

- JDK5中增加了一个Lock接口的实现类ReentrantLock

- 多了锁投票、定时锁、等候和中断锁。竞争不是很激烈时，Synchronized的性能优于ReentrantLock,在竞争特别激烈时，syn的性能会下降的非常快


