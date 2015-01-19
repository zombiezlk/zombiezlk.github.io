---
title: Chrome Uncaught SyntaxError Unexpected end of input
author: Nick
layout: post
categories: tech
---
{% include JB/setup %}

昨晚修改了下我的微博发送器,添加了将数据存入cookie的功能,第二天起来调试的时候就报错了,报错内容如下:

>Uncaught SyntaxError: Unexpected end of input

>Resource interpreted as Script but transferred with MIME type text/plain

从stackoverflow上面找到[这个答案][1],基本解决方法就在里面没跑了, 我看了下大意导致问题发生的原因有二:

1.	 你在编码过程中漏了一些符号,比如: `eval('[{"test": 4}') // 注意你缺了右边的中括号 ]`

2.	你请求的是二进制数据但是Content-Type确是text/html,将其修改为text/plain就解决了.

看到这个之后开始果断觉得我犯了第二个错误,便果从各种请求找错误,但是打开chrome的控制台看到的是有一个请求的Content-Type是text/plain,资源类型是javascript,但是查看代码又确实没错:

而且我除了加上新的代码并没有修改原有代码,但是之前是可以获得资源的,所以还是慢慢看是不是少了符号.果然,for循环少了一个},真是万分羞愧.

修复完之后运行可以发微博,但是控制台依然会出现:

>Resource interpreted as Script but transferred with MIME type text/plain

问题应该是出现在新浪微博的SDK,因为我只是利用他的接口传入内容发送微博,并不能对其请求头进行设置,所以便出现以上问题,但并不妨碍实际上的使用.

 [1]: http://stackoverflow.com/questions/3594923/chrome-uncaught-syntaxerror-unexpected-end-of-input