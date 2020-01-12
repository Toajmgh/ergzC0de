title: 深入Java反射机制
author: ergz
tags: [Java]
categories:
  - Java基础
date: 2015-05-14 22:13:00
---
### java反射机制的认识

Java的反射机制是在**运行状态中**（程序运行时），对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取类自身的信息及动态调用对象的方法的功能称为Java语言的反射机制。
简单来说，反射机制是指程序运行时能够获取自身的信息。在java中，只要给定类的名称，那么久可以通过反射机制来获取来的所有信息。

<!--more-->

在Object中有个getClass的方法,此方法被所有的子类继承。
	
	public final Class<?> getClass()

以上的方法返回值是一个**Class类型**,实际上此类是java反射的源头,也就是说可以通过对象反射来获得类的所有信息.  
Class本身表示一个类的本身,通过Class可以完整的得到一个类的完整结构,包括此类中的方法定义,属性定义.

### Class类的常用方法：  

方法摘要 | 说明
-|-
public static Class<?> forName(String className) throws ClassNotFoundException | 传入完整的“包.类”名称实例化Class对象 |
public Constructor<?>[] getConstructors() throws SecurityException|得到一个类中的全部构造方法 |
public Field[] getDeclaredFields() throws SecurityException| 得到一个类父类中的全部属性 |
public Field[] getFields() throws SecurityException | 取得本类中的全部属性|
public Method[] getMethods() throws SecurityException | 取得一个类中的全部方法 |
public Method getMethod(String name,Class<?>... parameterTypes) throws NoSuchMethodException,SecurityException | 返回一个Method对象，并设置一个方法中的全部参数类型 |
public Class<?>[] getInterfaces() | 得到一个类中所实现的全部接口 |
public String getName() | 得到一个类完整的“包.类”名称 |
public Package getPackage() | 得到一个类的包 |
public Class<? super T> getSuperclass() | 得到一个类的父类Class对象 |
public T newInstance() throws InstantiationException,IllegalAccessException | 根据Class定义的类实例化对象 |
public Class<?> getComponentType() | 返回表示数组类型的Class|
public boolean isArray() |判断此Class是否是一个数组|

### 实例化Class对象的三种方式： 

- 通过forName()方法
- 类.class
- 对象.getClass()

**实例化Class类对象**

	package 反射机制01;
	public class getClassDemo02 {
	 
	 public static void main(String[] args) {
	  //返回值类型为Class类型,支持泛型(通配符)
	  Class<?> c1=null;
	  Class<?> c2=null;
	  Class<?> c3=null;
	 
	  try{
	   c1=Class.forName("反射机制01.Y");
	  }catch(ClassNotFoundException e){
	   e.printStackTrace();
	  }
	  c2=new Y().getClass();
	  c3=Y.class;
	  System.out.println(c1.getName());//getName()方法的得到完整的类路径,"包.类"名称
	  System.out.println(c2.getName());
	  System.out.println(c3.getName());
	 }
	 
	}
	class Y{
	 
	}

**Class类的使用**   
1.可以使用Class类完成类对象的实例化的操作(一般情况下使用new关键字实例化对象)  


代码：

	package 反射机制02;
	public class getClassDemo01 {
	 
	 public static void main(String[] args) {
	  Person per=null;
	  Class<?> c=null;
	  try {
	   c=Class.forName("反射机制02.Person");
	   per=(Person)c.newInstance();//通过反射实例化对象
	  } catch (Exception e) {
	   e.printStackTrace();
	  }
	  per.setName("xxx");
	  per.setAge(23);
	  System.out.println(per);
	 
	 }
	 
	}
	class Person{
	 
	 private String name;
	 private int age;
	 
	 public String getName() {
	  return name;
	 }
	 public void setName(String name) {
	  this.name = name;
	 }
	 public int getAge() {
	  return age;
	 }
	 public void setAge(int age) {
	  this.age = age;
	 }
	 @Override
	 public String toString() {
	  return "Person [name=" + name + ", age=" + age + "]";
	 }
	}

注意：类中必须存在无参构造函数，否则无法进行实例化的操作。要记住的是，使用Class类完成类对象的实例化，也需要类中构造方法的支持，这样才符合对象实例化的要求。

