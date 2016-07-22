---
title: 关于JavaScript中this的使用的一点知识
author: Nick
layout:  post
category: tech
---
{% include JB/setup %}

今天无意中见到的一道前端招聘面试题:

请说明要输出正确的myName的值要如何修改程序?并解释原因:

		foo = function(){
			this.myName = "Foo function.";
		}
		foo.prototype.sayHello = function(){
			alert(this.myName);
		}
		foo.prototype.bar = function(){
			setTimeout(this.sayHello, 1000);
		}
		var f = new foo;
		f.bar();

问题的关键就在这一句` this.myName = "Foo function";`我们都知道this是函数体对当前对象的引用,但是在对this的使用上常常会出现误区.我们都知道在函数调用中会有不同的调用方式:

*	函数调用:如` var a = func(arg);`  
*	方法调用:如 `var a = o.func(arg);`

在《JavaScript权威指南》第六版中有这么一句话:根据ECMAScript3和非严格的ECMAScript5对函数调用的规定,调用上下文(this的值)是全局对象.然而,在严格模式下,调用上下文则是undefined.

而上述代码无论是在哪一种模式的情况都会得到undefined的结果,而将 `this.myName ="Foo function";` 中的this去掉后,浏览器正常的显示出Foo function.

同样是《JavaScript权威指南》第六版中的一句话:而当嵌套函数作为方法调用时,其this的值指向调用它的对象.如果嵌套函数作为函数调用,其this值不是全局变量(非严格模式下)就是undefined(严格模式下).  
上述代码中出现了:  
` f.bar(); `  
这一句应该就属于嵌套函数作为方法调用的情况(bar()是f中的一个方法),而`setTimeout(this.sayHello, 1000)`中sayHello函数却是以函数调用的方式作为嵌套函数（嵌套于bar中）被调用, alert(this.myName)中的this就成了全局对象或者undefined
而如果 `this.myName = "Foo function."`被改成 `myName = "Foo function."，` myName变成了全局属性,所以也就可以显示出正确的Foo function.

但是，其实还有一种方式可以解决这个问题。

请说明要输出正确的myName的值要如何修改程序?并解释原因

	foo = function(){
		this.myName = "Foo function.";
	}
	foo.prototype.sayHello = function(){
		alert(this.myName);
	}
	foo.prototype.bar = function(){
		var self = this;   //将bar中的this的值存在变量self中，这样self所指向的对象就跟bar中的this一样了。
		setTimeout(self.sayHello, 1000);
	}
	var f = new foo;
	f.bar();
