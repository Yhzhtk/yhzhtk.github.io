---
layout: post
title: 抽象工厂模式 - 一个生产CPU的抽象工厂
description: 抽象工厂模式在工厂模式的基础上又增加了一层抽象。将抽象工厂模式与工厂模式比较，很明显是添加了一个新的抽象层。抽象工厂是一个创建其他工厂的超级工厂。我们可以把它叫做“工厂的工厂”。
tags: [设计模式, 翻译, 抽象工厂]
---

<em>翻译自 <a href="http://www.programcreek.com/2013/02/java-design-pattern-abstract-factory/" target="_blank">Java Design Pattern: Abstract Factory</a></em>

**Java设计模式：抽象工厂**

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

**由于博主水平有限，抱着锻炼为主的目的而翻译，若有不准确之处，请包涵，欢迎批评指正。**
