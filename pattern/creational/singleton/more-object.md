# 怎样获得更多的单例对象

答： 反射机制

```java
Constructor con = Singleton.class.getDeclaredConstruction();
con.setAccessible(true);

//通过反射获取实例
Singleton instance = (Singleton)con.newInstance();
```

##如何防止反射破坏单例模式

构造函数调用时进行处理，当构造函数第2次被调用时抛出异常！


