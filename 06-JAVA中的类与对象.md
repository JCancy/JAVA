# JAVA中的类与对象

JAVA语言最大特点在于面向对象的编程设计

面向对象的三个主要特征：
* 封装性：内部操作对外部不可见；
* 继承性：在已有结构的基础上继续进行功能的扩充；
* 多态性：在继承性的基础上扩展而来，指类型的转换处理。

面向对象程序开发中的三个步骤：
* OOA：面向对象分析；
* OOD：面向对象设计；
* OOP：面向对象编程。

## 类与对象简介

类是对某一类事物的共性的抽象概念，对象描述的是一个具体的产物。

类是一个模板，对象是一个实例。先有类再有对象。

类中一般有两个组成：
* 成员属性(Field):有时为简化称其为属性；
* 操作方法(Method):定义对象具有的处理行为。


## 类与对象定义

JAVA 中类是一个独立结构体，需使用 class 进行定义，类中主要由属性和方法组成。

属性是一个个具体的变量，方法是可以重复执行的代码。


类中有两个属性（name,age）和一个方法（tell()）。有类后，想使用类需使用对象来完成。

若产生对象，需使用如下语法格式：
* 声明并实例化对象：类名称 对象名称 = new 类名称();
* 分步骤完成：
	* 声明对象：类名称 对象名称 = null;
	* 实例化对象：对象名称 = new 类名称()。

当获取了实例化对象之后，需要通过对象进行类中的操作调用，此时有两种操作方式：
* 调用类中的属性：实例化对象.成员属性；
* 调用类中的方法：实例化对象.方法名称()。

使用对象操作类：
```java
class Person { //定义一个类
	String name ; //人员姓名
	int age ;  //人员年龄
	public void tell(){
		System.out.println("姓名："+name+"、年龄："+age);
	}
}
public class JavaDemo {
	public static void main(String args[]){
		Person per = new Person(); //声明并实例化对象
		per.name = "张三";
		per.age = 18;
		per.tell(); //进行方法的调用
	}
}
```
若属性未进行定义，则使用默认值



## 对象内存分析

Java 之中类属于引用数据类型，最困难之处在于进行内存管理。

最常用内存空间：
* 堆内存：保存对象的具体信息，在程序中堆内存空间的开辟是通过 new 完成的；
* 栈内存：保存的是一块堆内存的地址，即通过地址找到堆内存，而后找到对象内容。可简单理解为，对象的名称保存在了栈内存之中。


```java
public class JavaDemo {
	public static void main(String args[]){
		Person per = new Person(); //声明并实例化对象
		per.name = "张三";
		per.age = 18;
		per.tell(); //进行方法的调用
	}
}
```

