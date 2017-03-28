# 策略模式

策略模式是对**算法的包装**，是把使用算法的责任和算法本身分割开来，委派给不同的对象管理。

策略模式通常把一个系列的算法包装到一系列的策略类里面，作为一个抽象策略类的子类。用一句话来说，就是：“准备一组算法，并将每一个算法封装起来，使得它们可以互换”。

这个模式涉及到三个角色：

- **环境(Context)**：持有一个Strategy的引用

- **抽象策略(Strategy)**：这是一个抽象角色，通常由一个接口或抽象类实现。此角色给出所有的具体策略类所需的接口

- **具体策略(ConcreteStrategy)**：包装了相关的算法或行为

##源码
###Test
```java
public class Test {
	public static void main(String[] args){
    	//创建策略对象
        Strategy strategy = new StraA();
        
        //创建环境
        Context context = new Context(strategy);
        
        //调用方法
        context.useStrategy();
    }
}
```
###Contex
```java
public class Context {
	//持有一个具体的策略对象
	private Strategy strategy;
    
    //传入策略对象
    public Context(Strategy strategy){
    	this.strategy = strategy;
    }
    
    //策略方法
    public void useStrategy() {
    	strategy.func();
    }
}
```
###Strategy：接口
```java
public interface Strategy {
	public void func();
}
```
###具体策略:接口的实现
```java
pulibc class StraA implements Strategy {
	@Override
    public void func(){
    	//...实现
    }
}
```

##与简单工厂模式区别
工厂模式实例化一个产品的操作是在服务端来做的，换句话说客户端传达给服务端的只是某种标识，服务端根据该标识实例化一个对象。

而策略模式的客户端传达给服务端的是一个实例，服务端只是将该实例拿过去在服务端的环境里执行该实例的方法


参考博文：
1. http://www.cnblogs.com/java-my-life/archive/2012/05/10/2491891.html
2. http://www.cnblogs.com/justinw/archive/2007/02/06/641414.html
3. http://xiewenbo.iteye.com/blog/1294207
4. http://lh-kevin.iteye.com/blog/1981574