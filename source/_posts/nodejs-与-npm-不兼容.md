---
title: nodejs 与 npm 不兼容
abbrlink: 52361
date: 2020-01-14 10:54:34
tags:
---









使用npm install 装包的时候：

npm install antd babel-plugin-import --save

报这样的错误：

`Unhandled rejection RangeError: Maximum call stack size exceededill install loadIdealTree` 



<!--more-->

![1586833220991](/nodejs与npm不兼容/1.png)





原因：

新版本的nodejs与npm最新版本出现不兼容



处理方式：

给npm降级： npm install -g npm@5.4.0 

然后重新使用npm装包：npm install antd babel-plugin-import --save

![1586833460421](/nodejs与npm不兼容/2.png)

发现降级后的npm与node依旧不兼容

解决办法：

1.npm uninstall -g npm

2.npm install -g npm

重装一下

重新使用npm装包：npm install antd babel-plugin-import --save

![1586835210208](/nodejs与npm不兼容/3.png)

发现溢出，超过最大栈问题，

这边可以重复试一次，有可能就装上了，如果还是装不上，可以试一下cnpm

成功