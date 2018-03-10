---
title: github pull 冲突解决方法
date: 2017-12-19 21:21:52
comments: ture
categories:
	- github
tags:
	- git pull问题
---


多人协作git pull 问题解决

<!-- more -->

## 1.首先将本地修改存储

- 存储修改

```
git stash
```

- 查看保存信息

```
git stash list

```

其中stash@{0}是保存的标记

##　2.pull远程仓库

```
git pull
```

## 还原暂存内容


```
git stash pop stash@{0}

```

## 手动解决文件冲突部分


其中Updated upstream 和=====之间的内容就是pull下来的内容，====和stashed changes之间的内容就是本地修改的内容


## 正常push到远程仓库

```
git push origin master

```


--------------------------------------------------------------

作者：梦不若星辰
链接：https://aqqje.github.io
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
