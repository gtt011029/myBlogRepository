---
title: 杂乱排序transform
abbrlink: 10966
date: 2017-04-18 15:09:00
tags:
---



![](/css3动画/杂乱排序.gif)

<!--more--> 

代码：

```
<!DOCTYPE html>
<html lang="en">
<head> 
  	<meta charset="UTF-8" /> 
  	<title>Document</title> 
  	<style type="text/css">
  		*{  			
  			margin: 0;  			
  			padding: 0;  		
  		}

  		body{			
  			background-color: #31965b;		
  		}		
  		.box{			
  			width: 640px;			
  			margin: 100px auto;		
  		}		
  		.box>img{			
  			transition: transform 1s;		
  		}		
  		.box>img:nth-of-type(1){			
  			transform: translate(100px,100px) rotate(30deg);		
  		}		
  		.box>img:nth-of-type(2){			
  			transform: translate(100px,100px) rotate(-30deg);		
  		}		
  		.box>img:nth-of-type(3){			
  			transform: translate(200px,200px) rotate(60deg);		
  		}		
  		.box>img:nth-of-type(4){			
  			transform: translate(200px,200px) rotate(-60deg);		
  		}		
  		.box>img:nth-of-type(5){			
  			transform: translate(150px,150px) rotate(90deg);		
  		}		
  		.box>img:nth-of-type(6){			
  			transform: translate(50px,150px) rotate(-90deg);		
  		}		
  		.box>img:nth-of-type(7){	        
  			transform: translate(-150px,-150px) rotate(60deg);	    
  		}	    
  		.box>img:nth-of-type(8){	        
  			transform: translate(10px,-250px) rotate(-90deg);	    
  		}	    
  		.box>img:nth-of-type(9){	        
  			transform: translate(-250px,10px) rotate(45deg);	    
  		}	    
  		.box:hover>img{	    	
  		transform: none;	    
  	}	
 	</style>
</head>
<body> 

  	<div class="box"> 
   	<img src="1.png" alt="" /> 
   	<img src="2.png" alt="" /> 
   	<img src="3.png" alt="" /> 
   	<img src="4.png" alt="" /> 
   	<img src="5.png" alt="" /> 
   	<img src="6.png" alt="" /> 
   	<img src="7.png" alt="" /> 
   	<img src="8.png" alt="" /> 
   	<img src="9.png" alt="" />
  	</div> 

</body>
</html>
```

