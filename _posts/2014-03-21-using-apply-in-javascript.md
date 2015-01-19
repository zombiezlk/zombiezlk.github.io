---
title: 关于javascript中的apply()使用中的疑问
author: Nick
layout: post
category: tech
---
{% include JB/setup %}

今天看到一段有关于apply()函数的代码,但是对里面apply()函数的用法还是有点疑惑.  
代码如下:

		//将对象o中名为m()的方法替换为另一个方法
		//可以在调用原始的方法之前和之后记录日志消息
		function trace(o,m){
       		var original = o[m];                //在闭包中保存原始方法
       		o[m] = function(){                          //定义新的方法
       		console.log(new Date(),"ENTERING",m);        //输出日志信息
      		 	var result = original.apply(this,arguments); //调用原始函数
       		console.log(new Date(),"EXITING",m);        //输出日志信息
       		return result;                              //返回结果
       		};
		}

在这里trace()函数接收两个参数,一个对象和一个方法名,它将指定的方法替换为一个新方法,这个新方法是"包裹";原始方法的另一个泛函数.这种动态修改已有方法的做法有时称作"monkey-patching".

对于我来说,比较费解的是 `var result = original.apply(this,arguments);`这一句,其中的this和arguments分别指的是哪个对象和哪个函数的参数对象.光说没用,还是自己写下代码来测试一下.  
接下来就打开chrome的console直接进行调试(测试过程中还要注意一点,写完一次代码最好进行清屏,否则如果你下次代码的变量会跟上一次的名字是有些是一样的话,会造成错误的结果).  
首先来测试下this到底指的是original呢还是全局对象,编写代码如下:

		var original = function(a,b){
        		var x=1 ,y=1;
        		return a+b;
		}
		var result = original.apply(this,[this.x,this.y]);
		console.log(result);

结果控制台的输出是NaN.  

接下来再编写如下代码:

		var original = function(a,b){
        		return a+b;
		}
		var x=1 ,y=1;
		var result = original.apply(this,[this.x,this.y]);
		console.log(result);
		
结果同样为2.  
结论:this的确指的是全局变量.

针对arguments编写如下代码:

		function trace(x,y){
        		var result = original.apply(this,arguments);
        		console.log(result);
		}
		trace(1,1);

结果同样为2.说明arguments指的是函数trace()的参数列表对象.