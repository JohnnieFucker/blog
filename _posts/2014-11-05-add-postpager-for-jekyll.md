---
layout: post
title: 给jekyll增加自定义分页
author: JohnnieFucker
date: '2014-11-03 20:46'
post_id: 497
category: TECHNOLOGY
cover: '/images/480bf44c6f52ef4c567.jpg'
---
<p>把blog改造到jekyll后遇到的第一个问题是文章如何分页。这在动态网页时代完全不是一个问题。但是在玩静态网页的时候就是非常大的一个问题了。jekyll自己内置了一套自动生成post分页的机制，网上也有很多教程，但是都是比较粗暴的把所有页码全部显示出来，在这些基础之上我做了一些小改造，让分页只显示当前分页的前后两页。</p>
<!--break-->

配置jekyll的分页是在_config.yml中添加  
<pre>
<code>
paginate : 5
paginate_path: "page:num"
</code>
</pre>

在index.html中添加以下js代码  
{% highlight html linenos %}  
{% raw %}
<div class="pager">
    {% if paginator.previous_page %}
    <a href="{{ paginator.previous_page_path |replace: '//', '/'}}">prev</a>
    {% endif %}
    {% if paginator.page == 1 %}
    <span class="active">1</span>
    {% else %}
    <a href="/index.html">1</a>
    {% endif %}
    {% if paginator.page > 4 %}
    <span>…</span>
    {% endif %}
    {% for page in (2..paginator.total_pages) %}
    {% if page == paginator.page %}
    <span class="active">{{ page }}</span>
    {% else %}
    {% assign c1 = page|minus:paginator.page %}
    {% assign c2 = paginator.page|minus:page %}
    {% if page > paginator.page and c1 < 3 %}
    <a href="/{{ site.paginate_path |  replace: '//', '/' | replace: ':num', page }}">{{ page }}</a>
    {% endif %}
    {% if paginator.page > page and c2 < 3 %}
    <a href="/{{ site.paginate_path |  replace: '//', '/' | replace: ':num', page }}">{{ page }}</a>
    {% endif %}
    {% endif %}
    {% endfor %}
    {% assign c3 = paginator.page|plus:3 %}
    {% if c3 < paginator.total_pages %}
    <span>…</span>
    {% endif %}
    {% if c3 <= paginator.total_pages %}
    <a href="/{{ site.paginate_path |  replace: '//', '/' | replace: ':num', paginator.total_pages }}">{{paginator.total_pages
        }}</a>
    {% endif%}
    {% if paginator.next_page %}
    <a href="{{ paginator.next_page_path|replace: '//', '/' }}">next</a>
    {% endif %}
</div>
{% endraw %} 
{% endhighlight %}