# JAVA基础类库

## StringBuffer类

String类特点：
* 每一个字符串常量都属于String类的匿名对象，且不可更改；
* String有两个常量池：静态常量池/运行时常量池；
* String类对象实例化建议使用直接赋值的形式完成，这样可以直接将对象保存到常量池。

String类缺点：内容不允许修改。因此出现StringBuffer类可以解决此问题。

String类：
```java
package cancy.code.demo;
public class JavaAPIDemo {
	public static void main(String[] args) {
		String str = "Hello" ;
		change(str);
		System.out.println(str);
	}	
	public static void change(String temp) {
		temp += "World!" ; //内容并未发生改变
	}
}
```

StringBuffer类不像String类拥有两种实例化方式，需要向普通类一样进行处理：
	* 构造方法：public StringBuffer()；
	* 构造方法：public StringBuffer(String str), 接受初始化字符串；
	* 数据类型：public StringBuffer append(数据类型 变量), 相当于字符串中 “+”;

StringBuffer：
```java
package cancy.code.demo;
public class JavaAPIDemo {
	public static void main(String[] args) {
		StringBuffer buf = new StringBuffer("Hello ") ;
		change(buf);
		System.out.println(buf.toString());
	}	
	public static void change(StringBuffer temp) {
		temp.append("World!") ; //内容并未发生改变
	}
}
```

所有 “+” 在编译之后都变为 StringBuffer 中的 append() 方法，且在程序中 StringBuffer 和 String 类可以直接互相转换：
	* String类转为StringBuffer可以靠StringBuffer类的构造方法或append()；
	* 所有类对象都可以通过toString()方法变为String类型。

在StringBuffer中除了可以支持有字符串内容的修改之外，也提供有String类不具备的方法：
* 插入数据：public StringBuffer insert​(int offset, 数据类型 b)
* 删除指定范围的数据：public StringBuffer delete​(int start, int end)
* 字符串内容反转：pubic StringBuffer reverse()


插入数据：
```java
package cancy.code.demo;
public class JavaAPIDemo {
	public static void main(String[] args) {
		StringBuffer buf = new StringBuffer() ;
		buf.append(".cn").insert(0,"www.").insert(4, "github");
		System.out.println(buf);
	}
}
```

删除指定范围内数据：
```java
package cancy.code.demo;
public class JavaAPIDemo {
	public static void main(String[] args) {
		StringBuffer buf = new StringBuffer() ;
		buf.append("Hello World !").delete(6,12).insert(6, "github");
		System.out.println(buf);
	}
}
```

字符串内容反转：
```java
package cancy.code.demo;
public class JavaAPIDemo {
	public static void main(String[] args) {
		StringBuffer buf = new StringBuffer() ;
		buf.append("Hello World !");
		System.out.println(buf.reverse());
	}
}
```

与StringBuffer类类似的功能类：StringBuilder类，JDK1.5之后提供，其方法与StringBuffer类功能相同。区别在于StringBuffer类中方法属于线程安全的，全部使用 synchronized 关键字进行标注，而StringBuilder类属于非线程安全的。

面试题： String, StringBuffer, StringBuilder 的区别：
* String 类是字符串的首选类型，其最大特点是内容不允许修改；
* StringBuffer 与 StringBuilder 类的内容允许修改；
* StringBuffer 是在 JDK1.0 提供的，属于线程安全操作；StringBuilder在 JDK1.5 提供的，属于非线程安全操作。


## CharSequence接口

CharSequence 是描述字符串结构的接口，该接口内一般存在三种常用子类：
* String类:
	* public final class String extends Object implements Serializable, Comparable<String>, CharSequence
* StringBuffer类：
	* public final class StringBuffer extends Object implements Serializable, Comparable<StringBuffer>, CharSequence
* StringBuilder类：
	* public final class StringBuilder extends Object implements Serializable, Comparable<StringBuilder>, CharSequence


```java
public class JavaAPIDemo {
	public static void main(String[] args) {
		CharSequence str = "www.github.com"; //子类实例向父接口转型
	}
}
```

