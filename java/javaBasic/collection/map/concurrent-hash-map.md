#ConcurrentHashMap与锁分离
ConcurrentHashMap是Java 5中支持高并发、高吞吐的HashMap。

它的关键技术是：**锁分离技术**。它使用了多个锁来控制对hash表的不同部分进行修改。

##ConcurrentHashMap实现原理
ConcurrentHashMap包含了两个静态内部类：

- HashEntry
- Segment

ConcurrentHashMap中的Segment就相当于一个小的HashTable，每个HashTable由多个HashEntry组成。每个Segment持有自己的锁，只要修改操作发生在不同的Segment上，就可以并发执行

![组成](http://askingwindy-gitcafe.qiniudn.com/ConcurrentHashMap.png)

###HashEntry
HashEntry用来封装映射表的键值对

###Segment
Segment用来充当锁的角色，是一种可重入锁ReentrantLock
利用了**锁分离**技术来保护不同的segment

####具体实现
- 每个Segment对象守护整个散列表的若干个桶
- 每个桶由若干个HashEntry对象连接起来的

##ConcurrentHashMap是弱一致的
ConcurrentHashMap进行操作时，put操作将一个元素加入到底层数据结构后，get可能在某段时间内还看不到这个元素。

ConcurrentHashMap的弱一致性主要是为了提升效率，是一致性与效率之间的一种权衡。

要成为强一致性，就得到处使用锁，甚至是全局锁，这就与Hashtable和同步的HashMap一样了


##锁分离
每个Segment持有自己的锁，只要修改操作发生在不同的Segment上，就可以并发执行
###跨Segment的锁
有些方法需要跨Segment执行:`size()`、`containsValue()`，他们可能需要锁定整个表而不仅仅是某个Segment。 

需要按顺序锁定所有的段，操作完毕后，需要按顺序释放所有段的锁（确保不发生死锁）。
