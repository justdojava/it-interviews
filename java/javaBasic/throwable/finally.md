#一道关于finally的面试题
##程序求输出
```java
public class Test{
	public static void main(String[] args){
		System.out.println(new Test().test());
	}
	public int test(){
		int res = 1;
		try{
			throw new Exception();
		}catch(Exception e){
			return res;
		}finally{
			res = 2;
			System.out.println("finally");
		}
	}
}
```
##输出结果：
```java
finally
1
```
##解释：
return 之前跳到finally，此时要return 的值已经被存到一个局部引用变量里了，finally中对res的改变对返回值没有影响
