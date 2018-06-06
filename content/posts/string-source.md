---
title: "Java String 类学习"
date: 2015-10-11T18:46:51+08:00
draft: true
tags: ["Java","String"]
categories: ["Java","源码"]
---

## 概述

String 类位于 java.lang 包下， java.lang 这个包里面放置的所有类构成了 Java 编程语言的基石。跟 String 类同在一包下面的类还有两个 String 类的生成类: StringBuffer、StringBuilder，稍后会谈到它们之间的关系。

String 表示字符串，在 Java 中所有字符串的字面量（双引号中的字符串）都是 String 的实例，例如“abc”。String 对象是常量，在定义之后不能被改变，Stringbuild、StringBuffer 支持可变的字符串。正因为 String 对象是不可变的，所以可以是可共享的。一旦 String 对象被初始化，那么这个对象直到被 JVM 回收都不会发生改变。为了操作方便 Java 语言还重写了“+”操作符为连接操作符。当然，这虽然带来了便利，但也有不好的一面，当我们每使用一次 “+” 来连结两个字符串变量时，每进行一次 “+” 操作都会生成一个临时的字符串对象来存储运算过程中的中间值，造成性能低下。于是也就催生了 StringBuffer 以及后面的 StringBuilder 的诞生。

以下片段给出了 String 类的定义：

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence{
    /** The value is used for character storage. */  
    private final char value[];
}
```

final 关键字声明 String 是一个不可被继承的类，private final char value[] 是用来存储该字符串所表示的字符序列，可以看到它是被 final 关键字修饰的常量，也就是一经创建不能更改的。由此观之，String 类的实质就是一个不可变的字符数组。

## String 对象的创建

String 对象的创建方式可分为显式和隐式两种，显式创建就是我们常用的 new 关键字来创建一个对象，而隐式创建则是 JVM 运行时帮我们创建的。

1. 显式

   ```java
   String str = new String("new object"); 
   ```

   以上就是我们常见的用 new 关键字创建了一个 value 为 “new object”的字符串对象。对象的内存空间分配在 JVM 堆上，然后将对象的地址返回，str 变量的值就是该字符串对象在内存上的地址。

2. 隐式

   1. JVM 在加载包含 String 字面量的 class 或者 interface 的时候会隐式的创建一个表示 String 字面量的 String 对象。
   示例代码如下：

      ```java
      // 1.定义一个 String 字面量  
      String str = "test str";  
      // 2.根据 String 字面量显式创建一个字符串对象  
      String strObject = new String("test str");  
      // 3.常量表达式  
      String literalExp = "abc" + "def";
      ```

      以上三行代码中都有 String 类型字面量出现，字符串字面量值是在编译完成以后就会以二进制形式存在于 class 文件中，class 文件中有一部分区域叫做常量池，当类被加载的时候，虚拟机会根据常量池中的字符串字面量创建一个对应的 String 对象，内容相同的字符串字面量在类加载时只会被创建一个，即常量池中的字符串对象是可以被共享的。

      第二句代码，实际上是创建了两个 String 对象，一个是在类加载的时候由虚拟机隐式创建的；另外一个是在那行代码运行的时候根据字面量重新构建了另外一个新的 String 对象。

      第三句代码，使用 “+” 操作符进行了字符串连结操作。在编译器进行编译的时候会对常量表达式进行简单的运算因此在编译过后的 class 文件中我们看到常量池中存在的字面量值是 "abcdef" 而不是 "abc" 和 "def" 这两个 String 字面量。

      将上述三行代码反编译后我们看到如下结果：

      ![WX20180606-171147@2x](/Users/sean/Documents/GitHub/blogsource/static/images/WX20180606-171147@2x.png)

      如图所示，“test str”被共享，常量池中只有“abcdef”。

   2. String 的“+”连接操作符出现在变量表达式中。
   代码如下：

      ```java
      // 变量表达式  
      String a = "a";  
      String b = "b";  
      String c = a + b; 
      ```

      最后一句代码是对两个字符串变量进行 “+” 操作而不是对 String 字面量进行连结操作，所以这个连结操作在编译器处理的时候并不会进行运算，而是在代码真正执行的时候才进行计算的，并根据连结后的字符串在堆中生成一个新的 String 对象。

      如图所示：

      ![WX20180413-212518@2x](/Users/sean/Documents/GitHub/blogsource/static/image/WX20180413-212518@2x.png)

      在编译完成后的 class 文件中编译器会自己隐式的创建一个 StringBuilder 对象，然后利用 StringBuilder 连结字符串，并生成一个新的字符串对象。

      再来看下面这一段代码

      ```java
      package io.seanzhang.test.string;

      public class StringDemo {

          public static void main(String[] args) {
              String hello = "Hello", lo = "lo";
              System.out.println((hello == "Hello") + "");
              System.out.println((Other.hello == hello) + "");
              System.out.println((io.seanzhang.other.Other.hello == hello) + "");
              System.out.println((hello == "Hel" + "lo") + "");
              System.out.println((hello == "Hel" + lo) + "");
              System.out.println((hello == ("Hel" + lo).intern()) + "");
          }
      }

      class Other{
          static String hello = "Hello";
      }

      package io.seanzhang.other;

      public class Other{
          public static String hello = "Hello";
      }
      ```

      输入结果为：

      ```java
      true
      true
      true
      true
      false
      true
      ```

      这段代码主要说明了：

      1. 同包同类的相同字面量引用相同的字符串对象；
      2. 同包不同类的相同字面量引用相同的字符串对象；
      3. 不同包不同类的相同字面量引用相同的字符串对象；
      4. 常量表达式在编译器被运算，经由常量表达式获得的字符串也被当做字面量处理；
      5. 运行时才进行计算的字符串连结操作会创建新的字符串对象；
      6. 调用 intern() 会将之前已经存在的跟该字符串一样的字符串引用返回;

## JVM 中的 String 对象



## 常用方法



## 与 StringBuffer、StringBuilder的关系





