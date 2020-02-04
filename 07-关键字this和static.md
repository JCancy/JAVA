# 关键字

## this 关键字

this 方法可实现三类结构的描述：
* 当前类中的属性： this.属性；
* 当前类中的方法（普通方法，构造方法）：this, this.方法名称();
* 描述当前对象；

使用this调用当前类中属性
```
class Person { //定义一个类
	private String name ; //人员姓名
	private int age ;  //人员年龄
	// 方法名称与类名称相同，并且无返回值定义
	public Person(String name, int age) { //定义有参构造
		this.name = name; //为类中的属性赋值（初始化）
		this.age = age; //为类中的属性赋值（初始化）
	}
	public void tell(){
		System.out.println("姓名："+this.name+"、年龄："+this.age);
	}
}
public class JavaDemo {
	public static void main(String args[]){
		Person per = new Person("张三",20); 
		per.tell();
	}
} 
```
以后访问类中属性时，加上“this”实现访问。


使用this调用方法

对于方法的调用需考虑构造方法和普通方法：
* 构造方法调用（this()）：使用关键字 new 实例化对象时才调用构造方法；
* 普通方法调用（this.方法名称()）：实例化对象产生之后就可以调用普通方法。

调用类中的普通方法
```
class Person { //定义一个类
	private String name ; //人员姓名
	private int age ;  //人员年龄
	// 方法名称与类名称相同，并且无返回值定义
	public Person(String name, int age) { //定义有参构造
		this.setName(name); //
		setAge(age); // 加与不加都表示类方法
	}
	public void tell(){
		System.out.println("姓名："+this.name+"、年龄："+this.age);
	}
	public void setName(String name) {
		this.name = name;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getName() {
		return this.name;
	}
	public int getAge() {
		return this.age;
	}
}
public class JavaDemo {
	public static void main(String args[]){
		Person per = new Person("张三",20); 
		per.tell();
	}
} 
```

构造方法的调用（传统做法）：
```
class Person { //定义一个类
	private String name ; //人员姓名
	private int age ;  //人员年龄
	public Person(){
		System.out.println("一个新的Person类对象实例化");
	}
	public Person(String name){
		System.out.println("一个新的Person类对象实例化");
		this.name = name;
	}
	public Person(String name, int age) { //定义有参构造
		System.out.println("一个新的Person类对象实例化");
		this.name = name;
		this.age = age;
	}
	public void tell(){
		System.out.println("姓名："+this.name+"、年龄："+this.age);
	}
	public void setName(String name) {
		this.name = name;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getName() {
		return this.name;
	}
	public int getAge() {
		return this.age;
	}
}
public class JavaDemo {
	public static void main(String args[]){
		Person per = new Person("张三",20); 
		per.tell();
	}
} 
```

评价代码好坏：
* 代码结构可以重用，提供的是中间独立的支持；
* 没有重复


利用this构造调用优化：
```
class Person { //定义一个类
	private String name ; //人员姓名
	private int age ;  //人员年龄
	public Person(){
		System.out.println("一个新的Person类对象实例化");
	}
	public Person(String name){
		this(); //调用本类无参构造
		this.name = name;
	}
	public Person(String name, int age) { //定义有参构造
		this(name); //调用本类单参构造
		this.age = age;
	}
	public void tell(){
		System.out.println("姓名："+this.name+"、年龄："+this.age);
	}
	public void setName(String name) {
		this.name = name;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getName() {
		return this.name;
	}
	public int getAge() {
		return this.age;
	}
}
public class JavaDemo {
	public static void main(String args[]){
		Person per = new Person("张三",20); 
		per.tell();
	}
} 
```

调用需要注意以下几点重要问题：
* 构造方法必须在实例化新的想的时候调用，所以"this()"的语句只允许放在构造方法的首行；
* 构造方法互相调用时需保留有程序的出口，避免出现死循环。

构造方法互相调用案例：
```
class Emp {
	private long empno; //员工编号
	private String ename;//员工姓名
	private String dept;//部门名称
	private double salary;//基本工资
	public Emp(){
		this.empno=1000;
		this.ename="无名氏";
	}
	public Emp(long empno){
		this.empno=empno;
		this.ename="新员工";
		this.dept="未定";
	}
	public Emp(long empno,String ename,String dept){
		this.empno=empno;
		this.ename=ename;
		this.dept=dept;
		this.salary=2500.00;
	}
	public Emp(long empno,String ename,String dept,double salary){
		this.empno=empno;
		this.ename=ename;
		this.dept=dept;
		this.salary=salary;
	}

	//setter,getter略
	public String getInfo(){
		return "雇员编号："+this.empno+
				"、雇员姓名："+this.ename+
				"、所在部门："+this.dept+
				"、基本工资："+this.salary;
	}

}
public class JavaDemo {
	public static void main(String args[]){
		Emp emp = new Emp(7369L,"史密斯","财务部",6500.00);
		System.out.println(emp.getInfo());
	}
} 
```

