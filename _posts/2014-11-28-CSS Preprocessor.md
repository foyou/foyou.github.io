---
layout:        post
title:         CSS预处理器——Sass、LESS和Stylus实践
description:   "CSS(Cascading Style Sheet)被译为级联样式表，做为一名前端从业人员来说，这个专业名词并不陌生，在行业中通常称之为风格样式表（Style Sheet），它主要是用来进行网页风格设计的。通过设立样式表，可以统一地控制HTML（XHTML）中各标签的显示属性。可以使人更能有效的控制Web页面（或Web应用程序）外观，可以精确指定Web元素位置，外观以及创建特殊效果的能力。"
tags: [CSS3]
image:
  background: body_bg.gif
comments: true
share: true
---

CSS(Cascading Style Sheet)被译为级联样式表，做为一名前端从业人员来说，这个专业名词并不陌生，在行业中通常称之为"风格样式表（Style Sheet）"，它主要是用来进行网页风格设计的。通过设立样式表，可以统一地控制HTML（XHTML）中各标签的显示属性。可以使人更能有效的控制Web页面（或Web应用程序）外观，可以精确指定Web元素位置，外观以及创建特殊效果的能力。CSS拥有对网页对象和模型样式编辑能力，并能够进行初步交互设计，是目前基于文本展示最优秀的表现设计语言。CSS能够根据 不同使用者的理解能力，简化或者优化写法，针对各类人群有较强的易读性。
    
<!--more-->

就CSS本身而言，对于大多数Web前端从业人员来说就不是问题。学过CSS的人都知道，它不是一种编程语言。你可以用它开发网页样式，但是没法用它编程。换句话说，CSS基本上是设计师的工具，不是程序员的工具。在程序员的眼里，CSS是很头痛的事情，它并不像其它程序语言，比如说PHP、Javascript等等，有自己的变量、常量、条件语句以及一些编程语法，只是一行行单纯的属性描述，写起来相当的费事，而且代码难易组织和维护。

很自然的，有人就开始在想，能不能给CSS像其他程序语言一样，加入一些编程元素，让CSS能像其他程序语言一样可以做一些预定的处理。这样一来，就有了"CSS预处器（CSS Preprocessor）"。

##什么是CSS预处器

CSS预处理器定义了一种新的语言，其基本思想是，用一种专门的编程语言，为CSS增加了一些编程的特性，将CSS作为目标生成文件，然后开发者就只要使用这种语言进行编码工作。通俗的说，CSS预处理器用一种专门的编程语言，进行Web页面样式设计，然后再编译成正常的CSS文件，以供项目使用。CSS预处理器为CSS增加一些编程的特性，无需考虑浏览器的兼容性问题，例如你可以在CSS中使用变量、简单的逻辑程序、函数等等在编程语言中的一些基本特性，可以让你的CSS更加简洁、适应性更强、可读性更佳，更易于代码的维护等诸多好处。

CSS预处理器技术已经非常的成熟，而且也涌现出了很多种不同的CSS预处理器语言，比如说：Sass（SCSS）、LESS、Stylus、Turbine、Swithch CSS、CSS Cacheer、DT CSS等。如此之多的CSS预处理器，那么"我应该选择哪种CSS预处理器？"也相应成了最近网上的一大热门话题，在Linkedin、Twitter、CSS-Trick、知呼以及各大技术论坛上，很多人为此争论不休。相比过计我们对是否应该使用CSS预处理器的话题而言，这已经是很大的进步了。

到目前为止，在众多优秀的CSS预处理器语言中就属Sass、LESS和Stylus最优秀，讨论的也多，对比的也多。本文将分别从他们产生的背景、安装、使用语法、异同等几个对比之处向你介绍这三款CSS预处理器语言。相信前端开发工程师会做出自己的选择——我要选择哪款CSS预处理器。

##Sass、LESS和Stylus背景介绍

为了能更好的了解这三款流行的CSS预处理器，我们先从其背景入手，简单的了解一下各自的背景信息。

###Sass背景介绍

Sass是对CSS（层叠样式表）的语法的一种扩充，诞生于2007年，最早也是最成熟的一款CSS预处理器语言，它可以使用变量、常量、嵌套、混入、函数等功能，可以更有效有弹性的写出CSS。Sass最后还是会编译出合法的CSS让浏览器使用，也就是说它本身的语法并不太容易让浏览器识别，因为它不是标准的CSS格式，在它的语法内部可以使用动态变量等，所以它更像一种极简单的动态语言。

其实现在的Sass已经有了两套语法规则：一个依旧是用缩进作为分隔符来区分代码块的；另一套规则和CSS一样采用了大括号（`｛｝`）作为分隔符。后一种语法规则又名SCSS，在Sass3之后的版本都支持这种语法规则。

注：Sass官网地址：<a href="http://sass-lang.com" target="_blank" rel="external">http://sass-lang.com</a>

###LESS的背景介绍

2009年开源的一个项目，受Sass的影响较大，但又使用CSS的语法，让大部分开发者和设计师更容易上手。LESS提供了多种方式能平滑的将写好的代码转化成标准的CSS代码，在很多流行的框架和工具中已经能经常看到LESS的身影了（例如Twitter的Bootstrap框架就使用了LESS）。

根据维基百科上的介绍，其实LESS是Alexis Sellier受Sass的影响创建的一个开源项目。当时SASS采用了缩进作为分隔符来区分代码块，而不是CSS中广为使用的大括号（`｛｝`）。为了让CSS现有的用户使用起来更佳方便，Alexis开发了LESS并提供了类似CSS的书写功能。

注：LESS的官网：<a href="http://lesscss.org" target="_blank" rel="external">http://lesscss.org</a>

###Stylus背景介绍

Stylus，2010年产生，来自于Node.js社区，主要用来给Node项目进行CSS预处理支持，在此社区之内有一定支持者，在广泛的意义上人气还完全不如Sass和LESS。

Stylus被称为是一种革命性的新语言，提供一个高效、动态、和使用表达方式来生成CSS，以供浏览器使用。Stylus同时支持缩进和CSS常规样式书写规则。

注：Stylus官网：<a href="http://learnboost.github.com/stylus" target="_blank" rel="external">http://learnboost.github.com/stylus</a>

##Sass、LESS和Stylus的安装

简单了解以后，您是否有点蠢蠢欲动，想动手一试，看看哪种预处理器适合自己的编码习惯，或者说更适合自己的团队成员以及项目的开发。要试，肯定有必要了解一下三者各自的安装方法。在这一节中，主要为大家介绍三者的安装方法，以供大家对比。

###Sass的安装
Sass是Ruby语言写的，但是两者的语法没有关系。不懂Ruby，照样可以正常使用Sass。只是必须先安装Ruby，然后再安装Sass。

首先安装Ruby，如果你使用的是IOS系统，那么你的系统已经安装好了Ruby。如果你使用的是微软的Windows系统，那么安装就有些许的麻烦。不过也不用过于担心，按照下面的步骤就能帮你快速的安装好。

