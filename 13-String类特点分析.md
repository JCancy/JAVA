# String 类特点分析

## String 类简介

JDK 1.8以前 String 保存的是字符数组，JDK 1.9以后 String 保存的是字节数组。

实例化对象
```
public class StringDemo {
	public static void main(String args[]) {
		String str = "bt.byr.cn" ; //直接赋值
		System.out.println(str) ;
	}
}
```

在 String 类中除了可使用直接赋值形式对对象进行实例化，也可以按照传统方式使用构造方法进行实例化。
```
public String (String str) ;
```

利用构造方法进行实例化
```
public class StringDemo {
	public static void main(String args[]) {
		String str = new String("bt.byr.cn") ; //构造方法
		System.out.println(str) ;
	}
}
```

String 本身包装的是一个数组，且有两种对象的实例化形式：
* 直接赋值
* 构造方法实例化

## 字符串比较

进行 int 类型的 == 比较：
```
public class StringDemo {
	public static void main(String args[]) {
		int x = 10 ; 
		int y = 10 ;
		System.out.println(x == y) ;
	}
}
```

String 类也涉及相等判断问题，对于 String 类判断也可以使用“==”，但不准确

进行 String 类型的 == 比较：
```
public class StringDemo {
	public static void main(String args[]) {
		String x = "bt.byr.cn" ; 
		String y = new String("bt.byr.cn") ;
		System.out.println(x == y) ; //false
	}
}
```

两个字符串内容相同，但结果为 false 。

若想实现字符串的准确判断，可使用 String 类中提供的方法：
```
public boolean equals(String str) ;
```

eqauls 实现字符串比较：
```
public class StringDemo {
	public static void main(String args[]) {
		String x = "bt.byr.cn" ; 
		String y = new String("bt.byr.cn") ;
		System.out.println(x.equals(y)) ; //true
	}
}
```

String 中 “==” 与 equals() 的区别：
* “==”进行的是数值比较，若用于对象比较，比较的是两个对象的内存地址数值；
* equals() 是类提供的比较方法，可直接进行字符串内容的比较。



## 字符串常量

在程序开发中，任何整数默认都是 int 型，任何小数默认都是 double 型。

对于字符串而言，程序中不会提供字符串基本数据类型，只提供 String 类是一个模板，对象是一个实例。先有类再有对象。

任何使用“""” 定义的字符串常量描述的实际都是 String 类的匿名对象。

```
public class StringDemo {
	public static void main(String args[]) {
		String x = "bt" ; 
		System.out.println("bt".equals(x)) ; //true
	}
}
```
上述代码显示字符串常量可调用equals() 方法实现对象相等的判断。

结论：程序中没有字符串常量这种基本类型，有的只是String类的匿名对象。

*对象相等判断技巧：*进行项目开发时，若某些数据由用户输入，且要求这些数据为指定内容的情况下，建议将字符串常量写在前面。

```
public class StringDemo {
	public static void main(String args[]) {
		String input = null ; 
		System.out.println(input.equals("abc")) ; //true
	}
}
```
出现“Exception in thread "main" java.lang.NullPointerException”错误。


将字符串常量写在前面：
```
public class StringDemo {
	public static void main(String args[]) {
		String input = null ; 
		System.out.println("abc".equals(input)) ; //true
	}
}
```
执行结果为false，未出现上述错误。

equals()方法提供可回避null的判断，将字符串常量写在前面将不会出现 NullPointerException 错误。（字符串是匿名对象，开辟了内存空间）




## String 类对象两种实例化方式比较

*1. String 直接赋值实例化*
```
public class StringDemo {
	public static void main(String args[]) {
		String str = "mldn" ; 
	}
}
```
此时只会开辟一块堆内存，内存图如下：

