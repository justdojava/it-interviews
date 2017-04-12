## 简单工厂

就是建立一个工厂类，对实现了同一接口的一些类进行实例的创建。首先看下关系图

![](https://justdojava.gitbooks.io/it-interview/img/pattern/simple_factory.PNG)

举例如下：（我们举一个发送邮件和短信的例子）

首先，创建二者的共同接口：

```java
public interface Sender {  
    public void Send();  
}  
```

其次，创建实现类：

```java
public class MailSender implements Sender {  
    @Override  
    public void Send() {  
        System.out.println("this is mailsender!");  
    }  
}  
```
```java
public class SmsSender implements Sender {  
    @Override  
    public void Send() {  
        System.out.println("this is sms sender!");  
    }  
} 
```

最后，建工厂类：

```java
public class SendFactory {  
  
    public Sender produce(String type) {  
        if ("mail".equals(type)) {  
            return new MailSender();  
        } else if ("sms".equals(type)) {  
            return new SmsSender();  
        } else {  
            System.out.println("请输入正确的类型!");  
            return null;  
        }  
    }  
}  
```

测试：

```java
public class SendFactory {  
  
    public Sender produce(String type) {  
        if ("mail".equals(type)) {  
            return new MailSender();  
        } else if ("sms".equals(type)) {  
            return new SmsSender();  
        } else {  
            System.out.println("请输入正确的类型!");  
            return null;  
        }  
    }  
}  
```

输出：

```
this is sms sender!
```