到RubyInstaller官网上<a href="http://rubyinstaller.org/downloads" target="_blank" rel="external">下载Ruby安装文件</a>（随意选择一个版本），此处选择的是最新版本Ruby1.9.3-p385：

<a href="http://www.cn-sass.com/postimg/1410/preprocessor-22.jpg" title="下载Ruby安装文件" class="fancybox" rel="article0"><img src="http://www.cn-sass.com/postimg/1410/preprocessor-22.jpg" alt="下载Ruby安装文件" title="下载Ruby安装文件"></a><span class="caption">下载Ruby安装文件</span>

Ruby安装文件下载好后，可以按应用软件安装步骤进行安装Ruby。在安装过程中，个人建议将其安装在C盘下，在安装过程中选择第二个选项，如下图所示：

<a href="http://www.cn-sass.com/postimg/1410/preprocessor-23.jpg" title="Ruby安装" class="fancybox" rel="article0"><img src="http://www.cn-sass.com/postimg/1410/preprocessor-23.jpg" alt="Ruby安装" title="Ruby安装"></a><span class="caption">Ruby安装</span>

Ruby安装完成后，在开始菜单中找到新安装的Ruby，并启动Ruby的Command控制面板，如下图所示：

<a href="http://www.cn-sass.com/postimg/1410/preprocessor-24.jpg" title="启动Ruby命令面板" class="fancybox" rel="article0"><img src="http://www.cn-sass.com/postimg/1410/preprocessor-24.jpg" alt="启动Ruby命令面板" title="启动Ruby命令面板"></a><span class="caption">启动Ruby命令面板</span>

在启动的Ruby命令控制面板中输入下面的命令：

{% highlight JavaScript %}
gem install sass
{% endhighlight%}

输入上面命令，回车后就会自动安装Sass：

<a href="http://www.cn-sass.com/postimg/1410/preprocessor-25.jpg" title="安装Sass" class="fancybox" rel="article0"><img src="http://www.cn-sass.com/postimg/1410/preprocessor-25.jpg" alt="安装Sass" title="安装Sass"></a><span class="caption">安装Sass</span>

这样Sass就安装成功了，也就可以使用了。

###LESS的安装

LESS的安装和Sass安装有所不同，他不需要依赖于Ruby环境，就能直接安装使用。不过LESS安装分为两种：客户端和服务器端安装。

####客户端安装：

我们可以直接在客户端使用`.less`（LESS源文件），只需要在<a href="http://lesscss.org/" target="_blank" rel="external">官网下载</a>一个JavaScript脚本文件主`less.js`，然后在我们需要引入LESS源文件的HTML的`&lt;head&gt;`中加入如下代码：

{% highlight JavaScript %}
<link rel="stylesheet/less" type="text/css" href="文件路径/styles.less">
<script src="文件路径/less.js" type="text/javascript"></script>
{% endhighlight%}

需要注意的是：在引入`.less`文件中，`&lt;link&gt;`的`rel`属性要设置为`stylesheet/less`。还有更重要的一点需要注意的是：LESS源文件一定要在`less.js`引入之前引入，这样才能保证LESS源文件正确编译解析。

####服务器端安装

LESS在服务器端的使用主要是借助于LESS的编译器，将LESS源文件编译生成最终的CSS文件，目前常用的方式是利用node的包管理器（npm）安装LESS，安装成功后就可以在Node环境中对LESS文件进行编译。如此一来，LESS在服务器的安装和Sass的安装有点相似，不同之处是他们需依赖的环境不一样，LESS需要的是Node JS环境，而Sass需要的是Ruby环境，为了让大家能更清晰的知道如何在服务器端安装LESS，此处简单的过一下安装过程（本文演示的是在Window7下的安装方法）。

首先到Node Js的<a href="http://nodejs.org/download" target="_blank" rel="external">官网下载</a>一个适合Windows系统的安装文件，此处下载的是"node-v0.8.20-x86.msi"：

<a href="http://www.cn-sass.com/postimg/1410/preprocessor-26.jpg" title="下载Node JS" class="fancybox" rel="article0"><img src="http://www.cn-sass.com/postimg/1410/preprocessor-26.jpg" alt="下载Node JS" title="下载Node JS"></a><span class="caption">下载Node JS</span>

安装文件下载后，按正常应用程序方法一样安装。安装完成后在开始菜单中启用Node js的Command控制面板：
<a href="http://www.cn-sass.com/postimg/1410/preprocessor-27.jpg" title="启动NodeJs命令面板" class="fancybox" rel="article0"><img src="http://www.cn-sass.com/postimg/1410/preprocessor-27.jpg" alt="启动NodeJs命令面板" title="启动NodeJs命令面板"></a><span class="caption">启动NodeJs命令面板</span>

在启动的Node Js的Command控制面板直接输入下面的命令：

{% highlight JavaScript %}
$ npm install less
{% endhighlight%}

<a href="http://www.cn-sass.com/postimg/1410/preprocessor-28.jpg" title="安装LESS" class="fancybox" rel="article0"><img src="http://www.cn-sass.com/postimg/1410/preprocessor-28.jpg" alt="安装LESS" title="安装LESS"></a><span class="caption">安装LESS</span>
这样就安装完LESS的编译命令，可以在本地正常编译LESS源文件。如果您想下载最新稳定版本的LESS，还可以尝试在Node JS的Command控制面板输入：

{% highlight JavaScript %}
$ npm install less@latest
{% endhighlight%}

###Stylus的安装

Stylus的安装和LESS在服务器端的安装很相似，同样依赖于Node JS的环境，也就是说也要先安装Node Js（如果安装Node JS请参考前面的介绍）。唯一不同的是在Node JS的Command控制面板输入的命令不一样：

{% highlight JavaScript %}
$ npm install stylus
{% endhighlight%}

然后，就会自动下载安装最新的stylus库：

<a href="http://www.cn-sass.com/postimg/1410/preprocessor-29.jpg" title="安装Stylus" class="fancybox" rel="article0"><img src="http://www.cn-sass.com/postimg/1410/preprocessor-29.jpg" alt="安装Stylus" title="安装Stylus"></a><span class="caption">安装Stylus</span>

这样就算是安装完Stylus了，也可以正常使用Stylus。

##Sass、LESS和Stylus转译成CSS

Sass、LESS和Stylus源文件（除了LESS源文件在客户端下运行之外）都不能直接被浏览器直接识别，这样一来，要正常的使用这些源文件，就需要将其源文件转译成浏览器可以识别的CSS样式文件，这也是使用CSS预处理器很关键的一步，如果这一步不知道如何操作，那么意味着写出来的代码不可用。接下来按同样的方式，分别来看看三者之间是如何将其源文件转译成所需要的CSS样式文件。

###Sass源文件转译成CSS文件