2.可以通过Constructor类调用**有参构造方法**完成对象的实例化

	package 反射机制02;
	import java.lang.reflect.Constructor;
	public class getClassDemo02 {
	 
	 public static void main(String[] args) {
	  Student stu=null;
	  Class<?> c=null;
	  try {
	   c=Class.forName("反射机制02.Student");
	   Constructor cons[]=null;
	   cons=c.getConstructors();
	   stu=(Student)cons[0].newInstance("xxx",23);
	  } catch (Exception e) {
	   e.printStackTrace();
	  }
	  System.out.println(stu);
	 
	 }
	 
	}
	class Student{
	 
	 private String name;
	 private int age;
	 //使用Class类实例化对象,必须存在无参构造函数,否则无法进行实例化的操作
	 /*public Student() {
	  // TODO Auto-generated constructor stub
	 }*/
	 public Student(String name,int age) {
	  this.name=name;
	  this.age=age;
	 }
	 
	 public String getName() {
	  return name;
	 }
	 public void setName(String name) {
	  this.name = name;
	 }
	 public int getAge() {
	  return age;
	 }
	 public void setAge(int age) {
	  this.age = age;
	 }
	 @Override
	 public String toString() {
	  return "Student [name=" + name + ", age=" + age + "]";
	 }
	}

**取得类的结构**  
定义一个接口:China.java

	package 取得类结构 ;
	interface China{	// 定义China接口
	 public static final String NATIONAL = "China" ;	// 定义全局常量
	 public static final String AUTHOR = "xxx" ;	// 定义全局常量
	 public void sayChina() ;	 // 无参的，没有返回值的方法
	 public String sayHello(String name,int age) ;	// 定义有两个参数的方法，并返回内容
	}

定义一个实现类：Person.java

	public class Person implements China{
	 private String name ;
	 private int age ;
	 public Person(){	// 无参构造
	 }
	 public Person(String name){
	  this.name = name ;	// 设置name属性
	 }
	 public Person(String name,int age){
	  this(name) ;
	  this.age = age ;
	 }
	 public void sayChina(){	// 覆写方法
	  System.out.println("作者：" + AUTHOR + "，国籍：" + NATIONAL) ;
	 }
	 public String sayHello(String name,int age){
	  return name + "，你好！我今年：" + age + "岁了！" ;
	 }
	 public void setName(String name){
	  this.name = name ;
	 }
	 public void setAge(int age){
	  this.age = age ;
	 }
	 public String getName(){
	  return this.name ;
	 }
	 public int getAge(){
	  return this.age ;
	 }
	};

获取类的接口:GetInterfaceDemo.java  

	package 取得类结构;
	public class GetInterfaceDemo {
	 
	 public static void main(String[] args) {
	  Class<?> c1=null;
	   try {
	    c1=Class.forName("取得类结构.Person");
	   } catch (ClassNotFoundException e) {
	    e.printStackTrace();
	   }
	   Class<?> c2[]=c1.getInterfaces();
	   for(int i=0;i<c2.length;i++){
	    System.out.println(c2[i].getName());
	   }
	 }
	}

获得类的父类:GetSuperclassDemo.java

	package 取得类结构;
	public class GetSuperclassDemo {
	 
	 public static void main(String[] args) {
	  Class<?> c1=null;
	   try {
	    c1=Class.forName("取得类结构.Person");
	   } catch (ClassNotFoundException e) {
	    e.printStackTrace();
	   }
	   //一个类只能继承一个父类,如果没有指明的继承父类,默认直接父类为Object
	   Class<?> c2=c1.getSuperclass();
	    System.out.println(c2.getName());
	 }
	}

获得类的全部构造方法:GetConstructorDemo.java

	package 取得类结构;
	import java.lang.reflect.Constructor;
	import java.lang.reflect.Modifier;
	public class GetConstructorDemo {
	 
	 public static void main(String[] args) {
	  Class<?> c1=null;
	   try {
	    c1=Class.forName("取得类结构.Person");
	   } catch (ClassNotFoundException e) {
	    e.printStackTrace();
	   }
	   Constructor<?> cons[]=c1.getConstructors();
	   for(int i=0;i<cons.length;i++){
	    //得到构造函数的详细信息
	    String modifier=Modifier.toString(cons[i].getModifiers());// 得到修饰符
	    Class<?> p[]=cons[i].getParameterTypes();//得到构造方法的所有参数
	    System.out.print(modifier+" ");
	    System.out.print(cons[i].getName()+"(");
	    for(int j=0;j<p.length;j++){
	     System.out.print(p[j].getName()+" arg"+j);
	     if(j<p.length-1){
	      System.out.print(",");
	     }
	    }
	    System.out.println("){}");
	   //System.out.println(cons[i]);
	   }
	 }
	}

