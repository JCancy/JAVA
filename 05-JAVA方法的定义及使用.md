# JAVA方法的定义及使用

## 方法（method）的定义

方法在主类中定义，并且由主方法直接调用
```java
public static 返回值类型 方法名称（[参数变量 变量，……]）{
	// 该方法要执行的代码
	[return [返回值];]
}
```
若要返回数据，使用return描述，return返回的数据类型与方法的返回值类型一致，可为基本数据类型或引用数据类型

若不返回数据，则用void进行声明

方法名称与变量的定义命名要求：

第一个单词字母小写，之后每个单词的首字母大写

定义无参无返回值的方法：
```java
public class JavaDemo {
	public static void main(String args[]){
		printMassage();
		printMassage();	
	}
	public static void printMassage(){
		System.out.println("*****");
		System.out.println("** **");
		System.out.println("*****");
	}
}
```

定义有参有返回值的方法：
```java
public class JavaDemo {
	public static void main(String args[]){
		String result = get(20.0);
		System.out.println(result);
		System.out.println(get(1.0)); //返回值可直接输出
	}
	public static String get(double jinbi){
		if (jinbi >= 10.0) {
			return "剩余"+(jinbi-10.0);
		} else {
			return "不足";
		}
	}
}
```
执行方法时，若返回值类型为void，则可利用return结束调用。


## 方法重载

方法名称相同，参数的类型或个数不同的时候称为方法重载
```java
public class JavaDemo {
	public static void main(String args[]){
		int resultA = sum(10,20);
		int resultB = sum(10,20,30);
		double resultC = sum(10.2,20.3);
		System.out.println(resultA);
		System.out.println(resultB);
		System.out.println(resultC);

	}
	public static int sum(int x, int y){
		return x + y  ;
	}
	public static int sum(int x, int y, int z){
		return x + y + z ;
	}
	public static double sum(double x, double y){
		return x + y  ;
	}
}
```
注：方法重载与方法的返回值无关，只和参数有关。开发时应尽量保持返回值类型相同。


## 方法递归调用

需考虑问题：
* 一定要设置方法递归结束条件；
* 每一次调用过程中一定要修改传递的参数条件；

例：计算“1+2+……+100”
```java
public class JavaDemo {
	public static void main(String args[]){
		System.out.println(sum(100));

	}
	public static int sum(int num){ //执行累加
		if (num==1) {  //停止累加
			return 1 ;
		}
		return num + sum(num-1)  ; //递归调用
	}
}
```

例：计算“1！+ 2！+ …… + 90！”
```java
public class JavaDemo {
	public static void main(String args[]){
		System.out.println(sum(90));

	}
	public static double sum(int num){ //执行累加
		if (num==1) {  //停止累加
			return 1 ;
		}
		return fan(num) + sum(num-1)  ; //递归调用
	}
	public static double fan(int num){ //执行
		if (num==1) {  
			return 1 ;
		}
		return num * fan(num-1)  ; //递归调用
	}
}
```

