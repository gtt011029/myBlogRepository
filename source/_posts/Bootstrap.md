---
title: Bootstrap
abbrlink: 11629
date: 2018-10-23 20:08:19
tags:
---





## 介绍

Bootstrap 是最受欢迎的 HTML、CSS 和 JS 框架，用于开发响应式布局、移动设备优先的 WEB 项目。

Bootstrap的特点：

```
1.预处理脚本,less2.响应式布局（根据不同设备屏幕大小进行响应式布局）3.移动设备优先，Bootstrap 3设计目标是移动设备优先，然后才是桌面设备4.定义了大量的class样式，提高开发效率5.Bootstrap的所有JavaScript插件都依赖于jQuery，因此jQuery必须在Bootstrap之前引入
```

## bootstrap的下载

<!--more-->

 1) 生产环境 ： css /js /fonts 三个文件夹，正常开发时候使用

```
2)  bootstrap源码 : 包含bootstrap源码和less文件，可以做一些页面效果的定制。
```

 less —–>翻译成css

```
bootstrap/├── css/│   ├── bootstrap.css│   ├── bootstrap.css.map│   ├── bootstrap.min.css│   ├── bootstrap.min.css.map│   ├── bootstrap-theme.css      
//开启立体效果，可在组件中最后的主题预览中查看│   ├── bootstrap-theme.css.map     │   ├── bootstrap-theme.min.css    │   └── bootstrap-theme.min.css.map├── js/│   ├── bootstrap.js│   └── bootstrap.min.js└── fonts/    ├── glyphicons-halflings-regular.eot    ├── glyphicons-halflings-regular.svg    ├── glyphicons-halflings-regular.ttf    ├── glyphicons-halflings-regular.woff    └── glyphicons-halflings-regular.woff2
```

## bootstrap的浏览器兼容情况

### IE8和IE9下面属性不支持

![](/Bootstrap/bootstrap-1537450443274.png)

### IE8下媒体查询不支持

```
/*要让IE8支持媒体查询，需要手动引入respond.js文件*/
<!-- HTML5 shim 和 Respond.js 是为了让 IE8 支持 HTML5 元素和媒体查询（media queries）功能 -->
<!-- 警告：通过 file:// 协议（就是直接将 html 页面拖拽到浏览器中）访问页面时 Respond.js 不起作用 
-->
<!--[if lt IE 9]>	  
<!--让浏览器能够识别html5新标签-->      
<script src="https://cdn.bootcss.com/html5shiv/3.7.3/html5shiv.min.js"></script>	  <!--让浏览器支持媒体查询-->      
<script src="https://cdn.bootcss.com/respond.js/1.4.2/respond.min.js"></script>    
<![endif]-->
/*注意事项*/
1.css样式表内容.要放在css文件中,
2.css文件必须是无bom头格式的编码文件
3.css文件地址必须放在head标签内引用.放在html中的body中引用无效.
4.css文件必须放在respond.js之前引用,respond.js可以放在body或网页底部,但为了防止闪屏,建议放在head中
```

### IE兼容模式

```
/*设置IE的渲染模式7，这个仅做测试，一般都用最新模式edge*/
<meta http-equiv="X-UA-Compatible" content="IE=7">
/*不管用户用的是IE7 IE8  IE9，我们总希望IE 浏览器运行最新的渲染模式下，可以添加如下标签：*/
<meta http-equiv="X-UA-Compatible" content="IE=edge">
/*IE下查看渲染模式：右键--->审查元素---->文档模式 7 8 9 10*/
```

## bootstrap基本模板

