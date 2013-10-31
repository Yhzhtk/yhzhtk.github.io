---
layout: post
title: 工厂模式 - 一个生产人类的工厂
description: 工厂模式用来根据不同的参数创建对象。下面的例子是用工厂创造人类。如果我们问工厂要一个男孩，则工厂会产生一个男孩，如果我们问工厂要一个女孩，工厂将产生一个女孩。根据不同的参数，工厂会生产不同的东西。
tags: [设计模式, 翻译, 工厂]
---

<em>翻译自 <a href="http://www.programcreek.com/2013/02/java-design-pattern-factory/" rel="bookmark">Java Design Pattern: Factory</a></em>

**Java设计模式：工厂模式**

**1、工厂模式的故事**

工厂模式用来根据不同的参数创建对象。下面的例子是用工厂创造人类。如果我们问工厂要一个男孩，则工厂会产生一个男孩，如果我们问工厂要一个女孩，工厂将产生一个女孩。根据不同的参数，工厂会生产不同的东西。

2、工厂模式的类图

<img src="http://www.programcreek.com/wp-content/uploads/2013/02/factory-design-pattern.png"/>

<!--break-->

3、工厂模式的Java代码

{% include syntax-java.html %}

<pre class="brush: java;">
interface Human {
	public void Talk();
	public void Walk();
}
 
 
class Boy implements Human{
	@Override
	public void Talk() {
		System.out.println("Boy is talking...");		
	}
 
	@Override
	public void Walk() {
		System.out.println("Boy is walking...");
	}
}
 
class Girl implements Human{
 
	@Override
	public void Talk() {
		System.out.println("Girl is talking...");	
	}
 
	@Override
	public void Walk() {
		System.out.println("Girl is walking...");
	}
}
 
public class HumanFactory {
	public static Human createHuman(String m){
		Human p = null;
		if(m == "boy"){
			p = new Boy();
		}else if(m == "girl"){
			p = new Girl();
		}
 
		return p;
	}
}
</pre>

**4、Java标准库中的工厂设计模式**

对于java.util.Calendar，通过传递不同的参数，getInstance()返回不同的日历实例。

<pre>
java.util.Calendar – getInstance()
java.util.Calendar – getInstance(TimeZone zone)
java.util.Calendar – getInstance(Locale aLocale)
java.util.Calendar – getInstance(TimeZone zone, Locale aLocale)

java.text.NumberFormat – getInstance()
java.text.NumberFormat – getInstance(Locale inLocale)
</pre>

**由于博主水平有限，抱着锻炼为主的目的而翻译，若有不准确之处，请包涵，欢迎批评指正。**
