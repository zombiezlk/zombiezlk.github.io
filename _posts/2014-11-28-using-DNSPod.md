---
title: 用DNSPod的域名服务器解析Godaddy的域名
author: Nick
layout: post
category: tech
---
{% include JB/setup %}

大家都知道Godaddy的域名解析很蛋疼，打开网站经常要等上好久，普遍的解决办法就是将它默认的域名服务器换成别的，国内的DNSPod还算有名（不过貌似也有很多人黑），所以就打算换成DNSPod的域名服务器，按照官网上的操作很快很简单就设定好了，唯一蛋疼就是可能在最长72小时才会生效，改完之后网站就打不开了，显示是dns解析错误，手机电脑都是这样，dns确实需要一定时间刷新缓存，所以就洗洗睡了。

第二天，第三天，情况依旧，上网查了查，大家都说基本在一天内就可以恢复正常，不用72小时,ip也可以ping通。所以有如下几种可能：

1.	本地dns缓存没有刷新。
2.	服务器设置有问题。

对于第一种基本可以排除，ipconfig  /flushdns，重启电脑，重启浏览器全都试过没有用。关于在Godaddy上将默认域名服务器换成DNSPod的域名服务器的操作非常简单，可以说这个过程没有错误，会不会是最新的操作还需要额外的设置，但是搜索了一遍也没有发现答案。自己又上Godaddy上看了一圈，我 将域名服务器改回默认，之后又改了回去。


之后的事情：

1.	 手机可以正常访问。
2.	 电脑依旧无法访问。

所有的可能性：

  * 电脑接的是校园网，与校园网的dns服务器缓存一直没有刷新，所以电脑一直是上不了，接了电脑wifi的手机同样不能访问。
  * 手机可以访问到底是因为前几天的设置刚刚生效还是因为我设置了回了默认的域名服务器。

等待一段时间后，手机又不能访问了，至此基本可以确定是因为我设置回了默认的域名服务器才能访问的，所以过段时间后设置回了DNSPod的域名服务器这个动作生效了，所以手机访问不了。

从这里可以推断的是，修改完域名服务器之后其实是可以很快生效的，所以用了DNSPod的服务之后我访问不了我的网站基本上不可能是等待dns刷新的时间不够。但是最诡异的是，我的电脑是从头到尾都是无法访问的，所以关于校园网的猜想还是有可能的。

但是我能得出更明显的结论是：免费真的没啥好货，而且你想找客服解决问题基本没戏，除非你是DNSPod的vip用户。

现在能做的就是

  * .再次把域名服务器改回默认，这样至少除了校园网之外的其他途径还能访问。
  * 继续等&#8230;..因为我之前重新设置了一次，这意味着我可能要再观察72小时&#8230;.

结论是：果断选前者，因为改了域名服务器其实是能很快生效的，上面已经说了，所以基本上不可能是等待dns刷新的时间不够，我的电脑一直上不了不止单纯的校园网的问题，也说明DNSPod的域名服务其实很坑。

--------------------------------------更新---------------------------------------

改回去之后，电脑可以访问,手机访问不了了...........所以以上关于校园网的全是扯淡，有可能只是我没耐心刷多几次，这次手机刷多几次应该就可以了，所以DNSPod的服务很坑就是我的唯一结论了。先留个坑，以后继续试用，看看到底是什么问题。

-------------------------------------更新----------------------------------------

手机再怎么刷新也上不了了，过了一阵子，电脑也不能访问了。删掉DNSPod上面的域名，重新设置一次，过了一段时间，手机电脑都可以访问了。

我彻底凌乱了，再来理一理吧。

我做的事：

第一次改成DNSPod->改回默认->改回DNSPod->改回默认->重新设置DNSpod

结果：

（从手机角度来看，实际证明电脑的线路反应速度确实比手机的慢）接近72小时无法访问->正常->无法访问->正常

只能说明，第一次dns更新缓存的时间确实很长，之后dns的更新速度快了很多，我反复的修改才会很快地导致网站时好时坏，至于哪一次修改对应哪一次的改变实在无法得知，这是dns缓存更新的时间差别巨大所导致的，甚至服务器的不稳定也会让你的域名解析不了，不一定是设置的问题，仅凭这些无法得出一个正确的结论。最好的办法只能是真的等待72小时，确认不是缓存的问题后才去质疑服务器是否出现问题。