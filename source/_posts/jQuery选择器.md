---
title: jQuery选择器
date: 2018-02-18 22:13:42
tags:
	-jQuery
categories:
	-jQuery
---
{% note info %}
介绍常见的jQuery选择器：元素选择器，id选择器，.class选择器，css选择器
{% endnote %}

## 元素选择器
jQuery 元素选择器基于元素名选取元素.

$("p") $("button") ...

## #id选择器

jQuery #id 选择器通过 HTML 元素的 id 属性选取指定的元素。
页面中元素的 id 应该是唯一的，所以您要在页面中选取唯一的元素需要通过 #id 选择器.

$("#test") $("#lui")

## .class选择器

jQuery 类选择器可以通过指定的 class 查找元素

$(".test") $(".lui")

## css选择器

jQuery CSS 选择器可用于改变 HTML 元素的 CSS 属性

$("p").css("background-color","red");

## More

语法|描述
-|-
$("*")|选取所有元素
$(this)|选取当前 HTML 元素
$("p.intro")|选取 class 为 intro 的 <p> 元素
$("p:first")|选取第一个 <p> 元素
$("ul li:first")|选取第一个 <ul> 元素的第一个 <li> 元素
$("ul li:first-child")|选取每个 <ul> 元素的第一个 <li> 元素
$("[href]")|选取带有 href 属性的元素
$("a[target='_blank']")|选取所有 target 属性值等于 "_blank" 的 <a> 元素
$("a[target!='_blank']")|选取所有 target 属性值不等于 "_blank" 的 <a> 元素
$(":button")|选取所有 type="button" 的 <input> 元素 和 <button> 元素
$("tr:even")|选取偶数位置的 <tr> 元素
$("tr:odd")|选取奇数位置的 <tr> 元素

## <span id="inline-blue">CDN库引用</span>

百度：

- http://libs.baidu.com/jquery/1.10.2/jquery.min.js <a id="download" href="http://libs.baidu.com/jquery/1.10.2/jquery.min.js"><i class="fa fa-download"></i><span>Download Now</span></a>

又拍云:

- http://upcdn.b0.upaiyun.com/libs/jquery/jquery-2.0.2.min.js <a id="download" href="http://upcdn.b0.upaiyun.com/libs/jquery/jquery-2.0.2.min.js"><i class="fa fa-download"></i><span>Download Now</span></a>

新浪:

- http://lib.sinaapp.com/js/jquery/2.0.2/jquery-2.0.2.min.js <a id="download" href="http://lib.sinaapp.com/js/jquery/2.0.2/jquery-2.0.2.min.js"><i class="fa fa-download"></i><span>Download Now</span></a>

Google:

- http://ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js <a id="download" href="http://ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"><i class="fa fa-download"></i><span>Download Now</span></a>

Microsoft:

- http://ajax.htmlnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js <a id="download" href="http://ajax.htmlnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js"><i class="fa fa-download"></i><span>Download Now</span></a>