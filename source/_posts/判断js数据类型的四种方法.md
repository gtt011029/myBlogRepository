---
title: 判断js数据类型的四种方法
abbrlink: 9681
date: 2019-02-01 11:19:18
tags:
---







​		我们知道，es5中js的数据类型有7种，分别为：基本数据类型5种：Number、String、Boolean、Undefined 、NULL；引用数据类型2种：object、function。es6中又多加了一种基本数据类型，symbol



常见的判断js类型的方法主要有这4种： typeof、instanceof、constructor、toString 

<!--more--> 

### typeof

**描述：**typeof 返回一个表示数据类型的字符串，返回结果包括：number、boolean、string、object、undefined、function等。

​		typeof 可以对JS基本数据类型做出准确的判断（除了null），而对于引用类型返回的基本上都是object, 其实返回object也没有错，因为所有对象的原型链最终都指向了Object,Object是所有对象的`祖宗`。 但当我们需要知道某个对象的具体类型时，typeof 就显得有些力不从心了。

 注意：typeof null会返回object，因为特殊值null被认为是一个空的对象引用 ，这边我的上一篇博客有写到原因：null的类型转化为二进制以000开头，被认定为对象

**优点：**使用简单，能够对基本数据类型做出准确的判断

**缺点：**对引用数据类型的判断，有点力不从心





### instanceof

**描述：**判断对象和构造函数在原型链上是否有关系，如果有关系，返回true，否则返回false

例：

```js
  	var str = 'hello';
    console.log(str instanceof String);//false
    
    var bool = true;
    console.log(bool instanceof Boolean);//false
    
    var num = 123;
    console.log(num instanceof Number);//false
    
    var nul = null;
    console.log(nul instanceof Object);//false
    
    var und = undefined;
    console.log(und instanceof Object);//false
    
    var oDate = new Date();
    console.log(oDate instanceof Date);//true
    
    var json = {};
    console.log(json instanceof Object);//true
    
    var arr = [];
    console.log(arr instanceof Array);//true
    
    var reg = /a/;
    console.log(reg instanceof RegExp);//true
    
    var fun = function(){};
    console.log(fun instanceof Function);//true
    
    var error = new Error();
    console.log(error instanceof Error);//true
```



**优点：**能检测出引用类型

**缺点：**上述的例子可以看到，基本数据类型是没有检验出类型来的，（但是我们使用下面的方法创建number、str、boolean，是可以检测出类型来的）

```js
var num = new Number(123);
var str = new String('abcdef');
var boolean = new Boolean(true);
```





### constructor

**描述：**查看对象对应的构造函数。constructor在其对应对象的原型下面是自动生成的。当我们写一个构造函数的时候，程序会自动添加：构造函数名.prototype.constructor = 构造函数名

```js
  	var str = 'hello';
    console.log(str.constructor == String);//true

    var bool = true;
    console.log(bool.constructor == Boolean);//true

    var num = 123;
    console.log(num.constructor ==Number);//true

   	var nul = null;
   	console.log(nul.constructor == Object);//报错

    var und = undefined;
    console.log(und.constructor == Object);//报错

    var Date1 = new Date();
    console.log(Date1.constructor == Date);//true

    var json = {};
    console.log(json.constructor == Object);//true

    var arr = [];
    console.log(arr.constructor == Array);//true

    var reg = /a/;
    console.log(reg.constructor == RegExp);//true

    var fun = function(){};
    console.log(fun.constructor ==Function);//true

    var error = new Error();
    console.log(error.constructor == Error);//true
```

**优点：**基本能检验出所有的类型，除了undefined和null

**缺点：**上述举例可以看出，undefined和null是不能够判断出类型的，并且会报错。因为null和undefined是无效的对象，因此是不会有constructor存在的。其次，这边值得注意的是，constructor并不是那么的可靠哦，因为constructor是可以被修改的





###  Object.prototype.toString 

**描述：**tosString是Object原型对象上的一个方法，该方法默认返回其调用者的具体类型，更严格的讲，是toString运行时this指向的对象类型，返回的格式为【object xxx】，xxx是具体的数据类型

```js
 	var str = 'hello';
    console.log(Object.prototype.toString.call(str));//[object String]

    var bool = true;
    console.log(Object.prototype.toString.call(bool))//[object Boolean]

    var num = 123;
    console.log(Object.prototype.toString.call(num));//[object Number]

    var nul = null;
    console.log(Object.prototype.toString.call(nul));//[object Null]

    var und = undefined;
    console.log(Object.prototype.toString.call(und));//[object Undefined]

    var oDate = new Date();
    console.log(Object.prototype.toString.call(oDate));//[object Date]

    var json = {};
    console.log(Object.prototype.toString.call(json));//[object Object]

    var arr = [];
    console.log(Object.prototype.toString.call(arr));//[object Array]

    var reg = /a/;
    console.log(Object.prototype.toString.call(reg));//[object RegExp]

    var fun = function(){};
    console.log(Object.prototype.toString.call(fun));//[object Function]

    var error = new Error();
    console.log(Object.prototype.toString.call(error));//[object Error]
```

**优点：**能检测出所有的类型

**缺点：**IE6下，undefined和null均为Object

