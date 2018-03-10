---
title: jQuery效果
date: 2018-02-19 22:07:10
categories: jQuery
tags: jQuery
---

{% note info%}
jQuery效果:隐藏和显示[hide(),show(),toggle()],淡入淡出[fadeIn(),fadeOut(),fadeToggle(),fadeTo()],
滑动[slideDown(),slideUP(),slideToggle()],动画[animate()],停止动画[stop()]
{% endnote %}

## 隐藏和显示[hide(),show(),toggle()]

toggle语法：切换hide()与show()【隐藏和显示】

$(selector).toggle(speed, callback)

参数：
- speed:规定隐藏/显示的速度value:"slow"、"fast" 或毫秒
- callback:

>$(selector)选中的元素的个数为n个，则callback函数会执行n次
>callback函数名后加括号，会立刻执行函数体，而不是等到显示/隐藏完成后才执行
>callback既可以是函数名，也可以是匿名函数

```js
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("p").toggle("slow");
  });
});
</script>
```

**hide(),show()与toggle大同小异**

## 淡入淡出[fadeIn(),fadeOut(),fadeToggle(),fadeTo()]

fadeToggle语法：切换fadeIn()与fadeOut()【淡入淡出】

$(selector).fadeToggle(speed,callback)

参数：<span id="inline-red">同toggle()</span>

```js
<script>
$(document).ready(function(){
	$("button").click(function(){
		$("#div1").fadeToggle(1000);
		$("#div2").fadeToggle("slow");
		$("#div3").fadeToggle("fast");
	});
});
</script>
```



fadeTo()语法：【设不透明度】

$(selector).fadeTo(speed,opacity,callback);

参数：

- speed:必需参数,规定效果的时长
- opacity:必需参数,透明度value[0`1]
- callback:可选参数,该函数完成后所执行的函数名称

```js
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("#div1").fadeTo("slow",0.15);
    $("#div2").fadeTo("slow",0.4);
    $("#div3").fadeTo("slow",0.7);
  });
});
</script>
```

## 滑动[slideDown(),slideUP(),slideToggle()]

toggle语法：切换slideDown()与slideUP()【向下滑动与向上滑动】

$(selector).slideToggle(speed,callback);

参数：<span id="inline-red">同toggle()</span>

```js
<script> 
$(document).ready(function(){
  $("#flip").click(function(){
    $("#panel").slideToggle("slow");
  });
});
</script>
```

## 动画[animate()]【使用Camel 标记法书写属性名】

语法：

$(selector).animate({params},speed,callback);

参数：

- {params}:必需参数,定义形成动画的 CSS 属性
- speed:可选参数,规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒
- callback:可选参数,动画完成后所执行的函数名称

1.<div> 元素往右边移动了250px
```js
<script> 
$(document).ready(function(){
  $("button").click(function(){
    $("div").animate({left:'250px'});
  });
});
</script> 
```
2.<div> 元素往右边移动了250px,透明度为0.5,高变150px,宽为150px
<script> 
$(document).ready(function(){
  $("button").click(function(){
    $("div").animate({
      left:'250px',
      opacity:'0.5',
      height:'150px',
      width:'150px'
    });
  });
});
</script> 

3.animate()使用相对值

```js
<script> 
$(document).ready(function(){
  $("button").click(function(){
    $("div").animate({
      left:'250px',
      height:'+=150px',
      width:'+=150px'
    });
  });
});
</script> 
```

4.animate() - 使用预定义的值[valu:show,hide,toggle]

```js
<script> 
$(document).ready(function(){
  $("button").click(function(){
    $("div").animate({
      height:'toggle'
    });
  });
});
</script> 
```

5.animate() - 使用队列功能

```js
<script> 
$(document).ready(function(){
  $("button").click(function(){
    var div=$("div");
    div.animate({height:'300px',opacity:'0.4'});
    div.animate({width:'300px',opacity:'0.8'});
    div.animate({height:'100px',opacity:'0.4'});
    div.animate({width:'100px',opacity:'0.8'});
  });
});
</script> 
```

## 停止动画[stop()]

语法:

$(selector).stop(stopAll,goToEnd);

参数:

- stoppAll:可选参数,规定是否应该清除动画队列,默认是 false
- goToEnd:可选参数,规定是否立即完成当前动画,默认是 false

```js
<script> 
$(document).ready(function(){
  $("#flip").click(function(){
    $("#panel").slideDown(5000);
  });
  $("#stop").click(function(){
    $("#panel").stop();
  });
});
</script>
```
