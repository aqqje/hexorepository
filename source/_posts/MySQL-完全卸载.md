---
title: MySQL 完全卸载
date: 2018-05-30 15:01:41
tags: mysql
---

MySQL 完全卸载 note

<!-- more -->

## 卸载程序

卸载所有的 MySQL 组件程序

## 删除 MySQL 安装目录

C:\Program Files\MySQL
C:\ProgramData\MySQL
C:\Users\zhenghaishu\AppData\Roaming\MySQL
C:\Users\zhenghaishu\AppData\Local\Temp\MySQL Workbench
C:\Users\zhenghaishu\AppData\Roaming\Oracle\MySQL Notifier

## 删除注册表

1. 进入注册表 

使用 winq键 + r 输入 regedit 命令 打开注册表

2. 删除注册表

HKEY_LOCAL_MACHINE/SYSTEM/ControlSet001/Services/Eventlog/Application/MySQL
HKEY_LOCAL_MACHINE/SYSTEM/ControlSet002/Services/Eventlog/Application/MySQL
HKEY_LOCAL_MACHINE/SYSTEM/CurrentControlSet/Services/Eventlog/Application/MySQL

<a href="http://blog.itpub.net/29485627/viewspace-1278323/">Win7完全卸载MySQL的步骤</a>
 