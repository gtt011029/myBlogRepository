---
title: 细说Vue的nextTic
abbrlink: 24022
date: 2019-01-02 10:34:23
tags:
---







### 作用

```
该方法会在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。
```

应用场景：

> - 在Vue生命周期的`created()`钩子函数进行的DOM操作一定要放在`Vue.nextTick()`的回调函数中
> - 在数据变化后要执行的某个操作，而这个操作需要使用随数据改变而改变的DOM结构的时候，这个操作都应该放进`Vue.nextTick()`的回调函数中。

```
created() {     //改变数据    
	vm.message = 'changed'  //想要立即使用更新后的DOM。这样不行，因为设置message后DOM还没有更新     console.log(document.getElementById('testCount').innerHTML) // 并不会得到'changed'    //这样可以，nextTick里面的代码会在DOM更新后执行    
	Vue.nextTick(function(){        														console.log(document.getElementById('testCount').innerHTML) //可以得到'changed'    })},
```

<!--more--> 

### nextTick原理

#### 1、异步说明

```
Vue 实现响应式并**不是数据发生变化之后 DOM 立即变化**，而是按一定的策略进行 DOM 的更新。
```

 在 Vue 的文档中，说明 Vue 是**异步**执行 DOM 更新的。关于异步的解析，可以查看阮一峰老师的[这篇文章，](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)截取关键部分如下：

 具体来说，异步执行的运行机制如下。

> （1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。
> （2）主线程之外，还存在一个”任务队列”（task queue）。只要异步任务有了运行结果，就在”任务队列”之中放置一个事件。
> （3）一旦”执行栈”中的所有同步任务执行完毕，系统就会读取”任务队列”，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
> （4）主线程不断重复上面的第三步。

```
下图就是主线程和任务队列的示意图。
```

![](/Vue生命周期/vue-next3835190331-580cd2a9443a7_articlex.png)

#### 2、时间循环说明

```
简单来说，Vue 在修改数据后，视图不会立刻更新，而是等**同一事件循环**中的所有数据变化完成之后，再统一进行视图更新。图解：
```

![](/Vue生命周期/vue-nest1596618069-5a5da8c8522c2_articlex.png)

##### 事件循环：

 第一个 tick（图例中第一个步骤，即’本次更新循环’）：

> 1. 首先修改数据，这是同步任务。同一事件循环的所有的同步任务都在主线程上执行，形成一个执行栈，此时还未涉及 DOM 。
> 2. Vue 开启一个异步队列，并缓冲在此事件循环中发生的所有数据改变。如果同一个 watcher 被多次触发，只会被推入到队列中一次。

```
第二个 tick（图例中第二个步骤，即'下次更新循环'）：
```

> ```
>  同步任务执行完毕，开始执行异步 watcher 队列的任务，更新 DOM 。Vue 在内部尝试对异步队列使用原生的 </br>
> 
>  Promise.then 和MessageChannel 方法，如果执行环境不支持，会采用 setTimeout(fn, 0) 代替。
> ```

```
第三个 tick（图例中第三个步骤）：
```

> ```
>  此时就是文档所说的：`下次 DOM 更新循环结束之后`
> 
> 此时通过 Vue.nextTick 获取到改变后的 DOM 。通过 setTimeout(fn, 0) 也可以同样获取到。
> ```

 总结事件循环 ： **同步代码执行 -> 查找异步队列，推入执行栈，执行Vue.nextTick[事件循环1] ->查找异步队列，推入执行栈，执行Vue.nextTick[事件循环2]**…

```
**异步是单独的一个tick，不会和同步在一个 tick 里发生，也是 DOM 不会马上改变的原因**。
```

### nextTick()案例

```
//点击按钮显示文本输入框，同时让其获取焦点
methods:{   
	showsou(){      
		this.showit = true      
		this.$nextTick(function () {        // DOM 更新了        									document.getElementById("keywords").focus()      
		})    
	}}
```