# Annotation 注解

## Annotation 简介

Annotation 是 JDK 1.5 之后提出的新的开发技术结构，利用 Annotation 可有效减少程序配置代码，并可利用其进行一些结构化定义。

Annotation 是以注解的形式实现程序开发。

程序开发历史：

* 过程1：在程序定义的时候将所有可能使用到的资源全部定义在程序代码之中。
	* 如果此时服务器的相关地址发生改变，对于程序而言就需要进行源代码的修改，维护需要由开发人员完成。
* 过程2：引入配置文件，在配置文件中定义全部要使用的服务器资源。
	* 在配置项不多的情况下，此类配置较为好用，且十分简单；但也有可能会出现配置文件非常多的情况。
	* 所有操作都需要通过配置文件完成，开发的难度有所提升。
* 过程3：将配置文件重新写回到程序中，利用一些特殊标记与程序代码进行分离，即注解（Annotation）。
	* 如果全部都使用注解开发，难度太高，配置文件有好处也有缺点；目前的开发为配置文件+注解。

Java 中的几个基本注解：@Override, @Deprecated, @SuppressWarnings .


## 准确覆写: @Override

当子类继承一个父类后，如果发现父类中的某些方法功能不足时，往往会采用覆写形式对方法功能进行扩充。

```
class Channel {
	public void connect() {
		System.out.println("**********Channel******") ;
	}
}
class DatabaseChannel extends Channel{ // 1. extends 可能忘记
	public void connect() { // 2. connect可能写错
		System.out.println("子类操作") ；
	}
}
public class JavaDemo {
	public static void main(String args[]) {
		new DatabaseChannel().connect() ;
	}
}
```

开发中常出现的两个问题：
* 虽然明确继承一个父类并进行方法覆写，但有可能由于疏忽忘记编写 extends, 导致不是覆写 ;
* 方法覆写时可能单词拼写错误。

追加注解：
```
class Channel {
	public void connect() {
		System.out.println("**********Channel******") ;
	}
}
class DatabaseChannel extends Channel{ // 1. extends 可能忘记
	@Override  //明确表示该方法是一个覆写方法
	public void connect() { // 2. connect可能写错
		System.out.println("子类操作") ；
	}
}
public class JavaDemo {
	public static void main(String args[]) {
		new DatabaseChannel().connect() ;
	}
}
```
@Override 可以帮助开发者在程序编译时检查出程序的错误。


## 过期声明：@Deprecated

在软件项目的迭代开发中，可能有某个方法或某个类由于在最初设计时考虑不周（存在缺陷），导致新版本不适用（老版本不影响）。此时无法直接删除原始操作，需要进行过度。此时可以采用过期声明，告诉新用户不再使用这些操作。

```
class Channel {
	@Deprecated // 老版本使用，新版本不要使用
	public void connect() {
		System.out.println("**********Channel******") ;
	}
	public String connection() {
		return "获取XXX通道连接信息" ;
	}
}
class DatabaseChannel extends Channel{ // 1. extends 可能忘记
	@Override  //明确表示该方法是一个覆写方法
	public void connect() { // 2. connect可能写错
		System.out.println("子类操作") ;
	}
}
public class JavaDemo {
	public static void main(String args[]) {
		new DatabaseChannel().connect() ;
	}
}
```

## 压制警告: @SuppressWarnings

在进行上述程序编译时会出现错误提示信息：
```
注: JavaDemo.java使用或覆盖了已过时的 API。
注: 有关详细信息, 请使用 -Xlint:deprecation 重新编译。
```

若此时不愿见到这些提示信息（或已明确知道错误），就可以进行警告信息压制。

```
class Channel {
	@Deprecated // 老版本使用，新版本不要使用
	public void connect() {
		System.out.println("**********Channel******") ;
	}
	public String connection() {
		return "获取XXX通道连接信息" ;
	}
}
class DatabaseChannel extends Channel{ // 1. extends 可能忘记
	@Override  //明确表示该方法是一个覆写方法
	@SuppressWarnings({"deprecation"}) //压制警告，用deprecation压制@Deprecated警告
	public void connect() { // 2. connect可能写错
		System.out.println("子类操作") ;
	}
}
public class JavaDemo {
	@SuppressWarnings({"deprecation"}) //压制警告，用deprecation压制@Deprecated警告
	public static void main(String args[]) {
		new Channel().connect() ;
	}
}
```
