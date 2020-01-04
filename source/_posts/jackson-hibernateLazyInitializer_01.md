title: Hibernate查询Jackson序列化懒加载问题
author: ergz
tags:
  - jackson
categories:
  - Json
date: 2015-05-26 13:51:00
---
在Hibernate查询出数据实体类使用jackson序列化为json时,出现下面错误：

	org.springframework.http.converter.HttpMessageNotWritableException...
	...
	disable SerializationConfig.SerializationFeature.FAIL_ON_EMPTY_BEANS

引起以上问题的主要原因就是jackson把懒加载也作为pojo进行序列化了。
一般解决方法：在实体类上加上

	@JsonIgnoreProperties({"hibernateLazyInitializer", "handler"})

<!--more-->

如果实体类中有集合属性那么将类里面所有的集合都加入到**@JsonIgnoreProperties**

这个方法注解是放在类定义上面的,例如:
@JsonIgnoreProperties
class A
{}

如果你定义的类是这样的:

	class A{
		private String description;
		private Boolean enable;
		private Integer createUser;
		private Date createTime;
		private Integer updateUser;
		private Date updateTime;
		private Set operations = new HashSet(0);
		private Set roles = new HashSet(0);
		private Set menus = new HashSet(0);
	}

则把下面这句话加到类上

	@JsonIgnoreProperties(value={“hibernateLazyInitializer”,”handler”,”operations”,”roles”,”menus”})

**注意：其中hibernateLazyInitializer，handler必须添加！**

**本文链接：[Hibernate查询Jackson序列化懒加载问题]()
欢迎转载，请注明出处！**