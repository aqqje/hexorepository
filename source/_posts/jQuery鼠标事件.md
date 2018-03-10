---
title: jQuery鼠标事件
date: 2018-02-18 21:29:58
tags:
	-jQuery
categories:
	-jQuery
---

{% note info %}
jQuery常见的鼠标事件：单击click,双击dbclick,进入mouseenter,离开mouseleave,鼠标按键mousedown，
鼠标按钮mouseup,光标悬停hover,焦点focus,失焦blur
{% endnote %}

## click()

click() 方法是当按钮点击事件被触发时会调用一个函数。
该函数在用户点击 HTML 元素时执行。

```js
<script>
	$(document).ready(function(){
		$("p").click(function(){
			$(this).hide();
		});
	});
</script>
```

## dbclick()

当双击元素时，会发生 dblclick 事件。
dblclick() 方法触发 dblclick 事件，或规定当发生 dblclick 事件时运行的函数：

```js
<script>
	$(document).ready(function(){
		$("#p1").dblclick(function(){
			$(this).hide();
		});
	});
</script>
```

## mouseenter()

当鼠标指针穿过元素时，会发生 mouseenter 事件。
mouseenter() 方法触发 mouseenter 事件，或规定当发生 mouseenter 事件时运行的函数：

```js
<script>
	$(document).ready(function(){
		$("#p1").mouseenter(function(){
			alert("You entered p1!");
		});
	});
</script>
```

## mouseleave()

当鼠标指针离开元素时，会发生 mouseleave 事件。
mouseleave() 方法触发 mouseleave 事件，或规定当发生 mouseleave 事件时运行的函数

```js
<script>
	$(document).ready(function(){
		$("#p1").mouseleave(function(){
			alert("Bye! You now leave p1!");
		});
	});
</script>
```

## mousedown()

当鼠标指针移动到元素上方，并按下鼠标按键时，会发生 mousedown 事件。
mousedown() 方法触发 mousedown 事件，或规定当发生 mousedown 事件时运行的函数

```js
<script>
	$(document).ready(function(){
		$("#p1").mousedown(function(){
			alert("Bye! You now leave p1!");
		});
	});
</script>
```

## mouseup()

当在元素上松开鼠标按钮时，会发生 mouseup 事件。
mouseup() 方法触发 mouseup 事件，或规定当发生 mouseup 事件时运行的函数

```js
<script>
$(document).ready(function(){
	$("#p1").mouseup(function(){
		alert("Mouse up over p1!")
	});
});
</script>
```

## hover()

hover()方法用于模拟光标悬停事件。
当鼠标移动到元素上时，会触发指定的第一个函数(mouseenter);当鼠标移出这个元素时，会触发指定的第二个函数(mouseleave)。

```js
<script>
	$(document).ready(function(){
		$("#p1").hover(function(){alert("You entered p1!");
		},
		function(){
		alert("Bye! You now leave p1!");
		});
	});
</script>
```

## focus()&&blur()

1.当元素获得焦点时，发生 focus 事件。
当通过鼠标点击选中元素或通过 tab 键定位到元素时，该元素就会获得焦点。
focus() 方法触发 focus 事件，或规定当发生 focus 事件时运行的函数

2.当元素失去焦点时，发生 blur 事件。
blur() 方法触发 blur 事件，或规定当发生 blur 事件时运行的函数

```js
<script>
$(document).ready(function(){
  $("input").focus(function(){
    $(this).css("background-color","#cccccc");
  });
  $("input").blur(function(){
    $(this).css("background-color","#ffffff");
  });
});
</script>

```