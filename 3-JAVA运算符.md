# JAVA运算符

## 数学运算符

"++""--"的两类使用方法

* ++变量，--变量: 先进行变量的自增或自减，后进行数字的计算；
* 变量++，变量--：先使用变量进行计算，在进行自增或自减；
```
public class JavaDemo {
	public static void main(String args[]){
		int x = 10;
		int y = 20;
		// 1. ++x: x先自增（加1）变为11；
		// 2. x-y: 11-20 = -9;
		// 3. y--: y完成计算后自减，变为19；
		int result = ++x - y--;
		System.out.println("计算结果：" + result); // -9
		System.out.println("x = " + x); // 11
		System.out.println("y = " + y); // 19
	}
}
```

为使代码清晰易读，应写为：
```
public class JavaDemo {
	public static void main(String args[]){
		int x = 10;
		int y = 20;
		// 1. ++x: x先自增（加1）变为11；
		// 2. x-y: 11-20 = -9;
		// 3. y--: y完成计算后自减，变为19；
		++x;
		int result = x - y;
		y--;
		System.out.println("计算结果：" + result); // -9
		System.out.println("x = " + x); // 11
		System.out.println("y = " + y); // 19
	}
}
```

## 关系运算符

大于（>），小于（<），大于等于（>=），小于等于（<=），不等（!=），相等（==）


## 三目运算
```
public class JavaDemo {
	public static void main(String args[]){
		int x = 10;
		int y = 20;
		// 判断x与y的大小关系，决定最终max变量的内容
		int max = x > y ? x : y;
		System.out.println(max); 
	}
}
```
三目运算本身可以进行嵌套处理
```
public class JavaDemo {
	public static void main(String args[]){
		int x = 10;
		int y = 20;
		int z = 15;
		// 判断x与y的大小关系，决定最终max变量的内容
		int max = (x > y ? x : y) > z ? (x > y ? x : y): z;
		System.out.println(max); 
	}
}
```
嵌套使代码变短，但使可读性变差，谨慎使用


## 位运算

主要处理二进制数据的计算，与（&），或（|），异或（^），反码（~），移位处理

十进制与二进制的转换处理逻辑：数字连续除二取余并逆序。

例：

13/2 = 6 …… 1

6/2 = 3 …… 0

3/2 = 1 …… 1

1/2 = 1 …… 1

最终结果：1101

由于JAVA为32位，实际结果应为：00000000 00000000 00000000 00001101

与运算
```
public class JavaDemo {
	public static void main(String args[]){
		int x = 13;
		int y = 7;
		System.out.println(x & y); 
	}
}
```
x == 00000000 00000000 00000000 00001101 == 13
y == 00000000 00000000 00000000 00000111 == 7
x&y == 00000000 00000000 00000000 00000101 == 5


或运算
```
public class JavaDemo {
	public static void main(String args[]){
		int x = 13;
		int y = 7;
		System.out.println(x | y); 
	}
}
```
x == 00000000 00000000 00000000 00001101 == 13

y == 00000000 00000000 00000000 00000111 == 7

x|y == 00000000 00000000 00000000 00001111 == 15


移位计算
```
public class JavaDemo {
	public static void main(String args[]){
		int x = 2;
		System.out.println(x << 2);
		System.out.println(x); 
	}
}
```
x == 00000000 00000000 00000000 00000010 == 2

x<<2 == 00000000 00000000 00000000 00001000 == 8

&和&&，|和||的区别：
* &和|两个运算符可进行位运算和逻辑运算
	* 在进行逻辑运算时，所有判断条件都要执行；
	* 在进行位运算时，只是针对于当前数据进行与和或处理；
* &&和||用于逻辑运算
	* &&: 在若干个条件判断的时候，若前面的条件返回了 false，后续所有条件不再执行，最终返回 false；
	* ||: 在若干个条件判断的时候，若前面的条件返回了 true，后续所有条件不再执行，最终返回 true；
