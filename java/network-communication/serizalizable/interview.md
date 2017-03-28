# 常见面试题

##1. Serializable and Externalizable 接口的区别是什么？
* Externalizable继承了Serializable，并增加了两个新方法writeExternal()和readExternal(),只有在新方法中手动序列化的field才会被序列化

* Externalizable接口默认的数据化是不被序列化的

##2. Serializable接口中有多少方法？作用是什么？
* 0个方法，只是一个标记接口

* 为了提醒编译器使用JAVA序列化机制来序列化对象

##3. 什么是serialVersionUID？它的作用是什么？
* `public static final`的常量

* 根据此id保证序列化后的对象能正确被反序列化

* Java的序列化机制是通过在运行时判断类的serialVersionUID来验证版本一致性的。在进行反序列化时，JVM会把传来的字节流中的serialVersionUID与本地相应实体（类）的serialVersionUID进行比较，如果相同就认为是一致的，可以进行反序列化，否则就会出现序列化版本不一致的异常

##4. 对于不需要序列化的field，该如何声明？
* transient与static的变量不会被实例化

##5. 如果需要序列化的类里面引用了一个实例，这个实例并没有实现Serializable接口，会怎样？
* 运行时抛出`NotSerializableException`异常

##6. 如果子类实现了Serializable接口，而它的父类没有实现，从父类继承的field反序列化后会怎样？
* 对Serializable对象反序列化时，并不会调用任何构造函数 ，因此Serializable类无需默认构造函数，但是当Serializable类的父类没有实现Serializable接口时，反序列化过程会调用父类的默认构造函数，因此该父类必需有默认构造函数，否则会抛异常

* 父类的属性是直接被跳过不保存


