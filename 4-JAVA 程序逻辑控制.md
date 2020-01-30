# JAVA 程序逻辑控制

程序开发过程中一共会存在3种程序逻辑：顺序结构/分支结构/循环结构

## IF 分支结构

主要根据关系表达式进行判断处理的分支操作
```
if (布尔表达式) {
	条件满足时执行；
} else if (布尔表达式) {
	条件满足时执行；
} else if (布尔表达式) {
	条件满足时执行；
} [else {
	条件满足时执行；
}]
```
if 最大特点在于可以实现多条件判断


## SWITCH 分支语句

SWITCH 只能判断数据（int, char, 枚举, String）而不能使用逻辑判断
```
switch(数据){
	case 数值 :{
		数值满足时执行;
		[break;]
	}
	case 数值 :{
		数值满足时执行;
		[break;]
	}
	[default:
		所有判断数值不满足时执行;
		[break;]
	]
}
```
重点在 break 语句，若不加 break，则继续执行后续语句直到程序结束或遇见 break。
```
public class JavaDemo {
	public static void main(String args[]){
		int x = 2;
		switch(x){
			case 2 :{
				System.out.println("结果为2");
				break;
			}
			case 3 :{
				System.out.println("结果为3");
				break;
			}
			default:
				System.out.println("No Match!");
				break;
		} 
	}
}
```


## while 循环

while 循环
```
while (布尔表达式){
	条件满足时执行;
	修改循环条件;
}
```
数字累加：
```
public class JavaDemo {
	public static void main(String args[]){
		int sum = 0; //保存最终计算总和
		int num = 1; //进行循环控制
		while (num <= 100) { //循环的执行条件
			sum += num ; //累加
			num ++;  //修改循环条件
		}
		System.out.println(sum);
	}
}
```
do...while 循环
```
do{
	条件满足时执行;
	修改循环条件;
} while(布尔表达式);
```
数字累加：
```
public class JavaDemo {
	public static void main(String args[]){
		int sum = 0; //保存最终计算总和
		int num = 1; //进行循环控制
		do { //循环的执行条件
			sum += num ; //累加
			num ++;  //修改循环条件
		} while (num <= 100) ;
		System.out.println(sum);
	}
}
```

while 循环与 do...while 循环的最大差别：

while 循环先判断后执行，do...while 先执行一次后判断。


## for 循环

```
for (定义循环初始化数值；循环判断；修改循环数据){
	循环语句的执行;
}
```
数值累加：
```
public class JavaDemo {
	public static void main(String args[]){
		int sum = 0; //保存最终计算总和
		for (int x=1; x<=100; x++){
			sum += x ;
		}
		System.out.println(sum);
	}
}
```

对于 while 和 for 循环的选择参考标准：
* 在明确确定循环次数的情况下优先选择 for循环；
* 在不知道循环次数，但是直到循环结束条件的情况下使用 while循环


## 循环控制
两个控制语句：break, continue
* break 退出整个循环语句
* continue 结束当前的一次调用


## 循环嵌套
打印乘法口诀表：
```
public class JavaDemo {
	public static void main(String args[]){
		for (int x=1; x<=9; x++){
			for (int y=1; y<=x; y++) {
				System.out.print(y+"*"+x+"="+(x*y)+"\t");
			}
			System.out.println(); //换行
		}
	}
}
```

打印三角形：
```
public class JavaDemo {
	public static void main(String args[]){
		int line=5;
		for (int x=0; x<line; x++){
			for (int y=0; y<line-x; y++) {
				System.out.print(" ");
			}
			for (int y=0; y<=x; y++) {
				System.out.print("* ");
			}
			System.out.println(); //换行
		}
	}
}
```