Sass文件就是普通的文本文件，不过其文件后缀名有两种，一种为`.sass`；另一种为`.scss`。我们一般用`.scss`就好，至于这两种文件扩展名的区别在于`.sass`是Sass语言文件的扩展后缀名，而`.scss`是SCSS语言文件的扩展后缀名，至于Sass和SCSS的详细区别这里不做介绍。你可以简单的理解为他们都是CSS预处理器语言，而且两者功能特性都一样，只是书写规则不同而以。
要转译Sass文件的前提是有文件可转，因此先在项目中创建一个Sass文件，此例中将其命名为`style.scss`，并且将其放在对应的项目样式中，如D盘的`www/workspace/Sass/css`目录下。
启动Ruby的Command控制面板，找到需要转译的Sass文件，如此例此文件放置在D盘的`www/workspace/Sass/css`目录下：

<a href="http://www.cn-sass.com/postimg/1410/preprocessor-30.jpg" title="进入Sass文件目录" class="fancybox" rel="article0"><img src="http://www.cn-sass.com/postimg/1410/preprocessor-30.jpg" alt="进入Sass文件目录" title="进入Sass文件目录"></a>
如果仅针对于单文件的转译，需要将`style.scss`转译成`style.css`，只需要在对应的目录下输入下面的命令：

{% highlight JavaScript %}
$ sass style.scss style.css
{% endhighlight%}

这样原本在D盘的`www/workspace/Sass/css`目录下只有`style.scss`这个文件，现在增加了一个`style.css`文件，而这个CSS文件我们就可以用来调用，他也就是`style.scss`转译出来的样式文件。
当然，我们不可能每保存一次这个Sass文件就来执行一回这个转译命令。那么有没有更好的办法实现呢？回答是肯定的，可以通过下面的监听命令实现，这样一来只要Sass文件做了修改，就会自动更新对应的CSS样式文件。
单文件的监听，只需要在刚才的命令控制面板中输入：

{% highlight JavaScript %}
$ sass --watch style.scss:style.css
{% endhighlight%}

按下回车键（`Enter`）,就会看到下面的提示信息：
<a href="http://www.cn-sass.com/postimg/1410/preprocessor-31.jpg" title="Sass编译" class="fancybox" rel="article0"><img src="http://www.cn-sass.com/postimg/1410/preprocessor-31.jpg" alt="Sass编译" title="Sass编译"></a><span class="caption">Sass编译</span>
看到上图所示的提示信息就表示监听成功，这样一来，你只要修改了`style.scss`文件，`style.css`文件就会随着更新变化。
对于一个项目而言，不太可能只有一个CSS样式文件，如果有多个Sass文件需要监听时，就很有必要的组织一下。默认情况之下，我喜欢把所有的Sass文件放在`/css/sass`目录中，而生成的CSS文件则直接放在`/css`目录下。现在我们修改一下项目文件目录结构，在`/css`目录中再创建一个`sass`目录，并找刚才创建的`style.scss`文件移至`/css/sass`目录下。此时监听`css/sass`下的所有Sass文件时，可以在命令面板中输入如下格式的命令：

{% highlight JavaScript %}
$ sass --watch sassFileDirectory:cssFileDirectory
{% endhighlight%}

在本例中，冒号前面的`sassFileDirectory`是指`/css/sass`下的`sass`目录，冒号后面的`cssFileDirectory`是指`css`目录，对应的命令就是：

{% highlight JavaScript %}
$ sass --watch css/sass:css
{% endhighlight%}

回车后，同样可以得到提示信息：
<a href="http://www.cn-sass.com/postimg/1410/preprocessor-32.jpg" title="Sass多文件编译" class="fancybox" rel="article0"><img src="http://www.cn-sass.com/postimg/1410/preprocessor-32.jpg" alt="Sass多文件编译" title="Sass多文件编译"></a><span class="caption">Sass多文件编译</span>
除了使用Ruby的Command控制面板转译Sass之外还可以考虑第三方工具，比如说有名的Compass.app和fire.app。
<h3 id="LESS文件的转译成CSS文件">LESS文件的转译成CSS文件</h3>
LESS文件的转译和Sass文件转译可以说是大同小异，不同之处是LESS在安装的Node JS环境下通过其自己的命令来进行转译。

{% highlight JavaScript %}
$ lessc style.less
{% endhighlight%}

上面的命令会将编译的CSS传递给stdout，你可以将它保存到一个文件中：

{% highlight JavaScript %}
$ lessc style.less > style.css
{% endhighlight%}

除了上面的命令转译LESS源文件之外，现在还有很多第三方开发的工具，比较常见的有：SimpleLess、Less.app、LESS编译辅助脚本-LESS2CSS、WinLess和CodeKit.app等，我个人现在常用的是WinLess工具，简单易用，不过在IOS系统下LESS.app和CodeKit.app很好用。

###Stylus源文件转译成CSS文件

Stylus具有可执行性，因此Stylus能将自身转换成CSS。Stylus可以读取自"stdin"输出到"stdout"，因此Stylus可以像下面转译源文件：

{% highlight JavaScript %}
$ stylus –compress <some.styl> some.css
{% endhighlight%}

Stylus也像Sass一样，同时接受单个文件和整个目录的转译。例如，一个目录名为`css`将在同一个目录编译并输出`.css`文件。
{% highlight JavaScript %}
$ stylus css
{% endhighlight%}

下面的命令将输出到`./public/stylesheets`：
{% highlight JavaScript %}
$ stylus css –out public/stylesheets
{% endhighlight%}

还可以同时转译多个文件：
{% highlight JavaScript %}
$ stylus one.styl two.styl
{% endhighlight%}

如果你的浏览器安装了Firebug，那么可以使用<a href="https://github.com/LearnBoost/stylus/blob/master/docs/firebug.md" target="_blank" rel="external">FireStylus扩展</a>。
{% highlight JavaScript %}
$ stylus –firebug <path>
{% endhighlight%}

##Sass、LESS和Stylus的语法
每一种语言都有自己一定的语法规则，CSS预处理器语言也不例外，在真正使用CSS预处器语言之前还有一个不可缺少的知识点，就是对语法的理解。值得庆幸的是，这三款CSS预处理器语言的语法和CSS语法都差不多。
###Sass语法
Sass3.0版本开始使用的是标准的CSS语法，和SCSS可以说是一样的。这样Sass代码转换成CSS代码变得更容易。默认Sass使用`.scss`扩展名。Sass语法规则可以像CSS那样书写：
{% highlight JavaScript %}
/*style.sass新版语法规则*/
h1{
color:#936;
background-color:#333;
}
{% endhighlight%}

正如你所看到的，在Sass样式中，这样的代码是在简单不过的了。
重要的一点是，Sass也同时支持老的语法，老的语法和常规的CSS语法略有不同，他需要严格的语法，任何的缩进和字符的错误都会造成样式的编译错误。Sass可以省略大括号（`｛｝`）和分号（`；`），完全依靠严格的缩进和格式化代码，而且文件使用`.sass`扩展名，他的语法类似于：
{% highlight JavaScript %}
/*style.sass*/
h1
color:#936
background-color: #333
{% endhighlight%}

