---
layout: post
status: publish
published: true
title: ios 跑马灯图片轮转
author: JohnnieFucker
excerpt: "其实这个效果在很多app里面都有应用，其实很简单，就是UIScrollView+UIPageControl实现一个可滑动的图片分页。然后加个timer让他自动切换。刚开始的时候加了timer后pagecontrol是自动在轮转了，但是上面的图片不动⋯⋯\r\n"
wordpress_id: 377
wordpress_url: http://www.oushit.com/?p=377
date: '2011-08-14 23:16:12 +0800'
date_gmt: '2011-08-14 15:16:12 +0800'
category: Technology
tags:
- ios
- 编程
comments: []
---
<p>其实这个效果在很多app里面都有应用，其实很简单，就是UIScrollView+UIPageControl实现一个可滑动的图片分页。然后加个timer让他自动切换。刚开始的时候加了timer后pagecontrol是自动在轮转了，但是上面的图片不动⋯⋯<br />
<!--break--><a id="more-377"></a><br />
想了很久，有一天突然顿悟，要自己手动设置UIScrollView的ContentOffset！！</p>
<blockquote><p>[m_TopPages setContentOffset:CGPointMake(m_TopPages.frame.size.width*page, 0) animated:YES];</p></blockquote>
<p>加了这句关键语句后，效果达成，bingo！！！</p>
