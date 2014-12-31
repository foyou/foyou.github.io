---
layout:        post
title:         Xamarin 安装步骤
description:   "Xamarin 安装步骤"
tags: [Xamarin]
image:
  background: body_bg.gif
comments: true
share: true
---

VS2015虽然集成了手机端开发,但是毕竟还没来不是么?现在有想通过VS开发手机应用的,可以暂时用Xamarin代替下.以下是本人安装Xamarin的一些总结.

##Xamarin 安装步骤

###1.下载并解压吾乐吧软件站提供的“Mono for Android 离线包”

        安装：jdk-6u45-windows-i586.exe （就算你是64位系统，也要安装i586版）

   C:\Program Files (x86)\Java\jdk1.6.0_39\ JDK安装路径

   C:\Program Files (x86)\Java\jre6\        JAVA安装路径

    
<!--more-->

###2.win7下JDK环境变量设置方法

首先需要到官网上下载JDK这款软件，本人下载的是jdk-6u26-windows-i586版本，安装完成显示jdk1.6.0_26。

其次选择安装路径。本人的安装路径是C:\Program Files\Java\jdk1.6.0_26。

安装完成后需要设置环境变量从而使编译器正常使用。右击“计算机”à选择“属性”à选择左边“高级系统设置”à选择上面“高级”选项卡à点击右下角“环境变量”按钮。

接下来弹出的对话框会出现用户变量和系统变量。用户变量对当前登录账户有效，系统变量对所有用户都有效，读者可根据需要设置。

在系统变量里点击新建，变量名填写JAVA_HOME，变量值填写JDK的安装路径，在这里就填写“C:\Program Files\Java\jdk1.6.0_26”了.这里要注意路径 因为版本可能不同.最好去安装位置看下你的路径 

系统变量里点击新建变量名填写CLASSPATH，变量值填写“.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar”。注意不要忘记前面的点和中间的分号。 

在系统变量里找到Path变量，这是系统自带的，不用新建。双击Path，由于原来的变量值已经存在，故应在已有的变量后加上“;%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin”。注意前面的分号。 

至此，应有的环境变量已经配置完毕。  

###3.下载并解压吾乐吧提供的“Android SDK 本地离线完整版”：

把压缩包里的 android-sdk 解压到这个路径（路径不要含有中文）

D:\Program Files\Android\android-sdk

然后安装：installer_r22.0.4-windows.exe （程序也安装到上面这个路径）

###4.设置系统环境变量：

右键我的电脑——属性——高级系统设置——高级——环境变量——系统变量里，找到 Path，双击进行修改，在最前面增加：

D:\Program Files\Android\android-sdk\tools;

（注意末尾必须有 ; 英文分号，如果 Path 里面已经有这个值，就不需要添加）

###5.然后按照顺序，逐个安装：

1）gtk sharp

2）xamarin studio

3）Xamarin.VisualStudio_Setup 

4）全部弄完之后，最好重启电脑，应该就没问题了。剩下的你都懂，就不说了……


首次安装的话，设置这一个地方：

Mono For Android 无法启动模拟器，提示：Android SDK not found 解决方法 http://www.wuleba.com/?p=17227