###LESS语法
LESS是CSS的一种扩展形式，它并没有阉割CSS的功能，而是在现有的CSS语法上，添加了很多额外的功能。就语法规则而言，LESS和Sass一样，都是使用CSS的标准语法，只是LESS的源文件的扩展名是`.less`，其基本语法类似于：

{% highlight JavaScript %}
/*style.less*/
h1 {
color: #963;
background-color: #333;
}
{% endhighlight%}

###Stylus语法
Stylus的语法花样多一些，它的文件扩展名是`.styl`，Stylus也接受标准的CSS语法，但是他也像Sass老的语法规则，使用缩进控制，同时Stylus也接受不带大括号（`｛｝`）和分号的语法，如下所示：
{% highlight JavaScript %}
/*style.styl*/
/*类似于CSS标准语法*/
h1 {
color: #963;
background-color:#333;
}
/*省略大括号（｛｝）*/
h1
color: #963;
background-color: #333;
/*省略大括号（｛｝）和分号（;）*/
h1
color:#963
background-color:#333
{% endhighlight%}

在Stylus样式中，你也可以在同一个样式文件中使用不同的语法规则，下面这样的写法也不会报错：
{% highlight JavaScript %}
/*style.styl*/
h1 {
color #963
}
h2
font-size:1.2em
{% endhighlight%}

##Sass、LESS和Stylus特性

这三款CSS预处理器语言具有一些相同的特性，例如：变量、混入、嵌套、函数等。在这一节中，我们依次来对比一下这三款CSS预处理器语言各种特性的异同之处，以及使用方法。

###变量（Variables）

如果你是一个开发人员，变量应该是你最好朋友之一。在CSS预处理器语言中你也可以声明变量，并在整个样式表中使用。CSS预处理器语言支持任何变量（例如：颜色、数值、文本）。然后你可以在任意地方引用变量。

####Sass声明变量必须是`$`开头，后面紧跟变量名和变量值，而且变量名和变量值需要使用冒号（`：`）分隔开。就像CSS属性设置一样：
{% highlight JavaScript %}
/*声明变量*/

$mainColor: #963;
$siteWidth: 1024px;
$borderStyle: dotted;

/*调用变量*/ 　| /*转译出来的CSS*/
------------------------------------------+------------------------------
body { | body {
color: $mainColor; | color: #963;
border:1px $borderStyle $mainColor; | border:1px dotted #963;
max-width: $siteWidth; | max-width: 1024px;
} | }
{% endhighlight%}

####LESS的变量
LESS样式中声明变量和调用变量和Sass一样，唯一的区别就是变量名前面使用的是`@`字符：
{% highlight JavaScript %}
/*声明变量*/

@mainColor: #963;
@siteWidth: 1024px;
@borderStyle: dotted;

/*调用变量*/ 　| /*转译出来的CSS*/
----------------------------------------+-------------------------------
body { | body {
color: @mainColor; | color:#963;
border:1px @borderStyle @mainColor; | border:1px dotted #963;
max-width: @siteWidth; | max-width:1024px;
} | }
{% endhighlight%}

####Stylus的变量
Stylus样式中声明变量没有任何限定，你可以使用`$`符号开始。结尾的分号（`;`）可有可无，但变量名和变量值之间的等号（`=`）是需要的。有一点需要注意的是，如果我们使用`@`符号开头来声明（0.22.4）变量，Stylus会进行编译，但其对应的值并不会赋值给变量。换句话说，在Stylus中不要使用`@`符号开头声明变量。Stylus中调用变量的方法和LESS、Sass是完全相同的。
{% highlight JavaScript %}
/*声明变量*/

mainColor = #963;
siteWidth = 1024px;
$borderStyle = dotted;

/*调用变量*/ 　| /*转译出来的CSS*/
----------------------------------------+--------------------------------
body | body {
color mainColor | color: #963;
border 1px $borderStyle mainColor | border:1px dotted #963
max-width siteWidth | max-width:1024px;
| }
{% endhighlight%}

Stylus还有一个独特功能，不需要分配值给变量就可以定义引用属性：
{% highlight JavaScript %}
/*水平垂直居中*/ | /*转译出来的CSS*/
------------------------------------+------------------------------------
#logo | #logo {
position absolute | position:absolute;
top 50% | top:50%;
left 50% | left:50%;
width w = 150px | width:150px;
height h = 80px | height:80px;
margin-left -(w / 2) | margin-left:-75px;
margin-top -(h / 2) | margin-top:-40px;
| }
{% endhighlight%}

从上面的代码中我们可以看出，CSS预处理器语言中的变量是值级别的重复使用，可以将相同的值定义成变量统一管理起来。
CSS预处理器语言中变量的特性适用于定义主题（也就是我们常说的换肤），我们可以将背景颜色、字体颜色、边框属性等常规样式统一定义，这样不同的主题只需要定义不同的变量文件就可以。

###作用域（Scope）
CSS预处理器语言中的变量和其他程序语言一样，可以实现值的复用，同样它也存在生命周期，也就是Scope（变量范围，开发人员习惯称之为作用域），简单点讲就是局部变量还是全局变量的概念，查找变量的顺序是先在局部定义中找，如果找不到，则查找上级定义，直至全局。下面我们通过一个简单的例子来解释这三款CSS预处理器的作用域使用。

####Sass的作用域
Sass中作用域在这三款预处理器是最差的，可以说在Sass中是不存在什么全局变量。具体来看下面的代码：
{% highlight JavaScript %}
/*Sass样式*/
$color: black;
.scoped {
$bg: blue;
$color: white;
color: $color;
background-color:$bg;
}
.unscoped {
color:$color;
}
{% endhighlight%}

先看转译出来的CSS样式：
{% highlight JavaScript %}
.scoped {
color:white;/*是白色*/
background-color:blue;
}
.unscoped {
color:white;/*白色（无全局变量概念）*/
}
{% endhighlight%}

示例明显的告诉我们，在Sass样式中定义变量，调用变量是没有全局变量一个概念存在，因此在Sass中定义了相同变量名时，在调用之时千万要多加小心，不然会给你的样式带来错误。

####LESS的作用域

LESS中的作用域和其他程序语言中的作用域非常的相同，他首先会查找局部定义的变量，如果没有找到，会像冒泡一样，一级一级往下查找，直到根为止，同样上面的例子，我们来看看他在LESS下所起的变化。
{% highlight JavaScript %}
/*LESS样式*/
@color: black;
.scoped {
@bg: blue;
@color: white;
color: @color;
background-color:@bg;
}
.unscoped {
color:@color;
}
{% endhighlight%}

转译出来的CSS样式：
{% highlight JavaScript %}
.scoped {
color:white;/*白色（调用了局部变量）*/
background-color:blue;
}
.unscoped {
color:black;/*黑色（调用了全局变量）*/
}
{% endhighlight%}

