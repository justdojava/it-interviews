# Java中的序列化与反序列化

* java.io包有两个序列化对象的类。ObjectOutputStream负责将对象写入字节流，ObjectInputStream从字节流重构对象

使用一个输出流(如：FileOutputStream)来构造一个ObjectOutputStream(对象流)对象，接着，使用ObjectOutputStream对象的writeObject(Object obj)方法就可以将参数为obj的对象写出(即保存其状态

![序列化与反序列化代码](http://7d9o4k.com1.z0.glb.clouddn.com/serizable.PNG)

