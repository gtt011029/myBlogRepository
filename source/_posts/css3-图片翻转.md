---
title: css3-图片翻转
abbrlink: 61926
date: 2017-04-18 19:43:09
tags:
---









![](/css3动画/图片翻转.gif)



<!--more--> 









```html
<!DOCTYPE html>
<html lang="en">
 <head> 
  <meta charset="UTF-8" /> 
  <title>Document</title> 
  <style>		
  	*{
  		margin: 0;
  		padding: 0;		
  	}		
  	div{			
  		position: relative;			
  		width: 225px;			
		height: 225px;
		margin: 20px auto;
	}		
	img{			
		position: absolute;			
		top: 0;			
		left 0;			
		transition: transform 3s;		
	}		
	img:first-child{			
		z-index: 99;			
		backface-visibility: hidden;		
	}		
	div:hover img{			
		transform: rotateY(180deg);		
	}	
</style>
</head>
<body> 
  	<div> 
   		<img src="images/qian.svg" alt="" /> 
   		<img src="images/hou.svg" alt="" /> 
  	</div> 
</body>
</html>
```

