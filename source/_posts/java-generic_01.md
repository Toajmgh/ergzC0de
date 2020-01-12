title: 给程序员和开发者的10道Java泛型面试题
author: ergz
tags:
  - 面试题
categories:
  - Java基础
date: 2015-05-25 19:03:00
---
title: 给程序员和开发者的10道Java泛型面试题
author: ergz
tags: [面试题]
categories:
  - Java基础
date: 2015-05-25 19:03:00
---
## 介绍
关于泛型的面试题在Java面试中变得越来越常见，因为Java 5问世已经有相当长的时间了，越来越多的应用已经迁移到Java 5上来了，并且几乎所有新的Java开发工作也都是在Tiger(Java 5的项目代号)版本上进行的。泛型和其它Java 5特性比如 枚举、自动拆装箱、可变参数，还有集合工具类比如 CountDownLatch、CyclicBarrier和BlockingQueue ，这些在Java面试中变得越来越流行。如果你对 泛型的限定通配符和非限定通配符不熟悉，那么泛型面试题就会变得非常的棘手。 Java内部泛型是怎么工作的呢，答案是**类型擦除**，此外程序员也应当精通于编写带参数的Java泛型类和泛型方法。准备泛型面试的最好的方法是，尝试编写一些简单的程序来了解泛型的各种特性。而本文中，我们将会看到一些流行的Java泛型面试题及其答案。顺便说一下，Javarevisited网站上提供了很多资料来帮助你更好的为Java和J2EE面试做准备，你可以查看[15个线程面试题](http://javarevisited.blogspot.sg/2011/07/java-multi-threading-interview.html)和[排名前10的Java集合类面试题](http://javarevisited.blogspot.sg/2011/11/collection-interview-questions-answers.html)来准备多线程和集合类方面的知识，另外还有关于Spring、Struts、JSP和Servlet的问答文章。如果你是GUI开发人员，并且使用的是Java Swing技术，那么你也可以查看投资银行面试中通常会问到的Java Swing面试题。

<!--more-->

## Java泛型面试题
### 1. Java中的泛型是什么 ? 使用泛型的好处是什么?  
这是在各种Java泛型面试中，一开场你就会被问到的问题中的一个，主要集中在初级和中级面试中。那些拥有Java1.4或更早版本的开发背景的人都知道，在集合中存储对象并在使用前进行**类型转换**是多么的不方便。泛型防止了那种情况的发生。它提供了编译期的类型安全，确保你只能把正确类型的对象放入集合中，避免了在运行时出现ClassCastException。

### 2. Java的泛型是如何工作的 ? 什么是类型擦除 ?  
这是一道更好的面试题。泛型是通过**类型擦除**来实现的，编译器在编译时擦除了所有类型的相关信息，所以在运行时不存在任何类型相关的信息。例如List&#60;String&#62;在运行时仅用一个List来表示。这样做的目的，是确保能和Java5之前的版本开发二进制类库（jar）进行兼容。你无法在运行时访问到类型参数，因为编译器已经把泛型类型转换成了**原始类型**。根据你对这个泛型问题的回答情况，你会得到一些后续的提问，比如为什么泛型是由类型擦除来实现的或者给你展示一些会导致编译器出错的泛型代码。请阅读我的[Java中泛型是如何工作的](http://javarevisited.blogspot.com/2011/09/generics-java-example-tutorial.html)来了解更多信息。

### 3. 什么是泛型中的限定通配符和非限定通配符 ?
这是另外一个非常流行的Java泛型面试题。限定通配符对类型进行了限制。有2种限定通配符，一种是&#60;? extends T&#62;它通过确保类型必须是T的子类来设定类型的上限，另一种是&#60;? super T&#62;它确保类型必须是T的父类来设定类型的下限。泛型类型必须用限定内的类型来进行初始化，否则会导致编译错误。另一方面&#60;?&#62;表示非限定通配符，因为&#60;?&#62;可以使用任意类型来替代。更多信息请参阅我的文章[泛型中的限定通配符和非限定通配符的区别。](http://javarevisited.blogspot.com/2012/04/what-is-bounded-and-unbounded-wildcards.html)

### 4. List&#60;? extends T&#62;和List &#60;? super T&#62;之间有什么区别 ?  
这和上一个面试题有联系，有时面试官会用这个问题来评估你对泛型的理解，而不是直接问你什么是限定通配符和非限定通配符。这两个List的声明都是限[定通配符的例子](http://javarevisited.blogspot.sg/2012/04/what-is-bounded-and-unbounded-wildcards.html)，List&#60;? extends T&#62;可以接受任何继承自T的类型的List，而List &#60;? super T&#62;可以接受任何T的父类构成的List。例如List &#60;? super Number&#62;可以接受List&#60;Integer&#62;或List&#60;Float&#62;。在本段出现的链接中可以找到更多信息。

### 5. 如何编写一个泛型方法，让它能接受泛型参数并返回泛型类型?
编写泛型方法并不困难，你需要用泛型类型来替代原始类型，比如使用T，E，or K,V等被广泛认可的类型占位符。泛型方法的例子请参阅Java集合框架。最简单的情况下，一个泛型方法可能会像这样：

	public V put(K key, V value) {
        return cache.put(key, value);
	}

### 6. Java中如何使用泛型编写带有参数的类? 
这是上一道面试题的延伸。面试官可能会要求你用泛型编写一个类型安全的类，文不是编写一个泛型方法。关键仍然是使用泛型替代原始类型，而且要使用JDK中采用的标准占位符。

### 7. 编写一段泛型程序来实现LRU缓存? 
对于喜欢Java编程的人来说这相当于是一次练习。给你个提示，LinkedHashMap可以用来实现固定大小的LRU缓存，当LRU缓存已经满了的时候，它会把最老的键值对移出缓存。LinkedHashMap提供了一个称为removeEldestEntry()的方法，该方法会被put()和putAll()调用来删除最老的键值对。当然，如果你已经编写了一个可运行的JUnit测试，你也可以随意编写你自己的实现代码。参考：[LinkedHashMap实现LRU算法](#)

### 8.你可以把List&#60;String&#62;类型的对象传递给一个接受List&#60;Object&#62;类型参数的方法吗？   
对任何一个不太熟悉泛型的人来说，这个Java泛型题目看起来令人困惑，因为乍看起来String是一种Object，所以List&#60;String&#62;应当用在需要List&#60;Object&#62;的地方，但是事实上并非如此。真这样做的话会导致编译出错。如果你再深一步考虑，你会发现Java这样做是有意义的，因为List&#60;Object&#62;可以存储
任意类型的对象包括String，Integer等等，而List&#60;String&#62;却只能用来存储Strings。

	List<Object> objectList;
	List<String> stringList;
	     
	objectList = stringList;  //compilation error incompatible types

### 9. Array中可以用泛型吗?   
这可能是Java泛型面试题中最简单的一个了，当然前期是你要知道Array事实上并不支持泛型，这也是为什么Joshua Bloch在Effective Java一书中建议使用List来代替Array，因为List可以提供编译器的类型安全，而Array却不能。

### 10. 如何阻止Java中的类型未检查的警告? 
如果你把泛型和原始类型混合起来使用，例如下列代码，Java5的javac编译器会产生类型未检查的警告，例如：

	List<String> rawList = new ArrayList()
	注意: Hello.java使用了未检查或称为不安全的操作;

这种警告可以使用@SuppressWarnings("unchecked")注解来屏蔽。

## Java泛型面试题补充更新:  
我手头又拿到了几个Java泛型面试题跟大家分享下，这几道题集中在泛型类型和原始类型的区别上，以及我们是否可以用Object来代替限定通配符的使用等等：

### Java中List&#60;Object&#62;和原始类型List之间的区别?    
原始类型和带参数类型&#60;Object&#62;之间主要区别是，在编译时编译器不会对原始类型进行类型安全检查，却会对带参数的类型进行检查，通过使用Object作为类型，可以告诉编译器该方法可以接受任何类型的对象，比如String或Integer。这道题的考察点在于对泛型中原始类型的正确理解。他们之间的第二点区别是，你可以把任何带参数的类型传递给原始类型List，但却不能把List&#60;String&#62;传递给接受List&#60;Object&#62;的方法，因为会产生编译错误。更多详细信息请参阅[Java中的泛型是如何工作的](http://javarevisited.blogspot.sg/2011/09/generics-java-example-tutorial.html)。

### Java中List&#60;?&#62;和List&#60;Object&#62;之间的区别是什么?  
这道题跟上一道题看起来很像，实际上却完全不同。List&#60;?&#62;是一个未知类型的List，而List&#60;Object&#62;其实是任意类型的List。你可以把List&#60;String&#62;，List&#60;Integer&#62;赋值给List&#60;?&#62;，却不能把List&#60;String&#62;赋值给List&#60;Object&#62;。

	List<?> listOfAnyType;
	List<Object> listOfObject = new ArrayList<Object>();
	List<String> listOfString = new ArrayList<String>();
	List<Integer> listOfInteger = new ArrayList<Integer>();
	     
	listOfAnyType = listOfString; //legal
	listOfAnyType = listOfInteger; //legal
	listOfObject = (List<Object>) listOfString; //compiler error - in-convertible types 

想了解更多关于通配符的信息请查看[Java中的泛型通配符示例](http://javarevisited.blogspot.sg/2011/09/generics-java-example-tutorial.html)


### List&#60;String&#62;和原始类型List之间的区别.  

该题类似于“原始类型和带参数类型之间有什么区别”。带参数类型是类型安全的，因为编译时会对其进行类型安全检查，其类型安全是由编译器保证的，但原始类型List却不是类型安全的。你不能把String之外的任何其他类型的Object存入String类型的List中。使用泛型的带参数类型你不需要进行类型转换，但是对于原始类型，你则需要进行显示的类型转换。

	List listOfRawTypes = new ArrayList();
	listOfRawTypes.add("abc");
	listOfRawTypes.add(123); //编译器允许这样 - 运行时却会出现异常
	String item = (String) listOfRawTypes.get(0); //需要显式的类型转换
	item = (String) listOfRawTypes.get(1); //抛ClassCastException，因为Integer不能被转换为String
	     
	List<String> listOfString = new ArrayList();
	listOfString.add("abcd");
	listOfString.add(1234); //编译错误，比在运行时抛异常要好
	item = listOfString.get(0); //不需要显式的类型转换 - 编译器自动转换 

这些都是Java泛型面试中频繁出现的问题及其答案。所有的这些面试题都不困难，其实他们都是基于泛型的基础知识。任何对泛型有不错了解的Java程序员都肯定熟知这些泛型题目。如果你有任何好的面试题，不管是在什么面试中碰到的，或者如果你想知道任何Java泛型面试题的答案，请在评论中留言。


**原文链接：[给程序员和开发者的 10 道 Java 泛型面试题](https://www.oschina.net/translate/10-interview-questions-on-java-generics)  
欢饮转载，请注明出处！**
