# 异常处理

异常:程序运行时所发生的非正常情况或错误

## 异常机制

JAVA中把异常当做对象来处理，并定义了一个基类Throwable作为所有异常的父类

## Throwable类

Throwable类是Java 语言中所有错误或异常的超类，他有两个子类：Error与Exception

### Error

表示恢复是很困难的一件事，他用来表示系统错误或者低层资源的错误

### Exception

表示一种设计或实现问题。通常由一个程序员导致的错误

#### CheckedException

需要Try Catch显示的捕获，常见的 IOException

#### UncheckedException

又称RuntimeException，主要包括：

* NullPointerException 
* IndexOutOfBoundException
* ClassCastException \(类型强制转换异常\)
* IllegalArgumentException \(传递非法参数异常\)
* ArrayStoreException \(向数组中存放与声明类型不兼容对象异常\)

## 异常捕获

对于try..catch捕获异常的形式来说，对于异常的捕获，可以有多个catch。

对于try里面发生的异常，他会根据发生的异常和catch里面的进行匹配\(怎么匹配，按照catch块从上往下匹配\)，当它匹配某一个catch块的时候，他就直接进入到这个catch块里面去了，后面在再有catch块的话，它不做任何处理，直接跳过去，全部忽略掉。如果有finally的话进入到finally里面继续执行。

换句话说，如果有匹配的catch，它就会忽略掉这个catch后面所有的catch。

## 异常捕获的顺序

catch\(\)语句中异常捕获范围必须从小到大
以下例子无法**编译**通过：

```java
import java.io.IOException;   
public class ExceptionTryCatchTest {   
     public void doSomething() throws IOException{   
         System.out.println("do somthing");   
     }   
     public static void main(String[] args){   
         ExceptionTryCatchTest et = new ExceptionTryCatchTest();   
         try {   
             et.doSomething();   
         } catch (Exception e) {   
             //
         } catch (IOException e) {   
             //
         }   
     }  
}  
```

对我们这个方法来说，抛出的是IOException
当执行etct.doSomething\(\);时，一但抛出IOException，它首先进入到catch \(Exception e\) {}里面，先和Exception匹配，所以后面的catch都不会执行了，所以catch \(IOException e\) {}永远都执行不到，就给我们报出了**编译的错误**:已捕捉到异常 java.io.IOException。

