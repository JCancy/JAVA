# JAVA 多线程编程

## 进程与线程

JAVA 的特点是支持多线程开发（也是为数不多支持多线程编程的语言）。

传统DOS系统中若出现病毒，所有程序将无法执行。DOS采用单进程处理，即同一时间只允许一个程序执行。

Windows开启了多进程设计，表示一个时间段可同时运行多个程序，并且进行资源的轮流抢占，所以在同一个时间段会有多个程序依次执行，但在同一个时间点只会有一个进程执行，到多核CPU，即便有再多的进程，也可比单核CPU处理速度有所提升。

线程是在进程基础上创建并使用的，所以线程依赖于进程的支持，但是线程的启动速度比进程快很多，所以当使用多线程进行并发处理时，其执行速度更快。

JAVA 是多线程编程语言，所以JAVA 在并发访问中可以有更高的处理性能。

若想在JAVA中实现多线程定义，需要有专门的线程主体类进行线程的执行任务的定义，该主体类的定义是有要求的，必须实现特定的接口或者继承特定的父类才可以完成。

## Thread类实现多线程

JAVA中提供有java.lang.Thread程序类，一个类只要继承了此类，就表示这个类为线程的主体类。

创建MyThread子类，对Thread类中的run()方法进行覆写，该方法为线程的主方法。(文档：JAVASE->java.base->java.lang->Thread）

定义多线程主体类：
```java
class MyThread extends Thread{ //线程的主体类
	private String title ;
	public MyThread(String title) {
		this.title = title ;
	}
	@Override
	public void run() { //线程的主体方法
		for (int x = 0 ; x < 10 ; x ++) {
			System.out.println(this.title + "运行，x = " + x) ;
		}
	}
}
```

多线程要执行的功能都应在run()方法中进行定义。在正常情况下如果想要使用一个类中的方法，需要产生实例化对象然后调用类中的方法，但是run()方法不能被直接调用。若想启动多线程，必须使用start()方法完成（public void start()）。

```java
public class ThreadDemo {
	public static void main(String[] args) {
		new MyThread("线程A").start(); //若使用run()则依次运行
		new MyThread("线程B").start();
		new MyThread("线程C").start();
	}

}
```

此时虽然调用start()方法，但最终执行的是run()方法，且所有线程对象交替进行。

为何多线程启动不直接调用run()，而必须使用Thread类中的start()方法？（CTRL+单击查看start源代码）

```java
public synchronized void start() {
    if (threadStatus != 0) //判断线程状态
        throw new IllegalThreadStateException(); //抛出异常
    group.add(this);

    boolean started = false;
    try {
        start0(); //调用start0()方法
        started = true;
    } finally {
        try {
            if (!started) {
                group.threadStartFailed(this);
            }
        } catch (Throwable ignore) {
            /* do nothing. If start0 threw a Throwable then
              it will be passed up the call stack */
        }
    }
}
private native void start0(); //只定义了方法名称，没有实现
```

代码中抛出“IllegalThreadStateException”异常，但是并未使用throws或明确的try.catch处理，因为该异常是RuntimeException子类。每一个线程类对象只允许启动一次，若重复启动则抛出此异常。

重复执行线程：
```java
public class ThreadDemo {
	public static void main(String[] args) {
		MyThread mt = new MyThread("线程A").start(); //
		mt.start();
		mt.start();
	}
}
```
抛出异常“Exception in thread "main" java.lang.IllegalThreadStateException

JAVA程序执行的过程中考虑到不同层次开发者需求，支持本地操作系统函数调用，此技术称为JNI(JAVA Native Interface)技术，但JAVA开发过程中不推荐使用。利用此技术可以使用本地操作系统函数进行特殊处理，在Thread类中提供的start0()表示需要将此方法依赖于不同操作系统实现。

![Thread执行分析](https://github.com/JCancy/JAVA/blob/master/picture/Thread%E6%89%A7%E8%A1%8C%E5%88%86%E6%9E%90.PNG)

任何情况下，只要定义了多线程，启动方式永远只有一种：Thread类中的start()方法。



## Runnable接口实现多线程

第二种多线程主题定义结构形式：实现java.lang.Runnable接口。

```java
@FunctionalInterface //从JDK1.8引入Lambda表达式之后变为函数式接口
public interface Runnable {
	public void run() ;
}
```

通过Runnable实现多线程主体类：
```java
class MyThread implements Runnable{ //线程的主体类
	private String title ;
	public MyThread(String title) {
		this.title = title ;
	}
	@Override
	public void run() { //线程的主体方法
		for (int x = 0 ; x < 10 ; x ++) {
			System.out.println(this.title + "运行，x = " + x) ;
		}
	}
}
```

此时由于不再继承Thread()父类，MyThread中不再支持继承方法run()，用Thread.start()方法无法进行多线程启动。

Thread类构造方法：public Thread​(Runnable target)

启动多线程：
```java
public class ThreadDemo {
	public static void main(String[] args) {
		Thread threadA = new Thread(new MyThread("线程对象A")) ;
		Thread threadB = new Thread(new MyThread("线程对象B")) ;
		Thread threadC = new Thread(new MyThread("线程对象C")) ;
		threadA.start() ; //启动多线程
		threadB.start() ; //启动多线程
		threadC.start() ; //启动多线程
	}
```
此时多线程实现中由于只是实现了Runnable接口对象，主体类上不再有单继承局限。此设计为标准型设计。

从KDK1.8开始，Runnable接口使用了函数式接口定义，所以可以直接利用Lambda表达式实现多线程定义。

lambda表达式：
```java
public class ThreadDemo {
	public static void main(String[] args) {
		for (int x = 0 ; x < 3 ; x ++) {
			String title = "线程对象-" + x ;
			Runnable run = ()-> {
				for (int y = 0 ; y < 10 ; y ++) {
					System.out.println(title + "运行，y=" + y);
				}
			} ;
			new Thread(run).start() ;
		} 
	}
}
```

```java
public class ThreadDemo {
	public static void main(String[] args) {
		for (int x = 0 ; x < 3 ; x ++) {
			String title = "线程对象-" + x ;
			new Thread(()-> {
				for (int y = 0 ; y < 10 ; y ++) {
					System.out.println(title + "运行，y=" + y);
				}
			}).start() ;
		} 
	}
}
```

在多线程的实现中，优先考虑Runnable接口实现，并且通过Thread类启动多线程。


## Thread与Runnable关系

多线程实现有两种途径：Thread类+Runnable接口。其中Runnable较为方便，可以避免单继承局限，同时也可进行功能的扩充。

Thread类定义：
```java
public class Thread extends Object implements Runnable {}
```
Thread类也是Runnable接口的子类，之前继承Thread类时覆写的实际上还是Runnable接口的run()方法。

![Thread与Runnable](https://github.com/JCancy/JAVA/blob/master/picture/Thread%E4%B8%8ERunnable.PNG)

多线程设计中，使用了代理设计模式结构，用户自定义的线程主题只负责项目核心功能的实现，所有辅助实现全部交由Thread类处理。

在进行Thread启动多线程时调用的是start()方法，而后找到的是run()方法。当通过Thread类中的构造方法传递了一个Runnable接口对象时，该接口对象被Thread类中的target属性保存，在start()方法执行时，会调用Thread类中的run()方法，该run()方法调用Runnable接口子类中被覆写过的run()方法。

多线程开发的本质在于多个线程可以进行同一资源的抢占，Thread主要描述的时线程，而资源的描述通过Runnable完成。

![多线程开发](https://github.com/JCancy/JAVA/blob/master/picture/%E5%A4%9A%E7%BA%BF%E7%A8%8B%E5%BC%80%E5%8F%91.PNG)

卖票程序的资源并发访问：
```java
package cancy.code.demo;
class MyThread implements Runnable{ //线程的主体类
	private int ticket = 5 ;
	@Override
	public void run() { //线程的主体方法
		for (int x = 0 ; x < 100 ; x ++) {
			if (this.ticket > 0) {
				System.out.println("卖票，ticket = " + this.ticket --) ;
			}
		}
	}
}
public class ThreadDemo {
	public static void main(String[] args) {
		MyThread mt = new MyThread() ;
		new Thread(mt).start(); //第一线程
		new Thread(mt).start(); //第二线程
		new Thread(mt).start(); //第三线程
	}
}
```

![内存图](https://github.com/JCancy/JAVA/blob/master/picture/%E5%86%85%E5%AD%98%E5%9B%BE.PNG)



## Callable接口实现多线程

Runnable接口的缺点：当线程执行完毕后无法获取返回值。JDK1.5之后提出新的线程实现接口：java.util.concurrent.Callable接口。

接口定义：
```java
@FunctionalInterface
public interface Callable<V> {
	public V call() throws Exception ;
}
```
Callable定义的时候可设置一个泛型，此泛型的类型就是返回数据的类型，可以避免向下转型带来的安全隐患。

![Callable](https://github.com/JCancy/JAVA/blob/master/picture/Callable.PNG)

使用Callable实现多线程处理：
```java
package cancy.code.demo;
import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask;
class MyThread implements Callable<String>{
	@Override
	public String call() throws Exception {
		for (int x = 0 ; x < 10 ; x ++) {
			System.out.println("****线程执行,x=" + x) ;
		}
		return "线程执行完毕" ;
	}
}

public class ThreadDemo {
	public static void main(String[] args) throws Exception{
		FutureTask<String> task = new FutureTask<>(new MyThread()) ;
		new Thread(task).start() ;
		System.out.println("【线程返回数据】" + task.get()) ;
	}
}
```

面试题：Runnable与Callable的区别？
* Runnable是在JDK1.0提出的多线程实现接口，Callable在JDK1.5之后提出；
* java.lang.Runnable接口中只提供一个run()方法，且没有返回值；
* java.util.concurrent.Callable中提供有call()方法，且有返回值。



## 多线程运行状态

对于多线程开发而言，编写过程中总是按照：定义线程主体类，通过Thread类进行线程启动。但并非调用了start()方法，线程就已开始运行。

![线程运行状态](https://github.com/JCancy/JAVA/blob/master/picture/%E7%BA%BF%E7%A8%8B%E8%BF%90%E8%A1%8C%E7%8A%B6%E6%80%81.PNG)

1. 任何一个线程的对象都应该使用Thread类进行封装，所以线程的启动使用的是start()，启动时若干个线程都将进入一种就绪状态，现在并未执行。

2. 进入就绪状态之后需要等待进行资源调度，当一个线程调度成功后则进入运行状态（run()方法），但所有状态不可能一直持续执行下去，中间需产生一些暂停状态（某线程执行一段时间后让出资源），之后进入阻塞状态，随后重新回归到就绪状态。

3. 当run()方法执行完毕后，实际上该线程的主要任务就结束了，此时就可直接进入到停止状态。
