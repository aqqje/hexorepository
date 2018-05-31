---
title: win7 应用程序图标显示异常
date: 2018-05-31 16:35:28
tags: 生活
---

描述：有时不知道自己做了什么，突然有一两个应用图标显示总是不正常，让人十分不开心！

<!-- more -->

## 如何解决

win键 + R  -->  cmd  

-->   输入以下内容：

taskkill /im explorer.exe /f 
cd /d %userprofile%\appdata\local 
del iconcache.db /a 
start explorer.exe 
exit 

## 成功
