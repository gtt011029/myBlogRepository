---
title: 函数声明和函数表达式的区别----js
abbrlink: 17981
date: 2019-12-15 17:37:13
tags:
---



javaScript定义函数有两种类型：

#### 一、函数声明

```js
function declaration (type) {
	return type === 'declaration'
}
```



#### 二、函数表达式

```js
var expression = function (type) {
	return type === 'expression'
}
```

<!--more-->

#### 三、声明提前

**声明提前是函数声明和函数表达式最大的区别**

声明提前：

​	1、变量的声明会提前到函数的顶部

​	2、只有声明会提前、赋值并不会提前

​	3、在声明之前的变量值为undefined

例：

```js
sayHello(); //hello 函数声明
var sayHello = function () {
    return 'hello 函数表达式'
}
sayHello();  //hello 函数表达式
function sayHello() {
	return 'hello 函数声明'
}
sayHello();  //hello 函数表达式

```



javaScript解释器中存在一种变量声明被提升的机制

函数声明：

​	函数声明会被提升到最前面，即使写代码的时候写到最后面，也还是会被提升到最前面

函数表达式：

​	函数表达式创建的函数是运行时进行赋值，且要等到表达式赋值完成后才能调用



上述的代码，实际上是下面的代码格式

```js
function sayHello() {  //提升函数声明
	return 'hello 函数声明'
}
var sayHello;   //提升变量声明
sayHello(); //hello 函数声明
sayHello = function () {
    return 'hello 函数表达式'
}
sayHello();  //hello 函数表达
sayHello();  //hello 函数表达式
```



#### 总结：

这个区别看似微不足道，但在某些情况下确实是一个难以察觉并且致命的陷阱。出现这个陷阱的本质是两种类型在函数提升和运行时机（解析时/运行时）上的差异。

JavaScript中函数声明和函数表达式是存在区别的，**函数声明**在js解析时进行函数提升，因此在同一个作用域内，不管函数声明在哪里定义，该函数都可以进行调用。而**函数表达式**的值是在js运行时确定的，并且在表达式赋值完成后，该函数才能调用。这个微小的区别，可能导致js代码出现意想不到的bug，让你陷入莫名的陷阱中。

