---
title: css3动画-3D
abbrlink: 29476
date: 2017-04-18 12:15:42
tags:
---







![](/css3动画/css3.gif)



<!--more--> 

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		*{
			margin: 0;
			padding: 0;
		}
		section{
			position: relative;
			background: url(images.jpg) 100% 100%;
			width: 300px;
			height: 300px;
			margin:100px auto;
			transform-style: preserve-3d;
			transform: rotate3d(1,1,0,45deg);
			transition: transform linear 20s;
		}
		div{
			position: absolute;
			width: 100%;
			height: 100%;
			opacity: 0.6;
			top: 0;
			left: 0;
			
		}
		div:first-child{
			background: yellow;
			transform: translateZ(150px);
		}
		div:nth-child(2){
			background: red;
			transform: translateZ(-150px) rotateY(180deg);

		}
		div:nth-child(3){
			background: red;
			transform: translateX(-150px) rotateY(-90deg);
		}
		div:nth-child(4){
			background: green;
			transform: translateX(150px) rotateY(90deg);
		}
		div:nth-child(5){
			background: pink;
			transform: translateY(150px) rotateX(-90deg);
		}
		div:nth-child(6){
			transform: translateY(-150px) rotateX(90deg);
			background: gray;
		}
		section:hover{
			transform: rotate3d(1,1,0,-720deg);
			
		}
	</style>
</head>
<body>
	<section>
		<div>font</div>
		<div>back</div>
		<div>left</div>
		<div>right</div>
		<div>top</div>
		<div>bottom</div>
	</section>
	
</body>
</html>
```