####Stylus的作用域
Stylus虽然起步比较晚，但其作用域的特性和LESS一样，可以支持全局变量和局变量。会向上冒泡查找，直到根为止。
###混合(Mixins)
Mixins是CSS预处理器中语言中最强大的特性，简单点来说，Mixins可以将一部分样式抽出，作为单独定义的模块，被很多选择器重复使用。平时你在写样式时肯定有碰到过，某段CSS样式经常要用到多个元素中，这样你就需要重复的写多次。在CSS预处理器语言中，你可以为这些公用的CSS样式定义一个Mixin，然后在你CSS需要使用这些样式的地方直接调用你定义好的Mixin。这是一个非常有用的特性，Mixins被当作一个公认的选择器，还可以在Mixins中定义变量或者默认参数。
####Sass的混合
Sass样式中声明Mixins时需要使用`@mixin`，然后后面紧跟Mixins的名，他也可以定义参数，同时可以给这个参数设置一个默认值，但参数名是使用"$"符号开始，而且和参数值之间需要使用冒号（`：`）分开。
在选择器调用定义好的Mixins需要使用`@include`，然后在其后紧跟你要调用的Mixins名。不过在Sass中还支持老的调用方法，就是使用加号`+`调用Mixins，在`+`后紧跟Mixins名。
一起来看个简单的例子，比如说在你的Sass样式中定义了一个名叫`error`的Mixin，这个`error`设置了一个参数`$borderWidth`，在没特别定义外，这个参数的默认值设置为`2px`：
{% highlight JavaScript %}
/*声明一个Mixin叫作“error”*/
@mixin error($borderWidth:2px){
border:$borderWidth solid #f00;
color: #f00;
}
/*调用error Mixins*/
.generic-error {
@include error();/*直接调用error mixins*/
}
.login-error {
@include error(5px);/*调用error mixins，并将参数$borderWidth的值重定义为5px*/
}
{% endhighlight%}

####LESS的混合
在LESS中，混合是指将定义好的`ClassA`中引入另一个已经定义的`Class`，就像在当前的`Class`中增加一个属性一样。
不过LESS样式中声明Mixins和Sass声明方法不一样，他更像CSS定义样式，在LESS可以将Mixins看成是一个类选择器，当然Mixins也可以设置参数，并给参数设置默认值。不过设置参数的变量名是使用`@`开头，同样参数和默认参数值之间需要使用冒号（`：`）分隔开。
正如Sass混合是的示例，同样在LESS样式中定义一个名叫`error`的Mixin，这个`error`设置了一个参数`@borderWidth`，在没有特别定义外，这个参数的默认值是`2px`：
{% highlight JavaScript %}
/*声明一个Mixin叫作“error”*/
.error(@borderWidth:2px){
border:@borderWidth solid #f00;
color: #f00;
}
/*调用error Mixins*/
.generic-error {
.error();/*直接调用error mixins*/
}
.login-error {
.error(5px);/*调用error mixins，并将参数@borderWidth的值重定义为5px*/
}
{% endhighlight%}

####Stylus的混合
Stylus中的混合和前两款CSS预处理器语言的混合略有不同，他可以不使用任何符号，就是直接声明Mixins名，然后在定义参数和默认值之间用等号（`=`）来连接。
{% highlight JavaScript %}
/*声明一个Mixin叫作“error”*/
error(borderWidth=2px){
border:borderWidth solid #f00;
color: #f00;
}
/*调用error Mixins*/
.generic-error {
error();/*直接调用error mixins*/
}
.login-error {
error(5px);/*调用error mixins，并将参数$borderWidth的值重定义为5px*/
}
{% endhighlight%}

三个示例都将会转译成相同的CSS代码：
{% highlight JavaScript %}
.generic-error {
border: 2px solid #f00;
color:#f00;
}
.login-error {
border:5px solid #f00;
color: #f00;
}
{% endhighlight%}

###嵌套（Nesting）
CSS预处理器语言中的嵌套指的是在一个选择器中嵌套另一个选择器来实现继承，从而减少代码量，并且增加了代码的可读性。比如说，我们在CSS中多个元素有一个相同的父元素，那么写样式会变得很乏味，我们需要一遍一遍的在每个元素前写这个父元素，除非给特定的元素添加类名`class`或者`ID`。
{% highlight JavaScript %}
section {
margin:10px;
}
section nav {
height:25px;
}
section nav a {
color: #0982c1;
}
section nav a:hover {
text-decoration: underline;
}
{% endhighlight%}

相反，使用CSS预处理器语言的嵌套特性，我们可以在父元素的大括号（`｛｝`）里写这些元素。同时可以使用`&amp;`符号来引用父选择器。对于Sass、LESS和Stylus这三款CSS预处理器语言的嵌套选择器来说，他们都具有相同的语法：
{% highlight JavaScript %}
section {
margin:10px;
nav {
height:25px;
a {
color:#0982c1;
&:hover {
text-decoration:underline;
}
}
}
}
{% endhighlight%}

上面的预处理器转译出来的CSS代码和我们开始展示的CSS代码是相同的，非常的方便吧！
###继承（Inheritance）
对于熟悉CSS的同学来说，对于属性的继承并不陌生。平时在写CSS样式常碰到多个元素应用相同的样式时，我们在CSS中通常都是这样写：
{% highlight JavaScript %}
p,ul,ol{/*样式写在这里*/}
{% endhighlight%}

这样做非常的好，但往往我们需要给单独元素添加另外的样式，这个时候我们就需要把其中选择器单独出来写样式，如此一来我们维护样式就相当的麻烦。为了应对这个问题，CSS预处理器语言可以从一个选择继承另个选择器下的所有样式。
####Sass和Stylus的继承
Sass和Stylus的继承是把一个选择器的所有样式继承到另个选择器上。在继承另个选择器的样式时需要使用`@extend`开始，后面紧跟被继承的选择器：
{% highlight JavaScript %}
.block {
margin: 10px 5px;
padding: 2px;
}
p {
@extend .block;/*继承.block选择器下所有样式*/
border: 1px solid #eee;
}
ul,ol {
@extend .block; /*继承.block选择器下所有样式*/
color: #333;
text-transform: uppercase;
}
{% endhighlight%}

上面的代码转译成CSS：
{% highlight JavaScript %}
.block,p,ul,ol {
margin: 10px 5px;
padding:2px;
}
p {
border: 1px solid #eee
}
ul,ol {
color:#333;
text-transform:uppercase;
}
{% endhighlight%}

####LESS的继承
LESS支持的继承和Sass与Stylus不一样，他不是在选择器上继承，而是将Mixins中的样式嵌套到每个选择器里面。这种方法的缺点就是在每个选择器中会有重复的样式产生。
{% highlight JavaScript %}
.block {
margin: 10px 5px;
padding: 2px;
}
p {
.block;/*继承.block选择器下所有样式*/
border: 1px solid #eee;
}
ul,ol {
.block; /*继承.block选择器下所有样式*/
color: #333;
text-transform: uppercase;
}
{% endhighlight%}

