# 2 JAVA数据类型划分

![JAVA基本数据类型](https://github.com/JCancy/JAVA/blob/master/picture/%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B.PNG)

1. 基本数据类型：描述具体的数字单元，如1，1.1；
* 数值型：
	* 整型：byte,short,int,long; (默认值：0)
	* 浮点型：float，double;     (默认值：0.0)
* 布尔型：boolean;               (默认值：false)
* 字符型：char;                  (默认值：'\u0000')
2. 引用数据类型：涉及到内存关系的使用；
* 数组，类，接口                 (默认值：null)

参考原则：
* 描述数字首选 int（整数）, double（小数）；
* 数据传输或文字编码转换使用 byte 类型（二进制处理操作）；
* 处理中文最便捷的操作是使用 char （可选）；
* 描述内存或文件大小，描述表的主键列（自动增长）可使用 long;


## 整型数据
由小到大 byte，short，int，long；
```
public class JavaDemo {
	public static void main(String args[]){
		//int 变量名称=常量（10是常量，整数类型为int）；
		int x=10; //定义了一个整型变量x
		//int型变量*int型变量=int型数据
		System.out.println(x*x);
	}
}
```

```
public class JavaDemo {
	public static void main(String args[]){
		int max=Integer.MAX_VALUE; //获取int最大值
		int min=Integer.MIN_VALUE; //获取int最小值
		System.out.println(max);   //2147483647
		System.out.println(min);   //-2147483648
		// int型变量+int型常量=int型结果
		System.out.println(max+1);   //-2147483648, 最大值+1=最小值，出现循环，即数据溢出
	}
}
```
加入注释若提示编码错误，可设置文件编码类型为GBK

解决数据溢出：
* 预估数据范围，不够则扩大；
* 常量后+L 或变量前加（long）进行转换：max+1L,(long)max+1
* byte 字符做了特殊处理，以byte存整型，不超过范围不转换，超过则转换


## 浮点型数据
```
public class JavaDemo {
	public static void main(String args[]){
		double x=10.2;
		int y=10;
		//double*int=double
		double result=x*y;
		System.out.println(result);
	}
}
```
所有数据类型进行自动转换是均由小类型向大类型进行自动转换处理

```
public class JavaDemo {
	public static void main(String args[]){
		float x=(float)10.2; //转换double为float
		float y=4F;          //转换double为float
		System.out.println(x*y); //40.80004
	}
}
```
正确结果应为40.8，小数点后0004为JAVA float 存在的bug

```
public class JavaDemo {
	public static void main(String args[]){
		int x=10;
		int y=4;
		System.out.println(x/y); //2
		System.out.println((double)x/y); //2.5
	}
}
```
数据类型将直接决定小数点是否存在


## 字符型数据：char,''
```
public class JavaDemo {
	public static void main(String args[]){
		char c='B'; //字符变量
		int num=c; //可获得字符编码
		char d='我';
		System.out.println(c); //
		System.out.println(num); 
		num=num+32; //大写转换为小写
		System.out.println((char)num);
		System.out.println(d);
		num=d;
		System.out.println(num);
	}
}
```
char 可处理中文，使用Unicode编码；

char 与 int 可相互转换

编码范围：
* 大写字母：'A'(65)-'Z'(90);
* 小写字母：'a'(97)-'z'(122);
* 数字：'0'(48)-'9'(57);

大小写相差32个数字，可进行大小写转换


## 布尔型：boolean
```
public class JavaDemo {
	public static void main(String args[]){
		boolean flag=true;
		if (flag) { //判断flag的内容，若true则执行
			System.out.println("哈哈哈"); 
		}
	}
}
```
取值只有 true, false; 不用0和非0代替
 

## 字符串：String,""
```
public class JavaDemo {
	public static void main(String args[]){
		String str="Hello";
		str = str + " World";
		str += "!!!"
		System.out.println(str);
	}
}
```
字符串可使用"+"进行连接

```
public class JavaDemo {
	public static void main(String args[]){
		double x=10.1;
		int y=20;
		String str="计算结果：" + x + y;
		System.out.println(str);
		str="计算结果：" + (x + y);
		System.out.println(str);
		System.out.println("\tHello World !!!\nHello\"JAVA\"!!!");
	}
}
```
数据范围大的和数据范围小的类进行计算，自动转换为数据范围大的；

若有 String 字符串，所有类型转换为 String;

描述字符串是使用转义字符进行处理，如TAB(\t),"(\"),'(\')
