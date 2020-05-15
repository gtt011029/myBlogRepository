---
title: react新特性--Hook
abbrlink: 47207
date: 2019-09-07 13:53:57
tags:
---





Hook是React 16.8的新增特性，它可以让你在不编写class的情况下使用state以及其他的React特性。



## Hook解决的问题

1、在组件之间复用状态很难  (这边可以自定义Hook，把复用的部分提出来)

<!--more-->

2、复杂组件变得难以理解  (这边就是有的不是相关逻辑的代码因生命周期的限制放在一起，是相关逻辑的代码分开，使得代码不便于理解，这边可以使用Effect Hook，拆分成更小的函数)

3、难以理解的class  (一句话解释Hook：Hook是一些可以让你在函数组件里“钩入”React state及生命周期等特性的函数。Hook不能在class中使用)



## Hook的使用规则

Hook就是javaScript函数，但是使用它们会有两个额外的规则：

#### 1、只能在函数最外层调用Hook。不要在循环、条件判断或者函数中调用。

这边是因为use state在每次渲染的时候必须按照相同的顺序调用，这边可以添加 [`eslint-plugin-react-hooks`](https://www.npmjs.com/package/eslint-plugin-react-hooks) 插件来强执行这个问题

#### 2、只能在React的函数组件中调用Hook。不要在其他JavaScript函数中调用（除了自定义Hook）



## Hook钩子详解

### 1、useState

这就是其中一个钩子

useState会返回一对值：当前的状态和一个让你更新它的函数

其唯一的参数就是state的初始值

```
import React, {useState} from 'react';

function UseState() {
    const [count, setCount] = useState(0); // 其唯一的参数就是state的初始值
    return (
        <div>
            <h2>当前count为：{count}</h2>
            <input type= "button" value="点击Count++" onClick={() => setCount(count + 1)}/>
        </div>
    )
}

export default UseState
```





### 2、Effect  Hook

在React组件中执行数据获取、订阅、或者手动修改DM。我们把这些统称为“副作用”或者简称为“作用”

useEffect就是一个Effect Hook。给函数组件增加了操作副作用的能力。它跟class组件中的componentDidMount、componentDidUpdate、componentWillUmount具有相同的作用，只不过被合并成了api

##### 需要清除的effect

（对于设置的一些订阅事件，需要在后面进行清除）如果是class组件，会在componentDidMount中订阅，在componentWillUmount中清除。

对于useEffect的清除操作是放在一起的（在同一地方执行），如果你的effect返回一个函数（这边可以return一个函数，用来清除effect，这边呢，react会在组件卸载的时候执行清除操作），react将会在执行清除操作时调用它

这边的useEffect可以使用多次，这样方便把逻辑相关的放在一起，便于代码的可读性（Hook允许我们按照代码的用途分离他们）

##### 通过跳过effect进行性能优化

因为每次渲染的时候都会去执行effect，这样可能导致性能问题，在class组件中呢，我们可以通过在componentDidUpdate中添加prevProps或prevState的比较逻辑解决。

那在这边呢，就会给useEffect添加第二个参数，表示仅在这个参数改变的情况下才会去执行这个effect

例：

```
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // 仅在 count 更改时更新
```

注意：这边对于清除effect同样有效

这边如果当前effect，你只想执行一次，可以设置第二个参数为 []，这样就表示不依赖外部的数据，那这边就只会渲染一次



### 3、自定义Hook

自定义Hook是一个函数，其名称以‘use’开头，函数内部可以调用其他的Hook，这边在我看来就是一个可以使用非自定义Hook的普通函数，主要用来提取公共代码，避免代码冗余





### 4、useContext

```
const value = useContext(MyContext)
```

接收一个context对象并返回该context的当前值



### 5、额外的Hook

看官网

