---
title: js基础之setTimeout与setInterval原理分析
abbrlink: 39007
date: 2019-10-26 10:01:29
tags:
---







setTimeout与setInterval是JavaScript引擎提供的两个定时器方法，分别用于函数的延迟执行和循环调动。setTimeout是通过一个定时器，让函数在定时结束后才开始调用；setInterval是每隔一段时间执行一次函数。



<!--more-->



## 知识铺垫

这些知识点可以看我这篇博客：https://gtt011029.github.io/posts/55049/   （对javaScript事件驱动的理解）









### 单线程模型

由于js被设计用在浏览器环境，而该环境下存在大量可能发生冲突的DOM操作，为了避免进行复杂的冲突处理，javaScript设计者舍弃了Java的多线程模型，将其设计成一门单线程语言。

ps：这里的单线程是指javaScript的主线程只有一个。除了主线程还有I/O线程（子线程），通过事件循环来处理I/O问题，其受主线程的控制。

### 任务队列

所谓任务队列，就是用于存储等待执行的任务的队列。由于javaScript是一门单线程语言，如果当前有一个任务需要执行，但javaScript引擎正在执行其他的任务，那么这个任务就会被放在任务队列中，当javaScript引擎处理完手中的事，再去查看任务队列中是否有待执行的任务，如果有的话就去执行。



### setTimeout与setInterval

setTimeout(func, delay, args)：设置超时调用，js引擎会为func设置一个定时器，delay毫秒后，将func添加到任务队列等待执行。

SetInterval(func, interval, args)：设置循环调用，js引擎每隔interval毫秒，就会把func添加到队列中一次。



相同点：

1、都会加入到队列中，等待线程空闲时执行

2、两者都无法保证何时执行回调（因为不知道线程什么时候空闲）



不同点：

1、setTimeout只会往队列中添加一次，而setInterval会每隔一段时间往队列中添加一次

2、setTimeout可以保证函数在指定时间间隔不会执行，而setInterval无法保证



### 运行机制



setTimeout：

​		在执行该语句时设置一个定时器，定时器的时间为设置的延时时间，计时结束后，将传入的函数添加到任务队列中。

​		setTimeout函数本身会返回一个句柄，我们可以在函数执行之前通过clearTimeout取消该定时器。



setInterval：

​		本质上就是每隔一段时间把回调函数放进任务队列中，但是其有一个原则，就是如果队列中存在之前由其添加的回调函数，就会放弃本次的添加（不会影响之后的计时），当然也可以通过clearInterval移除定时器。

​		由于setInterval只负责向队列中添加函数，而不考虑函数的执行，就会出现下面的情况：

​		假设线程执行完setInterval(func, 100, args)后处于完全空闲状态（即只要向任务队列添加函数就会立即执行）。而func是一个相对复杂的函数，执行该函数需要90毫秒。那么函数的执行过程就会变成下图所示：	

​	

![](/js基础之setInterval和setTimeout原理分析/setInterval.png)

从图中可以看到，从上次函数执行完毕，到下次开始执行，之间只间隔了10毫秒，而不是我们所希望的每隔100毫秒执行一次（因为setInterval只关注任务添加，不关注任务执行）。

由于上述机制，在很多情况下，setInterval都会遇到一些性能问题。就拿上面的例子来说，我们的本意可能是每隔100毫秒执行一次函数，结果只等待了10毫秒就又执行了一次。另外，对于复杂的实际情况，setInterval经常出现两次的执行间隔相差甚远的情况，对于用户能感知到的操作，这会带来很不好的用户体验。因此在实际编码中，开发者通常会使用setTimeout来模拟实现setInterval效果（下面会有举例）。

而如果线程一开始是繁忙的，直到150毫秒处才进入空闲状态（假设func执行时长为10毫秒），那么实际的运行将变成下图所示：
![在这里插入图片描述](/js基础之setInterval和setTimeout原理分析/setInterval-2.png)
这里在100毫秒处向队列添加func时，由于线程繁忙，上次添加的func还在队列中等待，因此直接丢弃本次要添加的函数，但在200毫秒时仍然重新向队列中添加func。





## 应用场景

#### setTimeout

主要用于需要进行延时滴啊用的场景中，例如“节流”、“防抖”，此外由于setInterval的性能问题，通过我们会用setTimeout模拟setInterval

```javascript
function simulateInterval(callback, interval) {
  let timerId = null;
  const fn = () => {
    callback();
    const prevTimerId = timerId;
    timerId = setInterval(fn, interval);
    clearTimeout(prevTimerId);
  };
  return setTimeout(fn, interval);
}

simulateInterval(() => {console.log('test')}, 1000)
```

利用setTimeout保证在指定时间内不会执行的特点，我们可以在执行完上次的回调函数后，重置定时器，实现循环执行func的效果，并且从上次执行完毕到下次执行开始，至少会经过100毫秒。这在实际的编码中通常会带来较大的性能提升，同时函数的执行间隔也会相对稳定。



#### setInterval

尽管存在上述性能问题，setInterval的使用场景相对较少，但当所使用的接口来自外部（即回调函数本身无法修改）时，就必须通过setInterval来实现循环执行了。此外，对于动画效果来说，我们通常会希望动画运行的更加平滑（也就是希望函数运行得更频繁），这时使用setInterval往往更加流畅，

除了这类情况，开发者一般不会使用setInterval方法进行循环调用。





## 补充说明

setTimeout与setInterval的第一个参数可以是一个匿名函数，也可以是一个函数名，或者是一个字符串，如下面的写法都是合法的：

```javascript
function func(msg){
  ...
}
//传入回调函数名
setTimeout(func, 100, "夕山雨");
//传入匿名函数
setTimeout(function(name){
  ...
}, 100, "夕山雨");
//传入字符串，js引擎会将其解析为函数体
setTimeout("alert('夕山雨')", 100);

123456789101112
```

但是传入如下的格式就可能报错：

```javascript
setTimeout(func("夕山雨"), 100);
1
```

因为这种写法实际上是先调用func函数，然后再将返回值添加到任务队列。如果func的返回值不是函数（或可执行的字符串），那么程序就会报错；如果返回值是函数，则会将返回的函数添加到任务队列。该情况可以写成下面的形式：

```javascript
//将其作为字符串传入，就可以被正确解析
setTimeout("func('夕山雨')", 100)；
12
```

此外，当给setTimeout传入的延迟时间为0时，**并不代表回调函数会立即执行。实际上浏览器规定的有一个默认的最短计时时间，对于现代浏览器，这个时间一般为4毫秒（老版本的浏览器则会更长一些）**。也就是说，即使传入的延迟时间为0，浏览器也会至少在4毫秒后才会执行。

上述补充说明同样适用于setInterval。

