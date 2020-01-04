title: Java中的final关键字
author: ergz
tags:
  - Java
  - 面试题
categories:
  - Java基础
date: 2015-05-07 15:55:00
---
### 介绍
**Java中的final关键字非常重要，它可以应用于类、方法以及变量。这篇文章中我将带你看看什么是final关键字？将变量，方法和类声明为final代表了什么？使用final的好处是什么？最后也有一些使用final关键字的实例。final经常和static一起使用来声明常量，你也会看到final是如何改善应用性能的。**

### final关键字的含义?
final在Java中是一个保留的关键字，可以声明成员变量、方法、类以及本地变量。一旦你将引用声明作final，你将不能改变这个引用了，编译器会检查代码，如果你试图将变量再次初始化的话，编译器会报编译错误。

<!--more-->

### 什么是final变量？
凡是对成员变量或者本地变量(在方法中的或者代码块中的变量称为本地变量)声明为final的都叫作final变量。final变量经常和static关键字一起使用，作为常量。下面是final变量的例子：
	
	public static final String LOAN = "loan";
	LOAN = new String("loan") //invalid compilation error
final变量是只读的。

### 什么是final方法?
final也可以声明方法。方法前面加上final关键字，代表这个方法不可以被子类的方法重写。如果你认为一个方法的功能已经足够完整了，子类中不需要改变的话，你可以声明此方法为final。final方法比非final方法要快，因为在编译的时候已经静态绑定了，不需要在运行时再动态绑定。下面是final方法的例子：

	class PersonalLoan{
    public final String getName(){
        return "personal loan";
    	}
	}
 
	class CheapPersonalLoan extends PersonalLoan{
    @Override
    public final String getName(){
		//compilation error: overridden method is final
        return "cheap personal loan"; 
       }
	}
### 什么是final类？
使用final来修饰的类叫作final类。final类通常功能是完整的，它们不能被继承。Java中有许多类是final的，譬如String, Interger以及其他包装类。下面是final类的实例：

	final class PersonalLoan{
 
    }
 	//compilation error: cannot inherit from final class
    class CheapPersonalLoan extends PersonalLoan{  
	}
### final关键字的好处
下面总结了一些使用final关键字的好处

- final关键字提高了性能。JVM和Java应用都会缓存final变量
- final变量可以安全的在多线程环境下进行共享，而不需要额外的同步开销
- 使用final关键字，JVM会对方法、变量及类进行优化

### 不可变类
创建不可变类要使用final关键字。不可变类是指它的对象一旦被创建了就不能被更改了。String是不可变类的代表。不可变类有很多好处，譬如它们的对象是只读的，可以在多线程环境下安全的共享，不用额外的同步开销等等。

### 关于final的重要知识点

- final关键字可以用于成员变量、本地变量（局部变量）、方法以及类
- final成员变量必须在声明的时候初始化或者在构造器中初始化，否则就会报编译错误
- 不能够对final变量再次赋值
- 局部变量必须在声明时赋值
- 在匿名类中使用的所有外部变量都必须是final变量（*）
- final方法不能被重写
- final类不能被继承
- final关键字不同于finally关键字，后者用于异常处理
- final关键字容易与finalize()方法搞混，后者是在Object类中定义的方法，是在垃圾回收之前被JVM调用的方法
- 接口中声明的所有变量本身是final的（*）
- final和abstract这两个关键字是反相关的，final类就不可能是abstract的
- final方法在编译阶段绑定，称为静态绑定(static binding)
- 没有在声明时初始化final变量的称为空白final变量(blank final variable)，它们必须在构造器中初始化，或者调用this()初始化。不这么做的话，编译器会报错“final变量(变量名)需要进行初始化”
- 将类、方法、变量声明为final能够提高性能，这样JVM就有机会进行估计，然后优化
- 按照Java代码惯例，final变量就是常量，而且通常常量名要大写：
	
	private final int COUNT = 10;
- 对于集合对象声明为final指的是引用不能被更改，但是你可以向其中增加，删除或者改变内容，譬如：

	private final List Loans = new ArrayList();
	list.add(“home loan”);  //valid
	list.add("personal loan"); //valid
	loans = new Vector();  //not valid

我们已经知道final变量、final方法以及final类是什么了。必要的时候使用final，能写出更快、更好的代码的。


**本文链接：[Java中的final关键字]()
欢迎转载，请注明出处！**