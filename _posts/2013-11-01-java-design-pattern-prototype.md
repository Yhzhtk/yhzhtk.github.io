---
layout: post
title:  原型模式 - 创建很多相似的对象
description: 原型设计模式用在当需要经常使用非常相似的对象的情况下，当需要相似的对象时，原型模式会克隆原始对象并且只修改不同的地方，这样会消耗更少的资源。想想为什么有更少的资源消耗？
tags: [设计模式, 翻译, 原型模式]
---

<em>翻译自 <a href="http://www.programcreek.com/2013/02/java-design-pattern-prototype/" target="_blank">Java Design Pattern: Prototype</a></em>

**Java设计模式：原型模式**

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
 
        for (int i = 2; i < 10; i++) {
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

**由于博主水平有限，抱着锻炼为主的目的而翻译，若有不准确之处，请包涵，欢迎批评指正。**
