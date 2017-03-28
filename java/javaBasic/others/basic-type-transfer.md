# 基本类型之间的转换
##基本类型
Java中默认的类型：整型是`int`，浮点数是`double`

|名字|长度|
|--|--|
|boolean|1位|
|byte|8位|
|char|16位|
|short|16位|
|int|32位|
|float|32位|
|long|64位|
|double|64位|

##自动转换
**低位向高位**的转换时自动完成的
- 任何类型可以转换为`double`，其次是`float`，再是`long`, 再其次是`int`
- boolean不能和其他任何类型相互转换

##运算符`+-*/%`时类型转换：
1. byte/char/short + int ==> int

2. int + float ==> float

3. int + double/long ==>double/long

4. float + double/long ==> double/long

5. long + double ==> double

##两个例子
![int与double之间转换](http://askingwindy-gitcafe.qiniudn.com/1.png)
![int与float之间转换](http://askingwindy-gitcafe.qiniudn.com/2.png)