```html
<!DOCTYPE html>
<html lang="zh-CN">
 <head> 
  <meta charset="utf-8" /> 
  <meta http-equiv="X-UA-Compatible" content="IE=edge" /> 
  <meta name="viewport" content="width=device-width, initial-scale=1" /> 
  <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ --> 
  <title>Bootstrap 101 Template</title> 
  <!-- Bootstrap --> 
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" rel="stylesheet" /> 
  <!-- HTML5 shim 和 Respond.js 是为了让 IE8 支持 HTML5 元素和媒体查询（media queries）功能 --> 
  <!-- 警告：通过 file:// 协议（就是直接将 html 页面拖拽到浏览器中）访问页面时 Respond.js 不起作用 --> 
  <!--[if lt IE 9]>      <script src="https://cdn.jsdelivr.net/npm/html5shiv@3.7.3/dist/html5shiv.min.js"></script>      <script src="https://cdn.jsdelivr.net/npm/respond.js@1.4.2/dest/respond.min.js"></script>    <![endif]--> 
 </head> 
 <body> 
  <h1>你好，世界！</h1> 
  <!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) --> 
  <script src="https://cdn.jsdelivr.net/npm/jquery@1.12.4/dist/jquery.min.js"></script> 
  <!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 --> 
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/js/bootstrap.min.js"></script> 
 </body>
</html>
```

## Normalize.css

```
##1 排版和标签中的     
@font-family-base、@font-size-base 是less的语法，先不用管reset.css		
重置样式库，增强元素在不同浏览器中的一致性##
2 normalize.css和reset.css区别：
	1.normalize.css和reset.css     都是重置样式库，增强元素在不同浏览器中的一致性
	2.区别在normalize.css中，已经样式一致性的元素不会再去重置样式，但reset.css不一定
	3.举个例子：ul	reset.css   list-style:none ---因为需求	normalize.css 不会重置ul样式 ---本身已经在每个浏览器表现一致的元素
```

## 布局容器

```html
## container是一个响应式的布局容器  
<!--响应式布局容器，宽度会随着浏览器的窗口发生改变-->
<div class="container"></div>
<!--流式布局容器，宽度为屏幕宽度-->
<div class="container-fluid"> </div>
```

## 栅格系统

### 基本使用

```html
<!--响应式布局容器-->
<html>
 <head></head>
 <body>
  <div class="container"> 
   <!--栅格系统：其实就是行和列的布局，网格状布局--> 
   <!--行：row--> 
   <!-- .container容器默认有15px的左右内间距  .row 填充父容器的15px的左右内间距   margin-left,margin-right -15px拉伸 --> 
   <div class="row"> 
    <!--列：col-*-*  *不确定（参数） --> 
    <!--        col：列样式        大屏设备     lg > 1200   大屏设备以上生效包含本身        中屏设备     md > 992   中屏设备以上生效包含本身        小屏设备     sm > 768   小屏设备以上生效包含本身        超小屏设备   xs < 768  超小屏设备以上生效包含本身        --> 
    <div class="col-xs-4"></div> 
    <div class="col-xs-4"></div> 
    <div class="col-xs-4"></div> 
    <!--上面代码的含义：在超小屏及以上每行都显示三个div--> 
   </div> 
   <div class="row"> 
    <div class="col-md-4"></div> 
    <div class="col-md-4"></div> 
    <div class="col-md-4"></div> 
    <!--上面代码的含义：在中等屏幕及以上每行都显示三个div--> 
   </div>
  </div>
  <style>    
  .container{        
  		height: 100px;        
  		background: pink;    
  	}    
  	.container > .row{        
  		background: green;    
  	}    
  	.container > .row > div{        
  		height: 25px;        
  		border: 1px solid #ccc;    
  	}
 </style>
 </body>
</html>
```

### 响应式布局

```html
<!--大屏设备   让div平均分成6等份中屏设备   让div平均分成4等份小屏设备   让div平均分成3等份超小屏设备 让div平均分成2等份-->
<div class="container">    
	<div class="row">        
		<div class="col-lg-2 col-md-3 col-sm-4 col-xs-6"></div>        
		<div class="col-lg-2 col-md-3 col-sm-4 col-xs-6"></div>        
		<div class="col-lg-2 col-md-3 col-sm-4 col-xs-6"></div>        
		<div class="col-lg-2 col-md-3 col-sm-4 col-xs-6"></div>        
		<div class="col-lg-2 col-md-3 col-sm-4 col-xs-6"></div>        
		<div class="col-lg-2 col-md-3 col-sm-4 col-xs-6"></div>    
	</div>
</div>
<style>    
.container {
	height: 100px;
	background: pink;
}

.container > .row {
	height: 50px;
	background: green;
}

.container > .row > div {
	height: 25px;
	border: 1px solid #ccc;
}
</style>
```

