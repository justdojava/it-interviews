# HashMap与HashTable部分源码比较
##HashMap与HashTable源码的异同
|喵|允许null插入|支持线程同步|如何得到hash值|
|--|--|--|--|
|HashMap|是|不支持|调用hash()方法，二次hash|
|HashTable|否|支持|直接使用对象的hashCode|
##准备工作：Entry类
Entry是用于实现链表的一个节点，有key，value用于存储自身节点数据，还有next用于下个节点的引用。
```java
static class Entry<K,V> implements Map.Entry<K,V> {//Entry继承了Map接口中的静态子类Entry；Entry<K,V>是槽中的元素，用作链表来解决散列冲突
    final K key;//键
    V value;//值
    Entry<K,V> next;//用来实现链表结构，同一链表中的key的hash是相同的
    int hash;

	protected Entry(int hash, K key, V value, Entry<K,V> next) {
		this.hash=hash;this.key=key;this.value=value;this.next=next;
    }
}
```
###为什么`Entry`是静态类？
`Map.Entry<K,V>`是`Map`接口中的子接口，默认为static，继承了static类的子类自然必须是静态类

##HashMap源码简要分析
HashMap找寻value的过程：

1. 对nullkey做单独处理，其实就是放在table[0]中
2. 根据key的hash值，决定entry在哪个桶
3. 对桶内的entry链表进行遍历，当查找到时返回value
4. 找不到，返回null


```java
public class HashMap<K,V>{
     static final int DEFAULT_INITIAL_CAPACITY = 16 ; // 默认初始容量是16。（必须是2的次方）
     static final int MAXIMUM_CAPACITY = 1 << 30 ; // 即2的30次方
     static final float DEFAULT_LOAD_FACTOR = 0.75f; // 默认装载因子
 
     Entry[] table;  // Entry表
     int size; // Entry[]实际存储的Entry个数
     int threshold; // reash的阈值，=capacity * loadfactor
     final float loadFactor;
}

```

- 为什么容量是2的倍数？
为了寻址的快速。寻址是通过 index & (table.length-1)实现的，其实就是一个取模，如果table.length 是2的倍数的话，table.length-1 总是 111111... 的结构，与运算可以方便的得到mod值

###put()方法
```java
public V put(K key, V value) {
     int hash = hash(key.hashCode());     // key的hash值，调用了hash()方法，相当于做了二次hash
     int i = indexFor(hash,table.length);     // 得到槽位

     // 寻找是否已经有key存在，如果已经存在，使用新值覆盖旧值，返回旧值
     for (Entry<K,V> e = table[i]; e != null; e = e.next) {
       Object k;
       if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
           V oldValue = e.value;
           e.value = value;
           return oldValue;
       }
     }

     // 添加Entry
     addEntry(hash,key,value,i);
     return null;
}
```
put()方法中，如果遇见的键值对是新的，那么会调用addEntry()方法，将这个键值对存储在hash值相同的槽位的头部（链表的头插入）
```java
void addEntry(int hash, K key, V value, int bucketIndex) {
    Entry<K,V> e = table[bucketIndex];	//bucketIndex是hash值所在的槽位,得到此时的槽位头元素
    table[bucketIndex] = new Entry<>(hash, key, value, e);//利用头插入法，在构造函数中为链表插入新的元素
    if (size++ >= threshold)
        resize(2 * table.length);	//如果容量超出范围，进行rehash()的过程，即调用resize()方法
}
```
###hash()方法
这个函数以位移和异或的方式保证了hashcode不会有最坏情况出现（即大多数kv都分到了同一个桶）。在默认的加载因子下最坏不超过8的冲突。其实就是让分布更加的均化。
```java
static int hash(int h){
	h ^= (h>>>20)^(h>>>12);
	return h^(h>>>7)^(h>>>4);
}
```
###resize()方法
扩容做的事情包括：
1. 重新申请空间存放table
2. transfer（把老数据移到新的table中），并将table指向到新的table（之后老table自动垃圾收集掉）
3. 调整新的threshold值，以便下一次的resize()能够在put方法中被激活

```java
void resize(int newCapacity) {
    Entry[] oldTable = table;
    int oldCapacity = oldTable.length;
    Entry[] newTable = new Entry[newCapacity];	//构造一个新的桶来存储元素
    transfer(newTable); //把老数据移到新的表中
    table = newTable; //table指向新的表
    threshold = (int)(newCapacity * loadFactor); //调整新的threshold值
}
```
####transfer()方法
tansfer()方法将老数据移到了新的表中，由于链表构造是头插入，新表中每一个槽位的链表元素顺序与原顺序相反
```java
void transfer(Entry[] newTable) {
    Entry[] src = table;
    int newCapacity = newTable.length;
    for (int j = 0; j < src.length; j++) {
        Entry<K,V> e = src[j];
        if (e != null) {
            src[j] = null;    
            do {
                Entry<K,V> next = e.next;
                int i = indexFor(e.hash, newCapacity);
                e.next = newTable[i];
                newTable[i] = e;
                e = next;
            } while (e != null);
        }
    }
}
```

##HashTable源码简要分析
HashTable与HashMap相似，它们利用Entry[]数组来存储元素，利用链表法来解决冲突。Entry类来当做链表的节点，将hash值相同的键值对放入同一个链表中
###put()方法
```java
public synchronized V put(K key,V value) {
     // 检查key是否已经存在，若存在则覆盖已经存在value，并返回被覆盖的value。
     Entry tab[] = table;
     int hash = key.hashCode();	//直接得到hash值，不需要二次散列
     int index = (hash & 0x7FFFFFFF) % tab.length; // 存储槽位索引。
     for(Entry<K,V> e = tab[index]; e!=null; e=e.next ) { // 在冲突链表中寻找
          if( (e.hash == hash ) && e.key.equals(key) ) {
                    V old = e.value;
                    e.value = value; // 新value覆盖旧value
                    return old;
               }
     }

     // 是否需要rehash
     if(count >= threshold){
          rehash();
          tab = table; // rehash完毕后，修正tab指针指向新的Entry[]
          index =  (hash & 0x7FFFFFFF) % tab.length; // 重新计算Slot的index
     }

     // 存储到槽位，如果有冲突，新来的元素被放到了链表前面。
     Entry<K,V> e = tab[index]; // 旧有Entry
     tab[index] = new Entry<>(hash,key,value,e/* 旧有Entry成为了新增Entry的next */);
     count ++;
     return null;
}
```
###rehash()方法
当Entry[]的实际元素数量（Count）超过了分配容量（Capacity）的75%时，新建一个Entry[]是原先的2倍，并重新Hash（rehash）
```java
protected void rehash() {
     int oldCapacity = table.length;
     Entry[] oldMap = table;
     int newCapacity = (oldCapacity << 1) + 1; // 2倍+1
     Entry[] newMap = new Entry[newCapacity];
     threshold = (int)(newCapacity * loadFactor);
     table = newMap;

     for( int i=oldCapacity; i-- >0;){ //  i的取值范围为 [oldCapacity-1,0]
          for (Entry<K,V> old = oldMap[i]; old!=null;){ // 遍历旧Entry[]
               Entry<K,V> e = old;
               int index = (e.hash & 0x7FFFFFFF) % newCapacity;     // 重新计算各个元素在新Entry[]中的槽位index。
               e.next = newMap[index]; // 已经存在槽位中的Entry放到当前元素的next中
               newMap[index]=e;     // 放到槽位中
               old = old.next;
          }
     }

}
```


