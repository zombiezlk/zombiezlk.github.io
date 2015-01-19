---
title: JAVA中一些容易忽视的基础小知识
author: Nick
layout: post
category: tech
---
{% include JB/setup %}

1.	for循环语句中的判断语句不仅仅可以这样写`for(int i=0;i<3;i++)`,也可以这样写` for	(System.out.println("a");i<3;System.out.println("a")`.

2.	break和continue语句在多重嵌套循环中可以选择跳出哪一个循环,但是对应的for循环需要命名,	如:

		first:for(int i=0;i<3;i++){
			System.out.println("a");
			for(int n=o;n<3;n++)"{
				System.out.println("a");
				break first;
			}
		}

3.	关于逻辑运算符&,\|和&&,\|\|的区别,&&和\|\|在判断是若遇到第一个条件之后便可判断真假则不再判断第二个条件.

4.	交换变量的另外几种写法:	            
	*	
			a=a+b;/*不推荐使用,如果两个数的数值过大,会超出int的范围从而导致数据强制转换int类型,数据精度丢失*/
			b=a-b;
			a=a-b;    		
	*		   
			a=a^b;/*采用异或的方式,根据一个二进制数异或同一个二进制数两次得到其本身这个原理来实现*/
			b=a^b;
			a=a^b;