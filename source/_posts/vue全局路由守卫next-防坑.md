---
title: vue全局路由守卫next()--防坑
abbrlink: 1089
date: 2019-09-15 14:36:41
tags:
---





最近入了vue全局守卫的一个坑，现在记录下来



### 全局守卫：

全局守卫是指实例上直接操作的钩子函数，特点是所有路由配置的组件都会触发

```js
beforeEach (to, from, nest) 
beforeResolve (to, from, next)
afetrEach (to, from)
```

<!--more-->

### 回调参数介绍：

to：即将要进入的目标路由对象

from：即将要离开的路由对象

next：涉及到next参数的钩子函数，必须调用next（）方法来resolve这个钩子，否则路由会中断在这。不会继续往下执行

next()：进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是confirmed(确认的)。

next( false )中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到from路由对应的地址。

next( ' / ')或者next({ paht：' / '  })：跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。可传递的参数可以是router-link标签中的to属性参数或router.push中的选项

next( error )：如果传入next的参数是一个Error实例，则导航会被终止且该错误会被传递给router.onError()注册过的回调。



在写全局守卫时，不注意的话会遇到死循环，举例：

```
if (to.path === '/user/login') {
	next({ path: '/user/login' })
}   // 这边想要进入login页面就会进行next（{ path: '/user/login' }），next里面有参数就会认为是一种新的跳转，就会再次执行beforeEach（）导致死循环
```



### 总结：

​		当执行钩子函数时 如果遇到next("/xxxx")时，会将原本的导航中断，然后将to.path改成next中的地址，然后重新触发这个离开的钩子。注意：会重新触发执行这个钩子，而不是在这个钩子函数中继续执行。以前只认为next("/xxx")就直接去跳转了。所以当重新触发后就会继续执行next('/xxxx')所以会一直循环。至于解决办法就是判断下，如果已经是/xxxx了就next()。 