### 栅格系统扩展

```html
<html>
 <head></head>
 <body>
  <div class="container"> 
   <div class="row"> 
    <!--栅格嵌套--> 
    <div class="col-xs-4"> 
     <div class="row"> 
      <div class="col-xs-6"></div> 
      <div class="col-xs-6"></div> 
     </div> 
    </div> 
    <!--列的偏移--> 
    <div class="col-xs-4"> 
     <div class="row"> 
      <div class="col-xs-3"></div> 
      <div class="col-xs-6 col-xs-offset-1"></div> 
     </div> 
    </div> 
    <!--列的排序--> 
    <div class="col-xs-4"> 
     <div class="row"> 
      <!--                push 往后推                pull 往前拉                --> 
      <div class="col-xs-3 col-xs-push-9">
       col-xs-3
      </div> 
      <div class="col-xs-9 col-xs-pull-3">
       col-xs-9
      </div> 
     </div> 
    </div> 
   </div>
  </div>
  <style>    
 .container {
	height: 100px;
	background: pink;
}

.container > .row {
	height: 50px;
	background: green;
}

.container > .row > div {
	height: 25px;
	border: 1px solid #ccc;
}

.container > .row > div > .row > div {
	height: 10px;
	border: 1px solid red;
}
</style>
</body>
</html>
```

### 影藏显示

```
表格表单按钮图片等复制就可以了。
<!--大屏设备    显示中屏设备    隐藏小屏设备    显示超小屏设备  隐藏visible-lg     大屏显示其他隐藏visible-mdvisible-smvisible-xs3.2版本以后  建议使用hiddenhidden-lghidden-mdhidden-smhidden-xs-->
<!--<div class="box visible-lg visible-sm">内容</div>--><div class="box hidden-md hidden-xs">内容</div>
```

### 导航条

```HTML
<html>
 <head></head>
 <body>
  ##1.折叠组件：
  <!--     属性：        data-toggle="collapse"  申明是折叠组件        data-target="#bs-example-navbar-collapse-1" 控制的目标元素=选择器-->
  <button data-toggle="collapse" data-target=".box">切换</button>
  <a href=".box" data-toggle="collapse">切换</a>
  <div class="box">
    内容
   <br /> 内容
   <br /> 内容
   <br /> 内容
   <br /> 内容
   <br />
  </div>
 </body>
</html>
```

### 轮播图

#### 基本使用

```html
<!-- carousel 轮播图的模块       slide是否加上滑动效果 -->
<!-- data-ride="carousel" 初始化轮播图属性-->
<html>
 <head></head>
 <body>
  <div id="carousel-example-generic" class="carousel slide" data-ride="carousel"> 
   <!-- 点盒子 --> 
   <ol class="carousel-indicators"> 
    <!--            data-target="#carousel-example-generic" 控制目标轮播图            data-slide-to="0" 控制的是轮播图当中的第几张 （索引）            class="active" 当前选中的点        --> 
    <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li> 
    <li data-target="#carousel-example-generic" data-slide-to="1"></li> 
    <li data-target="#carousel-example-generic" data-slide-to="2"></li> 
   </ol> 
   <!-- 图片盒子 --> 
   <!-- role="listbox" 提供给屏幕阅读器使用 --> 
   <div class="carousel-inner"> 
    <!--需要轮播的容器--> 
    <div class="item active"> 
     <!--图片--> 
     <img src="..." alt="..." /> 
     <!--说明--> 
     <div class="carousel-caption">
       ... 
     </div> 
    </div> 
    <div class="item"> 
     <img src="..." alt="..." /> 
     <div class="carousel-caption">
       ... 
     </div> 
    </div> ... 
   </div> 
   <!-- 上一张下一张按钮 --> 
   <!--     data-slide="prev"     data-slide="next"     href="#carousel-example-generic"   控制目标轮播图    --> 
   <a class="left carousel-control" href="#carousel-example-generic" data-slide="prev"> <span class="glyphicon glyphicon-chevron-left"></span> </a> 
   <a class="right carousel-control" href="#carousel-example-generic" data-slide="next"> <span class="glyphicon glyphicon-chevron-right"></span> </a>
  </div>
 </body>
</html>
```

