---
layout: post
title: 行为型设计模式 - 观察者、状态、策略、模板、访问者、职责链、命令、解释器、迭代器、中介者、备忘录
description: 设计模式中的行为型模式，包括观察者模式、状态模式、策略模式、模板模式、访问者模式、职责链模式、命令模式、解释器模式、迭代器模式、中介者模式、备忘录模式，本文主要用简单的实例介绍这几种设计模式。
tags: [设计模式, 翻译, 观察者, 状态, 策略, 模板, 访问者, 职责链, 命令, 解释器, 迭代器, 中介者, 备忘录]
---

设计模式中的行为型模式，包括观察者模式、状态模式、策略模式、模板模式、访问者模式、职责链模式、命令模式、解释器模式、迭代器模式、中介者模式、备忘录模式，本文主要用简单的实例介绍这几种设计模式。

· &nbsp;&nbsp;[观察者模式 - 找工作或观察工作](http://yhzhtk.info/2013/11/05/design-patterns-behavioral.html#observer) <small>原文 <a href="http://www.programcreek.com/2011/01/an-java-example-of-observer-pattern/" target="_blank">Observer – Look for a job or observe a job?</a></small>

· &nbsp;&nbsp;[观察者模式 - 一个简单的Swing GUI例子](http://yhzhtk.info/2013/11/05/design-patterns-behavioral.html#observer-swing) <small>原文 <a href="http://www.programcreek.com/2009/01/the-steps-involved-in-building-a-swing-gui-application/" target="_blank">Observer – A simple Swing GUI example</a></small>

· &nbsp;&nbsp;[状态模式 - 当生活困难时要努力工作](http://yhzhtk.info/2013/11/05/design-patterns-behavioral.html#state) <small>原文 <a href="http://www.programcreek.com/2011/07/java-design-pattern-state/" target="_blank">State – Work hard when life is hard</a></small>

· &nbsp;&nbsp;[策略模式 - 如果加速你会得到票吗](http://yhzhtk.info/2013/11/05/design-patterns-behavioral.html#strategy) <small>原文 <a href="http://www.programcreek.com/2011/01/a-java-example-of-strategy-design-pattern/" target="_blank">Strategy – Will you get a ticket if speeding</a></small>

· &nbsp;&nbsp;[模板模式 - 测试车辆]() <small>原文 <a href="http://www.programcreek.com/2012/08/java-design-pattern-template-method/" target="_blank">Template – Test a vehicle</a></small>

· &nbsp;&nbsp;[访问者模式 - 访问纽约]() <small>原文 <a href="http://www.programcreek.com/2011/05/visitor-design-pattern-example/" target="_blank">Visitor – Visit New York City</a></small>

· &nbsp;&nbsp;[职责链模式 - 职责链]() <small>原文 <a href="http://www.programcreek.com/2013/02/java-design-pattern-chain-of-responsibility/" target="_blank">Chain of responsibility – The responsibility chain</a></small>

· &nbsp;&nbsp;[命令模式 - 使用不同的命令控制电脑]() <small>原文 <a href="http://www.programcreek.com/2013/02/java-design-pattern-command/" target="_blank">Command – Use different command to control computer</a></small>

· &nbsp;&nbsp;[解释器模式 - 解释一些内容]() <small>原文 <a href="http://www.programcreek.com/2013/02/java-design-pattern-interprete/" target="_blank">Interpreter – Interpret some context</a></small>

· &nbsp;&nbsp;[迭代器模式 - 迭代一个对象集合]() <small>原文 <a href="http://www.programcreek.com/2013/02/java-design-pattern-iterator/" target="_blank">Iterator – Iterate a collection of objects</a></small>

· &nbsp;&nbsp;[中介者模式 - 两个同事的交流]() <small>原文 <a href="http://www.programcreek.com/2013/02/java-design-pattern-mediator/" target="_blank">Mediator – Mediate two colleagues</a></small>

· &nbsp;&nbsp;[备忘录模式 - 使用备忘录记录时间旅行]() <small>原文 <a href="http://www.programcreek.com/2013/02/java-design-pattern-memento/" target="_blank">Memento – Use memento to time travel</a></small>

---

<h3><span id="observer">Java设计模式：观察者模式</span></h3>

<em>翻译自 <a href="http://www.programcreek.com/2011/01/an-java-example-of-observer-pattern/" target="_blank">Java Design Pattern: Observer</a></em>

简单地说，观察者模式=出版者+订阅者。

观察者模式已被用于在GUI动作监听器上。Swing GUI 示例显示了动作监听器如何像观察者一样工作。

下面是一个典型的关于猎头的例子。在这个图中有两个角色 - 猎头和求职者 。求职者订阅猎头，当有一个新的工作机会时猎头会发布招聘消息，求职者就能收到订阅。

**类图**

<img src="http://www.programcreek.com/wp-content/uploads/2011/01/observer-pattern.gif"/>

**Java代码**

{% include syntax-java.html %}

<pre class="brush: java;">
//Subject interface.
public interface Subject {
	public void registerObserver(Observer o);
	public void removeObserver(Observer o);
	public void notifyAllObservers();
}

//Observer interface.
public interface Observer {
	public void update(Subject s);
}

//HeadHunter class implements Subject.
import java.util.ArrayList;
 
public class HeadHunter implements Subject{
 
	//define a list of users, such as Mike, Bill, etc.
	private ArrayList&lt;Observer&gt; userList;
	private ArrayList&lt;String&gt; jobs;
 
	public HeadHunter(){
		userList = new ArrayList&lt;Observer&gt;();
		jobs = new ArrayList&lt;String&gt;();
	}
 
	@Override
	public void registerObserver(Observer o) {
		userList.add(o);
	}
 
	@Override
	public void removeObserver(Observer o) {}
 
	@Override
	public void notifyAllObservers() {
		for(Observer o: userList){
			o.update(this);
		}
	}
 
	public void addJob(String job) {
		this.jobs.add(job);
		notifyAllObservers();
	}
 
	public ArrayList&lt;String&gt; getJobs() {
		return jobs;
	}
 
	public String toString(){
		return jobs.toString();
	}
}

//JobSeeker is an observer.
public class JobSeeker implements Observer {
 
	private String name;
 
	public JobSeeker(String name){
		this.name = name;
	}
	@Override
	public void update(Subject s) {
		System.out.println(this.name + " got notified!");
		//print job list
		System.out.println(s);
	}
}

//Start Point.
public class Main {
 
	public static void main(String[] args) {
		HeadHunter hh = new HeadHunter();
		hh.registerObserver(new JobSeeker("Mike"));
		hh.registerObserver(new JobSeeker("Chris"));
		hh.registerObserver(new JobSeeker("Jeff"));
 
		//Each time, a new job is added, all registered job seekers will get noticed.
		hh.addJob("Google Job");
		hh.addJob("Yahoo Job");
	}
}
</pre>

**JDK中的观察者模式**

<pre>
java.util.EventListener
</pre>

[Swing GUI example](#observer-swing)

---

<h3><span id="observer-swing">一个简单的观察者设计模式例子 - Swing GUI</span></h3>

<em>翻译自 <a href="http://www.programcreek.com/2009/01/the-steps-involved-in-building-a-swing-gui-application/" target="_blank">A simple Swing GUI example for Observer Design Pattern</a></em>

这个例子显示了如何创建一个Swing GUI的例子，并解释为什么它是观察者设计模式的用法的例子。

**完整代码**

<pre class="brush: java;">
import java.awt.BorderLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JTextArea;
 
public class SimpleSwingExample {
 
	public static void main(String[] args) {
		JFrame frame = new JFrame("Frame Title");
		final JTextArea comp = new JTextArea();
		JButton btn = new JButton("click");
		frame.getContentPane().add(comp, BorderLayout.CENTER);
		frame.getContentPane().add(btn, BorderLayout.SOUTH);
 
		btn.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent ae) {
				comp.setText("Button has been clicked");
			}
		});
 
		int width = 300;
		int height = 300;
		frame.setSize(width, height);
 
		frame.setVisible(true);
	}
}
</pre>

**一步一步的解释**

首先，我们需要一个容器像一个Frame，一个Window，或一个Applet，用来展现组件如panels、buttons、text areas等。

<pre class="brush: java;">
JFrame frame = new JFrame("Frame Title");
</pre>

创建一些组件如面板，按钮，文本等

<pre class="brush: java;">
final JTextArea comp = new JTextArea();
JButton btn = new JButton("click");
</pre>

添加组件到显示区域，并使用LayoutManager安排其布局。

<pre class="brush: java;">
frame.getContentPane().add(comp,BorderLayout.CENTER);
frame.getContentPane().add(btn, BorderLayout.SOUTH);
</pre>

为按钮添加一个监听器，用来与组件发生的事件交互。也就是要关联组件和用户操作，就添加一个监听器到组件上。

这里的addActionListener方法就是订阅者的注册观察方法。观察者设计模式对于一个完整的例子，查看另一个观察者例子。

<pre class="brush: java;">
btn.addActionListener(new ActionListener(){
       public void actionPerformed(ActionEvent ae){
             comp.setText("Button has been clicked");
       }
});
</pre>

<pre>
public interface ActionListener extends EventListener
</pre>

listener 接口用来接收动作事件。该类（这个例子中主要的类）主要关注处理来自事件接口的动作事件，创建该类的对象并使用该组件的addActionListener方法注册组件。动作事件发生时，该对象的actionPerformed方法将被调用。

显示帧

<pre class="brush: java;">
int width = 300;
int height = 300;
frame.setSize(width, height);
frame.setVisible(true);
</pre>

---

<h3><span id="state">Java设计模式：状态模式</span></h3>

<em>翻译自 <a href="http://www.programcreek.com/2011/07/java-design-pattern-state/" target="_blank">Java Design Pattern: State</a></em>

状态设计模式主要是在运行时改变状态。

**状态模式的故事**

人们可以生活在不同的经济条件下。他们可以是富人，也可以是穷人。这两种状态 - 穷人和富人 - 可以随时互相转换。例子是：当人很穷时，他们工作非常刻苦，当人和富裕时，他们就可以更多的玩。他们是富是穷取决于他们的生活现状。他们可以通过他们的行动改变自己的生活状态，否则，社会是不公平的。

**状态模式的类图**

下面是类图。你可以比较这个策略模式 ，以对比他们的不同之处。

<img src="http://www.programcreek.com/wp-content/uploads/2011/07/state-pattern-class-diagram.jpg"/>

**状态模式的Java代码**

下面的Java示例显示了状态模式是如何工作的。

<pre class="brush: java;">
//State classes.
package com.programcreek.designpatterns.state;

interface State {
public void saySomething(StateContext sc);
}

class Rich implements State{
@Override
public void saySomething(StateContext sc) {
System.out.println("I’m rick currently, and play a lot.");
sc.changeState(new Poor());
}
}

class Poor implements State{
@Override
public void saySomething(StateContext sc) {
System.out.println("I’m poor currently, and spend much time working.");
sc.changeState(new Rich());
}
}

//StateContext class
package com.programcreek.designpatterns.state;

public class StateContext {
private State currentState;

public StateContext(){
currentState = new Poor();
}

public void changeState(State newState){
this.currentState = newState;
}

public void saySomething(){
this.currentState.saySomething(this);
}
}

//Main class for testing
import com.programcreek.designpatterns.*;

public class Main {
public static void main(String args[]){
StateContext sc = new StateContext();
sc.saySomething();
sc.saySomething();
sc.saySomething();
sc.saySomething();
}
}
</pre>

结果：

<pre>
 I’m poor currently, and spend much time working.
 I’m rick currently, and play a lot.
 I’m poor currently, and spend much time working.
 I’m rick currently, and play a lot. 
</pre>

---

<h3><span id="strategy">Java设计模式：策略模式</span></h3>

<em>翻译自 <a href="http://www.programcreek.com/2011/01/a-java-example-of-strategy-design-pattern/" target="_blank">Java Design Pattern: Strategy</a></em>

策略模式(英文strategy pattern也可叫做policy pattern)

下面是一个关于策略模式的故事。假设Mike开车时，有时会加速行驶，但并不总是加速。当通过有交警的地方，他可能会停下来。有时候警察是非常好的，让他没有任何罚单或一个警告便顺利通过。（这种警察称“NicePolice”。）也有可能是他会被不好的警察拦下并开一张罚单。（我们称这种警察为“HardPolice”。）他并不知道什么样的警察会阻止他，直到他被拦下，只能看情况而定。这正好就是策略模式关注的点。

**策略模式类图**

<img src="http://www.programcreek.com/wp-content/uploads/2011/01/strategy-pattern-class-diagram.jpg"/>

**策略模式的Java代码**

<pre class="brush: java;">
//Define a interface Strategy, which has one method processSpeeding()
public interface Strategy {
	//defind a method for police to process speeding case.
	public void processSpeeding(int speed);
}

//Now we have two kinds of police officers.
public class NicePolice implements Strategy{
	@Override
	public void processSpeeding(int speed) {
		System.out.println("This is your first time, be sure don't do it again!");		
	}
}
public class HardPolice implements Strategy{
	@Override
	public void processSpeeding(int speed) {
		System.out.println("Your speed is "+ speed+ ", and should get a ticket!");
	}
}

//Define a situation in which a police officer will be involved to process speeding.
public class Situation {
	private Strategy strategy;
 
	public Situation(Strategy strategy){
		this.strategy = strategy;
	}
 
	public void handleByPolice(int speed){
		this.strategy.processSpeeding(speed);
	}
}

//Finally, try the result.
public class Main {
	public static void main(String args[]){
		HardPolice hp = new HardPolice();
		NicePolice ep = new NicePolice();
 
		// In situation 1, a hard officer is met
                // In situation 2, a nice officer is met
		Situation s1 = new Situation(hp);
		Situation s2 = new Situation(ep);
 
		//the result based on the kind of police officer.
		s1.handleByPolice(10);
		s2.handleByPolice(10);        
	}
}
</pre>

输出是：

<pre>
 Your speed is 10, and should get a ticket!
 This is your first time, be sure don’t do it again! 
</pre>

你可以比较策略模式和[状态模式](http://yhzhtk.info/2013/11/05/design-patterns-behavioral.html#state)，他们非常相似。主要的区别是，状态模式是当对象的状态变化时改变对象的行为，而策略模式主要是在不同情况下使用不同的算法。

**JDK中的策略模式**

<pre>
Java.util.Collections#sort(List list, Comparator c) 
</pre>

Sort 方法在不同情况下使用不同的Comparator。要知道更多关于Comparator的内容，查看[compare Comparator with Comparable](http://www.programcreek.com/2011/12/examples-to-demonstrate-comparable-vs-comparator-in-java/)。

---

