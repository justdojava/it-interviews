# Collection集合类
##Java集合框架
Java集合框架是一组实现集合数据结构的类和接口。

Collection接口是一组允许重复的对象，有3个接口继承了该接口。值得注意的是，Map接口不由Collection派生。
![Java集合框架](http://askingwindy-gitcafe.qiniudn.com/collections framework overview.png)
##Collection
Collection是集合层次的根。一个集合包含一组对象作为其元素。 
Java平台不提供任何直接实现这个接口。
###Set 
是一个不能包含重复的元素的集合。此接口模型代表数学Set的抽象，用来代表一组Set，如一副扑克牌。
###List
是有序集合，可以包含重复的元素。
可以从它的索引访问任何元素。更像是动态长度的数组列表。
#### ArryList、LinkedList、Vector
- ArrayList底层实现是一个可变的数组
- LinkedList底层实现是一个链表(可被用作Stack、Queue、Deque)
- Vector与ArrayList相同，不过它是同步的

如果涉及到堆栈，队列等操作，应该考虑用List，对于需要快速插入，删除元素，应该使用LinkedList，如果需要快速随机访问元素，应该使用ArrayList
####Stack
Stack继承自Vector。
Stack模拟了一个后进先出的栈
###Queue
先进先出的队列
##Map
没有继承Collection接口，是一个键映射值的对象。一个Map不能包含重复键：每个key只能映射一个值。
## Iterator
Iterator接口提供遍历集合的方法。
迭代器允许呼叫者在迭代过程中从集合中删除元素。
#### Iterator
只能正向遍历集合，适用于获取移除元素。
####ListIerator
继承Iterator,可以双向列表的遍历，同样支持元素的修改。