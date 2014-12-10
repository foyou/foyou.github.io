---
layout:        post
title:         纯CSS实现各类气球泡泡对话框效果
description:   "纯CSS实现各类气球泡泡对话框效果"
tags: [CSS,气泡,对话框]
image:
  background: body_bg.gif
comments: true
share: true
---

使用纯CSS实现各类气球泡泡对话框的效果

##“字符法”与“边框法”

###字符法

世界上的语言多种多样，字符的种类也是千差万别，形状也是千奇百怪，所以，有些字符就可以当作图形来使用，以模拟一些web表现效果。例如，上面的人人网对话框尖角就可以使用棱形字符(◆)来实现，看到这个形状上面的90度的尖角了吗，我们可以让其溢出div显示，不就有了尖角效果了吗！定位可以使用margin负值或是absolute绝对定位。

    
<!--more-->

CSS代码:
{% highlight JavaScript %}
.org_box{
    height:80px;
    line-height:80px; 
    margin-bottom:30px; 
    padding-left:2em;
    background:#F3961C; 
    position:relative; 
}
.org_bot_cor_2{
    height:30px;
    line-height:0; 
    font-size:60px; 
    color:#f3961c; 
    position:absolute; 
    left:60px; 
    bottom:-25px;
}
{% endhighlight %}

html代码:
{% highlight JavaScript %}
<div class="org_box">
    <span class="org_bot_cor_2">◆</span>
    字符法实现底部90度尖角对话框
</div>
{% endhighlight %}

###边框法

关于“边框法”，这说起来话就长了。不急，慢慢来。
 我们首先来看下面一段样式代码：
 
