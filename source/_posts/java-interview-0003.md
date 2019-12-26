title: 面试官：说一下Spring StringBuffer StringBuilder的区别
author: ergz
tags:
  - Java
  - 面试题
categories:
  - 搞定Java面试题
date: 2019-01-26 22:23:00
---
### 介绍

先来看String类的实现

	public final class String implements  	java.io.serializable,Comparable<String>,CharSequence{
		/** The value is used for character storage **/

		private final char value[];
	}

先来说说final关键字的作用
 
 - final修饰类时，表明这个类不能被继承
 - final修饰方法时，表明方法不能被重写
 - final修饰变量时，如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改；如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象
 
<!--more-->

可以看到String类和保存变量的value数组都被final修饰，表明String类是不可变的。

StringBuffer和StringBuilder都继承自AbstractStringBuilder类，看一下AbstractStringBuilder类的定义
	
	abstract class AbstractStringBuilder implements Appendable,CharSequence{
		/**
		 *The value is userd for character storage
		 */
		char[] value;
		}

看到区别了吗？ value数组没有用private和final修饰，说明了StringBuffer和StringBuilder是可变的。

而StringBuilder和StringBuffer的方法是差不多的，只不过StringBuffer在方法上添加了`synchronized`关键字，所以在多线程环境下用StringBuffer来获得更高的效率。

看2个类中同一个方法的定义

StringBUffer类

	//StringBuffer append方法
	@Override
	public synchronized StringBuffer append(char[] str){
		toStringCache = null;
		super.append(str);	
		return this;
	}

StringBuilder类

	//StringBuilder append方法
	@Override
	public StringBuilder append(char[] str){
		super.append(str);
		return this;
	}

因为StringBuffer和StringBulider的实现类似，所以性能比较就落在String和StringBuilder之间了。

1.String是不可变对象，每次操作都会生成新的String对象，然后指针指向新的对象。

2.抽象类AbstractStringBuilder内部提供了一个自动扩容机制，当发现长度不够的时候，会自动进行扩容工作（具体扩容可以看源码，很容易理解），会创建一个新的数组，并将原来数组的数据复制到新的数组，不会创建新的对象，所以拼接字符串的效率高。

用源码证实一下

String类
	
	public String substring(int beginIndex) {
        if (beginIndex < 0) {
            throw new StringIndexOutOfBoundsException(beginIndex);
        }
        int subLen = value.length - beginIndex;
        if (subLen < 0) {
            throw new StringIndexOutOfBoundsException(subLen);
        }
        return (beginIndex == 0) ? this : new String(value, beginIndex, subLen);
    }

StringBuilder类

	public StringBuilder append(String str) {
        super.append(str);
        return this;
    }

介绍完毕，所以你应该知道这道题应该怎么答了

### 常见面试题

**1. 说一下String StringBuffer StringBuilder的区别**

- 都是final类，不能被继承
- String长度是不可变的，StringBuffer和StringBuilder长度是可变的
- StringBuffer是线程安全的，StringBuilder不是线程安全的。但他们的方法很相似，StringBuffer在方法上添加了`synchronized`关键字修饰，保证了线程安全
- StringBuilder比StringBuffer拥有更好的性能
- 如果一个String类型的字符串，在编译时可以确定是一个字符串常量，则编译完成后，字符串会自动拼接成一个常量，此时String比StringBuffer和StringBuilder的性能好的多

我用例子解释一下第五条

	String a = "a";
	String b = "b";
	String c = a+b;
	String d = "a"+"b"+"c";

反编译class的文件是这样的
	
	String a = "a";
	String b = "b";
	(new StringBuilder()).append(a).append(b).toString();
	String d = "abc";

看到String d,理解了吗？

同时看c的拼接过程，先生成一个StringBuilder对象，再调用2次append方法，最后再返回一个String对象，知道StringBuilder比String慢的愿意了吧。

**本文链接：[面试官：说一下Spring StringBuffer StringBuilder的区别](http://www.ergzcode.com/2019/01/26/java-interview-0003.html)
欢迎转载，请注明出处！**