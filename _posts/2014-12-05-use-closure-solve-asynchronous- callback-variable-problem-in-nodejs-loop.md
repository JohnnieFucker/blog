---
layout: post
title: 用闭包解决node.js循环中异步回调取值问题
author: JohnnieFucker
date: '2014-12-05 14:21'
post_id: 501
category: Technology
tags:
- node.js
- 闭包
---
今天解决后台消息系统问题的时候，发现了一个以前的bug。大概的代码片段如下：

{% highlight javascript linenos %}  
{% raw %}
for(var i=0;i<10;i++){
	var a = i;
	functionABC(a,function(){
		console.log(a);
		//省略逻辑
	});
}
{% endraw %} 
{% endhighlight %}

原来的目的是在一个循环中，调用某个函数functionABC，在执行完成后，执行一个回调函数，且回调函数中需要访问函数外的变量。期望的输出值是1 2 3 4 5……
<!--break-->

看出问题所在了么？由于回调函数是异步执行的，而a变量的值是随着循环变化而变化的。所以异步回调函数里面访问到得到a的值是不定的，所以输出的数字是跳跃的或者都是10（functionABC执行需要的时间超过了for循环本身的时间）。

这个时候需要用一个闭包就可以轻松解决上面的问题，把代码改成

{% highlight javascript linenos %}  
{% raw %}
for(var i=0;i<10;i++){
	var a = i;
	(function(a){
		functionABC(a,function(){
			console.log(a);
			//省略逻辑
		});
	})(a);
}
{% endraw %} 
{% endhighlight %}