转译出来的CSS代码：
{% highlight JavaScript %}
.block {
margin: 10px 5px;
padding:2px;
}
p {
margin: 10px 5px;
padding:2px;
border: 1px solid #eee
}
ul,ol {
margin: 10px 5px;
padding:2px;
color:#333;
text-transform:uppercase;
}
{% endhighlight%}

正如所看到的，上面的代码`.block`的样式将会被插入到相应的你要继承的选择器中，但需要注意的是优先级的问题。
###运算符（Operations）
CSS预处理器语言还具有运算的特性，其简单的讲，就是对数值型的Value（如：数字、颜色、变量等）进行加减乘除四则运算。这样的特性在CSS样式中是想都不敢想的，但在CSS预处理器语言中对样式做一些运算一点问题都没有了，例如：
{% highlight JavaScript %}
@base_margin: 10px;
@double_margin: @base_margin * 2;
@full_page: 960px;
@half_page: @full_page / 2;
@quarter_page: (@full_page / 2) / 2;
{% endhighlight %}

上面代码是LESS的运算示例，声明一下，在取得`@quarter_page`变量时，我们可以直接除以`4`，但是在这里，我们只是想演示一下圆括号组成的"运算顺序"（这个运算顺序小学生也知道）。在复合型运算中，小括号也是很有必要的，例如：
{% highlight JavaScript %}
border: (@width / 2) solid #000;
{% endhighlight %}

Sass在数字运算上要比LESS更专业，他可以直接换算单位了。Sass可以处理无法识别的度量单位，并将其输出。这个特性很明显是一个对未来的尝试——证明W3C作出的一些改变。
Stylus的运算是三款预处理器语言中最强大的一款，他拥有其他程序语言一样的运算功能，简单点的加减乘除，复杂的有关系运算、逻辑运算等。受限于篇幅，感兴趣的同学可以到官网上仔细阅读。
###颜色函数
颜色函数是CSS预处理器语言中内置的颜色函数功能，这些功能可以对颜色进行处理，例如颜色的变亮、变暗、饱和度控制、色相控制，渐变颜色等处理十分的方便。
####Sass颜色函数
{% highlight JavaScript %}
lighten($color, 10%); /* 返回的颜色在$color基础上变亮10% */
darken($color, 10%); /* 返回的颜色在$color基础上变暗10% */
saturate($color, 10%); /* 返回的颜色在$color基础上饱和度增加10% */
desaturate($color, 10%); /* 返回的颜色在$color基础上饱和度减少10% */
grayscale($color); /* 返回$color的灰度色*/
complement($color); /* 返回$color的补色 */
invert($color); /* 返回$color的反相色 */
mix($color1, $color2, 50%); /* $color1 和 $color2 的 50% 混合色*/
{% endhighlight %}

这只是Sass中颜色函数的一个简单列表，更多详细的介绍可以阅读<a href="http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html" target="_blank" rel="external">Sass文档</a>。
颜色函数可以运用到任何一个元素上，只要其有颜色的属性，下面是一个简单的例子：
{% highlight JavaScript %}
$color: #0982C1;
h1 {
background: $color;
border: 3px solid darken($color, 50%);/*边框颜色在$color的基础上变暗50%*/
}
{% endhighlight %}

####LESS颜色函数
{% highlight JavaScript %}
lighten(@color, 10%); /* 返回的颜色在@color基础上变亮10% */
darken(@color, 10%); /* 返回的颜色在@color基础上变暗10%*/
saturate(@color, 10%); /* 返回的颜色在@color基础上饱和度增加10% */
desaturate(@color, 10%); /* 返回的颜色在@color基础上饱和度降低10%*/
spin(@color, 10); /* 返回的颜色在@color基础上色调增加10 */
spin(@color, -10); /* 返回的颜色在@color基础上色调减少10 */
mix(@color1, @color2); /* 返回的颜色是@color1和@color2两者的混合色 */
{% endhighlight %}

LESS的完整颜色函数功能，请阅读<a href="http://lesscss.org/#-color-functions" target="_blank" rel="external">LESS文档</a>。
下面是LESS中如何使用一个颜色函数的简单例子：
{% highlight JavaScript %}
@color: #0982C1;
h1 {
background: @color;
border: 3px solid darken(@color, 50%);
}
{% endhighlight %}

####Stylus的颜色函数
{% highlight JavaScript %}
lighten(color, 10%); /* 返回的颜色在'color'基础上变亮10% */
darken(color, 10%); /* 返回的颜色在'color'基础上变暗10% */
saturate(color, 10%); /* 返回的颜色在'color'基础上饱和度增加10% */
desaturate(color, 10%); /* 返回的颜色在'color'基础上饱和度降低10% */
{% endhighlight %}

有关于Stylus的颜色函数介绍，请阅读<a href="http://learnboost.github.com/stylus/docs/bifs.html" target="_blank" rel="external">Stylus文档</a>。
下面是Stylus颜色函数的一个简单实例：
{% highlight JavaScript %}
color = #0982C1
h1
background color
border 3px solid darken(color, 50%)
{% endhighlight %}

从上面展示的部分颜色函数可以告诉我们，Sass、LESS和Stylus都具有强大的颜色函数功能，功能特性上都大同小异，只是在使用方法上略有不同。而且他们都具有相同的一个目的，就是方便操作样式中的颜色值。
###导入（Import）
在CSS中，并不喜欢用`@import`来导入样式，因为这样的做法会增加http的请求。但是在CSS预处理器中的导入(`@import`)规则和CSS的有所不同，它只是在语义上导入不同的文件，但最终结果是生成一个CSS文件。如果你是通过`@import "file.css"`导入`file.css`样式文件，那效果跟普通CSS导入样式文件一样。
<strong>注意：</strong>导入文件中定义了变量、混合等信息也将会被引入到主样式文件中，因此需要避免他们的相互冲突。
Sass、LESS和Stylus三款CSS预处理器语言，导入样式的方法都是一样：
被导入文件的样式：
{% highlight JavaScript %}
/* file.{type} */
body {
background: #EEE;
}
{% endhighlight %}

需要导入样式的文件：
{% highlight JavaScript %}
@import "reset.css";
@import "file.{type}";
p {
background: #0982C1;
}
{% endhighlight %}

转译出来的CSS代码：
{% highlight JavaScript %}
@import "reset.css";
body {
background: #EEE;
}
p {
background: #0982C1;
}
{% endhighlight %}

###注释（Comment）
CSS预处理器语言中的注释是比较基础的一部分，这三款预处理器语言除了具有标准的CSS注释之外，还具有单行注释，只不过单行注释不会被转译出来。
####Sass、LESS和Stylus的多行注释
多行注释和CSS的标准注释，他们可以输出到CSS样式中，但在Stylus转译时，只有在"compress"选项未启用的时候才会被输出来。
{% highlight JavaScript %}
/*
*我是注释
*/
body
padding 5px
{% endhighlight %}

