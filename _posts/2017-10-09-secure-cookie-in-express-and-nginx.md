---
layout: post
title:  在express中配置cookie secure
author: JohnnieFucker
date: '2017-10-09 16:22'
post_id: 515
category: Technology
tags:
- cookie
- express
- nginx
---

最近在做某个项目的渗透测试的时候，其中有一条反馈，指出cookie没有添加secure属性，这样可能导致中间人攻击。


其实在express的设置中配置cookie非常简单，但是由于我们在node前端增加了nginx作为反向的代理。所以在express和nginx的配置中都需要做相关的配置。否则会导致请求session的丢失。


express 的nginx配置:
```
app.enable('trust proxy');  //这句很重要
app.use(express.bodyParser());
app.use(express.cookieParser());
app.use(express.session({
   secret: 'Super Secret Password',
   proxy: true, //这句很重要 
   key: 'session.sid',
   cookie: {secure: true},//其他配置项可以查看文档
   store: new sessionStore() 
}));
```

nginx 中的配置：
```
server {
    #此处省略

    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header X-Forwarded-Proto $scheme; 
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
        proxy_read_timeout 5m;
        proxy_connect_timeout 5m;
        proxy_pass http://nodeserver;
        proxy_redirect off;
    }
}
```