获得类的全部属性，成员变量:GetFieldDemo.java

	package 取得类结构;
	import java.lang.reflect.Field;
	import java.lang.reflect.Modifier;
	public class GetFieldDemo {
	 public static void main(String[] args) {
	  Class<?> c1 = null;// 声明Class实例
	  try {
	   c1 = Class.forName("取得类结构.Person");
	  } catch (ClassNotFoundException e) {
	   e.printStackTrace();
	  }
	  {// 这是一个块
	   Field f[] = c1.getFields();// 得到本类的公共属性,成员变量
	   for (int i = 0; i < f.length; i++) {
	    Class<?> type = f[i].getType();// 得到属性的返回值的类型
	    String modifier = Modifier.toString(f[i].getModifiers());
	    System.out.print(modifier + " " + type.getName() + " ");
	    System.out.println(f[i].getName() + ";");
	   }
	  }
	  System.out.println("----------------------------------");
	  {// 这是一个块
	   Field f[] = c1.getDeclaredFields(); //得到本类中的属性,该类的成员变量
	   for (int i = 0; i < f.length; i++) {
	    Class<?> type = f[i].getType();// 得到属性的返回值的类型
	    String modifier = Modifier.toString(f[i].getModifiers());
	    System.out.print(modifier + " " + type.getName() + " ");
	    System.out.println(f[i].getName() + ";");
	   }
	  }
	 }
	}

获得类中的方法：GetMethodDemo.java

	package 取得类结构;
	import java.lang.reflect.Field;
	import java.lang.reflect.Method;
	import java.lang.reflect.Modifier;
	public class GetMethodDemo {
	 public static void main(String[] args) {
	  Class<?> c1 = null;// 声明Class实例
	  try {
	   c1 = Class.forName("取得类结构.Person");
	  } catch (ClassNotFoundException e) {
	   e.printStackTrace();
	  }
	  {// 这是一个块
	   Method m[] = c1.getMethods();// 得到全部方法 ,包括继承父类的方法
	   for (int i = 0; i < m.length; i++) {
	    String modifier = Modifier.toString(m[i].getModifiers());//得到修饰符
	    Class<?> p[]=m[i].getParameterTypes();//得到所有的形参类型
	    Class<?> type=m[i].getReturnType();//得到该方法的返回类型
	    Class<?> ex[]=m[i].getExceptionTypes();//取出方法可能抛出的异常
	    System.out.print(modifier+" "+type.getName()+" ");
	    System.out.print(m[i].getName()+"(");
	    for(int j=0;j<p.length;j++){
	     System.out.print(p[j].getName()+" arg"+j+" ");
	     if(j<p.length-1){
	      System.out.print(",");
	     }
	    }
	    if(ex.length>0){
	     System.out.print(") throws ");
	     for(int j=0;j<ex.length;j++){
	      System.out.println(ex[j].getName());
	     }
	     
	    }else{
	     System.out.println(")");
	    }
	   }
	  }
	  System.out.println("----------------------------------");
	  {// 这是一个块
	   Method m[] = c1.getDeclaredMethods();// 得到本类中的方法
	   for (int i = 0; i < m.length; i++) {
	    String modifier = Modifier.toString(m[i].getModifiers());//得到修饰符
	    Class<?> p[]=m[i].getParameterTypes();//得到所有的形参类型
	    Class<?> type=m[i].getReturnType();//得到该方法的返回类型
	    Class<?> ex[]=m[i].getExceptionTypes();//取出方法可能抛出的异常
	    System.out.print(modifier+" "+type.getName()+" ");
	    System.out.print(m[i].getName()+"(");
	    for(int j=0;j<p.length;j++){
	     System.out.print(p[j].getName()+" arg"+j+" ");
	     if(j<p.length-1){
	      System.out.print(",");
	     }
	    }
	    if(ex.length>0){
	     System.out.print(") throw ");
	     for(int j=0;j<ex.length;j++){
	      System.out.println(ex[j].getName());
	     }
	     
	    }else{
	     System.out.println(")");
	    }
	   }
	  }
	 }
	}

