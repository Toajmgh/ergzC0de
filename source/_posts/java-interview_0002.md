title: 面试官：说一下构造函数，静态代码块，构造代码块的加载顺序
author: ergz
tags:
  - Java
  - 面试题
categories:
  - 搞定Java面试题
date: 2019-01-25 11:00:00
---
### 介绍

**构造函数，静态代码块，构造代码块的执行顺序**
	
	class Test{
		//静态代码块1
		static{
			System.out.println("我的静态代码块1");		
		}
		//构造代码块1
		{
			System.out.println("我的构造代码块1");
		}
		//构造函数1
		public Test(){
			System.out.println("我是无参数构造函数");			
		}
		//构造函数2
		public Test(int t){
			System.out.println("我是带参数构造函数，"+"参数是"+t);			
		}

		//静态代码块2
		static{
			System.out.println("我的静态代码块2");		
		}
		//构造代码块2
		{
			System.out.println("我的构造代码块2");
		}
	} 

<!--more-->
来New一个Test类看看会输出什么？

	public class TestShow{
		public static void main(String[] args){
			/*创建第一个对象
			 我是静态代码块1
			 我的静态代码块2
			 我的构造代码块1
			 我的构造代码块2
			 我是无参数构造函数
			*/
			System.out.println("创建第一个对象：");
			Test test1 = new Test();
			
			/*创建第二个对象
			  我的构造代码块1
			  我的构造代码块2
			  我是带参数构造函数，参数是5
			*/
			System.out.println("创建第二个对象：");
			Test test2 = new Test(5);
		}	

	}

从输出我们可以总结处如下结论：

执行时间：**静态代码块 > 构造代码块 > 构造函数，静态代码块只会在类被加载入内存时加载一次，构造代码块和构造函数都是在对象创建的时候执行，有几个对象就会执行几次**，所以一般将加载配置文件的过程写在静态代码块中。

**没有继承的加载顺序**

先定义一个类作为成员变量，方便看打印输出
	
	public class Parameter {
		public Parameter(String str){
			System.out.println(str);
		}	
	}

写一个测试类
	
	public Test1{
		public static Prarmeter parameter1 =  new Parameter("静态成员变量");
		public Prarmeter parameter2 =  new Parameter("非静态成员变量");	
		public Test1(){
			System.out.println("构造函数");		
		}
		//静态代码块
		static{
			System.out.println("静态代码块");		
		}
		//构造代码块a
		{
			System.out.println("构造代码块a");
		}
		//构造代码块b
		{
			System.out.println("构造代码块b");
		}
	}

输出如下
	
	public class ForShow{
		public static void main(String[] args){
			/*静态成员变量
			  静态代码块
			  非静态成员变量
			  构造代码块a	
			  构造代码块b
			  构造函数
			*/
			new Test1();
			/*非静态成员变量
			  构造代码块a	
			  构造代码块b
			  构造函数
			*/
			new Test1();
		}	
	
	}

**可以看出静态成员变量和静态代码块一样，只会在类被加载到内存时加载唯一的一次**

改变一下Test类中定义的顺序，看看有什么变化。

	public Test2{
		
		//静态代码块
		static{
			System.out.println("静态代码块");		
		}
		//构造函数
		public Test2(){
			System.out.println("构造函数");		
		}
		//构造代码块b
		{
			System.out.println("构造代码块b");
		}
		//构造代码块a
		{
			System.out.println("构造代码块a");
		}
		public static Prarmeter parameter1 =  new Parameter("静态成员变量");
		public Prarmeter parameter2 =  new Parameter("非静态成员变量");	
	}

输出如下
	
	public class ForShow{
		public static void main(String[] args){
			
			/*静态代码块
			  静态成员变量
			  构造代码块b	
			  构造代码块a
			  非静态成员变量
			  构造函数
			*/
			new Test2();
		}	
	
	}

**没有继承情况的执行顺序**

- 静态代码块和静态成员变量，加载的顺序由编写的顺序决定
- 构造代码块和非静态成员变量，加载顺序由编写顺序决定
- 构造函数

**有继承情况的执行顺序**

定义父类
	
	public class Father{
		public static Prarmeter parameter1 =  new Parameter("父类静态成员变量");
		public Prarmeter parameter2 =  new Parameter("父类非静态成员变量");	
		
		public Father(){
			System.out.println("父类构造函数");		
		}
	}

定义子类

	public class Son extends Father{
		public static Prarmeter parameter1 =  new Parameter("子类静态成员变量");
		public Prarmeter parameter2 =  new Parameter("子类非静态成员变量");	
		
		public Son(){
			System.out.println("子类构造函数");		
		}
	}

**有继承情况的执行顺序**

- 父类的静态（静态代码块，静态成员变量），子类的静态（静态代码块，静态成员变量）
- 父类的非静态（构造代码块，非静态成员变量），父类的构造函数
- 子类的非静态（构造代码块，非静态成员变量），子类的构造函数

记住这几条规则足以应付这一类型的所有面试题

### 常见的面试题

**1.Java类定义如下，写出main函数执行后的输出结果**
	
	public class A{
		static {
			System.out.println("static A");
		}	
		public A(){
			System.out.println("class A");
		}
	}

	public class B extends A{
		public B(){
			System.out.println("class B");
		}
		public static void main(String[] args){
			B instance = new B();			
		}
	}

上面的知识点如果理解的很清楚的话，这个结果立马就能写出来

所以输出如下
	
	static A
	class A
	class B

**2.问题是如果main函数中的代码如下呢？就是B这个类new了2次**
	
	public static void main(String[] args){
			B instance1 = new B();
			B instance2 = new B();			
		}

万变不离其宗，输出如下
	
	static A
	class A
	class B
	class A
	class B

因为静态成员变量和静态代码块只会在类被载入内存时加载一次。


**本文链接：[面试官：说一下构造函数，静态代码块，构造代码块的加载顺序]()
欢迎转载，请注明出处！**