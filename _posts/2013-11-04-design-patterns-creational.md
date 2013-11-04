---
layout: post
title: 创造型设计模式 - 单例、工厂、抽象工厂、生成器、原型
description: 设计模式中的创造型模式，包括单例模式、工厂模式、抽象工厂模式、生成器模式、原型模式，本文主要用简单的实例介绍这几种设计模式。
tags: [设计模式, 翻译, 单例, 工厂, 抽象工厂, 生成器, 原型]
---

###Java设计模式：单例模式###

<em>翻译自 <a href="http://www.programcreek.com/2011/07/java-design-pattern-singleton/" target="_blank">Java Design Pattern: Singleton</a></em>

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

---

###Java设计模式：工厂模式###

<em>翻译自 <a href="http://www.programcreek.com/2013/02/java-design-pattern-factory/" target="_blank">Java Design Pattern: Factory</a></em>

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

---

###Java设计模式：抽象工厂###

<em>翻译自 <a href="http://www.programcreek.com/2013/02/java-design-pattern-abstract-factory/" target="_blank">Java Design Pattern: Abstract Factory</a></em>

抽象工厂模式在工厂模式的基础上又增加了一层抽象。将抽象工厂模式与工厂模式比较，很明显是添加了一个新的抽象层。抽象工厂是一个创建其他工厂的超级工厂。我们可以把它叫做“工厂的工厂”。

**抽象工厂类图**

<img src="http://www.programcreek.com/wp-content/uploads/2013/02/abstract-factory-design-pattern.png"/>

<!--break-->

**抽象工厂的Java代码**

{% include syntax-java.html %}

<pre class="brush: java;">
interface CPU {
    void process();
}
 
interface CPUFactory {
	CPU produceCPU();
}
 
class AMDFactory implements CPUFactory {
    public CPU produceCPU() {
        return new AMDCPU();
    }
}
 
class IntelFactory implements CPUFactory {
    public CPU produceCPU() {
        return new IntelCPU();
    }
}
 
class AMDCPU implements CPU {
    public void process() {
        System.out.println("AMD is processing...");
    }
}
 
class IntelCPU implements CPU {
    public void process() {
        System.out.println("Intel is processing...");
    }
}
 
class Computer {
	CPU cpu;
 
    public Computer(CPUFactory factory) {
    	cpu = factory.produceCPU();
        cpu.process();
    }
}
 
public class Client {
    public static void main(String[] args) {
        new Computer(createSpecificFactory());
    }
 
    public static CPUFactory createSpecificFactory() {
        int sys = 0; // based on specific requirement
        if (sys == 0) 
        	return new AMDFactory();
        else 
        	return new IntelFactory();
    }
}
</pre>

**实际使用的例子**

事实上，现代的框架，抽象是一个非常重要的概念。
<a href="http://stackoverflow.com/questions/1993397/abstract-factory-pattern-on-top-of-ioc/1994455#1994455" target="_blank">“Abstract factory pattern on top of IoC?”</a>是stackoverflow上的一个关于抽象工厂的问题。

---

###Java设计模式：生成器模式###

<em>翻译自 <a href="http://www.programcreek.com/2013/02/java-design-pattern-builder/" target="_blank">Java Design Pattern: Builder</a></em>

生成器模式的主要特征是，通过一步一步的方式生成一些东西。每个生成的东西，即使其中的任何一步都不相同，但也将遵循同样的过程。

在下面的故事中，我们定义一个饮料生成器StarbucksBuilder，用来生成星巴克饮料。StarbucksBuilder有几个步骤来建立一杯星巴克的饮料，如buildSize()和buildDrink()。最后返回生成的饮料。

**1、Builder设计模式的类图**

<img src="http://www.programcreek.com/wp-content/uploads/2013/02/builder-design-pattern.png"/>

<!--break-->

**2、Builder设计模式的Java代码示例**

{% include syntax-java.html %}

<pre class="brush: java;">
package designpatterns.builder;
 
// produce to be built
class Starbucks {
	private String size;
	private String drink;
 
	public void setSize(String size) {
		this.size = size;
	}
 
	public void setDrink(String drink) {
		this.drink = drink;
	}
}
 
//abstract builder
abstract class StarbucksBuilder {
	protected Starbucks starbucks;
 
	public Starbucks getStarbucks() {
		return starbucks;
	}
 
	public void createStarbucks() {
		starbucks = new Starbucks();
		System.out.println("a drink is created");
	}
 