**任何引用数据类型也都可以用Object对象接收(比如:数组)**  

**反射的深入**  
我们知道类实例化对象后,就可以调用该类的方法,但是调用的时候必须要知道方法的名称.  
java中的反射机制让我们不仅可以通过对象调用方法,也可以通过反射来调用类的方法.

调用无参方法，无返回值：InvokeSayChinaDemo.java

	package 反射的深入研究;
	import java.lang.reflect.Method;
	public class InvokeSayChinaDemo {
	 
	 public static void main(String[] args) {
	  Class<?> c1=null;
	  try {
	   c1=Class.forName("反射的深入研究.Person");
	   Method m=c1.getMethod("sayChina");
	   //通过反射调用方法
	   m.invoke(c1.newInstance());
	  } catch (Exception e) {
	   e.printStackTrace();
	  }
	 }
	}

调用有参方法，有返回值：InvokeSayHelloDemo.java

	package 反射的深入研究;
	import java.lang.reflect.Method;
	public class InvokeSayHelloDemo {
	 
	 public static void main(String[] args) {
	  Class<?> c1=null;
	  try {
	   c1=Class.forName("反射的深入研究.Person");
	   Method m=c1.getMethod("sayHello",String.class,int.class);
	   //通过反射调用方法,这个方法有个String的返回值
	   String val=null;
	       //c1.newInstance()表示此Class对象表示的类的一个新实例
	                   //对带有指定参数的指定对象调用由此 Method 对象表示的底层方法
	   val=(String)m.invoke(c1.newInstance(),"xxx",23);
	   System.out.println(val);
	  } catch (Exception e) {
	   e.printStackTrace();
	  }
	 }
	}

属性赋值：InvokeSetDemo.java

	package 反射的深入研究;
	import java.lang.reflect.Method;
	public class InvokeSetDemo {
	 
	 public static void main(String[] args) {
	  Class<?> c1=null;
	  Object obj=null;
	  try {
	   c1=Class.forName("反射的深入研究.Person");
	   obj=c1.newInstance();
	  } catch (Exception e) {}
	  setter(obj,"name","xxx",String.class) ;	// 调用setter方法
	  setter(obj,"age",23,int.class) ;	// 调用setter方法
	  System.out.print("姓名：") ;
	  getter(obj,"name") ;
	  System.out.print("年龄：") ;
	  getter(obj,"age");
	 }
	 
	 public static void setter(Object obj,String attr,Object value,Class<?> type){
	  try {
	   Method m=obj.getClass().getMethod("set"+initStr(attr), type);
	   //set没有返回值,所以m.invoke(obj, value)为空值
	   //System.out.println(m.invoke(obj, value));
	   m.invoke(obj, value);
	  } catch (Exception e) {
	   e.printStackTrace();
	   }
	 }
	 
	 public static void getter(Object obj,String attr){
	  try {
	   Method m=obj.getClass().getMethod("get"+initStr(attr));
	    System.out.println(m.invoke(obj));
	  } catch (Exception e) {
	   e.printStackTrace();
	   }
	 }
	 //单词首字母大写的实现
	 public static String initStr(String val){
	  String str=val.substring(0, 1).toUpperCase()+val.substring(1, val.length());
	  return str;
	 }
	}

私有属性的赋值：InvokeFieldDemo.java

	package 反射的深入研究;
	import java.lang.reflect.Field;
	//私有属性的操作
	public class InvokeFieldDemo {
	 
	 public static void main(String[] args) throws Exception {
	 
	  Class<?> c1=Class.forName("反射的深入研究.Person");
	  Object obj=c1.newInstance();//表示此Class对象表示的类的实例
	  Field nameField = null ;
	  Field ageField = null ;
	  nameField=c1.getDeclaredField("name");
	  ageField=c1.getDeclaredField("age");
	  nameField.setAccessible(true);//设置此属性对外部可见
	  ageField.setAccessible(true);//设置此属性对外部可见
	  //设置值
	  nameField.set(obj, "xxx");
	  ageField.set(obj, 23);
	 
	  System.out.println("姓名：" + nameField.get(obj)) ;//返回指定对象上此 Field 表示的字段的值。
	  System.out.println("年龄：" + ageField.get(obj)) ;
	 
	 
	 }
	}