![CharSequence](https://github.com/JCancy/JAVA/blob/master/picture/CharSequence.PNG)

CharSequence本身是一个接口，在该接口中定义有如下操作方法：
* 获取指定索引字符：public char charAt​(int index);
* 获取字符串的长度：public int length();
* 截取部分字符串：public CharSequence subSequence​(int start, int end)


## AutoCloseable接口

AutoCloseable主要用于日后进行资源开发的处理上，以实现资源的自动关闭（释放资源），例如：在文件、网络、数据库开发过程中由于服务器的资源有限，故使用之后一定要关闭资源，这样才能被更多的使用者使用。

手工实现资源处理：
```java
public class JavaAPIDemo {
	public static void main(String[] args) {
		NetMessage nm = new NetMessage("www.github.com");
		if (nm.open()) {//是否打开了链接
			nm.send(); //消息发送
			nm.close(); //关闭连接
		}
	}
}
interface IMessage {
	public void send(); //消息发送
}
class NetMessage implements IMessage { //实现消息的处理机制
	private String msg ;
	public NetMessage(String msg) {
		this.msg = msg;
	}
	public boolean open() {
		System.out.println("【open】获取消息发送连接资源");
		return true;
	}
	@Override
	public void send() {
		System.out.println("【***发送消息***】" + this.msg);
	}
	public void close() {
		System.out.println("【close】关闭消息发送通道");
	}
}
```

JDK1.7 之后提出了 AutoCloseable 接口，可以实现接口的自动关闭。该接口只提供一个方法：
* 关闭方法：public void close() throws Exception；

![AutoCloseable](https://github.com/JCancy/JAVA/blob/master/picture/AutoCloseable.PNG)

要想实现自动关闭处理，除了要使用AutoCloseable外，还需结合异常处理。

```java
package cancy.code.demo;
public class JavaAPIDemo {
	public static void main(String[] args) throws Exception {
		try (NetMessage nm = new NetMessage("www.github.com")) {
			nm.send();
		} catch (Exception e) {}
	}
}
interface IMessage extends AutoCloseable {
	public void send(); //消息发送
}
class NetMessage implements IMessage,AutoCloseable { //实现消息的处理机制
	private String msg ;
	public NetMessage(String msg) {
		this.msg = msg;
	}
	public boolean open() {
		System.out.println("【open】获取消息发送连接资源");
		return true;
	}
	@Override
	public void send() {
		if (this.open()) {
			System.out.println("【***发送消息***】" + this.msg);
		}	
	}
	public void close() throws Exception {
		System.out.println("【close】关闭消息发送通道");
	}
}
```

日后接触到资源关闭问题时，都会见到AutoCloseable的使用。

## Runtime类

Runtime描述运行时状态，Runtime类是唯一一个与JVM运行状态有关的类，并且默认提供一个该类的实例化对象。

由于每一个JVM进程里只允许提供一个Runtime类对象，所以该类方法被默认私有化，故该类使用的是单例设计模式，单例设计模式会提供有static方法获取本类实例。

![Runtime](https://github.com/JCancy/JAVA/blob/master/picture/Runtime.PNG)

Runtime类属于单例设计模式，若想获取实例化对象，可依靠类中的getRuntime()方法完成：
* 获取实例化对象：public static Runtime getRuntime()；

获取Runtime类对象：
```java
public class JavaAPIDemo {
	public static void main(String[] args) throws Exception {
		Runtime run = Runtime.getRuntime();//获取实例化对象
		System.out.println(run.availableProcessors()); //获取本机CPU内核数
	}
}
```

Runtime类中提供四种重要方法：
* 获取最大可用内存空间：public long maxMemory()，默认配置为本机系统内存的1/4；
* 获取可用内存空间：public long totalMemory()，默认配置为本机系统内存的1/64；
* 获取空闲内存空间：public long freeMemory()；
* 手工进行GC处理：public void gc().

观察内存状态：
```java
public class JavaAPIDemo {
	public static void main(String[] args) throws Exception {
		Runtime run = Runtime.getRuntime();//获取实例化对象
		System.out.println("[1]max_memory:" + run.maxMemory());
		System.out.println("[1]total_memory:" + run.totalMemory());
		System.out.println("[1]free_memory:" + run.freeMemory());
		String str = "";
		for (int x=0; x<30000; x++) {
			str += x; //产生大量垃圾空间
		}
		System.out.println("[2]max_memory:" + run.maxMemory());
		System.out.println("[2]total_memory:" + run.totalMemory());
		System.out.println("[2]free_memory:" + run.freeMemory());
		run.gc();
		System.out.println("[3]max_memory:" + run.maxMemory());
		System.out.println("[3]total_memory:" + run.totalMemory());
		System.out.println("[3]free_memory:" + run.freeMemory());
	}
}
```

什么是GC?如何处理？
* GC(Garbage Collector)垃圾收集器，可由系统调用的垃圾释放功能，或者使用Runtime类中的gc()手工调用。

## System类

System类中定义的处理方法：
* 数组拷贝：public static void arraycopy​(Object src, int srcPos, Object dest, int destPos, int length)；
* 获取当前日期时间数值：public static long currentTimeMillis()；
* 进行垃圾回收：public static void gc()；

操作耗时统计：
```java
public class JavaAPIDemo {
	public static void main(String[] args) throws Exception {
		long start = System.currentTimeMillis();
		Runtime run = Runtime.getRuntime();//获取实例化对象
		String str = "";
		for (int x=0; x<30000; x++) {
			str += x; //产生大量垃圾空间
		}
		long end = System.currentTimeMillis();
		System.out.println("操作耗时：" + (end - start));
	}
}
```

System中也提供有gc()方法，该方法为Runtime中的gc()方法，即Runtime.getRuntime().gc()。



## Cleaner类

Cleaner是在JDK1.9之后提供的对象清理方法，主要功能是进行finalize()方法替代。JAVA提供了收尾操作，每一个对象再回收之前提供喘息之机。最初实现处收尾理的方法是Object类中提供的finalize()方法：
* @Deprecated(since="9") protected void finalize() throws Throwable；

观察传统回收：
```java
public class JavaAPIDemo {
	public static void main(String[] args) throws Exception {
		Member mem = new Member(); //诞生
		mem = null; //成为垃圾
		System.gc();
		System.out.println("新的一天");
	}
}

class Member {
	public Member() {
		System.out.println("构造：诞生");
	}
	@Override
	protected void finalize() throws Throwable {
		System.out.println("回收：死亡");
		throw new Exception("我还要。。。");
	}
}
```

从JDK9开始，该操作不建议使用，从JDK1.9开始建议使用AutoCloseable或java.lang.ref.Cleaner类进行回收处理（Cleaner也支持有AutoCloseable）处理。

新清除：
```java
package cancy.code.demo;

import java.lang.ref.Cleaner;

public class JavaAPIDemo {
	public static void main(String[] args) throws Exception {
		try (MemberCleaning mc = new MemberCleaning()) {
			// 可执行代码
		} catch(Exception e){}
	}
}

class Member implements Runnable{
	public Member() {
		System.out.println("构造：诞生");
	}
	@Override
	public void run() { //清除时执行
		System.out.println("[回收]死亡");
	}
}
 class MemberCleaning implements AutoCloseable { //实现清除处理
	 private static final Cleaner cleaner = Cleaner.create(); //创建一个清楚处理
	 private Member member ;
	 private Cleaner.Cleanable cleanable ;
	 public MemberCleaning() {
		 this.member = new Member(); //创建新对象
		 this.cleanable = this.cleaner.register(this,this.member);//注册使用的对象
	 }
	 @Override
	 public void close() throws Exception {
		 this.cleanable.clean(); //启动多线程
	 }
 }
```

新一代清除回收处理过程中，考虑的是多线程的使用，许多对象回收前的处理通过单独一个线程完成。

## 对象克隆

对象克隆指对象的复制，使用已有对象内容创建一个新的对象。若想进行对象克隆可以使用Object类中的clone()方法：
* 克隆：protected Object clone() throws CloneNotSupportedException；

所有类都继承Object父类，因此所有类都会有clone()方法，但并非所有类都希望被克隆。若想实现克隆，对象所在类需要实现Cloneable接口，此接口不提供任何方法，它描述的是一种能力。

实现对象克隆：
```java
package cancy.code.demo;

public class JavaAPIDemo {
	public static void main(String[] args) throws Exception {
		Member memberA = new Member("张三",30);
		Member memberB = (Member) memberA.clone();
		System.out.println(memberA);
		System.out.println(memberB);
	}
}

class Member implements Cloneable{
	private String name;
	private int age;
	public Member(String name, int age ) {
		this.name = name ;
		this.age = age ;
	}
	@Override
	public String toString() {
		// TODO Auto-generated method stub
		return super.toString() + ": name=" + this.name + ", age=" + this.age;
	}	
	@Override
	protected Object clone() throws CloneNotSupportedException {
		// TODO Auto-generated method stub
		return super.clone(); //调用父类中的clone()方法
	}
}
```

开发中除特别需求外，很少出现对象克隆需求。
