---
layout: post
title: js setMonth的坑爹溢出问题
author: JohnnieFucker
date: '2014-12-30 14:44'
post_id: 502
category: Technology
tags:
- js
---
项目里面一直有一些时间相关的操作，比如取相对某一个时间的下一个月的时间之类的。很简单的代码片段

{% highlight javascript linenos %}  
{% raw %}
	someFunction:function(dateStr){
        var dd = new Date();
        dd.setFullYear(dateStr.substr(0, 4));
        dd.setMonth(dateStr.substr(5, 2) - 1); 
        dd.setDate(dateStr.substr(8, 2));
        return dd;
    }
{% endraw %} 
{% endhighlight %}
预期的调用是传入'2014-02-15' 那么return的值应该是一个'2014年2月15日'这样一个时间对象。在绝大部分时间测试上面的代码都会返回正确结果，但是如果在比如某月29某月30日或者31日调用就会有问题了。假设是在1月30日调用，dd在初始化的时候日期是1月30日，在setMonth之后，dd变成了2月30日，但是2月没有30日，对于这种日期的溢出，js的自动处理是把整个时间对象往后延，dd实际的值是一个2014-03-02的时间对象。在这之后setDate，return的值就变成'2014年3月15日'这样一个时间对象了!
<!--break-->
这是JS很坑爹的一个bug，因为需要在特定的日子才会出现，很难发现问题。解决的方法也比较简单，在setMonth之前，先setDate一个每个月都有的日期，比如1号。

{% highlight javascript linenos %}  
{% raw %}
	someFunction:function(dateStr){
        var dd = new Date();
        dd.setFullYear(dateStr.substr(0, 4));
        dd.setDate(1);
        dd.setMonth(dateStr.substr(5, 2) - 1); 
        dd.setDate(dateStr.substr(8, 2));
        return dd;
    }
{% endraw %} 
{% endhighlight %}