####Sass、LESS和Stylus的单行注释
单行注释跟JavaScript语言中的注释一样，使用双斜杠（`//`），但单行注释不会输出到CSS中。
{% highlight JavaScript %}
//我是注释
@mainColor：#369;//定义主体颜色
{% endhighlight %}

在Stylus中除了以上两种注释之外，他还有一种注释，叫作多行缓冲注释。这种注释跟多行注释类似，不同之处在于始的时候，这里是`/*！`。这个相当于告诉Stylus压缩的时候这段无视直接输出。
{% highlight JavaScript %}
/*!
*给定数值合体
*/
add(a, b)
a + b
{% endhighlight %}

上面从九个常用的特性对Sass、LESS和Stylus三款CSS预处理器语言的使用做了对比，在某些特性上可以说是一模一样，而有一些特性上功能其实一样，只是在部分书写规则上有所不同。当然有些特性是完全不同。在这里几是从使用方法上做为一个比较，主要目的是让大家经过对比之后，使自己选择哪一款CSS预处理器语言有所方向和帮助。
##CSS预处理器的高级应用
我们知道，Sass、LESS和Stylus都具有变量、混合、嵌套、函数和作用域等特性，但这些特性都是一些普通的特性。其实除了这些特性之外，他们还拥有一些很有趣的特性有助于我们的开发，例如条件语句、循环语句等。接下来，我们同样从使用上来对比一下这三款CSS预处理器语言在这方面应用又有何不同。
###条件语句
说到编程，对于编程基本控制流，大家并不会感到陌生，除了循环就是条件了。条件提供了语言的可控制，否则就是纯粹的静态语言。提供的条件有导入、混合、函数以及更多。在编程语言中常见的条件语句：
{% highlight JavaScript %}
if/else if/else
{% endhighlight %}

`if`表达式满足（`true`）的时候执行后面语然块，否则，继续后面的`else if`或`else`。
在这三款CSS3预处理器语言中都具有这种思想，只不过LESS中表达的方式略有不现，接下来我们依次看看他们具体如何使用。
####Sass的条件语句
Sass样式中的条件语句和其他编程语言的条件语句非常相似，在样式中可以使用`@if`来进行判断：
{% highlight JavaScript %}
p {
@if 1 + 1 == 2 { border: 1px solid; }
@if 5 < 3 { border: 2px dotted; }
@if null { border: 3px double; }
}
{% endhighlight %}

编译出来的CSS：
{% highlight JavaScript %}
p {
border: 1px solid; 
}
{% endhighlight %}

在Sass中条件语句还可以和`@else if`、`@else`配套使用：
{% highlight JavaScript %}
$type: monster;
p {
@if $type == ocean {
color: blue;
} @else if $type == matador {
color: red;
} @else if $type == monster {
color: green;
} @else {
color: black;
}
}
{% endhighlight %}

转译出来的CSS：
{% highlight JavaScript %}
p {color:green;}
{% endhighlight %}

<h4 id="Stylus的条件语句">Stylus的条件语句</h4>
Stylus的条件语句的使用和其他编程的条件语句使用基本类似，不同的是他可以在样式去省略大括号（`｛｝`）：
{% highlight JavaScript %}
box(x, y, margin = false)
padding y x
if margin
margin y x
body
box(5px, 10px, true)
{% endhighlight %}

Stylus同样可以和`else if`、`else`配套使用：
{% highlight JavaScript %}
box(x, y, margin-only = false)
if margin-only
margin y x
else
padding y x
{% endhighlight %}
Stylus除了这种简单的条件语句应用之外，他还支持后缀条件语句。这就意味着`if`和`unless`（熟悉Ruby程序语言的用户应该都知道`unless`条件，其基本上与`if`相反，本质上是`(!(expr))`）当作操作符；当右边表达式为真的时候执行左边的操作对象。
例如，我们定义了`negative()`来执行一些基本的检查。下面我们使用块式条件：
{% highlight JavaScript %}
negative(n)
unless n is a 'unit'
error('无效数值')
if n < 0
yes
else
no
{% endhighlight %}

接下来，我们利用后缀条件让我们的方法简洁：
{% highlight JavaScript %}
negative(n)
error('无效数值') unless n is a 'unit'
return yes if n < 0
no
{% endhighlight %}

当然，我们可以更进一步。如这个`n &lt; 0 ? yes : no`可以用布尔代替：`n &lt; 0`。后缀条件适合于大多数的单行语句。如`@import,@charset`混合书写等。当然，下面所示的属性也是可以的：
{% highlight JavaScript %}
pad(types = margin padding, n = 5px)
padding unit(n, px) if padding in types
margin unit(n, px) if margin in types

body
pad()

body
pad(margin)

body
apply-mixins = true
pad(padding, 10) if apply-mixins
{% endhighlight %}

上面代码转译出来的CSS：
{% highlight JavaScript %}
body {
padding: 5px;
margin: 5px;
}
body {
margin: 5px;
}
body {
padding: 10px;
}
{% endhighlight %}

<h4 id="LESS的条件语句">LESS的条件语句</h4>
LESS的条件语句使用有些另类，他不是我们常见的关键词`if`和`else if`之类，而其实现方式是利用关键词`when`。
{% highlight JavaScript %}
.mixin (@a) when (@a >= 10) { 
background-color: black;
}
.mixin (@a) when (@a < 10) {
background-color: white;
}
.class1 { .mixin(12) }
.class2 { .mixin(6) }
{% endhighlight %}

转译出来的CSS：
{% highlight JavaScript %}
.class1 {
background-color: black; 
}
.class2 {
background-color: white; 
}
{% endhighlight %}

利用`When`以及`&lt;`、`&gt;`、`=`、`&lt;=`、`&gt;=`是十分简单和方便的。LESS并没有停留在这里，而且提供了很多类型检查函数来辅助条件表达式，例如：`iscolor`、`isnumber`、`isstring`、`iskeyword`、`isurl`等等。
{% highlight JavaScript %}
.mixin (@a) when (iscolor(@a)) {
background-color: black;
}
.mixin (@a) when (isnumber(@a)) { 
background-color: white;
}
.class1 { .mixin(red) }
.class2 { .mixin(6) }
{% endhighlight %}

转译出来的CSS
{% highlight JavaScript %}
.class1 {
background-color: black; 
}
.class2 {
background-color: white; 
}
{% endhighlight %}

