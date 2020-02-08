# 1 JAVA基本概念

```java
public class Hello {
	public static void main(String args[]){
		System.out.println("Hello World !");
	}
}
```

```java
class HelloWorld {
	public static void main(String args[]){
		System.out.print("Hello");
		System.out.println("World !");
		System.out.println("Hello World !");
	}
}
```

## java基本单元：类

“[public] class 类名称{}”：类名称必须与文件名称保持一致，一个*.java文件中只允许有一个public class定义；

“class 类名称{}”：类名称可以与文件名称不一致，但是编译后的*.class名称是class定义的类名称，解析的时候解析的是生成的*.class的文件名称；在一个*.java文件中可以定义多个class，且编译后会形成不同的*.class文件；

提示：关于以后原代码定义问题
* 在以后项目开发中，一个*.java文件只定义一个[public] class就够了；
* 类首字母必须大写


## 主方法

所有程序执行的起点，一定要定义在类中

```java
[public] class 类名称 {
	public static void main(String args[]){
		程序代码由此开始执行;
	}
}
```
主方法所在类成为“主类”，所有主类将用public class定义


屏幕打印（系统输出）：可直接在命令行方式下进行内容的显示
* 输出之后追加换行：System.out.println(输出内容);
* 输出之后不追加换行：System.out.print(输出内容);（ln==line==换行）


## JShell

只需编写核心结构代码

定义程序文件abc.txt:
```java
System.out.println("Hello A!");
System.out.println("www.sougou.com");
```
运行文件，jshell中输入：/open d:/Java/javacode/abc.txt

退出：/exit


## CLASSPATH

自动通过CLASSPATH所设路径进行类的加载

默认设置：SET CLASSPATH=.

PATH:操作系统提供的路经配置，定义所有可执行程序的路径；

CLASSPATH：由JRE提供，用于定义Java程序集程序解释时类加载路径，可使用“SET CLASSPATH=路径”的形式设置


## 注释
* 单行注释：//；
* 多行注释：/* ... */;
* 文档注释：/**   */,其中有较多选项，建议通过开发工具控制；


## 标识符与关键字

关键字50个

![JAVA关键字](https://github.com/JCancy/JAVA/blob/master/picture/%E5%85%B3%E9%94%AE%E5%AD%97.PNG)

JDK1.4出现assert关键字，用于异常处理；

JDK1.5出现enum关键字，用于枚举；

未使用到的关键字：goto，const;

特殊含义单词：true，false，null；
