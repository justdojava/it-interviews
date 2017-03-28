# 关键字final&finally&finalize
## final关键字
###final修饰类
final类通常功能是完整的，它们不能被继承。

Java中有许多类是final的，譬如String, Interger以及其他包装类。
###final修饰变量
凡是对成员变量或者本地变量(在方法中的或者代码块中的变量称为本地变量)声明为final的都叫作final变量。

final变量经常和static关键字一起使用，作为常量。

final变量可以在任何可以被始化的地方被始化,但只能被初始化一次.一旦被初始化后就不能再次赋 值(重新指向其它对象),作为成员变量一定要显式初始化,而作为临时变量则可以只定义不初始化(当然也不能引用) 
###final修饰方法
方法前面加上final关键字，代表这个方法不可以被子类的方法重写。
###final修饰方法形参
形参的值无法被更改（编译时报错）
参考博文：[JAVA方法中的参数用final来修饰的原因](http://blog.csdn.net/tavor/article/details/1920336)
```java
 //如果不是final 的话，我可以在checkInt方法内部把i的值改变（有意或无意的，
 //虽然不会改变实际调用处的值），特别是无意的，可能会引用一些难以发现的BUG
 publicstaticvoid checkInt(int i)
 {
       i = 200;//这样是可以的，不会编译出错的
       //do something
 }

 //如果是final 的话，我可以在checkInt方法内部就没办法把i的值改变
 //可以完全避免上面的问题
 publicstaticvoid checkInt(final int i)
 {
       i = 200;//这样是不可以的，会编译出错的
       //do something
 }

 //final 的引用类型方法参数
 publicstaticvoid checkLoginInfo(final LoginInfo login)
 {
       login = new LoginInfo();//error,编译不过去
       //do something
 }
 //非final的引用类型方法参数
 public staticvoid checkLoginInfo(LoginInfo login)
 {
       //没有任何问题，但是肯定不符合此参数存在的初衷
       login = new LoginInfo();
       //do something
 }
```

##finally关键字
用于try/catch语句中，表示这段语句最终总是被执行

##finalize关键字
finalize 是Object类的一个方法，在垃圾收集器执行的时候会调用被回收对象的此方法，可以覆盖此方法提供垃圾收集时的其他资源回收，例如关闭文件等.

一旦垃圾回收器准备好释放对象占用的存储空间，将首先调用其finalize()方法。并且在下一次垃圾回收动作发生时，才会真正回收对象占用的内存