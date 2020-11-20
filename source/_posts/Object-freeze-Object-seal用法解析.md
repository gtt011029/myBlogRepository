---
title: Object.freeze&&Object.seal用法解析
abbrlink: 38723
date: 2019-11-17 16:13:24
tags:
---







## 一、对象冻结

Object.freeze()

```javascript
let objectToFreeze = {
  age: 18,
  name: 'tt',
  like: ['apple', 'banana', 'orange'],
  sibling: {
    age: 25,
    name: 'tina'
  }
}

Object.freeze(objectToFreeze)
objectToFreeze.age = 19 // age 依旧为18，不会改变
objectToFreeze.sibling.age = 26 // age 为26，会改变
delete objectToFreeze.name // 不会被删除
console.log(objectToFreeze)   


// 1、不能向对象添加新的属性
// 2、如果属性本身不是对象数组，则无法修改（因为sibling的地址并没有改变）
// 3、不能从对象中删除属性

// 解析：这边我们所拥有的只是一个浅渐冻，因为冻结的是直接附加到对象的对象。不关心对象和数组内部的属性。
```

<!--more-->



Object.isFrozen（）

用于判断当前对象是否被冻结

```js
let objectToFreeze = {
  age: 18,
  name: 'tt',
  like: ['apple', 'banana', 'orange'],
  sibling: {
    age: 25,
    name: 'tina'
  }
}
let obj = {
  aa: 11
}

Object.freeze(objectToFreeze)
delete objectToFreeze.name
objectToFreeze.age = 19
objectToFreeze.sibling.age = 26
console.log(objectToFreeze)
console.log(Object.isFrozen(objectToFreeze))   // true
console.log(Object.isFrozen(obj))    // false
```





## 物体密封

像Frozen方法一样，Object.seal() 也将对象作为参数，Object.seal是Object.freeze较软的版本

1、不能删除或添加元素

2、但是可以修改现有元素

```javascript
let objectToSeal = {
  age: 18,
  name: 'tt',
  like: ['apple', 'banana', 'orange'],
  sibling: {
    age: 25,
    name: 'tina'
  }
}

Object.seal(objectToSeal)
delete objectToSeal.name   // 无法删除
objectToSeal.age = 19     //  可以改变
objectToSeal.sibling.age = 26  // 可以改变
delete objectToSeal.sibling.name   // 可以删除
console.log(objectToSeal)
```



Object.isSealed(obj)

判断当前对象是否密封

```javascript
let objectToSeal = {
  age: 18,
  name: 'tt',
  like: ['apple', 'banana', 'orange'],
  sibling: {
    age: 25,
    name: 'tina'
  }
}

Object.seal(objectToSeal)
delete objectToSeal.name
objectToSeal.age = 19
objectToSeal.sibling.age = 26
delete objectToSeal.sibling.name
console.log(objectToSeal)
console.log(Object.isSealed(objectToSeal))   // true
```





## 与const对比

上面两个方法适用于对象，而const适用于binding。Object.freeze使对象不可变，而const创建一个不可变的绑定，为变量分配值后，就无法为该绑定分配新值。





## 原型

注意：如果对一个对象冻结或者密封后，就无法改变其原型