title: Tomcat优化之修改内存配置
author: ergz
tags:
  - tomcat
categories:
  - web服务器
date: 2018-10-18 22:17:00
---
##### 一、配置
**Tomcat/conf/server.xml修改配置**
```xml
<connector port="8080" protocol="org.apache.coyote.http11.Http11NioProtocol"
        redirectPort="8443"
        URIEncoding="UTF-8"
        minSpareThreads="25"
        maxSpareThreads="300"
        maxThreads="500"
        acceptCount="500"
        connectionTimeout="30000"
        enableLookups="false"/>
```
##### 二、参数说明
<!--more-->
**maxIdleTime**：最大空闲时间，超过这个空闲时间，且线程数大于minSpareThreads的，都会被回收，默认值1分钟（60000ms）；

**minSpareThreads**：最小空闲线程数，任何情况都会存活的线程数，即便超过了最大空闲时间，也不会被回收，默认值4；

**maxSpareThreads**：最大空闲线程数，在最大空闲时间（maxIdleTime）内活跃过，此时空闲，当空闲时间大于maxIdleTime则被回收，小则继续存活，等待被调度，默认值50；

**maxThreads**：最大线程数，大并发请求时，tomcat能创建来处理请求的最大线程数，超过则放入请求队列中进行排队，默认值为200；

**acceptCount**：当最大线程数（maxThreads）被使用完时，可以放入请求队列排队个数，超过这个数返回connection refused（请求被拒绝），一般设置和maxThreads一样，不过这个具体需要根据自己的应用实际访问峰值和平均值来权衡，默认值为100；

**connectionTimeout**：网络连接超时，假设设置为0表示永不超时，这样设置隐患巨大，通常可设置为30000ms，默认60000ms；

Windows Tomcat允许每个进程maxThreads（最大线程数）2000，Linux Tomcat允许每个进程maxThreads（最大线程数）1000

![upload successful](/images/tomcat_201912182226.png)

**请看下面三种情况**
情况1：接受一个请求，此时tomcat起动的线程数没有到达maxThreads，tomcat会起动一个线程来处理此请求。

情况2：接受一个请求，此时tomcat起动的线程数已经到达maxThreads，tomcat会把此请求放入等待队列，等待空闲线程。

情况3：接受一个请求，此时tomcat起动的线程数已经到达maxThreads，等待队列中的请求个数也达到了acceptCount，此时tomcat会直接拒绝此次请求，返回connection refused

**maxThreads如何配置**
一般的服务器操作都包括两方面：1计算（主要消耗cpu），2等待（io、数据库等）。

第一种极端情况，如果我们的操作是纯粹的计算，那么系统响应时间的主要限制就是cpu的运算能力，此时maxThreads应该尽量设的小，降低同一时间内争抢cpu的线程个数，可以提高计算效率，提高系统的整体处理能力。

第二种极端情况，如果我们的操作纯粹是IO或者数据库，那么响应时间的主要限制就变为等待外部资源，此时maxThreads应该尽量设的大，这样才能提高同时处理请求的个数，从而提高系统整体的处理能力。此情况下因为tomcat同时处理的请求量会比较大，所以需要关注一下tomcat的虚拟机内存设置和linux的open file限制。

我在测试时遇到一个问题，maxThreads我设置的比较大比如3000，当服务的线程数大到一定程度时，一般是2000出头，单次请求的响应时间就会急剧的增加，百思不得其解这是为什么，四处寻求答案无果，最后我总结的原因可能是cpu在线程切换时消耗的时间随着线程数量的增加越来越大，cpu把大多数时间都用来在这2000多个线程直接切换上了，当然cpu就没有时间来处理我们的程序了。以前一直简单的认为多线程=高效率。其实多线程本身并不能提高cpu效率，线程过多反而会降低cpu效率。当cpu核心数<线程数时，cpu就需要在多个线程直接来回切换，以保证每个线程都会获得cpu时间，即通常我们说的并发执行。所以maxThreads的配置绝对不是越大越好。

现实应用中，我们的操作都会包含以上两种类型（计算、等待），所以maxThreads的配置并没有一个最优值，一定要根据具体情况来配置。最好的做法是：在不断测试的基础上，不断调整、优化，才能得到最合理的配置。acceptCount的配置，我一般是设置的跟maxThreads一样大，这个值应该是主要根据应用的访问峰值与平均值来权衡配置的。如果设的较小，可以保证接受的请求较快相应，但是超出的请求可能就直接被拒绝。如果设的较大，可能就会出现大量的请求超时的情况，因为我们系统的处理能力是一定的。


**原文链接：[Tomcat修改内存配置](https://www.toutiao.com/a6771285271473291783)
欢迎转载，请注明出处！**