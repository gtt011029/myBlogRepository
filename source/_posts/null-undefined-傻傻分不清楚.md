---
title: null && undefined 傻傻分不清楚
abbrlink: 60868
date: 2019-01-10 19:51:53
tags:
---



​		日常大家在写代码的时候总会碰到，null，和 undefined两种值，虽然这两个值的类型都不相同，但是大家潜意识中会把其看作的相同的意思，甚至，输入 undefined == null    得到的是true

现在简单的分析一下这两种值的区别：

<!--more-->

### undefined

​	 undefined 的字面意思就是：未定义的值 。这个值的语义是，希望**表示一个变量最原始的状态，而非人为操作的结果 。** 这种原始状态会在以下 4 种场景中出现： 

1、声明了一个变量但是没有赋值

```js
var foo
console.log (foo)   //undefined 
```

2、访问对象上不存在的属性

```js
console.log(Object.foo); // undefined
var arr = [];
console.log([0]); // undefined
```

3、函数定义了形参，但没有传递实参

```js
/函数定义了形参 a
function fn(a) {
    console.log(a); // undefined
}
fn(); //未传递实参
```

4、使用 void 对表达式求值

```
void 0 ; // undefined
void false; // undefined
void []; // undefined
void null; // undefined
void function fn(){} ; // undefined
```

ECMAScript  明确规定 void 操作符 对任何表达式求值返回的都是 undefined，这和函数执行造作没有返回值的作用是一样的，JavaScript 中的函数都有返回值，当没有return 操作时，就返回一个原始的状态值，这个值就是undefined，表明函数的返回值未被定义

因此，undefined一般都来自某个表达式最原始的状态值，不是认为的操作结果。当然，你也可以手动的给变量赋值undefined，但这样没有意义，因为不赋值就是undefined。





### null

null的字面意思是空值。这个值的语义是，希望表示一个被人为的重置为空对象，而非一个变量最原始的状态。在内存中的表示就是，栈中变量没有指向堆中的内存对象，简答理解就是一个变量他的指针为空，即：

![](/nullUndefined/1.png)

 当一个对象被赋值了null 以后，原来的对象在内存中就处于游离状态，GC 会择机回收该对象并释放内存。因此，如果需要释放某个对象，就将变量设置为 null，即表示该对象已经被清空，目前无效状态。试想一下，如果此处把 null 换成 undefined 会不会感到别扭? 显然语义不通，其操作不能正确的表达其想要的行为。 

与 null 相关的另外一个问题需要解释一下：

```
`typeof` `null` `==``'object'` 
```

null 有属于自己的类型 Null，而不属于Object类型，typeof 之所以会判定为 Object 类型，是因为JavaScript 数据类型在底层都是以二进制的形式表示的，**二进制的前三位为 0 会被 typeof 判断为对象类型**，而 null 的二进制位恰好都是 0 ，因此，null 被误判断为 Object 类型。

> 000 - 对象，数据是对象的应用
>
> 1 - 整型，数据是31位带符号整数
>
> 010 - 双精度类型，数据是双精度数字
>
> 100 - 字符串，数据是字符串
>
> 110 - 布尔类型，数据是布尔值

其实，我们可以通过另一种方法获取 null 的真实类型：

```js
`Object.prototype.toString.call(``null``) ;``// [object Null]`
```

通过 Object 原型上的toString() 方法可以获取到JavaScript 中对象的真实数据类型，当然 undefined 类型也可以通过这种方式来获取：

```js
`Object.prototype.toString.call(undefined) ;``// [object Undefined]`
```

### 相似性

虽然 undefined 和 null 的语义和场景不同，但总而言之，它们都表示的是一个**无效的值。** 因此，在JS中对这类值访问属性时，都会得到异常的结果：

```js
`null``.toString();``// Cannot read property 'toString' of null``undefined.toString();``// Cannot read property 'toString' of undefined　　`
```

ECMAScript 规范认为，既然 null 和  undefined 的行为很相似，并且都表示 一个无效的值，那么它们所表示的内容也具有相似性，即有

```js
`undefined ==``null``;``// true`
```

不要试图通过转换数据类型来解释这个结论，因为：

```js
`Number(``null``);``// 0``Number(undefined);``// NaN` `// 在比较相等性之前，null 没有被转换为其他类型``null` `== 0 ;``//false`
```

但 === 会返回 false ，因为全等操作 === 在比较相等性的时候，不会主动转换分项的数据类型，而两者又不属于同一种类型：

```js
`undefined ===``null``;``// false，类型不相同``undefined !==``null``; ``// true, 类型不相同`
```

### 总结

用一句话总结两者的区别就是：undefined 表示一个变量自然的、最原始的状态值，而 null 则表示一个变量被人为的设置为空对象，而不是原始状态。所以，在实际使用过程中，为了保证变量所代表的语义，不要对一个变量显式的赋值 undefined，当需要释放一个对象时，直接赋值为 null 即可。