#### PC端轮播图

```html
<!--1.需求：PC端在浏览器(小屏幕以上)缩放的时候图片高度不变并且始终显示中间一块图片，图片铺满容器。要满足这个需求我们不能直接给img设置src属性，我们可以使用背景图片来解决这个问题2.需求：在超小屏幕设备上缩放的时候图片能跟随屏幕的放大缩小等比例放大缩小-->
<html>
 <head></head>
 <body>
  <div id="carousel-example-generic" class="carousel slide" data-ride="carousel"> 
   <ol class="carousel-indicators"> 
    <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li> 
    <li data-target="#carousel-example-generic" data-slide-to="1"></li> 
    <li data-target="#carousel-example-generic" data-slide-to="2"></li> 
    <li data-target="#carousel-example-generic" data-slide-to="3"></li> 
   </ol> 
   <div class="carousel-inner"> 
    <div class="item active"> 
     <a href="#" class="pc_imgBox" style="background-image: url(../images/slide_01_2000x410.jpg)"></a> 
    </div> 
    <div class="item"> 
     <a href="#" class="pc_imgBox" style="background-image: url(../images/slide_02_2000x410.jpg)"></a> 
    </div> 
    <div class="item"> 
     <a href="#" class="pc_imgBox" style="background-image: url(../images/slide_03_2000x410.jpg)"></a> 
    </div> 
    <div class="item"> 
     <a href="#" class="pc_imgBox" style="background-image: url(../images/slide_04_2000x410.jpg)"></a> 
    </div> 
   </div> 
   <a class="left carousel-control" href="#carousel-example-generic" data-slide="prev"> <span class="glyphicon glyphicon-chevron-left"></span> </a> 
   <a class="right carousel-control" href="#carousel-example-generic" data-slide="next"> <span class="glyphicon glyphicon-chevron-right"></span> </a>
  </div>
  <style>    
  .pc_imgBox {
		display: block;
		height: 400px;
		width: 100%;
		background-size: cover;
		background-position: center;
		background-repeat: no-repeat;
	}
</style>
</body>
</html>
```

#### 手机端轮播图

```HTML
<!--需求：宽度自适应，高度自动变化实现思路：1.设置图片的src属性，指定宽度为父容器的100%2.因为是考虑移动端浏览器的自适应，所以在设置图片的时候应该选用小图-->
<html>
 <head></head>
 <body>
  <div id="carousel-example-generic" class="carousel slide" data-ride="carousel"> 
   <ol class="carousel-indicators"> 
    <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li> 
    <li data-target="#carousel-example-generic" data-slide-to="1"></li> 
    <li data-target="#carousel-example-generic" data-slide-to="2"></li> 
    <li data-target="#carousel-example-generic" data-slide-to="3"></li> 
   </ol> 
   <div class="carousel-inner"> 
    <div class="item active"> 
     <a href="#" class="m_imgBox"><img src="../images/slide_01_640x340.jpg" alt="" /></a> 
    </div> 
    <div class="item"> 
     <a href="#" class="m_imgBox"><img src="../images/slide_02_640x340.jpg" alt="" /></a> 
    </div> 
    <div class="item"> 
     <a href="#" class="m_imgBox"><img src="../images/slide_03_640x340.jpg" alt="" /></a> 
    </div> 
    <div class="item"> 
     <a href="#" class="m_imgBox"><img src="../images/slide_04_640x340.jpg" alt="" /></a> 
    </div> 
   </div> 
   <a class="left carousel-control" href="#carousel-example-generic" data-slide="prev"> <span class="glyphicon glyphicon-chevron-left"></span> </a> 
   <a class="right carousel-control" href="#carousel-example-generic" data-slide="next"> <span class="glyphicon glyphicon-chevron-right"></span> </a>
  </div>
  <style>
  	.m_imgBox {
		display: block;
		width: 100%;
	}

	.m_imgBox img {
		display: block;
		width: 100%;
	}
  </style>
 </body>
</html>
```

