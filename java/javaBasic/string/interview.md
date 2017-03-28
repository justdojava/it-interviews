# 关于String的面试题
Q1: `String s = new String("abc")`创建了几个对象?
A: 1个或2个。如果常量池原来有“abc”，那么只创建一个对象放在堆。如果没有，那么还要在常量池中新创建一个“abc”。

Q2: 请问s1==s3是true还是false，s1==s4是false还是true。s1==s5呢？
```java
String s1 = "abc";
String s2 = "a";
String s3 = s2 + "bc";
String s4 = "a" + "bc";
String s5 = s3.intern();`
```
intern()方法：String类的intern方法，返回一个值相同的String对象，但是这个对象就像一个字符串字面常数一样，意思就是，他也到**字符串池**（常量池）里面去了。

```java
String s1 = "abc";
String s2 = "a";
String s3 = s2 + "bc";（运行时才能决定，是new的，在堆中,s1!=s3）
String s4 = "a" + "bc";(在字符串池中,s1==s4)
String s5 = s3.intern();（是s3放到字符串池里返回的对象，即s1==s5）
```
