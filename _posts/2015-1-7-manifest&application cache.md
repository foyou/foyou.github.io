---
layout:        post
title:         HTML5 开发 Web App —— HTML5 离线缓存
description:   "HTML5 开发 Web App —— HTML5 离线缓存"
tags: [HTML5,Manifest]
image:
  background: body_bg.gif
comments: true
share: true
---

###前言

关于html5的离线存储，大致可分为：

`localStorage`, `sessionStorage`
`indexedDB`
`web sql`
`application cache`

本文要介绍的，是 HTML5 离线网络应用程序的特性，离线网络应用程序在 W3C 中的实际名称是 "Offline Web applications" ，也称离线缓存。当用户打开浏览器时，浏览器会将一个列表中指定的资源都下载并储存在本地。下次当用户再访问这个网络程序时，浏览器会自动引用本地缓存中相应的文件，而不会再从网络下载这些资源。不管离线网络应用程序是否专为 Web Apps 而设，但这对于 Web Apps 来说无疑是个非常实用的特性，它使到 Web Apps 相对于原生 Apps 的一个重要劣势 —— 高度依赖网络，得以大大减缓。开发者可以利用这个特性把 Web Apps 中的元素缓存到本地端，使到 Web Apps 可以脱机工作，即使是需要联网工作的 Apps ，也可以缓存部分文件到本地端，减少带宽占用，这样 Web Apps 相对于原生 Apps 就更加具有优势了。下面正式开始介绍这个特性。

    
<!--more-->

###用前准备

这些应用需要服务器支持。 

Apache服务器开启支持的方式是：在conf/mime.types中添加一段代码： 

text/cache-manifest manifest 

IIS服务器开启方式： 

右键--HTTP---MIME映射下，单击文件类型---新建---扩展名输入appcache，类型中输入text/cache-manifest 


###如何使用

首先，我们建立一个html文件，类似这样：

{% highlight JavaScript %}
<!DOCTYPE html>
<html lang="en" manifest="manifest.appcache">
<head>
    <meta charset="UTF-8">
    <title>APP CACHE</title>
    <link rel="stylesheet" type="text/css" href="test.css">
</head><!-- text/cache-mainfest -->
<body>
    <img src="img/1.jpg">
    <img src="img/2.jpg">
<script type="text/javascript">
    window.addEventListener('load', function(e){
        console.log(window.applicationCache.status);
    })
</script>
</body>
</html>
{% endhighlight %}

可能有些代码看不懂，我们先看最简单的，第一行配置了一个manifest=”manifest.appcache”（注意是mani不是main），这是使用app cache首先要配置的，然后我们在这个html文件里引入了两个img做为测试用，然后监听了load时间来查看看application的status，关于applicationCache的api，可以查看。

然后在相同目录下新建一个manifest.appcache文件，注意关于路径要和html页面配置时一致即可。

{% highlight JavaScript %}
CACHE MANIFEST
#version 1.3
CACHE:
    img/1.jpg
    img/2.jpg
    test.css
NETWORK:
    *
FALLBACK:
	./logo.png ./offline.png    
{% endhighlight %}

关于manifest.appcache文件，基本格式为三段： CACHE， NETWORK，与 FALLBACK，其中NETWORK和FALLBACK为可选项，而第一行CACHE MANIFEST为固定格式，必须写在前面。

CACHE:（必须）

标识出哪些文件需要缓存，可以是相对路径也可以是绝对路径。例如：aa.css www.baidu.com/aa.js.

NETWORK:（可选）

这一部分是要绕过缓存直接读取的文件，可以使用通配符＊，,也就是说除了上面的cache文件，剩下的文件每次都要重新拉取。例如＊，login.php。

FALLBACK:（可选）

指定了一个后备页面，当资源无法访问时，浏览器会使用该页面。该段落的每条记录都列出两个 URI—第一个表示资源，第二个表示后备页面。两个 URI 都必须使用相对路径并且与清单文件同源。可以使用通配符。例如*.html /offline.html。

有了上面两个文件之后还要配置服务器的mime.types类型，找大盘apache的mime.types文件，添加

{% highlight JavaScript %}
text/cache-manifest .appcache
{% endhighlight %}

OK，上面文件配置完成之后，application cache就可以运行了。

查看console：

<img src="http://images.cnblogs.com/cnblogs_com/hedgerow/628038/o_consoletest.png" />

可以看到，一下子这么多log，但是除了4是我们console的log之外，其他的都是appcache自己打的，因为我们配置了manifest，系统会默认打出appcache的log。关于status的值：

<table border="0">
<tbody>
<tr>
<td>可选值</td>
<td>匹配常量</td>
<td>描述</td>
</tr>
<tr>
<td>0</td>
<td>
appCache.UNCACHED
</td>
<td>未缓存</td>
</tr>
<tr>
<td>1</td>
<td>
appCache.IDLE
</td>
<td>闲置</td>
</tr>
<tr>
<td>2</td>
<td>
appCache.CHECKING
</td>
<td>检查中</td>
</tr>
<tr>
<td>3</td>
<td>
appCache.DOWNLOADING
</td>
<td>下载中</td>
</tr>
<tr>
<td>4</td>
<td>
appCache.UPDATEREADY
</td>
<td>已更新</td>
</tr>
<tr>
<td>5</td>
<td>
appCache.OBSOLETE
</td>
<td>失效</td>
</tr>
</tbody>
</table>

###更新缓存的方式

1.更新manifest文件

浏览器发现manifest文件本身发生变化，便会根据新的manifest文件去获取新的资源进行缓存。当manifest文件列表并没有变化的时候，我们通常通过修改manifest注释的方式来改变文件，从而实现更新。

2.通过javascript操作

浏览器提供了applicationCache供js访问，通过对于applicationCache对象的操作也能达到更新缓存的目的。

3.清除浏览器缓存

对于第一种，我们修改一下manifest文件，把version改为1.4，然后刷新页面。

我们可以发现，appcache更新了缓存重新从网络上拉去的cache的文件，但是，我们如果想要看到改变，必须再次刷新页面。 对于第二种方法，我们首先修改一下我们的js，添加一个监听事件：

{% highlight JavaScript %}
window.applicationCache.addEventListener('updateready', function(){
            console.log('updateready!');
            window.applicationCache.swapCache();
        });
{% endhighlight %}

清除浏览器缓存再试一次，这次我们在console里调用window.applicationCache.update();updateready事件触发了，同样，appcache也更新了缓存，其中swapCache方法的意思是重新应用跟新后的缓存来替换原来的缓存！，到这里后基本的appcache也差不多了。

####注意事项：

站点离线存储的容量限制是5M
如果manifest文件，或者内部列举的某一个文件不能正常下载，整个更新过程将视为失败，浏览器继续全部使用老的缓存
引用manifest的html必须与manifest文件同源，在同一个域下
FALLBACK中的资源必须和manifest文件同源
当一个资源被缓存后，该浏览器直接请求这个绝对路径也会访问缓存中的资源。
站点中的其他页面即使没有设置manifest属性，请求的资源如果在缓存中也从缓存中访问
当manifest文件发生改变时，资源请求本身也会触发更新
完！
