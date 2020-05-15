---
title: Postman 接口测试 请求失败 500
abbrlink: 29799
date: 2019-05-10 16:25:55
tags:
---









最近使用postman测接口的时候老是报500的错误，后找到原因，现总结如下：

![](/postman/错误.png)

<!--more-->

其实就是json格式的问题

1、header这边改为application/json

![1577953913031](/postman/content)

2、

使用raw，编写json请求体；

字段拼接，标点符号要带上；

字段名称一定要用双引号引起来

json格式，大括号一定要有

![1577954292400](/postman/正确)