**注意：**注意：不建议这样使用,因为用反射操作set和get会破坏程序的封装性.  
反射在数组方面的应用:  
根据反射得到指定对象数组的信息,并通过java.lang.reflect包下的Array类操作数组里的值  
ClassArrayDemo.java


	package 反射的深入研究;
	import java.lang.reflect.Array;
	public class ClassArrayDemo {
	 
	 public static void main(String[] args) {
	  //指定一个整型数组
	  int temp[]={3,2,1};// 返回表示数组类型的Class。
	  Class<?> c1=temp.getClass().getComponentType();
	  //以 String 的形式返回此 Class 对象所表示的实体（类、接口、数组类、基本类型或 void）名称。
	  System.out.println("类型: "+c1.getName());
	  System.out.println("长度: "+Array.getLength(temp));
	  System.out.println("第一个内容：" +Array.get(temp, 0));
	  Array.set(temp, 0, 9);//将指定对象数组temp中的索引组件的值设置为指定的新值。
	  System.out.println("第一个内容：" +Array.get(temp,0));
	 
	 }
	}

通过反射改变指定对象数组的开辟大小(长度):ChangeArrayDemo.java

	package 反射的深入研究;
	import java.lang.reflect.Array;
	public class ChangeArrayDemo {
	 
	 public static void main(String[] args) {
	  //指定一个整型数组
	  int temp[]={3,4,1};
	  int newInt[]=(int[])arrayInc(temp,5);
	  print(temp);
	  System.out.println("\n--------------------------");
	  print(newInt);
	  System.out.println("\n--------------------------");
	  String t[] = {"lxh","mldn","mldnjava"} ;
	  String nt[] = (String [])arrayInc(t,8) ;
	  print(nt);
	 
	 }
	 //写一个方法根据指定数组对象得到新的数组
	 //任何引用数据类型都可以用Object对象接受
	 public static Object arrayInc(Object obj,int len){
	  //obj-->temp
	  Class<?> c=obj.getClass();
	  Class<?> attr=c.getComponentType();//得到数组组件类型的Class对象
	  Object newObj=Array.newInstance(attr, len);//创建一个具有指定的数组组件类型和长度的新数组
	  //数组拷贝
	  System.arraycopy(obj, 0, newObj, 0, Array.getLength(obj));
	  return newObj;
	 }
	 
	 //写一个方法输出数组的信息
	 public static void print(Object obj){
	  Class<?> c=obj.getClass();
	  //判断是否是数组
	  if(!c.isArray()){
	   return;
	  }
	  Class<?> attr=c.getComponentType();
	  System.out.println("类型: "+attr.getName()+", 数组长度: "+Array.getLength(obj));
	  for(int i=0;i<Array.getLength(obj);i++){
	   System.out.print(Array.get(obj, i)+",");
	  }
	 }
	 
	}

**反射工厂模式实现**  
1.简单工厂实现

	package 反射实现工厂模式;
	interface Fruit{
	 public void eat();
	}
	class Apple implements Fruit{
	 @Override
	 public void eat() {
	  System.out.println("...apple");
	 }
	}
	class orange implements Fruit{
	 @Override
	 public void eat() {
	  System.out.println("...orange");
	 }
	}
	class Factory{//工厂类
	 
	 public static Fruit getInstance(String name){
	  Fruit fruit=null;
	  if("apple".equals(name)){
	   fruit=new Apple();
	  }else if("orange".equals(name)){
	   fruit=new orange();
	  }
	  return fruit;
	 }
	 
	}
	public class FactoryDemo01 {
	 
	 public static void main(String[] args) {
	  Fruit fruit=Factory.getInstance("apple");
	  fruit.eat();
	 }
	}

