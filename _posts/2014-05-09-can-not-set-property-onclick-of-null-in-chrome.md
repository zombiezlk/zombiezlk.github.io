---
title: Chrome浏览器Cannot set property 'onclick' of null错误
author: Nick
layout: post
category: tech
---
{% include JB/setup %}

在编写自己的wordpress主题中的javascript的时候Chrome浏览器开始报错:  **Cannot set property 'onclick' of null**  
以下是我的代码:

		function changeLength(){
			var searchbox=document.getElementById("search-box");
			searchbox.onclick=function(){this.style.width="150px";};	
		}
		changeLength();

代码简单一点容易看,主要是说明问题.

我们都知道wordpress是先加载index.php,然后在index.php里面又再开始加载其他模版文件,所以header.php会在index.php加载过程中加载,而我的header.php里面的尾部又有serachform.php这个模版,所以serachform.php又会在header.php加载过程中开始加载.而我把js文件的引入写在了header.php的头部,所以浏览器开始解析header.php的时候,serachform.php里面的内容还未能加载完成,所以出现了题目中所提示的错误.

解决的办法:直接把引入文件这一句写在serachform.php文件里面,也就是在id="search-box"的标签之后(此标签位于header.php中,在加载serachform.php模版的前面).

结果:重新打开页面,问题解决.