此时代码有重复，可进行简化定义：
```
class Emp {
	private long empno; //员工编号
	private String ename;//员工姓名
	private String dept;//部门名称
	private double salary;//基本工资
	public Emp(){
		this(1000,"无名氏",null,0.0); //调用四参构造
	}
	public Emp(long empno){
		this(empno,"新员工","未定",0.0);
	}
	public Emp(long empno,String ename,String dept){
		this(empno,ename,dept,2500.00);
	}
	public Emp(long empno,String ename,String dept,double salary){
		this.empno=empno;
		this.ename=ename;
		this.dept=dept;
		this.salary=salary;
	}

	//setter,getter略
	public String getInfo(){
		return "雇员编号："+this.empno+
				"、雇员姓名："+this.ename+
				"、所在部门："+this.dept+
				"、基本工资："+this.salary;
	}

}
public class JavaDemo {
	public static void main(String args[]){
		Emp emp = new Emp(7369L,"史密斯","财务部",6500.00);
		System.out.println(emp.getInfo());
	}
} 
```

代码的任何位置都可能有重复，所以消除重复的代码时先期学习中最需要考虑的部分。


## 简单JAVA类

简单Java类指可以描述一类信息的程序类，如：描述一个人/一本书，并且在该类中没有特别复杂的逻辑操作，只作为信息存储的媒介。

简单JAVA类的核心开发结构如下：
* 类名称一定要有意义，可以明确描述某一类事物；
* 类之中所有属性都必须使用 private 进行封装，同时封装后的属性必须提供有setter,getter方法；
* 类之中可以提供无数多个构造方法，但必须保留有无参构造方法；
* 类之中不允许出现任何输出语句，所有内容的获取必须返回；
* 【非必须】可以提供一个获取对象详细信息的方法，暂时将此方法名称定义为getInfo();

构造一个简单类：
```
class Dept { //类名称可以明确描述出某类事物
	private long deptno;
	private String dname;
	private String loc;
	public Dept() {} //必须提供无参构造方法
	public Dept(long deptno, String dname, String loc) {
		this.deptno = deptno;
		this.dname = dname;
		this.loc = loc;
	}
	public String getInfo(){
		return "【部门信息】部门编号："+this.deptno+"、部门名称："+this.dname+"、部门位置："+this.loc;
	}
	public void setDeptno(long deptno) {
		this.deptno = deptno;	
	}
	public void setDname(String dname) {
		this.dname = dname;
	}
	public void setLoc(String loc) {
		this.loc = loc;
	}
	public long getDeptno(){
		return this.deptno;
	}
	public String getDname(){
		return this.dname;
	}
	public String getLoc(){
		return this.loc;
	}
}
public class JavaDemo {
	public static void main(String args[]){
		Dept dept = new Dept(10,"技术部","北京");
		System.out.println(dept.getInfo());
	}
}
```
简单Java类融合了：数据类型划分，类的定义，private 封装，构造方法，方法定义，对象实例化。



## static 关键字

static 可以定义属性和方法。

static 定义属性

```
class Person{
	private String name;
	private int age;
	String country = "中华民国"; //国家，暂时不封装
	public Person(String name,int age){
		this.name = name;
		this.age = age;
	}
	//setter,getter省略
	public String getInfo(){
		return "姓名：" + this.name + "、年龄：" + this.age + "、国家：" + this.country;

	}
}
public class JavaDemo {
	public static void main(String args[]){
		Person perA = new Person("张三",10);
		Person perB = new Person("李四",10);
		Person perC = new Person("王五",11);
		System.out.println(perA.getInfo());
		System.out.println(perB.getInfo());
		System.out.println(perC.getInfo());
	}
}
```

若只修改一个对象的国家属性，则其他对象国家属性不变，需依次修改。

此时可将国家属性修改为公共属性，使用static进行标注：

```
class Person{
	private String name;
	private int age;
	static String country = "中华民国"; //用static进行标注
	public Person(String name,int age){
		this.name = name;
		this.age = age;
	}
	//setter,getter省略
	public String getInfo(){
		return "姓名：" + this.name + "、年龄：" + this.age + "、国家：" + this.country;

	}
}
public class JavaDemo {
	public static void main(String args[]){
		Person perA = new Person("张三",10);
		Person perB = new Person("李四",10);
		Person perC = new Person("王五",11);
		perA.country = "中华人民共和国"; //此时所有对象的 country 数据均会更改
		System.out.println(perA.getInfo());
		System.out.println(perB.getInfo());
		System.out.println(perC.getInfo());
	}
}
```

