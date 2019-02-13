---
title: 字节码工具javap
date: 2018-10-25 16:37:00
tags: 
categories: 
top:
---

# [javap命令详解](https://blog.csdn.net/zhaozheng7758/article/details/8623526)

javap是JDK自带的反汇编器，可以查看java编译器为我们生成的字节码。通过它，可以对照源代码和字节码，从而了解很多编译器内部的工作。可以在命令行窗口先用javap -help看下javap工具支持的选项：C:\>javap -help 

```shell
C:\>javap -help

Usage: javap <options> <classes>...

where options include:

   -c                      输出类中各方法的未解析的代码，即构成java字节码的指令

   -classpath <pathlist>       指定javap用来查找类的路径。目录用：分隔

   -extdirs <dirs>             覆盖搜索安装方式扩展的位置，扩展的缺省位置为jre/lib/ext

   -help                    输出帮助信息

   -J<flag>                  直接将flag传给运行时系统

   -l                       输出行及局部变量表

   -public                   只显示public类及成员

   -protected                只显示protected和public类及成员。

   -package                 只显示包、protected和public类及成员，，这是缺省设置

   -private                  显示所有的类和成员

   -s                        输出内部类型签名

   -bootclasspath <pathlist>    指定加载自举类所用的路径，如jre/lib/rt.jar或i18n.jar

   -verbose                 打印堆栈大小、各方法的locals及args参数，以及class文件的编译版本
```

平时一般用-c选项用得比较多，该命令用于列出每个方法所执行的JVM指令，并显示每个方法的字节码的实际作用。可以写个HelloWorld的程序来测试一下该命令。 

```java
public class HelloWorld {

         public static void main(String[] args){
    
           System.out.println("Hello World!");
    
         }

}
```
在将该java类编译生成HelloWorld.class文件后，即可通过javap进行具体的反编译分析。如：
```shell
$ javap -cHelloWorld

Compiled from "HelloWorld.java"

public class HelloWorld extends java.lang.Object{

public HelloWorld();

  Code:

   0:   aload_0

   1:   invokespecial   #1; //Method java/lang/Object."<init>":()V

   4:   return
```

```java
public static void main(java.lang.String[]);

  Code:

   0:   getstatic       #2; //Field java/lang/System.out:Ljava/io/PrintStream;

   3:   ldc     #3; //String Hello World!

   5:   invokevirtual   #4; //Method java/io/PrintStream.println:(Ljava/lang/String;)V

   8:   return

}
```

为了能够更清晰的了解javap反编译生成的字节码，下面来分析main方法中的指令,vcb用于转换Java语言中的代码行System.out.println("HelloWorld!");

`0:   getstatic       #2; //Fieldjava/lang/System.out:Ljava/io/PrintStream;`

最初始的整数表示方法中指令的偏移量，因此第一个指令是从0开始的。它表示的是从java.lang.system对象的out字段中检索PrintStream对象，getstatic指令即是将该静态域压缩并放到操作数栈中。按下来的指令则是引用一个地址，在当前情况下，指的是“#2;//Field java/lang/System.out:Ljava/io/PrintStream;”。在此你将会发现该域信息并没有直接嵌入进来。相反它是通过类似java类中的其它常量一样，该域信息被存储在一个共享池中。采用该常量池的方式能够减小字节码指令的长度。这也就是为什么指令中仅仅保存常量池的地址索引，而非所有的信息。在本示例中，域信息被存放在常量池中标识有#2的位置。

`3:   ldc    #3; //String Hello World!`

其实分析完第一条指令后，将非常容易的猜测到第二条指令的具体含义了。ldc(load constant)指令用于将HelloWorld！字符串压入至栈中。

`5:   invokevirtual   #4; //Method java/io/PrintStream.println:(Ljava/lang/String;)V`

该指令将调用println方法，它将从操作栈中弹出两个参数。千成别忘记了象println这样的实例方法其实是包含了两个参数的，一个是字符串，另一个是隐式的this索引。

上面的minor version: 0和majorversion: 49就是编译Worke.class时使用的jdk编译版本号。

但是它并不是我们所熟悉的jdk版本号（比如jdk1.5）。

不过我们可以把从 JDK 1.1 到 JDK 1.7 编译器编译出的 class 的默认minor.major version 汇总下就知道对应关系了。

| JDK 编译器版本 | target 参数 | 十六进制 minor.major | 十进制 minor.major |
| :---: | :---: | :---: | :---: |
| jdk1.1.8 | 不能带 target 参数 | 00 03 00 2D | 45.3 |
| jdk1.2.2 | 不带(默认为 -target 1.1) | 00 03 00 2D | 45.3 |
| jdk1.2.2 | -target 1.2 | 00 00 00 2E | 46.0 |
| jdk1.3.1_19 | 不带(默认为 -target 1.1) | 00 03 00 2D | 45.3 |
| jdk1.3.1_19 | -target 1.3 | 00 00 00 2F | 47.0 |
| j2sdk1.4.2_10 | 不带(默认为 -target 1.2) | 00 00 00 2E | 46.0 |
| j2sdk1.4.2_10 | -target 1.4 | 00 00 00 30 | 48.0 |
| jdk1.5.0_11 | 不带(默认为 -target 1.5) | 00 00 00 31 | 49.0 |
| jdk1.5.0_11 | -target 1.4 -source 1.4 | 00 00 00 30 | 48.0 |
| jdk1.6.0_01 | 不带(默认为 -target 1.6) | 00 00 00 32 | 50.0 |
| jdk1.6.0_01 | -target 1.5 | 00 00 00 31 | 49.0 |
| jdk1.6.0_01 | -target 1.4 -source 1.4 | 00 00 00 30 | 48.0 |
| jdk1.7.0 | 不带(默认为 -target 1.6) | 00 00 00 32 | 50.0 |
| jdk1.7.0 | -target 1.7 | 00 00 00 33 | 51.0 |
| jdk1.7.0 | -target 1.4 -source 1.4 | 00 00 00 30 | 48.0 |
| Apache Harmony 5.0M3 | 不带(默认为 -target 1.2) | 00 00 00 2E | 46.0 |
| Apache Harmony 5.0M3 | -target 1.4 | 00 00 00 30 | 48.0 |