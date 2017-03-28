# 生产者消费者模型
概述：
- 生产者：往篮子中放入bread

- 消费者：从篮子中拿出bread

- 篮子：容量固定，0<=capacity<=size；如果capacity == size，生产者不能再往篮子中放入bread;如果capacity==0，消费者不能再从篮子中拿出bread

## 普通的信号量通信

![普通信号量实现生产者消费者](http://7d9o4k.com1.z0.glb.clouddn.com/生产者消费lock.png)

## BlockingQueue
- `java.util.concurrent.BlockingQueue`的特性是：当队列是空的时，从队列中获取或删除元素的操作将会被阻塞，或者当队列是满时，往队列里添加元素的操作会被阻塞

```java
//生产者
class Producer implements Runnable {
	private final BlockingQueue<Integer> queue;

	public Producer(BlockingQueue<Integer> q){
		this.queue = q;
	}

	@Override
	public void run(){
		try{
			while(true){
				this.queue.put(1);//放入1；1相当于bread
			}
		}catch(InterruptException e){
			e.printStackTrace();
		}
	}
}
//消费者
class Consumer implements Runnable {
	private final BlockingQueue<Integer> queue;

	public Consumer(BlockingQueue<Integer> q){
		this.queue = q;
	}

	@Override
	public void run(){
		try{
			while(true){
				this.queue.take();//拿出一个bread
			}
		}catch(InterruptException e){
			e.printStackTrace();
		}
	}
}
//测试
public class Main　{
	public static void main(String[] args){
		int backetSize = 10;
		BlockingQueue<Integer> backet = new LinkedBlockingQueue<>(backetSize);

		Producer producer = new Producer(backet);
		Consumer consumer = new Consumer(backet);

		new Thread(producer).start();
		new Thread(consumer).start();
	}
}
```