对于 static 属性，由于其本身是公共属性，虽然可通过对象进行访问，但最好通过所有对象的最高代表（类）进行访问。

因此， static 属性可以由类名称直接调用。

```
class Person{
	private String name;
	private int age;
	static String country = "中华民国"; //用static进行标注
	public Person(String name,int age){
		this.name = name;
		this.age = age;
	}
	//setter,getter省略
	public String getInfo(){
		return "姓名：" + this.name + "、年龄：" + this.age + "、国家：" + this.country;

	}
}
public class JavaDemo {
	public static void main(String args[]){
		Person perA = new Person("张三",10);
		Person perB = new Person("李四",10);
		Person perC = new Person("王五",11);
		Person.country = "中华人民共和国"; //由类名称直接调用
		System.out.println(perA.getInfo());
		System.out.println(perB.getInfo());
		System.out.println(perC.getInfo());
	}
}
```

![static内存分析](https://github.com/JCancy/JAVA/blob/master/picture/%E5%86%85%E5%AD%98%E5%88%86%E6%9E%907.PNG)


static 属性虽然定义在类之中，但是并不受到实例化对象的控制。

static 属性可以在没有实例化对象的时候使用。


不产生实例化对象调用 static 属性：
```
class Person{
	private String name;
	private int age;
	static String country = "中华民国"; //用static进行标注
	public Person(String name,int age){
		this.name = name;
		this.age = age;
	}
	//setter,getter省略
	public String getInfo(){
		return "姓名：" + this.name + "、年龄：" + this.age + "、国家：" + this.country;

	}
}
public class JavaDemo {
	public static void main(String args[]){
		System.out.println(Person.country);
		Person.country = "中华人民共和国"; //由类名称直接调用
		Person per = new Person("张三"，20);
		System.out.println(per.getInfo());
	}
}
```

注：
* 进行类设计时，首选一定为非 static 属性，考虑到公共信息存储时才会使用到 static.
* 非 static 属性必须在实例化对象产生之后才可以使用，而 static 属性可以在没有实例化对象产生的情况下直接通过类名称进行调用。


定义 static 方法：
```
class Person{
	private String name;
	private int age;
	private static String country = "中华民国"; //用static进行标注
	public Person(String name,int age){
		this.name = name;
		this.age = age;
	}
	public static void setCountry(String c) { //static方法
		country = c;  //不能加this，country是static属性
	}
	//setter,getter省略
	public String getInfo(){
		return "姓名：" + this.name + "、年龄：" + this.age + "、国家：" + this.country;

	}
}
public class JavaDemo {
	public static void main(String args[]){
		Person.setCountry("中华人民共和国");
		Person per = new Person("张三",20);
		System.out.println(per.getInfo());
	}
}
```

此时对于程序而言方法有两种：static 方法，非 static 方法。两个方法之间的调用有限制：
* static 方法只允许调用 static 属性或 static 方法；
* 非 static 方法允许调用 static 属性或 static 方法；

所有的static定义的属性和方法都可以在没有实例化对象的前提下使用，而所有的非static属性和方法必须在有实例化对象的情况下才可以使用。

不能使用this:this 为当前对象，但 static 可能没有实例化对象。

```
public class JAvaDemo {
	public static void main(String args[]) {
		new JavaDemo().print(); //在主类中实例化对象
	}
	public void print() {
		System.out.println("www.github.com");
	}
}
```
static 定义的方法或属性都不是代码编写之初所需考虑的内容，只有在回避实例化对象调用并且描述公共属性的情况下，才会考虑 static 定义的方法或属性。


static 的应用

例: 编写程序类，实现实例化对象个数的统计

可以单独创建一个 static 属性，因为所有对象共享一个 static 属性，在构造方法中可实现数据的统计处理

```
class Book {
	private String title ;
	private static int count = 0 ;
	public Book(String title){
		this.title = title ;
		count ++ ;
		System.out.println("第" + count + "本新的图书创建出来。") ;
	}
}
public class JavaDemo{
	public static void main(String args[]) {
		new Book("Java");
		new Book("Python");
		new Book("C++");
	}
}
```

实现自动命名：
```
class Book {
	private String title ;
	private static int count = 0 ;
	public Book() {
		this("NOTITLE - " + count ++) ;
	}
	public Book(String title){
		this.title = title ;
	}
	public String getTitle() {
		return this.title ;
	}
}
public class JavaDemo{
	public static void main(String args[]) {
		System.out.println(new Book("Java").getTitle());
		System.out.println(new Book("Python").getTitle());
		System.out.println(new Book("LSE").getTitle());
		System.out.println(new Book().getTitle());
		System.out.println(new Book().getTitle());
	}
}
```
可以避免在没有设置title属性为null的重复问题。

