# Object 类

## Object 类的基本概念

Object 可以解决参数的统一问题，使用 Object 类可以接受所有的数据类型。

Java 中只有一个类不存在继承关系，即 Object 类，所有类在默认情况下都是 Object 的子类。

下述两种定义等价：
```java
class Person {} //一个类

class Person extends Object {} //
```

在 Object 类设计时考虑到了继承的问题，故该类提供有无参构造方法。

Object 类是所有类的父类，可以使用 Object 类接收所有子类对象。

```java
class Person {}
public class JavaDemo {
	public static void main(String args[]) {
		Object obj = new Person() ; //向上转型
		if (obj instanceof Person) {
			Person per = (Person) obj ;
			System.out.println("Person对象向下转型") ;
		}
	}
}
```

若有程序方法要求可以接收所有类对象，就可以利用 Object 进行处理。

注：在 JAVA 设计中，对于所有引用数据类型都可以使用 Objext 类进行接收，数组也可以。

使用 Object 类接收数组：
```java
class Person {}
public class JavaDemo {
	public static void main(String args[]) {
		Object obj = new int [] {1,2,3} ; //向上转型
		if (obj instanceof int[]) { //判断是否为整型数组
			int data [] = (int []) obj ; //向下转型
			for (int temp : data) {
				System.out.print(temp + ",") ;
			}
		}
	}
}
```

Object 是万能数据类型，更适合进行程序的标准设计。



## 取得对象信息: toString()

Object 类本身提供一些处理方法。


```java
public String toString() //获取对象完整信息
```

toString 的使用:
```java
class Person {}
public class JavaDemo {
	public static void main(String args[]) {
		Person per = new Person() ;
		System.out.println(per) ;
		System.out.println(per.toString()) ; //Object类继承
	}
}
```
对象信息的获得默认 per = per.toString 。

在以后的开发中，获取对象信息可以直接覆写该方法。

toString 方法覆写：
```java
class Person {
	private String name ;
	private int age ;
	public Person(String name, int age) {
		this.name = name ;
		this.age = age ;
	}
	@Override //覆写
	public String toString() {
		return "姓名：" + this.name + "，年龄：" + this.age ;
	}
}
public class JavaDemo {
	public static void main(String args[]) {
		Person per = new Person("张三", 20) ;
		System.out.println(per) ;
		System.out.println(per.toString()) ; //Object类继承
	}
}
```


## 对象比较: equals()

对象比较是指比较两个对象的内容是否完全相同。

对象比较的基础实现：
```java
class Person {
	private String name ;
	private int age ;
	public Person(String name, int age) {
		this.name = name ;
		this.age = age ;
	}
	@Override //覆写
	public String toString() {
		return "姓名：" + this.name + "，年龄：" + this.age ;
	}
	public String getName() {
		return this.name ;
	}
	public int getAge() {
		return this.age ;
	}
}
public class JavaDemo {
	public static void main(String args[]) {
		Person perA = new Person("张三", 20) ;
		Person perB = new Person("张三", 20) ;
		if (PerA.getName().equals(perB.getName()) && 
			perA.getAge() == perB.getAge()) {
			System.out.println("是同一个对象") ;
		} else {
			System.out.println("不是同一个对象") ;
		}
	}
}
```

上述代码实现了对象比较的功能，但比较麻烦:
* 需要对每一个属性都进行相等判断，在外部要调用大量 getter() 方法；
* 对象比较应该是一个类内部具备的功能，不应该在外部定义。

Object 类作为所有类的父类，提供了对象比较操作的支持，对于对象比较的操作可以使用 equals() 方法完成。

对象比较：
```java
public boolean equals(Object obj) { //可以接受所有类
	return(this == obj) ; //默认情况下只是进行了两个地址的比较
}
```

对于实际使用者，若要正确实现判断处理，就必须在子类中覆写此方法，并进行属性判断。

Object 类中的 equals() 方法覆写：
```java
class Person {
	private String name ;
	private int age ;
	public Person(String name, int age) {
		this.name = name ;
		this.age = age ;
	}
	@Override //覆写
	public String toString() {
		return "姓名：" + this.name + "，年龄：" + this.age ;
	}
	// equals() 方法此时会有两个对象：当前对象this，传入的Object
	public boolean equals(Object obj) {
		if (!(obj instanceof Person)) { //不属于该类
			return false ;
		}
		if (obj == null) { //避免null的比较
			return false ;
		}
		if (this == obj) { //同一个地址
			return true ;
		}
		Person per = (Person) obj ; // 目的是获取类中的属性
		return this.name.equals(per.name) && (this.age == per.age) ; //类内部可直接调用private属性
	}
}
public class JavaDemo {
	public static void main(String args[]) {
		Person perA = new Person("张三", 20) ;
		Person perB = new Person("张三", 20) ;
		System.out.println(perA.equals(perB)) ;
	}
}
```

String 类作为 Object 的子类，已对 equals() 方法进行了覆写。

