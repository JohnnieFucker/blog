---
layout: post
title: JS的类、对象和原型、原型链
author: JohnnieFucker
date: '2015-09-12 22:43'
post_id: 509
category: Technology
tags:
- js
---
javascrpt中的类，对象，原型是一个很重要也很绕的概念，面试过好多人，能说一点但讲不清楚。

js并不是真正面向对象（oo）的编程语言，所以语法中并没有class，但js是一种基于对象（object-based）的的语言。js中的一切都是对象，Object（普通对象）或者Function（函数对象），而Function是一种特殊的Object，我们可以通过很多方式来达到声明一个类的效果。比如：
<!--break-->
{% highlight javascript linenos %}  
{% raw %}
var User = function(name){
	//user类
	this.name = name;//对象属性
	this.showName =function(){
		//对象方法
		alert('the user’s name is'+name);
	};
}

var Post = function(){
	//post类
	this.content;//对象属性
	this.author;//对象属性
	this.showContent = function(){
		//对象方法
		alert('content:'+this.content);
	}
	
}
//类方法
Post.showContent = function(){
	alert('there’s no content');
}
Post.showNothing = function(){
	alert('nothing');
}

var postInstance = new Post();
postInstance.author = new User('JonnieFucker');
postInstance.content ='this is a test';

Post.showContent(); //类方法调用
Post.showNothing();//类方法调用
//postInstance.showNothing(); 对象是无法调用类方法
postInstance.showContent();//post对象方法调用postInstance.author.showName();//user对象方法调用
{% endraw %} 
{% endhighlight %}

上面的代码演示类和对象（instance）的基本使用方法。那么prototype又是干嘛的呢？js的每一种Type都提供了一个prototype属性，这个属性指向一个对象（Object），这个对象就是这个类的“原型”，所有由这个类所创建的实例对象都具有这个原型的特性。比如我们给User添加一个prototype方法

{% highlight javascript linenos %}  
{% raw %}
User.prototype.sayHello = function(){
	alert('hello, I’m '+this.name);
}
postInstance.author.sayHello();

{% endraw %} 
{% endhighlight %}
那么我们在postInstance.author就继承了sayHello方法，如果我们另外给postInstance.author特别设置一个sayHello方法，那么实例的方法会覆盖掉从类继承而来的原型方法。

{% highlight javascript linenos %}  
{% raw %}

postInstance.author.sayHello = function(){
	alert('hi, I’m '+this.name);
}
postInstance.author.sayHello();

{% endraw %} 
{% endhighlight %}

通过prototype可以快速的创建prototype对象的多个副本，这个副本是只读的，各个实例不能改变他，但是可以重写，而不会影响到其他实例。在实例对象读取某个属性的时候，首先会检查自己是否有这个属性，如果没有就去读prototype对象是否有这个属性，如果没有，就继续递归查询prototype对象指向的对象。这就引出了原型链的概念__proto__，在创建一个实例对象的时候，都有一个__proto__属性，指向创建他的类的原型对象prototype。而这个prototype是可以设置成某个类的实例，并且这个实例传递的是一个引用而不是值，所以就形成了一个链。最终这个__proto__都会指向Object的prototype，因为js是基于对象的语言，而Object的__proto__指向null，因为已经到头了。图就不画了，可以看参考的链接。



参考：[JavaScript原型及原型链详解](http://zhangjiahao8961.iteye.com/blog/2070650)



