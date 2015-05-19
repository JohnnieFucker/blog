---
layout: post
title: webkit中的event.target
author: JohnnieFucker
date: '2015-05-19 15:17'
post_id: 507
category: Technology
tags:
- js
- angular
---
前几天尝试用angular来做推事本里面的自定义应用，希望能够加速开发速度。遇到一个关于事件的问题。
{% highlight html linenos %}  
{% raw %}

<button ng-click=funcA($event);>
	<i>a</i>
	<span>bc</span>
</button>

{% endraw %} 
{% endhighlight %}
在很多angluar教程里面，用$event.target来取当前触发事件的dom元素，而实际在firefox下$event.target 指向的是button。在webkit的浏览器里，比如chrome，safari，$event.target 指向的是i或者span，视你点击的区域是哪个元素。这应该不是angluar的问题，而是不同浏览器对事件处理的不同。

后来在stackoverflow找到解决的方法，用$event.currentTarget代替$event.target，最保险的方法是加个判断。
{% highlight javascript linenos %}  
{% raw %}

var elem = $event.currentTarget ||$event.srcElement

{% endraw %} 
{% endhighlight %}


stackoverflow问题地址：[点此](http://stackoverflow.com/questions/23107613/angularjs-get-original-element-from-ng-click)
