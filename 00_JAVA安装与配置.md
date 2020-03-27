# JDK安装与配置

（JDK SE 11)下载地址：https://www.oracle.com/java/technologies/javase-jdk11-downloads.html

安装完成后需要修改系统的 path 环境属性实现，所有的可执行程序的路径在：

```JAVA
D:\Java\jdk-11.0.6\bin
```

配置步骤：
* 计算机->属性->高级系统设置->高级->环境变量
* 新建用户变量【JAVA_HOME】，变量值【D:\Java\jdk-11.0.6】（文件安装路径）
* 修改【Path】，添加【%JAVA_HOME%\bin】，保存
* 打开CMD，运行 “javac”, 若出现相关信息，则配置成功


# 添加右键运行CMD方法

## 1 使用reg文件进行编辑

新建文件，将以下内容保存成reg文件，如addCMD.reg，双击该文件自动导入设置

```java
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\Background\shell\此处打开命令行]
"ShowBasedOnVelocityId"=dword:00639bc8

[HKEY_CLASSES_ROOT\Directory\Background\shell\此处打开命令行\command]
@="cmd.exe /s /k pushd \"%V\""
```

## 2 手动设置

* 打开注册表
  * 快捷键 win+r
  * 运行中输入 “regedit” ，回车
* 创建与设置 OpenCMDHere
  * HKEY_CLASSES_ROOT\Directory\Background\shell\
  * 右击 shell ，新建项  “OpenCMDHere”，并在该项下，右击新建项  “command”
  * 直接点击  OpenCMDHere，将OpenCMDHere 的默认值数据改为 在此处打开命令窗口
  * （创建图标）右键 新建字符串名， 名字为 Icon，将值改为 cmd.exe
* 设置command
  * 右键修改默认值，在键内填写以下内容即可：
    * cmd.exe /s /k pushd \"%V\"
    
    


