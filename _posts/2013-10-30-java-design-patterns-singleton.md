---
layout: post
title: 单例模式 - 美国只有一个总统
description: 单例模式在Java中最常用的模式之一。它能阻止外部的实例化和修改，从而控制创建的对象的数量。这个概念可以应用到只允许一个对象存在时的操作，或者限制在一定数量的对象的操作
tags: [设计模式, 翻译, 单例]
---

<em>翻译自 
<a href="http://www.programcreek.com/2011/07/java-design-pattern-singleton/" rel="bookmark">Java Design Pattern: Singleton</a></em>

**Java设计模式：单例模式**

单例模式在Java中最常用的模式之一。它能阻止外部的实例化和修改，从而控制创建的对象的数量。这个概念可以应用到只允许一个对象存在时的操作，或者限制在一定数量的对象的操作，如：

	1. 私有构造函数 - 没有其他类可以实例化一个新的对象。
	2. 私有引用 - 没有外部修改。
	3. 公共静态方法是唯一可以得到对象引用的地方。

**单例模式的故事**

下面是一个简单的使用情况。一个国家只能有一个总统（这是正常的情况）。所以每当我们要引用总统对象，只需要通过AmericaPresident返回，其中的getPresident（）方法将确保始终只有一个总统创建。否则，它就不会符合正常情况。

**类图**

<img src="http://www.programcreek.com/wp-content/uploads/2011/07/singleton.jpg"/>

<!--break-->

**Java 代码**

{% include syntax-java.html %}
<pre class="brush: java;">
package com.programcreek.designpatterns.singleton;
 
public class AmericaPresident {
	private AmericaPresident() {	}
 
	private static AmericaPresident thePresident;
 
	public static AmericaPresident getPresident(){
		if(thePresident == null)
			thePresident = new AmericaPresident();
		return thePresident;
	}
}
</pre>

**单例模式在Java标准类库中的使用**

java.lang.Runtime#getRuntime()是Java标准库中的一种常用方法。 getRunTime()返回与当前Java应用程序的运行时对象。下面是一个简单的使用getRunTime()的例子，在Windows系统上读取网页。

<pre class="brush: java;">
Process p = Runtime.getRuntime().exec(
		"C:/windows/system32/ping.exe programcreek.com");
//get process input stream and put it to buffered reader
BufferedReader input = new BufferedReader(new InputStreamReader(
		p.getInputStream()));
 
String line;
while ((line = input.readLine()) != null) {
	System.out.println(line);
}
 
input.close();
</pre>

输出：
<pre>
Pinging programcreek.com [198.71.49.96] with 32 bytes of data:
Reply from 198.71.49.96: bytes=32 time=53ms TTL=47
Reply from 198.71.49.96: bytes=32 time=53ms TTL=47
Reply from 198.71.49.96: bytes=32 time=52ms TTL=47
Reply from 198.71.49.96: bytes=32 time=53ms TTL=47
Ping statistics for 198.71.49.96:
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
Minimum = 52ms, Maximum = 53ms, Average = 52ms
</pre>

