---
layout: post
title: 禁用ios webview中链接的自动识别
author: JohnnieFucker
date: '2015-05-22 16:06'
post_id: 508
category: Technology
tags:
- ios
---
最近做一个功能，在ios应用中通过webview打开一个页面。页面里面有类似www.oushit.com这样的文字，但并不是一个link元素。webview会自动识别为链接并且改变其样式。搜了一圈基本都是说通过meta的format-detection禁用电话号码识别。最后还是在stackoverflow上面找到了解决方案。要在ios应用里面声明禁用识别。代码如下


self.webView.dataDetectorTypes = UIDataDetectorTypeNone;
