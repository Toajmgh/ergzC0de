title: 面试官：写一下单例模式吧，知道单例模式有几种写法吗
author: ergz
tags:
  - Java
  - 面试题
categories:
  - 搞定Java面试题
date: 2019-01-28 12:24:00
---
参考之前的一篇文章：[深入Java单例模式](http://www.ergzcode.com/2016/05/22/design-pattern-for-singleton.html)

### 介绍
设计模式很多，单例模式绝对是问的最多的一个了，也是让手写的最多的一个设计模式。很多人只会写双重检测的设计模式，但问的深一点其实很多人答不上来。比如，变量为什么要用volatile修饰，为什么要写2个if，一个if不行吗？还有就是单例模式有很多种写法。我以演进的方式写一个单例模式的5种写法，面试的时候建议写双重检测的写法和静态内部类的写法。

有一些对象其实我们只需要一个，比如说：线程池，缓存，对话框，处理偏好设置和注册表的对象，日志对象，充当打印机，显卡等设备的驱动程序的对象。事实上，这类对象只能看到一个实例，如果制造出多个实例，就会导致许多问题产生，例如：程序的行为异常，资源使用过量，或者不一致的结果。

<!--more-->

### 第一种（懒汉）

	//code1
	public class Singleton {
   	 	private static Singleton uniqueInstance;

    	private Singleton(){}

    	public static Singleton getInstance(){
        	if (uniqueInstance == null){
            	uniqueInstance = new Singleton();
        	}
        	return uniqueInstance;
    	}
	}

当2个线程同时进入getInstance()的if语句里面，会返回2个不同的实例，因此这种方式是线程不安全的

	//code2
	public class Singleton {
   	 	private static Singleton uniqueInstance;

    	private Singleton(){}
		//同步方法
    	public static synchronized Singleton getInstance(){
        	if (uniqueInstance == null){
            	uniqueInstance = new Singleton();
        	}
        	return uniqueInstance;
    	}
	}

用所synchronized修饰可以保证线程安全，但是只有第一次执行此方法时才需要同步，设置好uniqueInstance，就不需要同步这个方法了，之后每次调用这个方法，同步都是一种累赘。

### 双重检查锁定（推荐）
**synchronized锁粒度太大**，人们就想到通过双重检查锁定来降低同步的开销，下面是实例代码

	//code3
	public class Singleton {
    	private static Singleton uniqueInstance;

    	private Singleton(){}

    	public static Singleton getInstance(){
       	 if (uniqueInstance == null){
				//同步代码块
            	synchronized(Singleton.class){
                	if (uniqueInstance == null){
                    	uniqueInstance = new Singleton();
                	}
            	}
        	}
        	return uniqueInstance;
    	}
	}

如上面的代码所示，如果第一次检查uniqueInstance不为null，那么就不需要执行下面的加锁和初始化操作，可以大幅降低synchronized带来的性能消耗，当在多线程环境下试图在同一时间创建对象时，会通过加锁来保证只有一个线程能创建对象，这就保证了只有一个实例的创建。

经常有人对code3中，为什么要执行2次if语句不太清楚，简答的描述一下，有可能有AB2个线程同时进入第一个if语句，然后A拿到锁，创建对象完成。因为B线程也判断了uniqueInstance为null,如果不再做一次判空处理的话，B拿到锁后会重新创建对象，但是加了第二个if语句后，就直接返回退出了。

双重检查锁定看起来似乎很完美，但这里一个错误的优化！在线程执行到getInstance()方法的第4行时，代码读取到uniqueInstance不为null，uniqueSingleton引用指向的对象有可能还没有完成初始化（半初始化）。

简单概述一下《Java并发编程的艺术》的解释，

uniqueInstance = new Singleton()可以分解为如下三行代码：

	memory = allocate();//1：申请一块内存空间分配给对象
	ctorInstance(memory);//2:调用构造方法初始化对象
	uniqueInstance = memory;//3：设置uniqueInstance指向刚分配的内存地址

3行伪代码中的2和3之间，可能会被重排序，重排序后执行的顺序如下

	memory = allocate();//1：申请一块内存空间分配给对象
	uniqueInstance = memory;//3：设置uniqueInstance指向刚分配的内存地址
							 //注意：此时对象还没有初始化
	ctorInstance(memory);//2:调用构造方法初始化对象

多个线程访问时可能出现如下的情况

时间 | 线程A|  线程B
-|-|-
11 | A1：分配对象的内存空间 |
12 | A3：设置uniqueInstance指向的内存地址 |
13 |  | B1：判断uniqueInstance是否为空 
14 |  | B2：由于uniqueInstance不为null，线程B访问uniqueInstance引用的对象
15 | A2：初始化对象  |
16 | A4：访问instance引用的对象 |

这样会导致线程B访问到一个还没有初始化的对象，此时可以用volatile修饰Singleton,这样3行伪代码中的2和3之间的重排序，在多线程环境中将会被禁止

	//code4
	public class Singleton {
	    private  volatile static Singleton uniqueSingleton;
	
	    private Singleton(){}
	
	    public static  Singleton getInstance(){
	        if (uniqueSingleton == null){
	            synchronized(Singleton.class){
	                if (uniqueSingleton == null){
	                    uniqueSingleton = new Singleton();
	                }
	            }
	        }
	        return uniqueSingleton;
	    }
	}

### 第三种（饿汉）
如果应用程序总是创建并使用单例模式，或者在创建和运行时方面的负担不太繁重，我们可以以饿汉式的方式来创建单例
code5（静态常量）

	//code5
	public class Singleton {
	    private static final Singleton INSTANCE =new Singleton();
	
	    private Singleton(){}
	
	    public static  Singleton getInstance(){
	        return INSTANCE;
	    }
	}

code6（静态代码块）

	//code6
	public class Singleton {
	    private static Singleton instance;
	
	    private Singleton(){}
	
	    static {
	         uniqueSingleton = new Singleton();
	    }
	
	    public static  Singleton getInstance(){
	        return instance;
	    }
	}
在类加载的时候直接创建这个对象，这样既可以提高效率，又能保证线程安全，code5和code6几乎没有区别，因为静态成员变量和静态代码块都是类初始化的时候加载执行。

### 第四种静态内部类（推荐用）

	//code7
	public class Singleton {
	    private static class SingletonHolder{
	        private static final Singleton instance = new Singleton();
	    }
	
	    private Singleton(){}
	
	    public static Singleton getInstance(){
	        return SingletonHolder.instance ;
	    }
	}

饿汉式的方式只要Singleton类被装载了，那么instance就会被实例化（没有达到lazy loading的效果），而这种方式是Singleton类被装载了，instance不一定被初始化。因为SingletonHolder类没有被主动使用，只有显示的通过调用getInstance()方法时，才会显示装载SingletonHolder类，从而实例化instance。

### 第五种枚举（推荐用）

	public enum Singleton {
	    INSTANCE;
	    public void whateverMethod() {
	 
	    }
	}


枚举是公认实现单例的最好方式。借助JDK1.5中添加的枚举来实现的单例模式，不仅能避免多线程安全问题，而且还能防止反序列化和反射对单例的破坏问题。

### 总结
不管采用何种方案，请记住使用单例的三大点：

- 线程安全
- 延迟加载
- 序列化和反序列化安全

**本文链接：[面试官：写一下单例模式吧，知道单例模式有几种写法吗]()
欢迎转载，请注明出处！**