---
title: Object.assign 是浅拷贝还是深拷贝？实现深拷贝的方法有哪些？
abbrlink: 59629
date: 2019-05-22 21:27:20
tags:
---









Object.assign()方法用于将所有可枚举属性的值从一个或者多个对象复制到目标对象。它将返回目标对象。

1、如果目标对象中的属性具有相同的键，则属性将被源对象中的属性覆盖。后面的源对象的属性将类似地覆盖前面的源对象的属性。

<!--more-->

2、Object.assign方法只会拷贝源对象的并且可枚举的属性到目标对象。该方法使用源对象的[[Get]]和目标对象的[[Set]],所以它会调用相关的getter和setter。因此，它==分配属性==，而不仅仅是复制或定义新的属性。如果合并源包含getter，这可能使其不适合将新属性合并到原型中。为了将属性定义（包含其可枚举型）复制到原型，应使用Object.getOwnProperyDescriptor()和 Object.defineProperty() 。 

3、String类型和Symbol类型的属性都会被拷贝。

4、 Object.assign 不会在那些source对象值为 `null`或 `undefined` 的时候抛出错误。 

5、针对==深拷贝==，需要使用其他办法，因为 Object.assign()拷贝的是属性值。假如源对象的属性值是一个对象的引用，那么它也只指向那个引用。也就是说，如果对象的属性值为简单类型（如string， number），通过Object.assign({},srcObj);得到的新对象为`深拷贝`；如果属性值为对象或其它引用类型，那对于这个对象而言其实是浅拷贝的。









### 深拷贝的几种实现方法

#### 1、JSON.stringify 和 JSON.parse

用JSON.stringify把对象转化为字符串，再用JSON.parse把字符串转换成新的对象。

注意： 可以转成 JSON 格式的对象才能使用这种方法，如果对象中包含 function 或 RegExp 这些就不能用这种方法了

```js
//通过js的内置对象JSON来进行数组对象的深拷贝
function deepClone(obj) {
  let _obj = JSON.stringify(obj);
  let objClone = JSON.parse(_obj);
  return objClone;
}
```



#### 2、Object.assign()拷贝

当对象中只有一级属性没有二级属性的时候，此方法为深拷贝，但是对象中有对象的时候，此方法，在二级属性以后就是浅拷贝。（注意：这边这要原因是拷贝的是属性值。）



#### 3、通过jQuery的extend方法实现深拷贝

```js
let $ = require('jquery');
let obj1 = {
   a: 1,
   b: {
     f: {
       g: 1
     }
   },
   c: [1, 2, 3]
};
let obj2 = $.extend(true, {}, obj1);
```



#### 4、lodash.cloneDeep()实现深拷贝

```js
let _ = require('lodash');
let obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
let obj2 = _.cloneDeep(obj1);
```



#### 5、使用递归的方式实现深拷贝

```js
function _deepClone(source) {
  let target;
  if (typeof source === 'object') {
    target = Array.isArray(source) ? [] : {}
    for (let key in source) {
      if (source.hasOwnProperty(key)) {
        if (typeof source[key] !== 'object') {
          target[key] = source[key]
        } else {
          target[key] = _deepClone(source[key])
        }
      }
    }
  } else {
    target = source
  }
  return target
}
```

