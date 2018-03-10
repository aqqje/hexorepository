---
title: github+hexo搭建博客
date: 2017-11-19 21:48:43
categories:
	- hexo
tags:
	- hexo
---
### 通过几天的努力终于把hexo+github形式的博客搭建好了！
![hexo](https://github.com/aqqje/images/raw/master/images/hexo.jpg "hexo")
<!--more-->
正文：
hexo搭建博客还是很容易的！更换主题也很简单！<br/>
## 配置环境
- 安装Node(必须)一路安装即可<br/>
作用：用于生成静态页面。<br/>
Node.js官网[https://nodejs.org/en/](https://nodejs.org/en/)<br/>
- 安装Git(必须)大部分按默认安装，一路安装即可<br/>
作用：把本地的hexo内容提交到github上去(博客备份地址不是你的网站地址)<br>
- 验证软件正确安装<br/>
	git --version<br/>
	node -v<br/>
	npm -v<br/>

## 申请GitHub<br/>
- 点击->GitHub进入官网注册帐号<br/>
- 分别输入用户名、邮箱、密码，然后点击注册<br/>
- "New repository"新建一个版本库<br/>
- 输入Repository name:yourname.github.io(yourname与你的注册用户名一致,这个就是你博客的域名了) <br/>
## 安装Hexo<br/>
- 因为有“墙”，安装hexo为了避免出现类似情况，使用淘宝NPM镜像,输入以下命令等待安装完成<br/>
	$ npm install -g cnpm --registry=https://registry.npm.taobao.org<br/>

- 使用淘宝NPM安装Hexo<br/>
	cnpm install -g hexo-cli<br/>
- 与原先的npm完全一样，只是命令改为cnpm,一样等待hexo安装完成<br/>
- 继续输入以下命令安装<br/>
	cnpm install hexo --save<br/>
- 验证hexo是否安装成功<br/>
	hexo -v<br/>


## 本地运行hexo
- 新建一个文件夹做为你的博客的文件<br/>
- 进入你的博客文件，使用 Git Bash 进行初始化hexo<br/>
	hexo init<br/>

- 安装生成器<br/>
	cnpm install<br/>
- 运行hexo,以后要在本地运行博客只要输入该命令即可<br/>
- 打开浏览器，输入localhost:4000,就可以在本地看到你的个人博客了
- 停止运行(按住Ctrl+C键即可停止) 
## 管理博客
- 配置信息 
打开您博客根目录下的_config.yml文件，进行配置

{％blockquote％}
博客名称
title: 我的博客
副标题
subtitle: 一天进步一点
简介
description: 记录生活点滴
博客作者
author: John Doe
博客语言
language: zh-CN
时区
timezone:
博客地址,与申请的GitHub一致
url: http://elfwalk.github.io
root: /
博客链接格式
permalink: :year/:month/:day/:title/
permalink_defaults:
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: true
  tab_replace:
default_category: uncategorized
category_map:
tag_map:
日期格式
date_format: YYYY-MM-DD
time_format: HH:mm:ss
分页，每页文章数量
per_page: 10
pagination_dir: page
博客主题
theme: landscape
发布设置
deploy: 
  type: git
  elfwalk改为你的github用户名
  repository: https://github.com/elfwalk/elfwalk.github.io.git
  branch: master
{％endblockquote％} 

- 写一篇文章<br/>
输入创建文章命令，生成一个md文件(/blog/source/_posts/)
	hexo new "hello"
- 用编辑器打开hello.md文件,编写完后保存
{％blockquote％}
title: hello
date: 2015-07-01 22:37:23
categories:
  - 日志
  - 二级目录
tags:
  - hello
---

摘要:
<!--more-->
正文:
{％endblockquote％}

## 发布博客
- 设置git身份信息
	
	git config --global user.name "你的用户名"
	git config --global user.email "你的邮箱"
- 安装hexo git插件
	cnpm install hexo-deployer-git --save
-  发布更新博客
	hexo d -g

- 发布成功后，访问yourname.github.io看下成果

参考：[http://hifor.net/2015/07/01/%E9%9B%B6%E5%9F%BA%E7%A1%80%E5%85%8D%E8%B4%B9%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2-hexo-github](http://hifor.net/2015/07/01/%E9%9B%B6%E5%9F%BA%E7%A1%80%E5%85%8D%E8%B4%B9%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2-hexo-github)<br/>
作者：梦不若星辰
链接：https://aqqje.github.io
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



