---
title: jQuery遍历
date: 2018-02-21 22:43:09
categories: jQuery
tags: jQuery
---

{% note info %}
-jQuery 遍历
-jQuery 遍历 – 祖先
-jQuery 遍历 – 后代-
-jQuery 遍历 – 同胞(siblings)
-jQuery 遍历 – 过滤
{% endnote %}


## jQuery 遍历
```js
              <div>
                |
			   <ul>
                |
--------------------------------
	 <li>                   <li>
	  |                      |
 ------------            --------
   |	  |                  |
<span>	<span>              <b>
```

图示解析：
```
    <div> 元素是 <ul> 的父元素，同时是其中所有内容的祖先。
	<ul> 元素是 <li> 元素的父元素，同时是 <div> 的子元素
	左边的 <li> 元素是 <span> 的父元素，<ul> 的子元素，同时是 <div> 的后代。
	<span> 元素是 <li> 的子元素，同时是 <ul> 和 <div> 的后代。
	两个 <li> 元素是同胞（拥有相同的父元素）。
	右边的 <li> 元素是 <b> 的父元素，<ul> 的子元素，同时是 <div> 的后代。
	<b> 元素是右边的 <li> 的子元素，同时是 <ul> 和 <div> 的后代。
```

<span id="inline-yellow">遍历 DOM</aspan>:遍历方法中最大的种类是树遍历（tree-traversal）。



## jQuery 遍历 – 祖先

向上遍历 DOM 树方法:

-parent():返回被选元素的直接父元素,该方法<span id="inline-blue">只会向上一级对</span> DOM 树进行遍历
-parents():返回被选元素的<span id="inline-blue">所有祖先元素</span>，它一路向上直到文档的根元素 
-parentsUntil():返回<span id="inline-blue">介于两个给定元素</span>之间的所有祖先元素。

parent():

返回所有 <span> 元素的所有祖先:
```js
<script>
$(document).ready(function(){
  $("span").parent().css({"color":"red","border":"2px solid blue"});
});
</script>
```


parents():

返回所有 <span> 元素的所有祖先，并且它是 <ul> 元素：

```js
<script>
$(document).ready(function(){
  $("span").parents("ul").css({"color":"red","border":"2px solid red"});
});
</script>
```

parentsUntil():

 <span> 与 <div> 元素之间的所有祖先元素：
 
 ```js
 <script>
$(document).ready(function(){
  $("span").parents("ul").css({"color":"red","border":"2px solid red"});
});
</script>
 ```
## jQuery 遍历 – 后代
 
-children():返回被选元素的所有直接子元素。
-find():返回被选元素的后代元素，一路向下直到最后一个后代。

children():

返回每个 <div> 元素的所有直接子元素：

```js
<script>
$(document).ready(function(){
	$("div").children().css({"color":"red","border":"2px solid red"});
});
</script>
```

返回类名为 "1" 的所有 <p> 元素，并且它们是 <div> 的直接子元素：

```js
<script>
$(document).ready(function(){
  $("div").children("p.1").css({"color":"red","border":"2px solid red"});
});
</script>
```


find():

返回属于 <div> 后代的所有 <span> 元素：

```js
<script>
$(document).ready(function(){
  $("div").find("span").css({"color":"red","border":"2px solid red"});
});
</script>
```

返回 <div> 的所有后代：

```js
<script>
$(document).ready(function(){
  $("div").find("*").css({"color":"red","border":"2px solid red"});
});
</script>
```
## jQuery 遍历 - 同胞(siblings)

-siblings():返回被选元素的所有同胞元素。
-next():返回被选元素的下一个同胞元素。
-nextAll():返回被选元素的所有跟随的同胞元素。
-nextUntil():返回介于两个给定参数之间的所有跟随的同胞元素。
-<span id="inline-blue">prev()&&prevAll()&&-prevUntil()</span>:工作方式与上面的方法类似，只不过方向相反而已：它们返回的是前面的同胞元素（在 DOM 树中沿着同胞元素向后遍历，而不是向前）。

siblings();

返回 <h2> 的所有同胞元素：<span id="inline-blue">[但返回中不包含被选元素]</span>

```js
<script>
$(document).ready(function(){
	$("h2").siblings().css({"color":"red","border":"2px solid red"});
});
</script>
```

返回属于 <h2> 的同胞元素的所有 <p> 元素：<span id="inline-blue">[返回中<span id="inline-yellow">包含</span>被选元素]</span>

```js
<script>
$(document).ready(function(){
  $("h2").siblings("h2").css({"color":"red","border":"2px solid red"});
});
</script>
```
next():

返回 <h2> 的下一个同胞元素：[返回中不包含被选元素]</span>
```js
<script>
$(document).ready(function(){
	$("h2").next().css({"color":"red","border":"2px solid red"});
});
</script>
```

nextAll():

返回 <h2> 的所有跟随的同胞元素：[返回中不包含被选元素]</span>
```js
<script>
$(document).ready(function(){
	$("h2").nextAll().css({"color":"red","border":"2px solid red"});
});
</script>
```

## jQuery 遍历 – 过滤

- first():返回被选元素的首个元素。
- last():返回被选元素的最后一个元素。
- eq():返回被选元素中带有指定索引号的元素,<sapn id="inline-blue">index从0开始</span>
- filter():允许您规定一个标准。不匹配这个标准的元素会被从集合中删除，匹配的元素会被返回。
- not():返回不匹配标准的所有元素,<span id="inline-yellow">not() 方法与 filter() 相反</span>

first():

选取首个 <div> 元素内部的第一个 <p> 元素：
```js
<script>
$(document).ready(function(){
  $("div p").first().css("background-color","yellow");
});
</script>
```

last():

选择最后一个 <div> 元素中的最后一个 <p> 元素：

```js
<script>
$(document).ready(function(){
	$("div p").last().css("background-color","yellow");
});
</script>
```

eq():

选取第二个 <p> 元素（索引号 1）：
```js
<script>
$(document).ready(function(){
  $("p").eq(0).css("background-color","yellow");
});
</script>
```

filter():

返回带有类名 "intro" 的所有 <p> 元素：
实例

```js
<script>
$(document).ready(function(){
   $("p").filter(".url").css("background-color","yellow");
});
</script>
```

not():

返回不带有类名 "intro" 的所有 <p> 元素：

```js
<script>
$(document).ready(function(){
   $("p").not(".url").css("background-color","yellow");
});
</script>
```

