# concurrent包中的常用类
##1. concurrentHashMap
##2. Excutor
- 一种工厂类，建立不同的线程

##3. ThreadPoolExcutor
- 生成线程池

##4. FutureTask
- 当有一个任务需要交给某个线程去处理时，可以用FutureTask

- FutureTask实现了Runnable接口，因此可以通过Thread启动，或者交给ExcutorService处理

- FutureTask提供了get()方法，可以返回执行结果。在任务执行结束之前该方法阻塞，知道任务执行完，并返回结果。

##5. LinkedBlockingQueue
- 用于生产者、消费者模型，取时队列为空就一直wait，存时队列满就一直wait