2.通过反射实现(通过配置文件与程序代码分离)

	package 反射实现工厂模式;
	import java.io.IOException;
	import java.io.InputStream;
	import java.util.Properties;
	class Init{
	 public Properties getPro(String PropName){
	  Properties prop=new Properties();
	  //this.getClass().getClassLoader().getResourceAsStream(PropName);
	  InputStream in=this.getClass().getResourceAsStream(PropName);
	  try {
	   prop.load(in);
	  } catch (IOException e) {
	   e.printStackTrace();
	  }
	  return prop;
	 }
	}
	class Factory03{//工厂类
	 
	 public static Fruit getInstance(String name){
	  Fruit fruit=null;
	  try{
	   fruit=(Fruit)Class.forName(name).newInstance();//根据此Class对象得到所表示的类的一个实例(对象)
	   
	  }catch(Exception e){
	   e.printStackTrace();
	  }
	  return fruit;
	 }
	}
	public class CopyOfFactoryDemo03 {
	 
	 public static void main(String[] args) {
	  Properties prop=new Init().getPro("/反射实现工厂模式/config.properties");
	  Fruit fruit=Factory03.getInstance(prop.getProperty("apple"));
	  if(fruit!=null){
	   fruit.eat();
	  }
	 }
	}

**反射代理模式实现**  
1.静态代理实现  
代理设计：一个操作的接口有两个子类，其中一个是真实主题的实现类，另外一个是代理类。代理类要完成比真实主题实现类更多的内容，而且本身还需要处理一些与具体业务相关的代码。

	package 反射代理的实现Proxy;
	interface Subject{
	 public String say(String name,int age);
	}
	//真实主题类
	class RealSubject implements Subject{

	 @Override
	 public String say(String name, int age) {
	  return "姓名: "+name+", 年龄: "+age;
	 }
	 
	}
	//代理主题类
	class ProxySubject implements Subject{
	 
	 private Subject sub=null;
	 public ProxySubject(Subject sub) {
	  this.sub=sub;
	 }
	 @Override
	 public String say(String name, int age) {
	  return this.sub.say(name, age);
	 }
	 
	}
	public class DynaProxyDemo {
	 
	 public static void main(String[] args) {
	  Subject sub=new ProxySubject(new RealSubject());
	  System.out.println(sub.say("xxx", 23));
	 }
	}

2.动态代理的实现    
静态代理的缺陷是:一个代理类只能为一个接口服务,那么多个接口的话,就需要多个代理类来服务,而所有的的代理操作除了调用的方法不一样,其他的操作都一样,这样就会有重复的代码.  
在Java中要想实现动态代理机制，则需要java.lang.reflect.Proxy类和java.lang.reflect.InvocationHandler接口的支持。

InvocationHandler接口中的invoke方法说明：  

	public Object invoke(Object proxy, Method method, Object[] args) 
		throws Throwable;

- Object proxy：要代理的对象
- Method method:调用的方法
- Object[] args:方法调用时需要的参数

可以将InvocationHandler接口的实现类想象成一个代理最终操作类,替换掉ProxySubject.
Proxy类是专门完成代理的操作类.

**关于类加载器的说明**
在Proxy中的newProxyInstance()方法中需要一个ClassLoader类的实例，ClassLoader实际上对应的是类加载器，在Java中有如下三种类加载器：

- Bootstrap ClassLoader：此加载器采用C++编写，一般开发中看不到
- Extension ClassLoader：用来进行扩展类的加载，一般对应的是jre/lib/ext目录下的类
- APPClassLoader：加载classpath下指定的类，是最常用的一种加载器

动态代理反射的代码实现：

	package 反射代理的实现Proxy;
	import java.lang.reflect.InvocationHandler;
	import java.lang.reflect.Method;
	import java.lang.reflect.Proxy;
	class myInvocationHandler implements InvocationHandler{

	 private Object obj;//真实主题类

	 public Object bind(Object obj){//绑定真实主题类,可以说是初始化操作
	  this.obj=obj;
	  return Proxy.newProxyInstance(obj.getClass().getClassLoader(), obj.getClass().getInterfaces(),this);
	 }
	 
	 @Override
	 public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
	  //调用方法前进行一些业务的处理
	  Object object=method.invoke(this.obj, args);//调用方法
	  //调用方法后进行一些处理
	  return object;
	 }
	 
	}
	public class DynaProxyDemo {
	 
	 public static void main(String[] args) {
	  Subject sub=(Subject) new myInvocationHandler().bind(new RealSubject());
	  System.out.println(sub.say("xxx", 23));
	 }
	}


**原文链接：[深入Java反射机制]()  
欢饮转载，请注明出处！**


	

	

	
	


	






