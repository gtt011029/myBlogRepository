---
title: Jquery实现的原理和核心
abbrlink: 39571
date: 2017-10-01 16:25:14
tags:
---





### 引言

之前接触到jquery，觉得jquery轻量好用，但不知其原理，近期了解了一下，做出如下的总结

jquery并不是一个js框架，而是一个轻量级的js库，它封装了一些方法供我们使用

### jquery的实现原理

![](/Jquery实现的原理和核心/jq1.png)

<!--more--> 



- 1)jQuery采用的是构造函数模式进行开发的,jQuery是一个类
- 2)我们常用的方法(CSS、属性、筛选、事件、动画、文档处理)都是定义在jQuery.prototype上的，只有jQuery的实例才能使用这些方法。

首先我们打开jq文件我们可以看到，jq是封装在一个闭包中的，所以做它可以字调用，其中还有一点好处就是，由于jq中有大量的命名，通过使用闭包可以防止命名污染，对外界不产生任何影响

```
//以下截取自jquery源码片段
(function( window, undefined ) {   
	/*    源码内容    */
})( window );
```

其次，我想大家一定想知道，jq既然用闭包吧自己装起来，那么它里面的方法我们是怎么可以使用的呢？

大家看jq代码就会发现，它把自己暴露给了window，这样我们就可以在外界使用了

### jq实例化

刚才我们说了，jquery将自己声明的变量全部都用外衣遮盖起来了，而我们平时使用的Jquery和$，却是真真实实的全局变量，这个是从何而来，谜底就在jquery的某一行代码，一般是在文件的末尾。

```
window.jQuery = window.$ = jQuery;
```

这一句话将我们在闭包当中定义的jQuery对象导出为全局变量jQuery和$，因此我们才可以在外部直接使用jQuery和$。window是默认的JS上下文环境，因此将对象绑定到window上面，就相当于变成了传统意义上的全局变量，就像下面这一小段代码的效果一样。

### 选择器

jq中最核心的功能，就是选择器。而选择器简单理解的话，其实就是在DOM文档中，寻找一个DOM对象的工具。

首先我们进入jquery源码中，可以很容易的找到jquery对象的声明，看过以后会发现，原来我们的jquery对象就是init对象。

```
jQuery = function( selector, context ) {     
	return new jQuery.fn.init( selector, context, rootjQuery ); 
}
```

这里出现了jQuery.fn这样一个东西，它的由来可以在jquery的源码中找到，它其实代表的就是jQuery对象的原型。

```
jQuery.fn = jQuery.prototype;
jQuery.fn.init.prototype = jQuery.fn;
```

　这两句话，第一句把jQuery对象的原型赋给了fn属性，第二句把jQuery对象的原型又赋给了init对象的原型。也就是说，init对象和jQuery具有相同的原型，因此我们在上面返回的init对象，就与jQuery对象有一样的属性和方法。

　　我们不打算深究init这个方法的逻辑以及实现，但是我们需要知道的是，jQuery其实就是将DOM对象加了一层包裹，而寻找某个或者若干个DOM对象是由sizzle选择器负责的，它的官方地址是http://sizzlejs.com/，有兴趣的猿友可以去仔细研究下这个基于CSS的选择器。

注：　很多时候，我们在jQuery和DOM对象之间切换时需要用到[0]这个属性。从截图也可以看出，jQuery对象其实主要就是把原生的DOM对象存在了[0]的位置，并给它加了一系列简便的方法。这个索引0的属性我们可以从一小段代码简单的看一下它的由来，下面是init方法中的一小段对DOMElement对象作为选择器的源码。

```
// Handle $(DOMElement)
if ( selector.nodeType ) {    
	/*     可以看到，这里将DOM对象赋给了jQuery对象的[0]这个位置  */    
	this.context = this[0] = selector;    
	this.length = 1;    
	return this;
}
```

### jq的ready方法

实现类似jquery的ready方法的效果我们是可以简单做到的，它的实现原理就是，维护一个函数数组，然后不停的判断DOM是否加载完毕，倘若加载完毕就触发所有数组中的函数。

### jq的extend方法

而对于jquery来说，extend方法大有用处，没有它我们依然可以很好的使用jquery，但是有了它，我们会更加畅快。

常用方法：

　1、使用jQuery.fn.extend可以扩展jQuery对象，使用jQuery.extend可以扩展jQuery，前者类似于给类添加普通方法，后者类似于给类添加静态方法。

　　2、两个extend方法如果有两个object类型的参数，则会将后面的参数对象属性扩展到第一个参数对象上面，扩展时可以再添加一个boolean参数控制是否深度拷贝。







