# 单例模式实现方法
单例模式有五种写法：懒汉、饿汉、双重检验锁、静态内部类、枚举

##饿汉模式

这种方式比较常用，但容易产生垃圾对象。它基于 classloder 机制避免了多线程的同步问题，不过，instance 在类装载时就实例化，虽然导致类装载的原因有很多种，在单例模式中大多数都是调用 getInstance 方法， 但是也不能确定有其他的方式（或者其他的静态方法）导致类装载，这时候初始化 instance 显然没有达到 lazy loading 的效果。

**优点**：没有加锁，执行效率会提高

**缺点**：类加载时就初始化，浪费内存

```java
public class Singleton {
	private static final Singleton instance = new Singleton();
    
    private Singleton(){}
    
    public static Singleton getInstance() {
    	return instance;
    }
}
```

##双检索
双重检验锁模式（double checked locking pattern），是一种使用同步块加锁的方法。

有两次检查 instance == null，一次是在同步块外，一次是在同步块内

* 为什么在同步块内还要再检验一次？
	* 因为可能会有多个线程一起进入同步块外的 if，如果在同步块内不进行二次检验的话就会生成多个实例了

* volatile来保证instance的顺序

```java
public class Singleton {
	private volatile static Singleton instance;
    
    private Singleton(){}
    
    public static Singleton getInstance() {
    	if(instance == null) {
        	synchronized(Singleton.class) {
            	if(instance == null) {
                	instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

##在getInstance()方法上同步有优势还是仅同步必要的块更优优势？

* 因为锁定仅仅在创建实例时才有意义，然后其他时候实例仅仅是只读访问的，因此只同步必要的块的性能更优，并且是更好的选择

* 锁有开销