![声明并实例化对象内存分析](https://github.com/JCancy/JAVA/blob/master/picture/%E5%86%85%E5%AD%98%E5%88%86%E6%9E%90.PNG)


```java
public class JavaDemo {
	public static void main(String args[]){
		Person per = null; //声明并实例化对象
		per = new Person();
		per.name = "张三";
		per.age = 18;
		per.tell(); //进行方法的调用
	}
}
```

![分步骤完成内存分析](https://github.com/JCancy/JAVA/blob/master/picture/%E5%86%85%E5%AD%98%E5%88%86%E6%9E%902.PNG)


注意：所有对象在调用类中的属性或方法时，必须要实例化完成后才可执行。

错误代码：
```java
public class JavaDemo {
	public static void main(String args[]){
		Person per = null; //声明并实例化对象
		per.name = "张三";
		per.age = 18;
		per.tell(); //进行方法的调用
	}
}
```
未实例化对象（没有开辟新的堆内存），出现“NullPointerException”错误。


## 对象引用分析
 
引用数据类型涉及内存的引用传递，即同一块堆内存空间可以被不同的栈内存所指向，也可以更换指向。

```java
public class JavaDemo {
	public static void main(String args[]){
		Person per1 = null; //声明并实例化对象
		per1 = new Person();
		per1.name = "张三";
		per1.age = 18;
		Person per2 = per1; //引用传递
		per2.age = 80;
		per1.tell(); //进行方法的调用
	}
}
```

![主方法引用传递内存分析](https://github.com/JCancy/JAVA/blob/master/picture/%E5%86%85%E5%AD%98%E5%88%86%E6%9E%903.PNG)

此时的引用传递直接在主方法中进行定义，也可以通过方法实现引用传递。

```java
public class JavaDemo {
	public static void main(String args[]){
		Person per = new Person(); //声明并实例化对象
		per.name = "张三";
		per.age = 18;
		change(per) ; // 等价于：Person temp = per;
		per.tell(); //进行方法的调用
	}
	public static void change(Person temp) {
		temp.age = 80;
	}
}
```

![方法引用传递内存分析](https://github.com/JCancy/JAVA/blob/master/picture/%E5%86%85%E5%AD%98%E5%88%86%E6%9E%904.PNG)

引用可以发生在方法上，此时一定要观察方法的参数类型，同时观察方法的执行过程。


##引用与垃圾产生分析

对于引用传递处理不当，将有垃圾产生。
```java
public class JavaDemo {
	public static void main(String args[]){
		Person per1 = new Person(); //声明并实例化对象
		Person per2 = new Person();
		per1.name = "张三";
		per1.age = 18;
		per2.name = "李四";
		per2.age = 19;
		per2 = per1 ; // 引用传递
		per2.age = 80;
		per.tell(); //进行方法的调用
	}
}
```

![垃圾产生内存分析](https://github.com/JCancy/JAVA/blob/master/picture/%E5%86%85%E5%AD%98%E5%88%86%E6%9E%905.PNG)

垃圾空间：没有任何栈内存指向的堆内存空间。

所有垃圾将被GC(Garbage Collector)定期进行回收并释放无用内存空间，但垃圾过多必将影响GC的处理性能，从而降低整体的程序性能。

在实际开发中，产生的垃圾空间越少越好。

一个栈内存只能保存一个堆内存的地址数据，若发生更改，则之前的地址数据将从此栈内存中彻底消失。



## 成员属性封装

类的组成为属性和方法：
* 方法对外提供服务，通常不会封装；
* 属性需要较高安全性，往往需要对其进行保护，此时需要对其进行封装。

在默认情况下，对于类中的属性可通过其他类利用对象进行调用。
```java
class Person { //定义一个类
	String name ; //人员姓名
	int age ;  //人员年龄
	public void tell(){
		System.out.println("姓名："+name+"、年龄："+age);
	}
}
public class JavaDemo {
	public static void main(String args[]){
		Person per = new Person(); //声明并实例化对象
		per.name = "张三";
		per.age = -18;
		per.tell(); //进行方法的调用
	}
}
```
未封装属性可在外部直接调用，但赋值可能为错误值。

可使用 private 关键字对属性进行封装处理。
```java
class Person { //定义一个类
	private String name ; //人员姓名
	private int age ;  //人员年龄
	public void tell(){
		System.out.println("姓名："+name+"、年龄："+age);
	}
}
public class JavaDemo {
	public static void main(String args[]){
		Person per = new Person(); //声明并实例化对象
		per.name = "张三";
		per.age = -18;
		per.tell(); //进行方法的调用
	}
}
```
属性封装后，外部不能直接访问。即，对外部不可见，但对类的内部是可见的。

若想在外部访问封装的属性，在Java开发标准中有如下要求：
* 【setter/ getter】设置或取得属性可以使用setXxx()\getXxx()方法
	* 设置属性方法：public void setName(String n);
	* 获取属性方法：public String getName();
```java
class Person { //定义一个类
	private String name ; //人员姓名
	private int age ;  //人员年龄
	public void tell(){
		System.out.println("姓名："+name+"、年龄："+age);
	}
	public void setName(String n) {
		name = n;
	}
	public String getName() {
		return name;
	}
	public void setAge(int a) {
		if (a>=0){
			age = a;
		}
	}
	public int getAge() {
		return age;
	}
}
public class JavaDemo {
	public static void main(String args[]){
		Person per = new Person(); //声明并实例化对象
		per.setName("张三");
		per.setAge(-18);
		per.tell(); //进行方法的调用
	}
}
```
注：以后进行任何类的定义时，类中所有属性都必须使用 private 进行封装,对属性进行访问需提供 setter和 getter方法。



## 构造方法与匿名对象

程序在使用类时一般按照如下步骤进行：
* 声明并实例化对象，此时实例化对象中的属性并没有任何数据，都是对应类型的默认值；
* 需通过setter方法为类中的属性设置内容。

即，要想获得可以正常使用的实例化对象，必须经过两个步骤。

```java
public class JavaDemo {
	public static void main(String args[]){
		// 1.对象初始化准备
		Person per = new Person(); //声明并实例化对象
		per.setName("张三");
		per.setAge(-18);
		// 2.对象的使用
		per.tell(); //进行方法的调用
	}
}
```

可通过构造方法实现实例化对象中属性初始化处理，即只有在 new 时使用构造方法。

Java 中构造方法定义如下：
* 方法名称必须与类名称保持一致；
* 构造方法不允许设置任何返回值类型，即：没有返回值定义；
* 构造方法是在使用关键字 new 实例化对象的时候自动调用的。


定义构造方法：

```java
class Person { //定义一个类
	private String name ; //人员姓名
	private int age ;  //人员年龄
	// 方法名称与类名称相同，并且无返回值定义
	public Person(String n, int a) { //定义有参构造
		name = n; //为类中的属性赋值（初始化）
		age = a; //为类中的属性赋值（初始化）
	}
	public void tell(){
		System.out.println("姓名："+name+"、年龄："+age);
	}
	public void setName(String n) {
		name = n;
	}
	public String getName() {
		return name;
	}
	public void setAge(int a) {
		if (a>=0){
			age = a;
		}
	}
	public int getAge() {
		return age;
	}
}
public class JavaDemo {
	public static void main(String args[]){
		Person per = new Person("张三",18); //声明并实例化对象
		per.tell(); //进行方法的调用
	}
}
```

对比分析：

Person per = new Person();

Person per = new Person("张三",18);

   1    2     3    4  :
* 1：Person 主要定义对象所属类型，类型决定了可以调用的方法；
* 2：per 实例化对象的名称，所有操作通过对象进行访问；
* 3：new 开辟一块新的堆内存空间；
* 4：Person("张三"，18) 调用有参构造，Person() 调用无参构造

Java 程序里考虑到程序结构的完整性，多有类都会提供有构造方法。

若类中没有定义任何构造方法，则会有默认的无参构造方法，该方法在程序编译时自动创建。

若已明确定义构造方法，则默认构造方法不会被创建。

结论：一个类至少存在一个构造方法。

注：构造方法不允许返回值，但不加 void，否则与普通方法结构一致。

在进行多个构造方法定义时，建议按照参数个数升序或降序进行排列。

构造方法与 setter：
* 构造方法是在初始化对象时对属性进行设置；
* setter除了设置数据的功能外，还有修改数据的功能


利用构造方法可以传递属性数据，进一步分析对象的产生格式：
* 定义对象名称： 类名称 对象名称 = null;
* 实例化对象：对象名称 = new 类名称()
此时只通过实例化对象名称进行类操作也可以，这种形式的对象由于没有名字，将其称为匿名对象。
```java
public class JavaDemo {
	public static void main(String args[]){
		new Person("张三",18).tell(); //进行方法的调用
	}
}
```
此时通过对象进行了类中tell()方法的调用，但由于此对象没有任何引用名称，所以该对象使用一次后就将成为垃圾，所有垃圾将被GC进行回收与释放。

构造方法内存分析：
```java
class Message {
	private String title;
	public Message(String t) {
		title = t;
	}
	public String getTitle() {
		return title;
	}
	public void setTitle(String t) { //具有修改功能 
		title = t;
	}
}
class Person { //定义一个类
	private String name ; //人员姓名
	private int age ;  //人员年龄
	// 方法名称与类名称相同，并且无返回值定义
	public Person(Message msg, int a) { //定义有参构造
		name = msg.getTitle(); //为类中的属性赋值（初始化）
		age = a; //为类中的属性赋值（初始化）
	}
	public Message getInfo(){
		return new Message(name+":"+age) ;
	}
	public void tell(){
		System.out.println("姓名："+name+"、年龄："+age);
	}
}
public class JavaDemo {
	public static void main(String args[]){
		Message msg = new Message("mldn");
		Person per = new Person(msg,20); 
		msg = per.getInfo();
		System.out.println(msg.getTitle());
	}
}
```

![构造方法内存分析](https://github.com/JCancy/JAVA/blob/master/picture/%E5%86%85%E5%AD%98%E5%88%86%E6%9E%906.PNG)

只要是都可以传递任意的数据类型（基本数据类型，引用数据类型）