	public abstract void buildSize();
	public abstract void buildDrink();
}
 
// Concrete Builder to build tea
class TeaBuilder extends StarbucksBuilder {
	public void buildSize() {
		starbucks.setSize("large");
		System.out.println("build large size");
	}
 
	public void buildDrink() {
		starbucks.setDrink("tea");
		System.out.println("build tea");
	}
 
}
 
// Concrete builder to build coffee
class CoffeeBuilder extends StarbucksBuilder {
	public void buildSize() {
		starbucks.setSize("medium");
		System.out.println("build medium size");
	}
 
	public void buildDrink() {
		starbucks.setDrink("coffee");
		System.out.println("build coffee");
	}
}
 
//director to encapsulate the builder
class Waiter {
	private StarbucksBuilder starbucksBuilder;
 
	public void setStarbucksBuilder(StarbucksBuilder builder) {
		starbucksBuilder = builder;
	}
 
	public Starbucks getstarbucksDrink() {
		return starbucksBuilder.getStarbucks();
	}
 
	public void constructStarbucks() {
		starbucksBuilder.createStarbucks();
		starbucksBuilder.buildDrink();
		starbucksBuilder.buildSize();
	}
}
 
//customer
public class Customer {
	public static void main(String[] args) {
		Waiter waiter = new Waiter();
		StarbucksBuilder coffeeBuilder = new CoffeeBuilder();
 
		//Alternatively you can use tea builder to build a tea
		//StarbucksBuilder teaBuilder = new TeaBuilder();
 
		waiter.setStarbucksBuilder(coffeeBuilder);
		waiter.constructStarbucks();
 
		//get the drink built
		Starbucks drink = waiter.getstarbucksDrink();
 
	}
}
</pre>

**3、Builder的设计模式的实际使用**

Builder模式已被用于很多类库。下面是Java核心编程里的一个例子。

<pre class="brush: java;">
StringBuilder strBuilder= new StringBuilder();
strBuilder.append("one");
strBuilder.append("two");
strBuilder.append("three");
String str= strBuilder.toString();
</pre>

append()方法就像是我们的星巴克的例子中的一个步骤，toString()方法是最后一步。从这个语句看，我们可以很容易地理解 `字符串是不可改变的` 这个问题。

以上类图较为复杂，但和StringBuilder继承自AbstractStringBuilder一样，正是生成器模式所表现的意思。

**4、生成器模式和工厂模式之间的差异**

当需要多个步骤来创建一个对象的时候，使用生成器模式。当需要通过调用一个方法就很容易就创建了整个对象的时候，使用工厂模式。

---

###Java设计模式：原型模式###

<em>翻译自 <a href="http://www.programcreek.com/2013/02/java-design-pattern-prototype/" target="_blank">Java Design Pattern: Prototype</a></em>

原型设计模式用在当需要经常使用非常相似的对象的情况下，当需要相似的对象时，原型模式会克隆原始对象并且只修改不同的地方，这样会消耗更少的资源。想想为什么有更少的资源消耗？

**1、原型模式类图**

<img src="http://www.programcreek.com/wp-content/uploads/2013/02/prototype-pattern-class-diagram.png"/>

<!--break-->

**2、原型模式的Java示例**

{% include syntax-java.html %}

<pre class="brush: java;">
package designpatterns.prototype;
 
//prototype
interface Prototype {
    void setSize(int x);
    void printSize();
 }
 
// a concrete class
class A implements Prototype, Cloneable {
    private int size;
 
    public A(int x) {
        this.size = x;
    }
 
    @Override
    public void setSize(int x) {
        this.size = x;
    }
 
    @Override
    public void printSize() {
        System.out.println("Size: " + size);
    }
 
 
    @Override
    public A clone() throws CloneNotSupportedException {
        return (A) super.clone();
    }
}
 
//when we need a large number of similar objects
public class PrototypeTest {
    public static void main(String args[]) throws CloneNotSupportedException {
        A a = new A(1);
 
        for (int i = 2; i &lt; 10; i++) {
            Prototype temp = a.clone();
            temp.setSize(i);
            temp.printSize();
        }
    }
}
</pre>

**3、Java标准库中的原型模式**

<pre>
java.lang.Object – clone()
</pre>


*由于博主水平有限，抱着锻炼为主的目的而翻译，若有不准确之处，请包涵，欢迎批评指正。*
