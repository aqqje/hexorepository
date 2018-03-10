---
title: jQuery其他
date: 2018-02-23 22:49:42
categories: jQuery
tags: jQuery
---
{% note info %}
jQuery其他:jQuery - noConflict() 方法
{% endnote %}

jQuery - noConflict() 方法:

- noConflict()方法,允许你在同一个页面加载多个jQuery实例，尤其是不同版本的jQuery，
以及与其他javaScrip框架并存
- noConflict()方法会释放对 $ 标识符的控制,这样其他脚本就可以使用它了

全名替代简写:
```js
<script>
$.noConflict();
jQuery(document).ready(function(){
  jQuery("button").click(function(){
    jQuery("p").text("jQuery 仍然在工作!");
  });
});
</script>
```

创建自己的简写:

```js
<script>
var jq=$.noConflict();
jq(document).ready(function(){
  jq("button").click(function(){
    jq("p").text("jQuery 仍然在工作!");
  });
});
</script>
```

如果你的 jQuery 代码块使用 $ 简写，并且您不愿意改变这个快捷方式，那么您可以把 $ 符号作为变量传递给 ready 方法。这样就可以在函数内使用 $ 符号了 - 而在函数外，依旧不得不使用 "jQuery"：:

```js
<script>
$.noConflict();
jQuery(document).ready(function($){
  $("button").click(function(){
    $("p").text("jQuery 仍然在工作!");
  });
});
</script>
```