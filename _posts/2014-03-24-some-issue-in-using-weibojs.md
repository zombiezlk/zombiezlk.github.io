---
title: 关于用weibo-JS调用api接口过程中的编码问题
author: Nick
layout: post
category: tech
---
{% include JB/setup %}

背景:因为平常在新浪看球的时候总是有一些发微博抽奖的活动,每次要发的微博内容都差不多,不是@那个就是@这个.老是写一样的东西让我有点烦,就想写一个插件,以后要发微博的时候直接调用这个插件就行了.有了这个想法就开始各种查文档,发现新浪还提供了很多种语言的SDK,其中weibo-JS就是javascript的SDK,别人写好的东西,不用白不用.  
步骤:  
1.首先你得先去新浪的微博平台申请一个appkey,然后开始填写各种资料,然后你就可以用这个appkey作为通行证来向新浪服务器提交各种申请.  
2.接下来就可以开始编码了,因为整个过程就是调用一个发微博的接口,再加上有SDK,所以编码大体来说没什么难度,我这里也不打算把全部编码贴出来,只贴出一段最能反映我问题的一段最简单的代码,代码写的烂,大家不要见笑(或许只有我自己会看吧.)

		var request,response,token;
          		var text = "微博内容";
          		var encodetext = encodeURIComponent(text);  //将微博内容进行urlencode

          		WB2.anyWhere(function(W){                               //这个就是SDK提供的调用api的函数
                   	W.parseCMD(
                              	"/statuses/update.json", 
                               	function(sResult, bStatus){console.log(sResult, bStatus)},
                               	{access_token:"2.00sswhgC6dZDOE5af9b45d4e01duvj", /**这个是我拿到的token,具体怎么拿的
                                	status:encodetext,                             不贴出来了,直接把参数改为具体的值**/
                                	visible:0		   
                               	},
                               	{method: 'post'}
                               	);                               
                         	}
          	);

3.函数执行后从我的微博里面看到的是一段乱码,然后弄了很久也不知道是为什么,最后在网上找到的转码工具中将我的微博内容进行utf-8编码,再用`console.log(encodetext)`将微博内容打印出来,然后我发现两者并不一样,于是我怀疑javascript用的编码不一样,但是javascript里面的encodeURIComponent()方法确实是将字符串进行utf-8编码,这时我再将微博内容用转码工具转成gb2312,这时再跟console.log(encodetext)比对,两者是一样的!这时我再单独在chrome控制台中写一段代码如下:

		var text = "微博内容"
		var encodetext = encodeURIComponent(text); 
		console.log(encodetext);

可是打印出来的就是utf-8编码没错,这让我想不明白,我将html页面的编码改为utf-8之后也无效,最后只能怀疑是新浪SDK的问题,话说不写不知道,一写发现新浪的文档那叫一个坑,已经2年没更新过了,很多接口返回的数据都改了,但是文档也没改,很多细节有错漏,但这足以让开发者折腾一会儿了.最后,我只能无奈地直接把经过utf-8编码后的内容直接写进参数里面,程序才得以正确执行.但是这个问题还没有解决,虽然有可能是新浪方面的问题,但我觉得更有可能的是我这个菜鸟的问题,希望有高手能看见并指正,当然我能自己解决更好.

更新:  
--------------------------------分割线-----------------------------  
今天在用vim编写我自己的wordpress主题文件的时候发现了一个问题,就是我在vim里面编辑的php文件里面的中文在浏览器上会变成乱码,几经查找,终于知道是vim的编码问题,网上的有关这个问题的文章很多了,我就随便找一篇出来吧,具体可以看[这一篇文章][1].  
这时候我突然想起我遇到的这个问题,原来就是vim会把编码保存为跟系统的locale字符集一样的编码,所以怎么转码都会被转不会变为我需要的utf-8,按照上面那篇文章将vim的默认fileencoding修改为utf-8问题就解决了.

 [1]: http://www.cnblogs.com/hopeworld/archive/2011/04/20/2022335.html