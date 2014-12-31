---
layout:        post
title:         WebApp_reset.css
description:   "分享一个可以用于webapp的reset样式表。"
tags: [CSS]
image:
  background: body_bg.gif
comments: true
share: true
---

分享一个可以用于webapp的reset样式表.

    
<!--more-->


{% highlight JavaScript %}
/* 浏览器重置 start */
body,p,ul,ol,li,dl,dt,dd,h1,h2,h3,h4,h5,form,fieldset,legend,input,select,textarea,button,th,td,menu,article,pre,xmp,plaintext,listing{margin:0;padding:0;}
pre,xmp,plaintext,listing{white-space:normal;}
article,aside,dialog,figure,footer,header,hgroup,nav,section,select,time{display:block;}
h1,h2,h3,h4,h5,h6,input,textarea,select,button,label{font-size:100%;font-weight:100;vertical-align:middle;}
ul,dl,ol{list-style:none;}
img,fieldset,input[type="submit"]{border:none;}
input{outline:none;background:transparent;vertical-align:top;}	
table{border-collapse:collapse;border-spacing:0;}		
button{cursor:pointer;border:none;}
textarea{word-wrap:break-word;resize:none;}
/* 去除IE下拉菜单箭头 */
select::-ms-expand{display:none;}
/* 去除IE下radio、checkbox默认样式 */
::-ms-check{display:none;}
/* 去除输入框、文本域默认边框和IOS4下的内阴影 */
input, textarea, select{-webkit-appearance:none;border:none;outline:none;}
/* 设置Gecko内核下输入框属性placeholder中值的文件颜色 */
input:-moz-placeholder{color:#C4C4C4;}
/* 设置webkit内核下输入框属性placeholder中值的文件颜色 */
::-webkit-input-placeholder{color:#C4C4C4;}
body{background:#EAEAEA;font-family:;-webkit-user-select:none;/* 禁止选中文本 */-webkit-text-size-adjust:none;-webkit-tap-highlight-color:rgba(0,0,0,0.05);/* 设置点击链接时遮罩层为全透明 */overflow:hidden;min-height:100%;font-family:"微软雅黑" medium sans-serif;background:#FFFFFF;color:#666666}
/* 设置webkit内核下文字大小不小于12px */
html,body{-webkit-text-size-adjust:none;height:100%;width:100%;overflow:hidden;}
/* 禁止长按链接、按钮、输入框出现系统默认弹框菜单 */
a,button,input{-webkit-touch-callout:none;outline:none;}
a,a:visited{text-decoration:none;-webkit-touch-callout:none;}
/* 浏览器重置 end */
{% endhighlight %}