![字符串内存1](https://github.com/JCancy/JAVA/blob/master/picture/%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%86%85%E5%AD%981.PNG)

利用直接赋值实例化 String 的形式还可以实现同一个字符串对象数据的共享操作。

String 直接赋值时的数据共享

```
public class StringDemo {
	public static void main(String args[]) {
		String strA = "mldn" ; 
		String strB = "mldn" ;
		System.out.println(strA == strB) ; //地址判断
	}
}
```
判断返回结果为 true， 即两个对象所指向的堆内存是同一个。

此时内存图如下：

![字符串内存2](https://github.com/JCancy/JAVA/blob/master/picture/%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%86%85%E5%AD%982.PNG)

在 JAVA 程序底层提供有专门的字符串池（字符串数组）。


分析字符串池
```
public class StringDemo {
	public static void main(String args[]) {
		String strA = "mldn" ; 
		String strB = "mldnjava" ;
		String strC = "mldn" ;
		System.out.println(strA == strB) ; //地址判断
	}
}
```

![String类池内存分析](https://github.com/JCancy/JAVA/blob/master/picture/String%E7%B1%BB%E6%B1%A0%E5%86%85%E5%AD%98%E5%88%86%E6%9E%90.PNG)

在采用直接赋值的处理过程中，对于字符串而言可实现吃数据的自动保存，有相同数据定义时可减少定义，减少对象产生，提升操作性能。


*2. String 类构造方法实例化*

```
public class StringDemo {
	public static void main(String args[]) {
		String str = new String("mldn") ; 
	}
}
```

![字符串构造方法实例化内存](https://github.com/JCancy/JAVA/blob/master/picture/%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95%E5%AE%9E%E4%BE%8B%E5%8C%96%E5%86%85%E5%AD%98.PNG)

此时会开辟两块内存空间，之后只会使用一块，另一块由于字符串常量所定义的匿名对象将成为垃圾空间。

另一种方式：
```
public class StringDemo {
	public static void main(String args[]) {
		String strA = "mldn" ;
		String strB = new String("mldn") ; 
	}
}
```

![字符串构造方法实例化内存2](https://github.com/JCancy/JAVA/blob/master/picture/%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95%E5%AE%9E%E4%BE%8B%E5%8C%96%E5%86%85%E5%AD%982.PNG)

在使用构造方法实例化 String 类对象时不会出现自动保存到字符串池山王处理。

构造方法实例化对象时的池操作：
```
public class StringDemo {
	public static void main(String args[]) {
		String strA = "mldn" ;
		String strB = new String("mldn") ; //不会入池
		System.out.println(strA == strB) ; //false
	}
}
```

构造方法实例化的对象有自己专用的内存空间，但在String类中也提供帮助开发者手工入池的方法：
```
public String intern() ;
```


手工入池：
```
public class StringDemo {
	public static void main(String args[]) {
		String strA = "mldn" ;
		String strB = new String("mldn").intern() ; //不会入池
		System.out.println(strA == strB) ; //true
	}
}
```
此时在构造方法定义之后使用了 intern()方法，此时可实现对象池的统一管理，但步骤较为繁琐。

String 类两种对象实例化方式的区别：
* 直接赋值只会产生一个实例化对象，并且可以自动保存到对象池中，以实现相应字符实例的重用；
* 构造方法会产生两个实例化对象，并且不会自动入池，无法实现对象重用；但可使用intern()方法进行手工入池处理。


## String 对象常量池

对象池的主要目的是实现数据的共享处理。

JAVA 中对象（常量）池分为两种：
* 静态常量池：程序（*.class）在加载的时候会自动将此程序中保存的字符串、普通的常量、类和方法的信息等，全部进行分配；
* 运行时常量池：当一个程序（*.class）加载之后，存在一些变量，此时提供的常量池。

静态常量池：
```
public class StringDemo {
	public static void main(String args[]) {
		String strA = "www.mldn.cn" ;
		String strB = "www." + "mldn" + ".cn" ; 
		System.out.println(strA == strB) ; //true
	}
}
```

本程序的内容都是常量数据（字符串的常量都是匿名对象），在程序加载时会自动处理相应连接。

![字符串内存3](https://github.com/JCancy/JAVA/blob/master/picture/%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%86%85%E5%AD%983.PNG)

运行时常量池：
```
public class StringDemo {
	public static void main(String args[]) {
		String info = "mldn" ;
		String strA = "www.mldn.cn" ;
		String strB = "www." + info + ".cn" ; 
		System.out.println(strA == strB) ; //false
	}
}
```
此时结果为false，程序加载时并不确定info的内容，因为在字符串连接时 info 为变量（变量内容可修改），程序不认为strB结果为最终结果。



## 字符串修改分析

String 类中包含的数组，数组最大的缺点在于长度不可改变。当设置了一个字符串后，会自动进行数组空间的开辟，开辟的内容长度是固定的。

![字符串内存4](https://github.com/JCancy/JAVA/blob/master/picture/%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%86%85%E5%AD%984.PNG)

```
public class StringDemo {
	public static void main(String args[]) {
		String str = "www." ;
		str += "mldn." ;
		str = str + "cn" ; 
		System.out.println(str) ;
	}
}
```

![字符串内存5](https://github.com/JCancy/JAVA/blob/master/picture/%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%86%85%E5%AD%985.PNG)

在整个过程中，字符串常量内容未发生任何改变，改变的只是String类对象的引用，且这种改变有可能带来大量垃圾空间。

错误案例：
```
public class StringDemo {
	public static void main(String args[]) {
		String str = "www." ;
		for (int x = 0 ; x < 1000 ; x ++) {
			str += x ;
		}
		System.out.println(str) ;
	}
}
```
该程序将会产生1000多个垃圾空间，且String对象的指向要修改1000多次，程序性能会很差。

注：String 类在开发之中不要进行内容的频繁修改。



## 主方法组成分析

Java 中的主方法：

```
public static void main(String args []) ;
```

组成分析：
* public: 描述的是访问权限，主方法是一切的开始点，开始点一定是公共的；
* static: 程序的执行是通过类名称完成的，表示此方法是由类直接调用的；
* void: 主方法是一切的起点，起点一旦开始就没有返回的可能；
* main: 系统定义好的方法名称；
* String args[]: 字符串数组，可以实现程序启动参数的接收。

输出启动参数：
```
public class {
	public static void main(String args [])  {
		for (String arg : args) {
			System.out.println(arg) ;
		}
	}
}
```

在程序执行时可以设置参数，每一个参数之间使用空格进行分割：
```
java StringDemo first second
```

但若参数本身有空格，则必须使用“""”包装：
```
java StringDemo "first one" "second one"
```

可通过启动参数实现数据输入的模拟。

