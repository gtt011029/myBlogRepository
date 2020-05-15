---
title: ajax之jsonp跨域
abbrlink: 11449
date: 2018-05-04 18:04:34
tags:
---







### 相关概念

```
同源策略是浏览器的一种安全策略，所谓同源是指  **域名，协议，端口完全相同** ，只有同源的地址才可以相互通过AJAX 的方式请求。
同源或者不同源说的是两个地址之间的关系，不同源地址之间请求我们称之为跨域请求
```

 什么是同源？例如：http://www.example.com/detail.html 与一下地址对比

<!--more--> 

```
http://api.example.com/detail.html 不同源 域名不同https://www.example.com/detail.html 不同源 协议不同http://www.example.com:8080/detail.html 不同源 端口不同http://api.example.com:8080/detail.html 不同源 域名、端口不同https://api.example.com/detail.html 不同源 协议、域名不同https://www.example.com:8080/detail.html 不同源 端口、协议不同http://www.example.com/other.html 同源 只是目录不同
<script>    
	// 当前页面访问地址：http://day-12.io/11-cross-domain.html    
	// 希望被AJAX的地址：http://locally.uieee.com/categories    
	// 这两个地址之间 协议相同 端口相同 域名不同 所以是两个不同源的地址    
	// 同源策略指的就是：不同源地址之间，默认不能相互发送AJAX请求    
	// 不同源地址之间如果需要相互请求，必须服务端和客户端配合才能完成    							$.get('https://locally.uieee.com/categories', function (res) {
		console.log(res)
	})
//No 'Access-Control-Allow-Origin' header is present on the requested resource
</script>
```

### 解决方案

现代化的 Web 应用中肯定会有不同源的现象，所以必然要解决这个问题，从而实现跨域请求。

#### 发送跨域请求的方式

```
##1. img
// 可以发送不同源地址之间的请求
// 无法拿到响应结果
var img = new Image()
img.src = 'http://locally.uieee.com/categories'
## 2. link
// 可以发送不同源地址之间的请求
// 无法拿到响应结果
var link = document.createElement('link')
link.rel = 'stylesheet'
link.href = 'http://locally.uieee.com/categories'document.body.appendChild(link)
## 3. script
// 可以发送不同源地址之间的请求
// 能够借助js的回调间接拿到结果
var script = document.createElement('script')
script.src = 'http://localhost/time2.php'document.body.appendChild(script) 
// 开始发起请求
// 相当于请求的回调
function foo (res) {    
	console.log(res)
}
	//time2.php<?phpheader('Content-Type: application/javascript');
	$json = json_encode(array(  'time' => time()));
	// 在 JSON 格式的字符串外面包裹了一个函数的调用，
	// 返回的结果就变成了一段 JS 代码echo "foo({$json})";
```

### JSONP

```
JSON with Padding，是一种借助于 script 标签发送跨域请求的技巧。
其原理就是在客户端借助 script 标签请求服务端的一个动态网页（php 文件），服务端的这个动态网页返回一段带有函数调用的 JavaScript 全局函数调用的脚本，
```

将原本需要返回给客户端的数据传递进去。
以后绝大多数情况都是采用 JSONP 的手段完成不同源地址之间的跨域请求

```
//客户端 
http://www.zce.me/users-list.html
<script src="http://api.zce.me/users.php"></script><script>	
	function foo(msg){ console.log(msg) }	
</script>    
## script标签中的async表示异步的意思，但是在使用JSONP的时候不能用异步的方式，因为异步之后就不能保证下面的script标签能够拿到上面script请求php的数据了(这两个script标签有顺序要求)
//服务端
http://api.zce.me/users.php 返回的结果
header('Content-Type: application/javascript');
echo "foo(['我', '是', '你', '原', '本', '需', '要', '的', '数', '据'])";
```

总结一下：由于 XMLHttpRequest 无法发送不同源地址之间的跨域请求，所以我们必须要另寻他法，script 这种方案就是我们最终选择的方式，我们把这种方式称之为 JSONP。

```
//JSONP存在的问题：1.JSONP需要服务端配合，服务端按照客户端的要求返回一段JavaScript调用客户端的函数2.只能发送GET请求注意：JSONP用的是script标签，和AJAX提供的XMLHttpRequest没有任何关系！！！
```

#### 为每次请求创建一个函数

```
##1.客户端
var funcName = 'haha_' + Date.now() + Math.random().toString().substr(2, 5)
var script = document.createElement('script')script.src = 'http://api.zce.me/users.php?callback=' + funcNamedocument.body.appendChild(script)window[funcName] = function (data) {     console.log('1111', data)}
##2.服务端
<?php$data = "['我', '是', '你', '原', '本', '需', '要', '的', '数', '据']";
if (empty($_GET['callback'])) {  
	header('Content-Type: application/json');  
	echo $data;  
	exit();
}
// 如果客户端采用的是 script 标记对我发送的请求
// 一定要返回一段 
JavaScriptheader('Content-Type: application/javascript');
$callback_name = $_GET['callback'];
echo "typeof {$callback_name} === 'function' && {$callback_name}({$data})";
```

#### JSONP的封装

```
function jsonp (url, params, callback) {   
//随机生成一个方法    
	var funcName = 'jsonp_' + Date.now() + Math.random().toString().substr(2, 5)    
	if (typeof params === 'object') {        
		var tempArr = []        
		for (var key in params) {            
			var value = params[key]            
			tempArr.push(key + '=' + value)        
		}        
		params = tempArr.join('&')    
	}    
	var script = document.createElement('script')    
	script.src = url + '?' + params + '&callback=' + funcName    		    			document.body.appendChild(script)    
	window[funcName] = function (data) {        
		callback(data)        
		delete window[funcName]        
		document.body.removeChild(script)    
	}
}
jsonp('http://api.zce.me/users.php', { id: 123 }, function (res) {    					console.log(res)
})
jsonp('http://api.zce.me/users.php', { id: 123 }, function (res) {    					console.log(res)
})
```







