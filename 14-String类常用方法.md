# String 类常用方法

## JAVADOC 文档简介

开发中会大量使用JAVA的API文档（JavaDoc），该文档可直接通过Oracle在线访问进行查看。

地址：https://docs.oracle.com/en/java/javase/11/docs/api/jdk.javadoc/module-summary.html

JDK 1.9之前，所有JAVA中的常用类库会在JVM启动时进行全部加载。

JDK 1.9开始提供模块化设计，将一些程序类放在了不同模块里。

模块中会包含大量程序开发包：

![JAVA 开发包](https://github.com/JCancy/JAVA/blob/master/picture/JAVA%E5%BC%80%E5%8F%91%E5%8C%85.PNG)

查看 String 类的相关定义，可以打开 java.lang 进行查看。

String 时系统提供的较为标准的类，以该类对文档结构进行说明，一般文档里的组成会有如下几个部分：
* 类的完整定义（Class String）
* 类的说明信息
* 成员属性摘要（Field Summary）
* 构造方法摘要（Constructor Summary），其中**Deprecated**表示不建议使用
* 方法摘要（Method Summary），左边为返回值，右边为方法名称和参数




## 字符串与字符

在 JDK 1.9 前，所有 String 都利用了字符数组实现了包装的处理，所以在 String 类中提供相应的转换处理方法，包含构造方法和普通方法。

No. | 方法名称 | 类型 | 描述
:-: | :- | :-: | :-
01 | public String(char[] value) | 构造 | 将传入的全部字符数组变为字符串
02 | public String(char[] value,int offset,int count) | 构造 | 将部分字符数组变为字符串
03 | public char charAt(int index) | 普通 | 获取指定索引位置的字符
04 | public char[] toCharArray() | 普通 | 将字符串中的全部数据以字符数组的形式返回


charAt 方法：
```
public class StringDemo {
	public static void main(String args [])  {
		String str = "www.abc.com" ;
		char c = str.charAt(5) ; //使用 -encoding utf-8 编译
		System.out.println(c) ;
	}
}
```
利用charAt()可以获取某一个指定索引位置的字符，但程序之中的索引下标均是从0开始的。

实现字符串与字符数组的转换：

```
public class StringDemo {
	public static void main(String args [])  {
		String str = "helloworld" ;
		char [] result = str.toCharArray() ; //将字符串变为字符数组
		for (int x = 0 ; x < result.length ; x ++) {
			result[x] -= 32 ; //编码减少32，变为大写
		}
		//将处理后的字符数组交给String变为字符串
		String newStr = new String(result) ;
		System.out.println(newStr) ; //全部变为字符串
		System.out.println(new String(result,0,5)) ; //将一部分变为字符串
	}
}
```

判断某一个字符串中的数据是否全由数字组成，可采用如下操作：
* 若想判断字符串中的每一位最好的做法是将字符串变为字符数组；
* 可以判断每一个字符是否在数字的范围之内('0'~'9');
* 如果有一位不是数字则表示验证失败。

实现字符串数字检查：
```
public class StringDemo {
	public static void main(String args [])  {
		String str = "helloworld" ;
		System.out.println(isNumber(str) ? "由数字组成" : "不是由数字组成") ;
	}
	// 该方法主要判断字符串是否由数字组成
	public static boolean isNumber(String str) {
		char [] result = str.toCharArray() ; //将字符串变为字符数组
		for(int x = 0 ; x < result.length ; x ++) {
			if (result[x] < '0' || result[x] > '9') {
				return false ; //后面不在判断
			}
		}
		return true ;
	}
}
```

实际开发中处理中文时往往使用 char 类型，因为其可以包含中文数据。




## 字符串与字节

字符串与字节数组之间也可以实现转换的处理操作。当金星字符串与字节转换时，主要目的是为了进行二进制数据传输，或者进行编码转换。

No. | 方法名称 | 类型 | 描述
:-: | :- | :-: | :-
01 | public String(byte[] bytes) | 构造 | 将全部字节数组变为字符串
02 | public String(byte[] bytes,int offset,int length) | 构造 | 将部分字节数组变为字符串
03 | public byte[] getBytes() | 普通 | 将字符串转为字节数组
04 | public byte[] getBytes(String charsetName) throws UnsupportedEncodingException| 普通 | 编码转换


字节与字符串的转换：
```
public class StringDemo {
	public static void main(String args [])  {
		String str = "helloworld" ;
		byte data [] = str.getBytes() ; //将字符串变为字节数组
		for (int x = 0 ; x < data.length ; x ++) {
			data[x] -= 32 ;
		}
		System.out.println(new String(data)) ;
		System.out.println(new String(data,0,5)) ;
	}
}
```
字节有长度限制，一个字节最多可以保存的范围是 -128~127之间


## 字符串比较

字符串比较最常用方法为 equals()，该方法区分分大小写。

```
public class StringDemo {
	public static void main(String args [])  {
		String strA = "ABC" ;
		String strB = "abc" ;
		System.out.println(strA.equals(strB)) ; //FALSE
	}
}
```

除了 equals() 外，还有其他比较方法：

No. | 方法名称 | 类型 | 描述
:-: | :- | :-: | :-
01 | public boolean equals​(String anObject) | 普通 | 区分大小写的相等判断
02 | public boolean equalsIgnoreCase​(String anotherString) | 普通 | 不区分大小写的比较
03 | public int compareTo​(String anotherString) | 普通 | 进行字符串大小比较，该方法返回 int 数据，有三种取值：大于(>)，小于(<)，等于(=)
04 | public int compareToIgnoreCase​(String str) | 普通 | 不区分大小写进行字符串大小比较


进行字符串大小比较：
```
public class StringDemo {
	public static void main(String args [])  {
		String strA = "A" ;
		String strB = "a" ;
		System.out.println(strA.compareTo​(strB)) ; // M - m = -32
		System.out.println(strB.compareTo​(strA)) ; // m - M = 32
		System.out.println("ABC".compareTo​("ABC")) ; // 0
	}
}
```

忽略大小写的比较：
```
public class StringDemo {
	public static void main(String args [])  {
		String strA = "A" ;
		String strB = "a" ;
		System.out.println(strA.compareToIgnoreCase​(strB)) ; //0
		System.out.println(strB.compareToIgnoreCase​(strA)) ; //0
		System.out.println("ABC".compareToIgnoreCase​("ABC")) ; // 0
	}
}
```


## 字符串查找

从一个完整的字符串中查找子字符串，在 String 类中提供下述查找方法：

No. | 方法名称 | 类型 | 描述
:-: | :- | :-: | :-
01 | public boolean contains​(String s) | 普通 | 判断子字符串是否存在
02 | public int indexOf​(String str) | 普通 | 从头查找指定字符串的位置，找不到返回-1
03 | public int indexOf​(String str, int fromIndex) | 普通 | 从指定位置查找指定字符串的位置
04 | public int lastIndexOf​(String str) | 普通 | 由后向前查找指定字符串的位置
05 | public int lastIndexOf​(String str, int fromIndex) | 普通 | 从指定位置由后向前查找指定字符串的位置
06 | public boolean startsWith​(String prefix) | 普通 | 判断是否以指定字符串开头
07 | public boolean startsWith​(String prefix, int toffset) | 普通 | 由指定位置判断是否以指定字符串开头
08 | public boolean endsWith​(String suffix) | 普通 | 判断是否已制定字符串结尾


判断子字符串是否存在：
```
public class StringDemo {
	public static void main(String args [])  {
		String str = "www.github.com" ;
		System.out.println(str.contains​("github")) ; //true
		System.out.println(str.contains​("love")) ; //false
	}
}
```

此方法是在 JDK 1.5 之后追加的。JDK 1.5 之前只能依靠 indexOf​() 方法。两个方法之间的调用有限制：

判断子字符串是否存在：
```
public class StringDemo {
	public static void main(String args [])  {
		String str = "www.github.com" ;
		System.out.println(str.indexOf​("github")) ; //4
		System.out.println(str.indexOf​("love")) ; //-1
		if (str.indexOf​("github") != -1) {
			System.out.println("数据存在") ;
		}
	}
}
```
indexOf​() 是为了进行字符串位置的查询，在开发中可以利用此形式进行索引位置的确定。

indexOf​() 是由前向后查找的，lastIndexOf()由后向前查找。


lastIndexOf()查找：
```
public class StringDemo {
	public static void main(String args [])  {
		String str = "www.github.com" ;
		System.out.println(str.lastIndexOf​(".")) ; //10
		System.out.println(str.lastIndexOf​(".",5)) ; //3
	}
}
```

判断是否以指定字符串开头或结尾：
```
public class StringDemo {
	public static void main(String args [])  {
		String str = "**@@www.github.com##" ;
		System.out.println(str.startsWith​("**")) ; //true
		System.out.println(str.startsWith​("@@",2)) ; //true
		System.out.println(str.endsWith​("##")) ; //true
	}
}
```


## 字符串替换

字符串替换指可以通过指定的内容进行指定字符串的替换显示，替换方法主要有两个。

No. | 方法名称 | 类型 | 描述
:-: | :- | :-: | :-
01 | public String replaceAll​(String regex, String replacement) | 普通 | 进行全部替换
02 | public String replaceFirst​(String regex, String replacement) | 普通 | 替换首个



```
public class StringDemo {
	public static void main(String args [])  {
		String str = "helloworld" ;
		System.out.println(str.replaceAll​("l","x")) ; 
		System.out.println(str.replaceFirst​("l","x")) ; 
		
	}
}
```


## 字符串拆分

字符串拆分可以根据指定字符串或表达式实现字符串拆分，且拆分完成的数据将以字符串数组的形式返回。

No. | 方法名称 | 类型 | 描述
:-: | :- | :-: | :-
01 | public String[] split​(String regex) | 普通 | 按照指定字符串全部拆分
02 | public String[] split​(String regex, int limit) | 普通 | 按照指定字符串拆分为指定个数，后面不拆



全部拆分：
```
public class StringDemo {
	public static void main(String args [])  {
		String str = "hello world hello China" ;
		String result [] = str.split​(" ") ; //空格拆分
		for (int x = 0 ; x < result.length ; x ++) {
			System.out.println(result[x]) ;
		}
	}
}
```

部分拆分：
```
public class StringDemo {
	public static void main(String args [])  {
		String str = "hello world hello China" ;
		String result [] = str.split​(" ",3) ; //空格拆分,拆分为3段
		for (int x = 0 ; x < result.length ; x ++) {
			System.out.println(result[x]) ;
		}
	}
}
```

有时会遇到无法拆分的情况，此时需要使用"\\"进行转义：

```
public class StringDemo {
	public static void main(String args [])  {
		String str = "192.168.2.231" ;
		String result [] = str.split​("\\.") ; //直接使用"."无法拆分
		for (int x = 0 ; x < result.length ; x ++) {
			System.out.println(result[x]) ;
		}
	}
}
```


## 字符串截取

从完整字符串中截取子字符串，主要有两种方法：

No. | 方法名称 | 类型 | 描述
:-: | :- | :-: | :-
01 | public String substring​(int beginIndex) | 普通 | 从指定索引截取到结束
02 | public String substring​(int beginIndex, int endIndex) | 普通 | 截取指定索引范围中的子字符串


```
public class StringDemo {
	public static void main(String args [])  {
		String str = "www.github.com" ;
		System.out.println(str.substring​(4)) ;
		System.out.println(str.substring​(4,8)) ;
	}
}
```

开发中许多开始或结束索引是通过indexOf() 计算得来的

```
public class StringDemo {
	public static void main(String args [])  {
		// 字符串结构：“用户id-photo-姓名.后缀”
		String str = "abc1-photo-张三.jpg" ;
		int beginIndex = str.indexOf​("-",str.indexOf​("photo")) + 1 ;
		int endIndex = str.lastIndexOf​(".") ;
		System.out.println(beginIndex) ;
		System.out.println(endIndex) ;
		System.out.println(str.substring​(beginIndex,endIndex)) ;
	}
}
```




## 字符串格式化

从 JDK 1.5 开始，JAVA 提供有格式化数据的处理操作，可以用占位符实现数据的输出。

常用的有：字符串(%s),字符(%c),整数(%d),小数(%f)

No. | 方法名称 | 类型 | 描述
:-: | :- | :-: | :-
01 | public static String format​(String format, 各种类型... args) | 普通 | 根据指定结构进行文本格式化显示


```
public class StringDemo {
	public static void main(String args [])  {
		// 字符串结构：“用户id-photo-姓名.后缀”
		String name = "张三.jpg" ;
		int age = 18 ;
		double score = 78.95623 ;
		String str = String.format("姓名：%s，年龄：%d，成绩：%5.2f",name,age,score) ;
		System.out.println(str) ;
	}
}
```

## 其他操作方法

String 类中还提供有其他方法：

No. | 方法名称 | 类型 | 描述
:-: | :- | :-: | :-
01 | public String concat​(String str) | 普通 | 字符串连接
02 | public String intern() | 普通 | 字符串入池
03 | public boolean isEmpty() | 普通 | 判断是否为空字符串，不是null
04 | public int length() | 普通 | 计算字符串的长度
05 | public String trim() | 普通 | 去除左右的空格信息
06 | public String toUpperCase() | 普通 | 转大写
07 | public String toLowerCase() | 普通 | 转小写
08 | public boolean endsWith​(String suffix) | 普通 | 判断是否已制定字符串结尾


concat​：
```
public class StringDemo {
	public static void main(String args [])  {
		// 字符串结构：“用户id-photo-姓名.后缀”
		String strA = "www.github.com" ;
		String strB = "www.".concat("github").concat(".com") ;
		System.out.println(strB) ;
		System.out.println(strA == strB) ; //FALSE
	}
}
```
结果为 false，说明此操作未实现静态操作

字符串定义时“""”和“null”不是一个概念，一个表示有实例化对象，一个表示没有实例化对象。

isEmpty()主要判断字符串的内容，要在有实例化对象的时候调用。

判断空字符串 isEmpty()：
```
public class StringDemo {
	public static void main(String args [])  {
		String str = "" ;
		System.out.println(str.isEmpty()) ; //true
		System.out.println("abc".isEmpty()) ; //false
	}
}
```

trim()消除左右两边空格:
```
public class StringDemo {
	public static void main(String args [])  {
		String str = "    www.github.com    " ;
		String trimstr = str.trim() ;
		System.out.println(str) ;
		System.out.println(trimstr) ; 
		System.out.println(str.length()) ;
		System.out.println(trimstr.length()) ;
	}
}
```

大小写转换toUpperCase(),toLowerCase():
```
public class StringDemo {
	public static void main(String args [])  {
		String str = "www.GitHub.com" ;
		System.out.println(str.toUpperCase()) ;
		System.out.println(str.toLowerCase()) ;
	}
}
```

String 类中没有首字母大写方法，可通过组合实现：
```
class StringUtil {
	public static String initcap(String str) {
		if (str == null || "".equals(str)) {
			return str ; //原样返回
		}
		if (str.length() == 1) {
			return str.toUpperCase() ;
		}
		return str.substring​(0,1).toUpperCase() + str.substring​(1) ;
	}
}
public class StringDemo {
	public static void main(String args [])  {
		System.out.println(StringUtil.initcap("hello")) ;
		System.out.println(StringUtil.initcap("m")) ;
	}
}
```
