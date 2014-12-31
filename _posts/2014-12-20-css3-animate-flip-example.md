---
layout:        post
title:         CSS3 animate flip下的纸牌翻转效果实例页面
description:   "CSS3 animate flip下的纸牌翻转效果实例页面"
tags: [CSS3]
image:
  background: body_bg.gif
comments: true
share: true
---

CSS3 animate flip下的纸牌翻转效果实例页面
转自：http://www.zhangxinxu.com/study/201210/css3-animate-flip-example.html

    
<!--more-->

###CSS代码:
{% highlight JavaScript %}
.box {
    width: 355px;
    height: 500px;
    padding-top: 30px;
    padding-bottom: 30px;
    margin-left: auto;
    margin-right: auto;
    position: relative;
}
.list {
    position: absolute;
}
{% endhighlight %}

###HTML代码:
{% highlight JavaScript %}
<div id="box" class="box viewport-flip" title="点击翻面">
    <a href="/" class="list flip out"><img src="http://image.zhangxinxu.com/image/blog/201210/puke-k.png" alt="纸牌正面" /></a>
    <a href="/" class="list flip"><img src="http://image.zhangxinxu.com/image/blog/201210/puke-back.png" alt="纸牌背面" /></a>
</div>
{% endhighlight %}

###JS代码:
{% highlight JavaScript %}
// 在前面显示的元素，隐藏在后面的元素
var eleBack = null, eleFront = null,
    // 纸牌元素们 
    eleList = $(".list");

// 确定前面与后面元素
var funBackOrFront = function() {
    eleList.each(function() {
        if ($(this).hasClass("out")) {
            eleBack = $(this);
        } else {
            eleFront = $(this);
        }
    });
};
funBackOrFront();
$("#box").bind("click", function() {
    // 切换的顺序如下
    // 1. 当前在前显示的元素翻转90度隐藏, 动画时间225毫秒
    // 2. 结束后，之前显示在后面的元素逆向90度翻转显示在前
    // 3. 完成翻面效果
    eleFront.addClass("out").removeClass("in");
    setTimeout(function() {
        eleBack.addClass("in").removeClass("out");
        // 重新确定正反元素
        funBackOrFront();
    }, 225);
    return false;
});
{% endhighlight %}
