## 类的适配器模式

类图:
![](https://justdojava.gitbooks.io/it-interview/img/pattern/adapter_class.PNG)

核心思想就是：有一个Source类，拥有一个方法，待适配，目标接口时Targetable，通过Adapter类，将Source的功能扩展到Targetable里，看代码：

```java
public class Source {  
    public void method1() {  
        System.out.println("this is original method!");  
    }  
}  
```

```java
public interface Targetable {  
    /* 与原类中的方法相同 */  
    public void method1();  
    /* 新类的方法 */  
    public void method2();  
}  
```

```java
public class Adapter extends Source implements Targetable {  
    @Override  
    public void method2() {  
        System.out.println("this is the targetable method!");  
    }  
}  
```

Adapter类继承Source类，实现Targetable接口，下面是测试类：

```java
public class AdapterTest {  
  
    public static void main(String[] args) {  
        Targetable target = new Adapter();  
        target.method1();  
        target.method2();  
    }  
}  
```

输出：
```
this is original method!
this is the targetable method!
```
这样Targetable接口的实现类就具有了Source类的功能。

