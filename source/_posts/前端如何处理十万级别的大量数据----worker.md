---
title: 前端如何处理十万级别的大量数据----worker
abbrlink: 4544
date: 2019-05-15 20:16:28
tags:
---







​		

## 一、简介

​		web worker为JavaScript创造多线程环境，允许主线程创建worker线程，将一些任务分配给后者运行。在主线程运行的同时，worker线程在后台运行，两者互不干扰。等到worker线程完成计算任务，再把结果返回给主线程。

<!--more-->

## 二、使用注意点

（1）同源限制

分配给worker线程运行的脚本文件，必须与主线程的脚本文件同源

（2）DOM限制

worker线程所在的全局对象，与主线程不一样，无法读取主线程所在网页的DOM对象，也无法使用document、window、parent这些对象。但是，worker线程是可以navigator对象和location对象。

（3）通信联系

worker线程和主线程不在同一个上下文环境，它们不能直接通信，必须通过消息完成。

（4）脚本限制

worker线程不能执行alert()和confirm()，但可以使用XMLHttpRequest对象发出ajax请求。

（5）文件限制

worker线程无法读取本地文件，即不能打开本机的文件系统（file://），它所加载的脚本必须来自网络。



## 三、基本用法

```js
var worker = new worker('work.js')      //work.js来自网络


worker.postMessage('hello world')       //主线程通过worker.postMessage()，向worker发消息



worker.onmessage = function (event) {
    console.log('recevied message'+ event.data);
}						//主线程通过worker.onmessage指定监听函数接收子线程发回来的消息

worker.terminate()      //完成工作之后，主线程就可以把它关掉             

```



```js
// 线程中
self.addEventListener('message',function(e){
	self.postMessage('you said'+ e.data);      //主线程发来的数据
},false)

self.close                      //关闭自身
```



#### 数据通信

主线程与worker线程的通信是拷贝关系，即是传值而不是传址