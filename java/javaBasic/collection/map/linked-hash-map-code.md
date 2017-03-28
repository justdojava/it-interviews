#LinkedHashMap源码研读
LinkedHashMap是HashMap+LinkedList的结合

新元素put进来的Entry会保存在HashMap中，同时它也会被加入一个header为头指针的双向循环链表的尾部！

![LinkedHashMap具体结构](http://askingwindy-gitcafe.qiniudn.com/LinkedHashMap.png)
##LinkedHashMap的有序性
能够保证某种有序性，非排序有序性，而是指某种稳定性：
- access-ordered
按照访问时间排序，最近访问的排在最前面。这一点可以被用作cache。
- insert-ordered
按照插入顺序排序，最后插入的排在最前面。更新不影响次序。

上面2点有序性是互斥的，即2者必居其一。 你可以通过下面的构造函数指定这种有序性，默认是插入有序:
```java 
public LinkedHashMap(int initialCapacity, float loadFactor, boolean accessOrder) {
	super(initialCapacity, loadFactor);
	this.accessOrder = accessOrder;
}
```
关于access-ordered特性的实现，通过覆盖HashMap.Entry.recordAccess方法。 HashMap.Entry.还有一个recordRemoval方法，在Entry被remove的时候调用。
##Entry类
这里的Entry类实现了一个双向链表
```java
private static class Entry<K,V> extends HashMap.Entry<K,V>{
	 // 继承自HashMap的Entry（已有key/value/hash/next字段）
     // 双向链表
     Entry<K,V> before;
     Entry<K,V> after;
 
     // 构造方法
     Entry(int hash, K key, V value, HashMap.Entry<K,V> next) {
            super(hash, key, value, next);
     }
}
```
###addBefore()方法
在当前结点的前面添加结点
```java
//将当前的entry插入到existingEntry的前面
private void addBefore(Entry<K,V> existingEntry){
       after  = existingEntry;
       before = existingEntry.before;
       before.after = this;
       after.before = this;         
}
```
###recordAccess()方法
 如果定义了LinkedHashMap的迭代顺序为访问顺序排序， 删除以前位置上的元素，并将最新访问的元素添加到链表尾部。  
```java
 void recordAccess(HashMap<K,V> m) {
      LinkedHashMap<K,V> lm = (LinkedHashMap<K,V>)m;     // 把调用者的this转化成LinkedHashMap
      if(lm.accessOrder){
           remove(); // 删除当前结点
           addBefore(lm.header); // 插入到header双链表尾部
      }
 }
```
##LinkedHashMap类
```java
public class LinkedHashMap<K, V> extends HashMap<K, V> implements Map<K, V>{
	//额外定义的双链表的指针，每次新put()进来的元素都要插入到双链表的尾部
	private transient Entry<K, V> header;
	//accessOrder==true,表示访问顺序排序
	//accessOrder ==false，表示按插入顺序排序
	private final boolean accessOrder;
}
```
###put()方法
put() 继承自 HashMap。添加元素时，不仅按照HashMap的方式散列存储，而且还通过双向链表记录先后顺序
```java
public V put(K key, V value) {
     int hash = hash(key.hashCode());     // key的特殊hash值
     int i = indexFor(hash,table.length);     // 槽位 index

     // key是否已经存在，存在则返回value。
     for (Entry<K,V> e = table[i]; e != null; e = e.next) {
       Object k;
       if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
           V oldValue = e.value;
           e.value = value;
           e.recordAccess(this); //！！LinkedHashMap特有
           return oldValue;
       }
         }

          // key不存在就添加Entry
          addEntry(hash,key,value,i);
          return null;
}
```
####addEntry()方法
与HashMap中addEntry()功能一致，将一个新的Entry类添加入桶中
```java
/**
 *@param bucketIndex [Entry[]中的槽位]
 */
void addEntry(int hash, K key, V value, int bucketIndex) {
	//将Entry加入桶中
    createEntry(hash, key, value, bucketIndex);

    //删除最近最少使用元素的策略定义 
    Entry<K,V> eldest = header.after;
    if (removeEldestEntry(eldest)) {
        removeEntryForKey(eldest.key);
    } else {
        if (size >= threshold)
            resize(2 * table.length);
    }
}
```
利用头插入法，将entry实例加入HashMap中，同时需要将它加入一个循环双链表的尾部！
```java
void createEntry(int hash, K key, V value, int bucketIndex) {
    HashMap.Entry<K,V> old = table[bucketIndex];
    //同样，利用头插入法
    Entry<K,V> e = new Entry<>(hash, key, value, old);
    table[bucketIndex] = e;
    e.addBefore(header);
    size++;
}
```

##LRU算法
首先，当accessOrder为true时，才会开启按访问顺序排序的模式，才能用来实现LRU算法。

无论是put方法还是get方法，都会导致目标Entry成为最近访问的Entry，因此便把该Entry加入到了双向链表的末尾

- get方法通过调用recordAccess方法来实现
- put方法在覆盖已有key的情况下，也是通过调用recordAccess方法来实现，在插入新的Entry时，则是通过createEntry中的addBefore方法来实现

这样便把最近使用了的Entry放入到了双向链表的后面

多次操作后，双向链表前面的Entry便是最近没有使用的，这样当节点个数满的时候，删除的最前面的Entry(header后面的那个Entry)便是最近最少使用的Entry。
