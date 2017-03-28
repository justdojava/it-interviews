# String/StringBuffer/StringBuilder
- String 字符串常量
- StringBuffer 字符串变量（线程安全）
- StringBuilder 字符串变量（非线程安全）

String 类型和 StringBuffer 类型的主要性能区别其实在于 String 是**不可变**的对象, 因此在每次对 String 类型进行改变的时候其实都等同于生成了一个新的 String 对象，然后将指针指向新的 String 对象
- 所以经常改变内容的字符串最好不要用 String ，因为每次生成对象都会对系统性能产生影响
- 特别当内存中无引用对象多了以后， JVM 的 GC 就会开始工作，那速度是一定会相当慢的

## 字符串拼接效率比较
1. 直接使用`+` 最慢

2. 使用`StringBuilder.append()`,速度最快，但有线程安全问题，只有JDK5支持

3. Join和StringBuffer速度差不多

4. StringUtils:join(Object[] array, String separator),将字符串数组转成字符串(和split对应)

5. string(s).concat()比`+`快不了多少
