---
layout: post
title:  生成器模式 - 生成一杯饮料
description: 生成器模式的主要特征是，通过一步一步的方式生成一些东西。每个生成的东西，即使其中的任何一步都不相同，但也将遵循同样的过程。
tags: [设计模式, 翻译, 生成器模式]
---

<em>翻译自 <a href="http://www.programcreek.com/2013/02/java-design-pattern-builder/" target="_blank">Java Design Pattern: Builder</a></em>

**Java设计模式：生成器模式**

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

**由于博主水平有限，抱着锻炼为主的目的而翻译，若有不准确之处，请包涵，欢迎批评指正。**
