---
title: jQuery HTML
date: 2018-02-20 20:41:59
categories: jQuery
tags: jQuery
---
{% note info %}
jQuery HTML：jQuery 捕获,jQuery 设置,jQuery 添加元素,jQuery 删除元素,jQuery CSS 类,jQuery css() 方法,jQuery 尺寸
{% endnote %}

## jQuery 捕获

获取内容：

- text():设置或返回所选元素的文本内容
- html():设置或返回所选元素的内容（包括 HTML 标记）
- val():设置或返回表单字段的值

获取属性：

- attr():用于获取属性值


text()&&html():
```js
<script>
$(document).ready(function(){
  $("#btn1").click(function(){
    alert("Text: " + $("#test").text());
	//Text:这是段落中的 粗体 文本。
  });
  $("#btn2").click(function(){
    alert("HTML: " + $("#test").html());
	//Html:这是段落中的<b>粗体</b>文本。
  });
});
</script>
```

val():
```js
<script>
$(document).ready(function(){
  $("button").click(function(){
  	alert("Val:" + $("#test").val());
  });
});
</script>
```

attr():

```js
<script>
$(document).ready(function(){
  $("button").click(function(){
	alert("Attr:" + $("#w3s").attr("href"));
  });
});
</script>
```

## jQuery 设置

text()&&html()&&val:

```js
<script>
$(document).ready(function(){
  $("#btn1").click(function(){
    $("#test1").text("Hello world!");
  });
  $("#btn2").click(function(){
    $("#test2").html("<b>Hello world!</b>");
  });
  $("#btn3").click(function(){
    $("#test3").val("W3Cschool");
  });
});
</script>
```

text()、html() 以及 val()，
同样拥有回调函数。回调函数由两个参数：被选元素列表中当前元素的下标，以及原始（旧的）值。然后以函数新值返回您希望使用的字符串

```js
$(document).ready(function(){
  $("#btn1").click(function(){
    $("#test1").text(function(i,origText){
      return "旧文本: " + origText + " 新文本: Hello world! (index: " + i + ")"; 
    });
  });

  $("#btn2").click(function(){
    $("#test2").html(function(i,origText){
      return "旧 html: " + origText + " 新 html: Hello <b>world!</b> (index: " + i + ")"; 
    });
  });

});
```

attr():

```js
<script>
$(document).ready(function(){
  $("button").click(function(){
  //$("#runoob").attr("href", "http://www.w3cschool.cn/jquery");
    $("#runoob").attr({
      "href" : "http://www.w3cschool.cn/jquery",
      "title" : "jQuery 教程"
    });
  });
});
</script>
```


## Query 添加元素

- append():在被选元素内部的结尾插入指定内容
- prepend():在被选元素内部的开头插入指定内容
- after():在被选元素之后插入内容
- before():在被选元素之前插入内容

append()&&prepend:

```js
<script>
$(document).ready(function(){
  $("#btn1").click(function(){
    $("p").prepend("<b>在开头追加文本</b>。 ");
  });
  $("#btn2").click(function(){
    $("ol").prepend("<li>在开头添加列表项</li>");
  });
});
</script>
```
append()&&prepend添加若干新元素:
```js
<script>
function appendText()
{
	var txt1="<p>文本。</p>";              // 使用 HTML 标签创建文本
	var txt2=$("<p></p>").text("文本。");  // 使用 jQuery 创建文本
	var txt3=document.createElement("p");
	txt3.innerHTML="文本。";               // 使用 DOM 创建文本 text with DOM
	$("body").append(txt1,txt2,txt3);        // 追加新元素
}
</script>
```

after()&&before():

```js
<script>
$(document).ready(function(){
  $("#btn1").click(function(){
    $("img").before("<b>之前</b>");
  });

  $("#btn2").click(function(){
    $("img").after("<i>之后</i>");
  });
});
</script>
```

after()&&before()添加若干新元素:

```js
<script>
function afterText(){
var txt1="<b>I  </b>";
var txt2 = $("<i></i>").text(" LOVE  ");
var txt3=document.createElement("big");
txt3.innerHTML = "LIULI";
$("img").after(txt1,txt2,txt3);	
}
function beforeText(){
  var text1 = "<b>I  </b>";
  var text2 = $("<i></i>").text(" LOVE  ");
  var text3 = document.createElement("big");
  text3.innerHTML=" LIULI";
  $("img").before(text1,text2,text3);
}
</script>
```

## jQuery 删除元素

- remove():删除被选元素（及其子元素）
- empty():从被选元素中删除子元素


remove():

```js
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("#div1").remove();
  });
});
</script>
```

remove()过滤被删除的元素:

```js
 //class="italic" 的所有 <p> 元素
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("p").remove(".italic");
  });
});
</script>
 ```
empty():(所有子类删除)

```js
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("#div1").empty();
  });
});
</script>
```

## jQuery CSS 类

- addClass():向被选元素添加一个或多个类
- removeClass():从被选元素删除一个或多个类
- toggleClass():对被选元素进行添加/删除类的切换操作

addClass():
```js
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("h1,h2,p").addClass("blue");
    $("div").addClass("important");
  });
});
</script>
```

addClass()规定多个类:
```js
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("div").addClass("important blue");
  });
});
</script>
```

removeClass(): 也可以删除多个class属性
```js
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("h1,h2,p").removeClass("blue");
  });
});
</script>
```

toggleClass()：
```js
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("h1,h2,p").toggleClass("blue");
  });
});
</script>
```

## jQuery css() 方法

- css():设置或返回样式属性


css():返回 CSS 属性

```js
<script>
$(document).ready(function(){
  $("button").click(function(){
    alert("背景颜色 = " + $("p").css("background-color"));
  });
});
</script>
```

css():设置 CSS 属性

```js
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("p").css("background-color","yellow");
  });
});
</script>
```

css():设置多个 CSS 属性

```js
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("p").css({"background-color":"yellow","font-size":"200%"});
  });
});
</script>
```

## jQuery 尺寸

- width():设置或返回元素的宽度（不包括内边距、边框或外边距）
- height():设置或返回元素的高度（不包括内边距、边框或外边距）
- innerWidth():返回元素的宽度（包括内边距）
- innerHeight():方法返回元素的高度（包括内边距）
- outerWidth():返回元素的宽度（包括内边距和边框）
- outerHeight():方法返回元素的高度（包括内边距和边框）

width()&&height():

```js
<script>
$(document).ready(function(){
  $("button").click(function(){
    var txt="";
    txt+="Width of div: " + $("#div1").width() + "</br>";
    txt+="Height of div: " + $("#div1").height();
    $("#div1").html(txt);
  });
});
</script>
```
innerWidth()&&innerHeight():

```js
<script>
$(document).ready(function(){
  $("button").click(function(){
    var txt="";
    txt+="div 宽度: " + $("#div1").width() + "</br>";
    txt+="div 高度: " + $("#div1").height() + "</br>";
    txt+="div 宽度，包含内边距: " + $("#div1").innerWidth() + "</br>";
    txt+="div 高度，包含内边距: " + $("#div1").innerHeight();
    $("#div1").html(txt);
  });
});
</script>
```
outerHeight()&&outerWidth():

```js
<script>
$(document).ready(function(){
  $("button").click(function(){
    var txt="";
    txt+="div 宽度: " + $("#div1").width() + "</br>";
    txt+="div 高度: " + $("#div1").height() + "</br>";
    txt+="div 宽度，包含内边距和边框: " + $("#div1").outerWidth() + "</br>";
    txt+="div 高度，包含内边距和边框: " + $("#div1").outerHeight();
    $("#div1").html(txt);
  });
});
</script>
```

<span id="inline-yellow">outerWidth(true) 方法返回元素的宽度（包括内边距、边框和外边距）</span>