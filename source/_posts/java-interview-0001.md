title: 面试官：说一下八种基本数据类型及其包装类吧
author: ergz
tags:
  - Java
  - 面试题
categories:
  - 搞定Java面试题
date: 2019-01-24 22:40:00
---
### 介绍
**Java基本数据类型分为4大类：**

- 整数型：byte,short,int,long
- 浮点型：flost,double
- 逻辑型：boolean
- 字符型：char

原始类型 | 原始类型所占字节数 |  包装类  
-|-|-
byte | 1个字节 | Byte |
short | 2个字节 | Short |
int | 4个字节 | Integer |
long | 8个字节 | Long |
float | 4个字节 | Float |
double | 8个字节 | Double |
boolean | 1个字节 | Boolean |
char | 2个字节 | Character |

<!--more-->
### 常见面试题
要注意的是基本数据的包装类很多都实现了享元模式。享元模式就是运用共享技术有效地支持大量细粒度对象的复用。用一个常见的面试题来解释

**1.判断如下代码的输出，并说出原因**

    Integer a1 = 40;
	Integer a2 = 40;
	System.out.println(a1==a2);
	
	Integer a3 = 200;
	Integer a4 = 200;
	System.out.println(a3 == a4);

由自动装箱和拆箱可以知道这2种写法是等价的
	
	Integer a1 = 40;
	Integer a1 = Integer.valueOf(40);
看一下Integer.valueOf(int i)的方法
	
	//...valueOf(int):Integer
    public static Integer valueOf(int i) {
        assert IntegerCache.high >= 127;
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }

	//...IntegerCache
	private static class IntegerCache {
        static final int low = -128;
        static final int high;
        static final Integer cache[];

        static {
            // high value may be configured by property
            int h = 127;
            String integerCacheHighPropValue =
                sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
            if (integerCacheHighPropValue != null) {
                int i = parseInt(integerCacheHighPropValue);
                i = Math.max(i, 127);
                // Maximum array size is Integer.MAX_VALUE
                h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
            }
            high = h;

            cache = new Integer[(high - low) + 1];
            int j = low;
            for(int k = 0; k < cache.length; k++)
                cache[k] = new Integer(j++);
        }

        private IntegerCache() {}
    }
IntegerCache是Integer的静态内部类，默认创建了[-128,127]的对象，并放到IntegerCache内部的一个cache数组中，在[-128,127]这个范围内的整数对象，不用创建。直接从IntegerCache中的cache数组中根据下标拿就可以了,超出这个范围的每次去创建新的对象。其他几种包装类型的常量池和Integer的思路差不多，源码都很相似。
所以答案如下：
	
	Integer a1 = 40;
	Integer a2 = 40;
	System.out.println(a1 == a2);//true
	
	Integer a3 = 200;
	Integer a4 = 200;
	System.out.println(a3 == a4);//false

包装类缓存数据范围如下：

包装类 | 缓存范围
-|-
Byte | -128~127 
Short | -128~127 
Integer | -128~127
Long | -128~127
Character | 0~127

**2.Java一个char类型可以存储中文吗？**

可以，因为Java中使用了Unicode字符，不论是中文还是英文固定占用2个字节。

	char a = '中'；
	// 中
	System.out.println(a);	

**3.什么是自动装箱，自动拆箱**

自动装箱就是Java自动将原始类型转换为对应的对象，比如将int的变量转换为Integer对象，这个过程就是装箱，反之将Integer对象转换为int类型的值，这个过程就是拆箱。因为这里的装箱和拆箱是自动进行的非人为转换的，所以就称作自动装箱和自动拆箱。

自动装箱时编译器调用ValueOf将原始类型值转换为对象，同时自动拆箱时，编译器通过调用类似intValue(),doubleValue()这类的方法将对象转换为原始类型值
	
	//jdk1.5之前的写法
	Integer tempNum1 = Integer.valueOf(5);
	int num1 = tempNum1.intValue();
	
	//jdk1.5之后的写法，自动装箱和拆箱
	Integer tempNum2= 5;
	int num1 = tempNum2;

**4.为什么要需要基本数据类型的包装类**

（1）Java是面向对象的语言，很多地方是需要使用的是对象而不是基本数据类型。例如，List,Map等容器类中基本数据类型是放不进去的。
（2）包装类在原先的基本数据类型上，新增加了很多方法，比如Integer.valueOf(String s)等
	
**5.既然包装类能完成所有的功能，为啥还需要基本数据类型**

基本数据类型基于数值，对象类型基于引用。基本数据类型存储在栈的局部变量表中。

而对象类型的变量则是存储堆中引用，实例放在堆中，因此对象类型的变量需要占用更多的内存空间，基本数据类型因为占用更少内存被保留下来。

**6.写出如下代码的输出**

	Integer i1 = 40;
	Integer i2 = 40;
	Integer i3 = 0;
	Integer i4 = new Integer(40);
	Integer i5 = new Integer(40);
	Integer i6 = new Integer(0);
	
	System.out.println(i1 == i2);
	System.out.println(i1 == i2 + i3);
	System.out.println(i1 == i4);
	System.out.println(i4 == i5);
	System.out.println(i4 == i5 + i6);
	System.out.println(40 == i5 + i6);


输出及解释如下
	
	Integer i1 = 40;
	Integer i2 = 40;
	Integer i3 = 0;
	Integer i4 = new Integer(40);
	Integer i5 = new Integer(40);
	Integer i6 = new Integer(0);
	
	System.out.println(i1 == i2);//true
	//Integer.valueOf()用了常量池，看上面的源码
	//比较是内存地址

	System.out.println(i1 == i2 + i3);//true
	// + 操作会导致左右两边都转换为基本数据类型
	//比较是值

	System.out.println(i1 == i4);//false
	//Integer.valueOf()使用常量池中的对象
	//new Integer每次都会创建新对象
	//比较是内存地址

	System.out.println(i4 == i5);//false
	//new关键字会创建新的实例所以这是2个不同的对象
	//比较是内存地址

	System.out.println(i4 == i5 + i6);//true
	//解释在下面，比较的是值


	System.out.println(40 == i5 + i6);//true
	//解释在下面，比较的是值

语句i4 = i5 + i6,因为这个 + 操作符不适用于Integer对象，首先i5和i6进行了自动拆箱的操作，进行数值相加，即i4 == 40。然后Integer对象无法与数值进行直接比较，所以i4自动拆箱转为int数据类型，值为40，最终这条语句转为40 == 40进行数值的比较。


**本文链接：[面试官：说一下八种基本数据类型及其包装类吧](http://www.ergzcode.com/2019/01/24/java-interview-0001.html)
欢迎转载，请注明出处！**