#### 响应轮播图

```html
<!--实现思路：1.设置两套轮播图2.背景图片的图片在超小屏幕上隐藏3.指定src的图片在小屏幕上显示，其余屏幕隐藏-->
<html>
 <head></head>
 <body>
  <div id="carousel-example-generic" class="carousel slide" data-ride="carousel"> 
   <ol class="carousel-indicators"> 
    <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li> 
    <li data-target="#carousel-example-generic" data-slide-to="1"></li> 
    <li data-target="#carousel-example-generic" data-slide-to="2"></li> 
    <li data-target="#carousel-example-generic" data-slide-to="3"></li> 
   </ol> 
   <div class="carousel-inner"> 
    <div class="item active"> 
     <a href="#" class="pc_imgBox hidden-xs" style="background-image: url(../images/slide_01_2000x410.jpg)"></a> 
     <a href="#" class="m_imgBox hidden-lg hidden-md hidden-sm"><img src="../images/slide_01_640x340.jpg" alt="" /></a> 
    </div> 
    <div class="item"> 
     <a href="#" class="pc_imgBox hidden-xs" style="background-image: url(../images/slide_02_2000x410.jpg)"></a> 
     <a href="#" class="m_imgBox hidden-lg hidden-md hidden-sm"><img src="../images/slide_02_640x340.jpg" alt="" /></a> 
    </div> 
    <div class="item"> 
     <a href="#" class="pc_imgBox hidden-xs" style="background-image: url(../images/slide_03_2000x410.jpg)"></a> 
     <a href="#" class="m_imgBox hidden-lg hidden-md hidden-sm"><img src="../images/slide_03_640x340.jpg" alt="" /></a> 
    </div> 
    <div class="item"> 
     <a href="#" class="pc_imgBox hidden-xs" style="background-image: url(../images/slide_04_2000x410.jpg)"></a> 
     <a href="#" class="m_imgBox hidden-lg hidden-md hidden-sm"><img src="../images/slide_04_640x340.jpg" alt="" /></a> 
    </div> 
   </div> 
   <a class="left carousel-control" href="#carousel-example-generic" data-slide="prev"> <span class="glyphicon glyphicon-chevron-left"></span> </a> 
   <a class="right carousel-control" href="#carousel-example-generic" data-slide="next"> <span class="glyphicon glyphicon-chevron-right"></span> </a>
  </div>
  <style>    
  .pc_imgBox {
	display: block;
	height: 400px;
	width: 100%;
	background-size: cover;
	background-position: center;
	background-repeat: no-repeat;
}

.m_imgBox {
	display: block;
	width: 100%;
}

.m_imgBox img {
	display: block;
	width: 100%;
}
</style>
  <!--缺点：1.准备了两套图片，不管这个图片有没有被使用，都会被加载，影响性能2.无法实现页面图片的动态生成(根据服务器的数据切换页面的轮播图)解决方案：可以通过js动态加载-->
 </body>
</html>
```

### 标签页

```html
<html>
 <head></head>
 <body>
  <div> 
   <!-- 页签 --> 
   <!-- Nav tabs --> 
   <ul class="nav nav-tabs"> 
    <li class="active"><a href="#home" data-toggle="tab">Home</a></li> 
    <li role="presentation"><a href="#profile" aria-controls="profile" role="tab" data-toggle="tab">Profile</a></li> 
    <li role="presentation"><a href="#messages" aria-controls="messages" role="tab" data-toggle="tab">Messages</a></li> 
    <li role="presentation"><a href="#settings" aria-controls="settings" role="tab" data-toggle="tab">Settings</a></li> 
   </ul> 
   <!--内容--> 
   <!-- Tab panes --> 
   <div class="tab-content"> 
    <div class="tab-pane active" id="home">
     1
    </div> 
    <div role="tabpanel" class="tab-pane" id="profile">
     2
    </div> 
    <div role="tabpanel" class="tab-pane" id="messages">
     3
    </div> 
    <div role="tabpanel" class="tab-pane" id="settings">
     4
    </div> 
   </div>
  </div>
 </body>
</html>
```