另外，LESS的条件表达式同样支持`AND`和`OR`以及`NOT`来组合条件表达式，这样可以组织成更为强大的条件表达式。需要特别指出的一点是，`OR`在LESS中并不是`or`关键词，而是用来表示`or`的逻辑关系。
{% highlight JavaScript %}
.smaller (@a, @b) when (@a > @b) {
background-color: black;
}
.math (@a) when (@a > 10) and (@a < 20) { 
background-color: red;
}
.math (@a) when (@a < 10)，(@a > 20) {
background-color: blue;
}
.math (@a) when not (@a = 10) {
background-color: yellow;
}
.math (@a) when (@a = 10) {
background-color: green;
}

.testSmall {.smaller(30, 10) }
.testMath1 {.math(15)}
.testMath2 {.math(7)}
.testMath3 {.math(10)}
{% endhighlight %}


转译出来的CSS
{% highlight JavaScript %}
.testSmall {
background-color: black;
}
.testMath1 {
background-color: red;
background-color: yellow; 
}
.testMath2 {
background-color: blue;
background-color: yellow; 
}
.testMath3 {
background-color: green; 
}
{% endhighlight %}


###循环语句

Sass和Stylus还支持`for`循环语句，而LESS并没支持`for`循环语句，但值得庆幸的是，在LESS中可以使用`When`来模拟出`for`循环的特性。

####Sass的循环语句

Sass中使用`for`循环语句需要使用`@for`，并且配合`from`和`through`一起使用，其基本语法：
{% highlight JavaScript %}
@for $var from <start> through <end> {语句块}
{% endhighlight %}

我们来看一个简单的例子：

{% highlight JavaScript %}
@for $i from 1 through 3 {
.item-#{$i} { width: 2em * $i; }
}
{% endhighlight %}

转译出来的CSS代码：
{% highlight JavaScript %}
.item-1 { width: 2em; }
.item-2 { width: 4em; }
.item-3 { width: 6em; }
{% endhighlight %}

在Sass中循环语句除了`@for`语句之外，还有`@each`语句和`@while`语句:
`@each`循环语法：

{% highlight JavaScript %}
@each $var in <list>{语句块}
{% endhighlight %}

来看个简单的实例：

{% highlight JavaScript %}
@each $animal in puma, sea-slug, egret, salamander {
.#{$animal}-icon {
background-image: url('/images/#{$animal}.png');
}
}
{% endhighlight %}

转译出来的CSS

{% highlight JavaScript %}
.puma-icon { background-image: url('/images/puma.png'); }
.sea-slug-icon { background-image: url('/images/sea-slug.png'); }
.egret-icon { background-image: url('/images/egret.png'); }
.salamander-icon { background-image: url('/images/salamander.png')    }
{% endhighlight %}

`@while`循环使用和其他编程语言类似：

{% highlight JavaScript %}
$i: 6;
@while $i > 0 {
.item-#{$i} { width: 2em * $i; }
$i: $i - 2;
}
{% endhighlight %}

转译出来的CSS

{% highlight JavaScript %}
.item-6 { width: 12em; }
.item-4 { width: 8em; }
.item-2 { width: 4em; }
{% endhighlight %}

####Stylus的循环语句

在Stylus样式中通过`for/in`对表达式进行循环，形式如下：

{% highlight JavaScript %}
for <val-name> [, <key-name>] in <expression>
{% endhighlight %}

例如：

{% highlight JavaScript %}
body
for num in 1 2 3
foo num
{% endhighlight %}

转译出来CSS

{% highlight JavaScript %}
body {
foo: 1;
foo: 2;
foo: 3;
}
{% endhighlight %}

下面这个例子演示了如何使用`&lt;key-name&gt;`：

{% highlight JavaScript %}
body
fonts = Impact Arial sans-serif
for font, i in fonts
foo i font
{% endhighlight %}

转译出来的CSS

{% highlight JavaScript %}
body {
foo: 0 Impact;
foo: 1 Arial;
foo: 2 sans-serif;
}
{% endhighlight %}

####LESS的循环语句

在LESS语言中并没有现在的循环语句，可是像其条件语句一样，通过`when`来模拟出他的循环功能。

{% highlight JavaScript %}
.loopingClass (@index) when (@index > 0) {
.myclass {
z-index: @index;
}
// 递归
.loopingClass(@index - 1);
}
// 停止循环
.loopingClass (0) {}
// 输出
.loopingClass (3);
{% endhighlight %}

相比之下，Sass和Stylus对条件语句和循环语句的处理要比LESS语言强大。因为他们具有真正的语言处理能力。
综上所述，我们对Sass、LESS和Stylus做一个简单的对比总结：

<ul>
<li>三者都是开源项目；</li>
<li>Sass诞生是最早也是最成熟的CSS预处理器，有Ruby社区和Compass支持；Stylus早期服务器Node JS项目，在该社区得到一定支持者；LESS出现于2009年，支持者远超于Ruby和Node JS社区；</li>
<li>Sass和LESS语法较为严谨、严密，而Stylus语法相对散漫，其中LESS学习起来更快一些，因为他更像CSS的标准；</li>
<li>Sass和LESS相互影响较大，其中Sass受LESS影响，已经进化到了全面兼容CSS的SCSS；</li>
<li>Sass和LESS都有第三方工具提供转译，特别是Sass和Compass是绝配；</li>
<li>Sass、LESS和Stylus都具有变量、作用域、混合、嵌套、继承、运算符、颜色函数、导入和注释等基本特性，而且以"变量"、"混合"、"嵌套"、"继承"和"颜色函数"称为五大基本特性，各自特性实现功能基本相似，只是使用规则上有所不同；</li>
<li>Sass和Stylus具有类似于语言处理的能力，比如说条件语句、循环语句等，而LESS需要通过When等关键词模拟这些功能，在这一方面略逊一层；</li>
</ul>

上面是CSS预处理器语言中的Sass、LESS和Stylus在某些方面的对比，相对于CSS来说，上面都是CSS预处理器语言的优点？那么他有没有缺点呢？

万物都有阴阳两极，有圆就有缺，CSS预处理器语言也逃避不了这个宿命。个人感觉CSS预处理器语言那是程序员的玩具，想通过编程的方式跨界解决CSS的问题。可以CSS应该面临的问题一个也少不了，只是增加了一个编译过程而已，简单来说CSS预处理器语言较CSS玩法变得更高级了，但同时降低了自己对最终代码的控制力。更致命的是提高了门槛，首先是上手门槛，其次是维护门槛，再来是团队整体水平和规范的门槛。这也造成了初学学习成本的昂贵。

全文从Sass、LESS和Stylus的背景介绍、安装步骤、转译方法、语法规则、基本特性和高级应用六个方面对目前世面上流行的三款CSS预处理器语法做了一个对比。他们各有各的优势，也各有各的劣势。现在我们又回到当初提的问题"我应该选择哪种CSS预处理器？"。不知道你是否可以做出选择？

转载自：<a href="http://www.cn-sass.com/%E6%95%99%E7%A8%8B/css-preprocessor-sass-vs-less-stylus.html" target="_blank" rel="external">http://www.cn-sass.com/%E6%95%99%E7%A8%8B/css-preprocessor-sass-vs-less-stylus.html</a>