{% highlight JavaScript %}
.test{width:10px; height:10px; border:10px solid; border-color:#ff3300 #0000ff #339966 #00ff00;}
{% endhighlight %}

当某个div应用了上面这个样式后，结果会如何？见下图（截自Firefox3.5，IE浏览器有细节上的差异）：
<img src="http://image.zhangxinxu.com/image/blog/201003/2010-03-13_001825.png" />

仔细观察边框色的交界处，四个角落有4个45°的斜边，将整个边框分成了四个等腰梯形。
现在，设想一下，如果我们现在只保留一个一个上边框，其余边框均transparent透明（或与背景色同色），那么是不是就只显示一个上面红色的边框了，我们测试下，与上面类似的代码，只是修改下其余三个边框的颜色。
{% highlight JavaScript %}
.test{width:10px; height:10px; border:10px solid; border-color:#ff3300 #ffffff #ffffff #ffffff;}
{% endhighlight %}
结果如下图（截自Firefox3.5）：
<img src="http://image.zhangxinxu.com/image/blog/201003/2010-03-13_002528.png" />

可见显示结果正如我们预想的。现在，再次开动脑筋，试想下，如果上面样式中的宽度和高度都是0，也就是width:10px; height:10px;变成了width:0; height:0;。对了，那么显示的将不会是个等腰梯形了，而是个等腰直角三角形。做个简单测试就知道答案了，如下的代码：
{% highlight JavaScript %}
.test{width:0; height:0; border:10px solid; border-color:#ff3300 #ffffff #ffffff #ffffff;}
{% endhighlight %}
结果如下（依旧截图自Firefox3.5）：
<img src="http://image.zhangxinxu.com/image/blog/201003/2010-03-13_003545.png" />
此时，我们惊奇的发现，使用border属性居然产生了一个等腰直角三角形，而这个等腰直角三角形用做气泡对话框的尖角再合适不过了。所以，上面的人人网对话框的尖角就可以用border实现了，只要让下边框有颜色，其他透明（或与外背景同色）就好了。

关于“边框法”实现的效果,CSS代码:
{% highlight JavaScript %}
.org_box{
    height:80px;
    line-height:80px; 
    margin-bottom:30px; 
    padding-left:2em;
    background:#F3961C; 
    position:relative; 
}
.org_bot_cor{
    width:0; 
    height:0; 
    font-size:0; 
    border-width:15px; 
    border-style:solid; 
    border-color:#f3961c transparent transparent; 
    _border-color:#f3961c white white; 
    overflow:hidden; 
    position:absolute; 
    left:60px; 
    bottom:-30px;
}
{% endhighlight %}
HTML代码:
{% highlight JavaScript %}
<div class="org_box">
    <span class="org_bot_cor"></span>
    边框法实现底部90度尖角对话框
</div>
{% endhighlight %}

还没完，我们现在再开动我们智慧的大脑，设想下，如果各个边框的宽度不一致，又会产生怎样的结果呢？我们把测试代码再修改一下，让其边框宽度不一致，如下：
{% highlight JavaScript %}
.test{width:0; height:0; border-width:20px 10px; border-style:solid; border-color:#ff3300 #ffffff #ffffff #ffffff;}
{% endhighlight %}
得到的结果如下图：
<img src="http://image.zhangxinxu.com/image/blog/201003/2010-03-13_005156.png" />
可以发现，尖角被拉高了，也就是相邻边框宽度的比例会影响单个边框形状的高与低的比例，这不难理解。
现在，我们再开动一下我们的脑筋，如果我们让相邻两个边框显示颜色会怎么样呢？测试一下吧，如下代码：
{% highlight JavaScript %}
.test{width:0; height:0; border-width:20px 10px; border-style:solid; border-color:#ff3300 #ff3300 #ffffff #ffffff;}
{% endhighlight %}
结果如下图：
<img src="http://image.zhangxinxu.com/image/blog/201003/2010-03-13_005753.png" />
这不就不多说什么了，大伙儿都看到，上个相邻边框尖角实现的气泡对话框效果吧。
<img src="http://image.zhangxinxu.com/image/blog/201003/2010-03-13_010328.png" />

##“边框法”的高级应用
说是高级应用，准确讲应该是“复杂应用”。就是说用两个标签（或无标签 – 使用:before与:after伪类）形成的两个不同的边框进行组合显示实现的一些效果。
看下面两个图，本单元就是要实现下面两种效果：
<img src="http://image.zhangxinxu.com/image/blog/201003/2010-03-13_014809.png" />
<img src="http://image.zhangxinxu.com/image/blog/201003/2010-03-13_020758.png" />
####1、效果(1)
CSS代码如下：
{% highlight JavaScript %}
.test{width:300px; padding:30px 20px; margin-left:60px; background:#beceeb; position:relative;}
.test span{width:0; height:0; font-size:0; overflow:hidden; position:absolute;}
.test span.bot{
    border-width:20px; 
    border-style:solid; 
    border-color:#ffffff #beceeb #beceeb #ffffff; 
    left:-40px; 
    top:40px;
}
.test span.top{
    border-width:10px 20px; 
    border-style:dashed solid solid dashed; 
    border-color:transparent #ffffff #ffffff transparent; 
    left:-40px; 
    top:60px;
}
{% endhighlight %}

HTML代码如下：
{% highlight JavaScript %}
<div class="test">
    <span class="bot"></span>
    <span class="top"></span>
    CSS “边框法”实现气泡对话框效果一
</div>
{% endhighlight %}

####2、效果(2)
CSS代码如下：
{% highlight JavaScript %}
.test{width:300px; padding:30px 20px; border:5px solid #beceeb; position:relative;}
.test span{width:0; height:0; font-size:0; overflow:hidden; position:absolute;}
.test span.bot{
    border-width:20px; 
    border-style:solid dashed dashed; 
    border-color:#beceeb transparent transparent; 
    left:80px; 
    bottom:-40px;
}
.test span.top{
    border-width:20px; 
    border-style:solid dashed dashed; 
    border-color:#ffffff transparent transparent; 
    left:80px; 
    bottom:-33px;
}            
{% endhighlight %}
HTML代码如下：
{% highlight JavaScript %}
<div class="test">
    <span class="bot"></span>
    <span class="top"></span>
    CSS “边框法”实现气泡对话框效果二
</div>
{% endhighlight %}

####3、效果原理简述
原理的关键字就是“覆盖”，另外一个边框形成的尖角覆盖之前的一个，只要控制好覆盖的位置，然后就形成了实际上的不规则尖角或是尖角边框。您还可以发挥您的创造力，实现更多其它的效果。

##关于“边框法”的一些说明
关于“边框法”，有一些知识必须要提一下。
###1. IE6的奇偶bug
如果定位外框高度或是宽度为奇数，则IE6下，绝对定位元素的低定位和右定位会有1像素的误差。所以，尽量保证外框的高度或宽度为偶数值。
###2. IE6的空div莫名高度bug
IE6下，空div会有莫名的高度，也就是说height:0;不顶用，此时形成的尖角的实际占高于其他浏览器是有差异的。可使用font-size:0; + overflow:hidden;修复此问题。
###3. IE6不支持实线边框透明transparent属性
IE6不支持实线边框透明transparent属性，当某边框设置为transparent属性，且有宽度的话，那么透明会以默认的黑色显示的。解决方法有两个，一是将需要隐藏的颜色设置为背景色，这样与透明效果一样，此法有局限性，在复杂“边框法”应用下是行不通的；二是可以使用dashed代替solid，可以实现IE6下边框transparent透明。为什么能够实现透明？您可以参考默尘的这篇文章Dotted&Dashed终极分析及IE6透明边框，我说点题外话，其对原因的解释似乎说的通，但是，是不是如此，我是相当怀疑的，我尚未有时间验证，这个可以标记一下。

##利用CSS3实现气泡样式的对话框
之前我们所说的一切，是全兼容现在所有主流浏览器的。而这里的方法，只支持对CSS3支持良好的浏览器，例如Firefox3.5+，chrome以及Safari。本文一开头的那张黄黄的大图就是使用的CSS3实现的圆形以及圆弧状的拐角尾巴。
效果的核心就是CSS3的圆角属性：border-radius，例如开始的大尾巴图片，就是使用border-radius实现的三个圆组成的，大圆形成主体内容，两个小一点的圆相互遮盖，形成圆弧小尾巴，但这个不具体讲，现在，以一个相对简单点的例子做示例。
CSS代码如下：
{% highlight JavaScript %}
.test{
    width:300px;
    padding:80px 20px;
    margin-left:100px;
    background:#beceeb;
    -webkit-border-top-left-radius:220px 120px;
    -webkit-border-top-right-radius:220px 120px;
    -webkit-border-bottom-right-radius:220px 120px;
    -webkit-border-bottom-left-radius:220px 120px;
    -moz-border-radius:220px / 120px;
    border-radius:220px / 120px;
    position:relative;
}
.test span{width:0; height:0; font-size:0; background:#beceeb; overflow:hidden; position:absolute;}
.test span.bot{
    width:30px;
    height:30px;
    -moz-border-radius:30px;
    -webkit-border-radius:30px;
    border-radius:30px;
    left:10px;
    bottom:-20px;
}
.test span.top{
    width:15px;
    height:15px;
    -moz-border-radius:15px;
    -webkit-border-radius:15px;
    border-radius:15px;
    left:0px;
    bottom:-40px;
}
{% endhighlight %}
HTML代码如下：
{% highlight JavaScript %}
<div class="test">
    <span class="bot"></span>
    <span class="top"></span>
    CSS3 border-radius实现气泡对话框效果 
</div>
{% endhighlight %}

最后的效果如下图：
<img src="http://image.zhangxinxu.com/image/blog/201003/2010-03-13_030217.png" />

From:张鑫旭-鑫空间-鑫生活[http://www.zhangxinxu.com]  <a href="http://www.zhangxinxu.com/wordpress/?p=651" target="_blank">http://www.zhangxinxu.com/wordpress/?p=651</a>

