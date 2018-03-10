---
title: jQuery Ajax
date: 2018-02-22 22:49:54
categories: jQuery
tags: jQuery
---
{% note info %}
jQuery Ajax:jQuery - AJAX 简介,jQuery - AJAX load() 方法,jQuery - AJAX get() 和 post() 方法
{% endnote %}

## jQuery - AJAX 简介

什么是 AJAX？

AJAX = 异步 JavaScript 和 XML（Asynchronous JavaScript and XML）。
简短地说，在不重载整个网页的情况下，AJAX 通过后台加载数据，并在网页上进行显示。
使用 AJAX 的应用程序案例：谷歌地图、腾讯微博、优酷视频、人人网等等。

关于 jQuery 与 AJAX

jQuery 提供多个与 AJAX 有关的方法。
通过 jQuery AJAX 方法，您能够使用 HTTP Get 和 HTTP Post 从远程服务器上请求文本、HTML、XML 或 JSON - 同时您能够把这些外部数据直接载入网页的被选元素中


## jQuery - AJAX load() 方法

load():从服务器加载数据，并把返回的数据放入被选元素中。

语法:

$(selector).load(URL,data,callback); 

参数：

- URL:必需，规定您希望加载的 URL。
- data:可选的,规定与请求一同发送的查询字符串键/值对集合。
- callback:可选,load() 方法完成后所执行的函数名称,回调函数可以设置不同的参数：
	- responseTxt - 包含调用成功时的结果内容
	- statusTXT - 包含调用的状态
	- xhr - 包含 XMLHttpRequest 对象

demo_test.txt:
```js
<h2>jQuery and AJAX is FUN!!!</h2>
<p id="p1">This is some text in a paragraph.</p>
```

文件 "demo_test.txt" 的内容加载到指定的 <div> 元素中：
```js:
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("#div1").load("/statics/demosource/demo_test.txt");
  });
});
</script>
```

把 "demo_test.txt" 文件中 id="p1" 的元素的内容，加载到指定的 <div> 元素中：
```js
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("#div1").load("/statics/demosource/demo_test.txt #p1");
  });
});
</script>
```

 load() 方法完成后显示一个提示框。如果 load() 方法已成功，则显示"外部内容加载成功！"，而如果失败，则显示错误消息：
 
 ```js
 <script>
$(document).ready(function(){
  $("button").click(function(){
    $("#div1").load("/statics/demosource/demo_test.txt",function(responseTxt,statusTxt,xhr){
      if(statusTxt=="success")
        alert("外部内容加载成功!");
      if(statusTxt=="error")
        alert("Error: "+xhr.status+": "+xhr.statusText);
    });
  });
});
</script>
 ```

## jQuery - AJAX get() 和 post() 方法

- $.get():通过 HTTP GET 请求从服务器上请求数据。

语法：

$.get(URL,callback);

参数:

- URL:必需,规定您希望请求的 URL。
- callback:可选,是请求成功后所执行的函数名。

$.get() 方法从服务器上的一个文件中取回数据：

demo_test.php:

```php
 <?php
 echo "This is some text from an external PHP file.";
 ?>
```


```js
<script>
$(document).ready(function(){
	$("button").click(function(){
		$.get("/statics/demosource/demo_test.php",function(data,status){
			alert("数据: " + data + "\n状态: " + status);
		});
	});
});
</script>
```
- $.post():通过 HTTP POST 请求从服务器上请求数据。

语法:

$.post(URL,data,callback); 

参数：

- URL:必需,规定您希望请求的 URL。
- data:可选,规定连同请求发送的数据。
- callback:可选,是请求成功后所执行的函数名。

使用 $.post() 连同请求一起发送数据：

demo_test_post.php:
```php

```
 <?php
 $name = isset($_POST['name']) ? htmlspecialchars($_POST['name']) : '';
 $city = isset($_POST['city']) ? htmlspecialchars($_POST['city']) : '';
 echo 'Dear ' . $name;
 echo 'Hope you live well in ' . $city;
 ?>
```js
<script>
$(document).ready(function(){
  $("button").click(function(){
    $.post("/statics/demosource/demo_test_post.php",
    {
      name:"W3Cschool",
      url:"http://www.w3cschool.cn"
    },
    function(data,status){
      alert("数据: " + data + "状态: " + status);
    });
  });
});
</script>
```


