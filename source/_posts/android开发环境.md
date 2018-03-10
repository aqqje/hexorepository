---
title: android开发环境
date: 2018-03-10 19:58:26
tags: android
categories: android
---
{% note info %}
{% cq %}
android开发主要有两种开发软件使用一是Eclipse二是Android studio,本文是从这两个方面来搭建环境.
Eclipse:jdk + Eclipse + ADT + Android SDK
Android studio: jdk + Android Studio + Android SDK
<span id="inline-blue">注意：</span>本文的搭建过程不需要vpn
{% endcq %}
{% endnote %}
<!-- more -->
## Eclipse<span id="inline-yellow">手动配置</span><br/>

{% note danger %}准备{% endnote %}<br/>

- jdk下载： <a id="download" href="http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html"><i class="fa fa-download"></i><span>Download Now</span></a><br/>

jdk的安装以及配置过程在这里就不做过多的讲述了。<br/>


- Eclipse下载：<a id="download" href="https://www.eclipse.org/downloads/"><i id="fa fa-download"></i><span>Download Now</span></a><br/>

jdk的安装过程在这里也略过了。


- ADT下载：<a id="download" href="https://dl.google.com/android/ADT-23.0.7.zip"><i id="fa fa-download"></i><span>Download Now</span></a><br/>

在这里点击默认下载：https://dl.google.com/android/ADT-23.0.7.zip<br/>

<span id="inline-blue">注意</span>：如果需要其他版本：https://dl.google.com/android/ADT-xx.x.x.zip把xx.x.x改为对应的版本<br/>

- Android SDK下载：<a id="download" href="http://tools.android-studio.org/index.php/sdk"><i id="fa fa-download"></i><span>Download Now</span></a><br/>

{% note danger %}配置{% endnote %}

- 配置ADT：<br/>

找到已下载好的ADT：进行如图配置<br/>

![](https://github.com/aqqje/Personal-repository/raw/master/images/android1.png "android1")<br/>

点击确定,等待安装完成即可！<br/>

- 配置Android SDK：<br/>


>环境变量：
	- 新增系统变量：变量名：ANDROID_SDK_HOME变量值：Android SDK的根目录，如下图：
	
![](https://github.com/aqqje/Personal-repository/raw/master/images/android2.png "android2")<br/>

    - 系统变量Path中增加：%ANDROID_SDK_HOME%\platform-tools

以上两种配置完成在DOS中输入adb就会出现如下图：<br/>

![](https://github.com/aqqje/Personal-repository/raw/master/images/android3.png "android3")<br/><br/>

>设置端口：打开SDK Manager.exe,设置如下图：<br/>

![](https://github.com/aqqje/Personal-repository/raw/master/images/android4.png "android4")<br/><br/>

端口设置：完成的之后随便选择一个Android版本下载即可<br/>

- 创建项目中的问题：如何可显化界面无法显示，原因是ADT与SDK版本不相匹配,<br/>

- 成功效果图：<br/>

![](https://github.com/aqqje/Personal-repository/raw/master/images/android5.png "android5")<br/>

## Eclipse<span id="inline-yellow">集成开发包<br/>

</span><a id="download" href="http://adt.android-studio.org/"><i id="fa fa-download"></i><span>Download Now</span></a><br/>

- 下载集成开发包,解压即可用！这里不做过多的讲述。<br/>

## Android studio<br/>

{% note danger %}准备{% endnote %}<br/>

- jdk下载：<a id="download" href="http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html"><i class="fa fa-download"></i><span>Download Now</span></a><br/>

jdk的安装以及配置过程在这里就不做过多的讲述了。<br/>

- Android Studio下载：<a id="download" href="http://www.android-studio.org/"><i class="fa fa-download"></i><span>Download Now</span></a>

这里作者用的是绿色版，解压即可使用。<br/>

- Android SDK：同上不做解释。<br/>

{% note danger %}配置{% endnote %}<br/>

创建项目中的问题：<br/>

- 问题一：如下图<br/>

![](https://github.com/aqqje/Personal-repository/raw/master/images/android6.png "android6")<br/>

解决方法：<br/>

文件 --> 配置 --> 构建,执行,部署 --> Gradle  如图设置Use local gradle distribution 确定即可<br/>

![](https://github.com/aqqje/Personal-repository/raw/master/images/android7.png "android7")<br/>

<span id="inline-blue">注意</span>：Global home的路径在你的Android Studio 的根目录下。例如:D:\Android-Studio\soruce\android-studio-ide-171.4443003-windows32\android-studio\gradle\gradle-4.1

- 问题二：如下图：<br/>

![](https://github.com/aqqje/Personal-repository/raw/master/images/android9.png "android9")<br/>

解决方法：<br/>

找到项目 --> app --> build.gradle 修改如下图：<br/>

![](https://github.com/aqqje/Personal-repository/raw/master/images/android10.png "android10")<br/>

- 成功效果图：<br/>

![](https://github.com/aqqje/Personal-repository/raw/master/images/android8.png "android8")<br/>
