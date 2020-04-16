# Eclipse 开发工具

Eclipse 是大型开源组织，主要以推广 Java 标准以及 IDE 为主。

## Eclipse 简介

下载界面： www.Eclipse.org


## 使用JDT开发JAVA程序

Eclipse 中提供 JDT 环境可实现 JAVA 程序开发。

1. 进行项目开发，先创建新项目：FirstProject

![eclipse 操作 1](https://github.com/JCancy/JAVA/blob/master/picture/eclipse%E6%93%8D%E4%BD%9C1.PNG)

2. 输入项目名称，同时会帮助用户找到可以使用的 JDK 版本；若没有相应 JDK 配置，需要自己进行 JRE 配置（Configure JREs）。

![eclipse 操作 2](https://github.com/JCancy/JAVA/blob/master/picture/eclipse%E6%93%8D%E4%BD%9C2.PNG)

![eclipse 操作 3](https://github.com/JCancy/JAVA/blob/master/picture/eclipse%E6%93%8D%E4%BD%9C3.PNG)

![eclipse 操作 4](https://github.com/JCancy/JAVA/blob/master/picture/eclipse%E6%93%8D%E4%BD%9C4.PNG)

电脑上有多个 JDK 的情况下，需手动处理。

3. 项目创建完成后会自动出现确认对话框：主要询问是否进行透视图切换。选择不切换可以直接建立好 JAVA 项目创建完成后会自动出现确认对话框：主要询问是否进行透视图切换。选择不切换可以直接建立好

建立好之后的工作区有两个文件夹：
* bin: 保存所有*.java源文件；
* src: 保存所有编译后的*.class程序文件，会自动进行编译处理。

4. 在项目的 src 源代码目录下创建新的 Java 类：cancy.javacode.demo.Hello.java

![eclipse 操作 5](https://github.com/JCancy/JAVA/blob/master/picture/eclipse%E6%93%8D%E4%BD%9C5.PNG)

若想改变字体大小，可使用 CTRL + '+' || '-' 进行调整.

5. 修改 Eclipse 支持的文件编码

对单个文件进行编码修改：右键点击单个文件->properties->UTF-8

![eclipse 操作 6](https://github.com/JCancy/JAVA/blob/master/picture/eclipse%E6%93%8D%E4%BD%9C6.PNG)

对整体进行编码修改：window->preferences->输入encoding搜索->workspace->UTF-8

![eclipse 操作 7](https://github.com/JCancy/JAVA/blob/master/picture/eclipse%E6%93%8D%E4%BD%9C7.PNG)

6. 编写基础程序

```java
package cancy.javacode.demo;

public class Hello {

	public static void main(String[] args) {
		System.out.println("Hello World !") ;
		System.out.println("www.github.com");
	}

}
```

程序代码保存后会自动进行编译，在 bin 中出现*.class文件。右键 Run As 之后可直接执行程序。


7. Eclipse 中的快捷键

* CTRL+1: 进行代码纠正提示；
* Alt+/: 进行代码提示；
* CTRL+Alt+↓：复制当前行；
* CTRL+/：单行注释；
* CTRL+shift+/：多行注释；
* CTRL+shift+F：格式化代码；
* CTRL+shift+O：自当导入所需包；
* main+Alt+/(Enter)：主方法快捷键；
* sysout+Alt+/: 输出快捷键.

其余快捷键可使用 CTRL+shift+L 自行查阅。

![eclipse 操作 8](https://github.com/JCancy/JAVA/blob/master/picture/eclipse%E6%93%8D%E4%BD%9C8.PNG)



8. 支持代码生成操作，可自动生成构造方法或 setter，getter 方法。

![eclipse 操作 9](https://github.com/JCancy/JAVA/blob/master/picture/eclipse%E6%93%8D%E4%BD%9C9.PNG)

9. 处理异常

![eclipse 操作 10](https://github.com/JCancy/JAVA/blob/master/picture/eclipse%E6%93%8D%E4%BD%9C10.PNG)

10. 执行程序时可使用初始化参数进行内容接收，eclipse中也可配置初始化参数，但需执行一次后才可以配置。

11. 可以直接导出*.jar文件：file->export->java->jar file->选择要导出文件内容并设置名称

12. jar文件导入（使用Java构建路径）：项目右键->properties->Java Build Path->Libraries->Classpath->Add External JARs




## 代码调试

Eclipse 支持代码调试操作。

1. 建立程序类用于测试
```java
package cancy.javacode.util;

public class Math {
	private Math() {} //构造方法自由化
	public static int add(int x,int y) {
		int result = 0 ;
		result = x + y ;
		return result ;
	}
}
```

2. 新建测试类
```java
package cancy.javacode.test;
import cancy.javacode.util.*;
public class TestMath {
	public static void main(String[] args) {
		int numA = 10 ;
		int numB = 20 ;
		System.out.println(cancy.javacode.util.Math.add(numA,numB));
	}
}
```

3. 进行代码调试需设置断点（Break Point），右侧双击相应行出现小蓝点。

右键 DebugAs 以调试模式执行程序。

4. 代码调试控制工具
* 【F5】单步跳入：进入代码中进行程序观察；
* 【F6】单步跳过：只关心最终结果而不关心里面到底执行了什么（观察程序表面执行）；
* 【F7】单步返回：进入之后不再继续观察，则直接返回；
* 【F8】恢复执行：取消断点的影响，程序正常执行完毕。


## junit测试工具

1. 定义测试程序类
```java
package cancy.javacode.util;

public class Math {
	private Math() {} //构造方法自由化
	public static int add(int x,int y) {
		int result = 0 ;
		result = x + y ;
		return result ;
	}
}
```

2. junit是第三方组件包，须在项目中配置相应*.jar文件，可通过Eclipse配置，选中要测试的类。

选中类->新建->junit->TestCase(一个用例)->设置package->next->选择要测试的方法->next->add junit 5->

junit 须在 JavaBuilderPath 中配置相应组件库，上述方法可以帮助完成*.jar文件的CLASSPATH环境配置。

编写测试案例：
```java
package cancy.javacode.test;
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;
import junit.framework.TestCase;
class MathTest {

	@Test
	void testAdd() {
		TestCase.assertEquals(cancy.javacode.util.Math.add(10, 20),30) ;
	}

}
```

RunAs Junit ->测试结果有两种：成功（Green Bar），失败（Red Bar）
