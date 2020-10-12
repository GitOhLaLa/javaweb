##### 什么是jQuery

jQuery对象是一个伪数组，$()核心函数把所有括号内的内容都包装成一个jQuery对象；

##### 什么是属性节点

```javascript
//在标签中添加的属性就是属性节点
<span name="myname"></span>
//属性和属性节点的区别:任何对象都有属性，但是只有DOM对象才有属性节点
```

##### jQuery中输出数据

- alert()
- console.log()     比较常用

##### jQuery和js方法不通用，如果两者需要转换

* jq -- > js : jq对象[索引] 或者 jq对象.get(索引)
* js -- > jq : $(js对象)

##### 事件绑定

获取b1按钮点击事件

```javascript
$("#b1").click(function(){
alert("abc");
});
```

##### 入口函数

```javascript
//使用 ready() 来使函数在文档加载后是可用的   jQuery的入口函数第一种写法
$(document).ready(function(){
});
//jQuery第二种写法
jQuery(document).ready(function(){
});
//jQuery第三种写法（推荐）
$(function () {
 });
//jQuery第四种写法
jQuery(function () {
 });
//释放$的使用权
jQuery.noConflict();
//自定义一个访问符号
var nj=jQuery.noConflict();
//原生JS的入口函数固定写法
window.onload=function(ev){
}
//原生JS和jQuery入口函数的加载模式不同
//原生JS会等到DOM元素加载完毕，并且图片也加载完毕才会执行
//jQuery会等到DOM元素加载完毕，但不会等到图片加载完毕，jQuery会直接执行
```

##### 静态方法和实例方法

```javascript
//静态方法通过类名调用
ACalss.staticMethod=function(){
	alert("staticMethod");
}
AClass.staticMethod();
//实例方法通过类的实例调用
AClass.prototype.instanceMethod=function(){
	alert("instanceMethod");
}
var a= new AClass();
a.instanceMethod();
```



#####  window.onload  和 $(function) 区别

* window.onload 只能定义一次,如果定义多次，后边的会将前边的覆盖掉
* $(function)可以定义多次的。

##### 样式控制

```javascript
$("#div1").css("background-color","red");
$("#div1").css("backgroundColor","pink");
```

## 										选择器

### 										1.基本选择器

##### 标签选择器（元素选择器）

- 语法： $("html标签名") 获得所有匹配标签名称的元素

##### id选择器 

* 语法： $("#id的属性值") 获得与指定id属性值匹配的元素

##### 类选择器

* 语法： $(".class的属性值") 获得与指定的class属性值匹配的元素

##### 并集选择器

* 语法： $("选择器1,选择器2....") 获取多个选择器选中的所有元素

  ### 									2.层级选择器

##### 后代选择器

* 语法： $("A B ") 选择A元素内部的所有B元素

```javascript
$("ul li")
```

##### 子选择器

* 语法： $("A > B") 选择A元素内部的所有B子元素

```javascript
$("ul>li")
```

### 									3.属性选择器

#####  属性名称选择器 

* 语法： $("A[属性名]") 包含指定属性的选择器

```javascript
$("div[id]")
```

##### 属性选择器

* 语法： $("A[属性名='值']") 包含指定属性等于指定值的选择器

```javascript
$("div[id='username']")
```

##### 复合属性选择器

* 语法： $("A[属性名='值'][]...") 包含多个属性条件的选择器

  ### 									4.过滤选择器

##### 首元素选择器 

* 语法： :first 获得选择的元素中的第一个元素

##### 尾元素选择器 

* 语法： :last 获得选择的元素中的最后一个元素

##### 非元素选择器

* 语法： :not(selector) 不包括指定内容的元素

##### 偶数选择器

* 语法： :even 偶数，从 0 开始计数

```javascript
$("li:even").css("background-color","blue");
```

##### 奇数选择器

* 语法： :odd 奇数，从 0 开始计数

```javascript
$("li:odd").css("background-color","blue");
```

##### 等于索引选择器

* 语法： :eq(index) 指定索引元素

```javascript
$("ul>li:eq('0')").css("background-color","blue");
```

##### 大于索引选择器 

* 语法： :gt(index) 大于指定索引元素

```javascript
$("ul li:gt(4):not(:last)").hide();
```

##### 小于索引选择器 

* 语法： :lt(index) 小于指定索引元素

##### 标题选择器

* 语法： :header 获得标题（h1~h6）元素，固定写法

  ### 									5.表单过滤选择器

##### 可用元素选择器

* 语法： :enabled 获得可用元素

##### 不可用元素选择器 

* 语法： :disabled 获得不可用元素

##### 选中选择器 

* 语法： :checked 获得单选/复选框选中的元素

##### 选中选择器 

* 语法： :selected 获得下拉框选中的元素

### 6.内容过滤选择器

##### empty

```javascript
<div></div>
<div>我是div</div>
<div><span></span></div>
var $div=$("div:empty");
//作用:找到既没有文本内容也没有子元素的指定元素
```

##### parent

```javascript
var $div=$("div:parent");
//作用:找到有文本内容或有子元素的指定元素
```

##### contains

```javascript
var $div=$("div:contains('我是div')");
//作用:找到包含指定文本内容的指定元素
```

##### has

```javascript
var $div=$("div:has('span')");
//作用:找到包含指定子元素的指定元素
```



------

#####  jquery标准的绑定方式

* jq对象.事件方法(回调函数)；

* 注：如果调用事件方法，不传递回调函数，则会触发浏览器默认行为。

* 表单对象.submit();//让表单提交

##### on绑定事件/off解除绑定

* jq对象.on("事件名称",回调函数)

* jq对象.off("事件名称")

* 如果off方法不传递任何参数，则将组件上的所有事件全部解绑

##### 事件切换：toggle

* jq对象.toggle(fn1,fn2...)

* 当单击jq对象对应的组件后，会执行fn1.第二次点击会执行fn2.....

* 注意：1.9版本 .toggle() 方法删除,jQuery Migrate（迁移）插件可以恢复此功能。

------

## 																																								jQuery方法

##### bind( )	

```javascript
//为一个元素绑定一个事件处理程序
//当点击鼠标时，隐藏或显示 p 元素
$("button").bind("click",function(){
  $("p").slideToggle();
});
//$(selector).bind(event,data,function)
//event	必需。规定添加到元素的一个或多个事件。由空格分隔多个事件。必须是有效的事件
//data	可选。规定传递到函数的额外数据。
//function	必需。规定当事件发生时运行的函数。
//slideToggle() 方法通过使用滑动效果（高度变化）来切换元素的可见状态。如果被选元素是可见的，则隐藏这些元素，如果被选元素是隐藏的，则显示这些元素。

//$(selector).slideToggle(speed,callback)
/*speed 可选。规定元素从隐藏到可见的速度（或者相反）。默认为 "normal"。
可能的值：毫秒 （比如 1500）
"slow"
"normal"
"fast"
在设置速度的情况下，元素在切换的过程中，会逐渐地改变其高度（这样会创造滑动效果）。*/
//callback 可选。toggle 函数执行完之后，要执行的函数。除非设置了 speed 参数，否则不能设置该参数。
```

##### live( )	

```javascript
//为被选元素附加一个或多个事件处理程序
//当点击按钮时，隐藏或显示 <p> 元素
$("button").live("click",function(){
$("p").slideToggle();
});
//$(selector).live(event,data,function)
//event	 必需。规定添加到元素的一个或多个事件。由空格分隔多个事件值。必须是有效的事件。
//data	可选。规定传递到该函数的额外数据。
//function	必需。规定当事件发生时运行的函数。
//live() 方法在 jQuery 版本 1.7 中被废弃，在版本 1.9 中被移除。请使用 on() 方法代替。live() 方法为被选元素添加一个或多个事件处理程序，并规定当这些事件发生时运行的函数。通过 live() 方法添加的事件处理程序适用于匹配选择器的当前及未来的元素（比如由脚本创建的新元素）。提示：如需移除事件处理程序，请使用 die() 方法。
```

##### on( )	

```javascript
//为被选元素绑定一个或多个事件处理程序
//向 <p> 元素添加 click 事件处理程序
$(document).ready(function(){
  $("p").on("click",function(){
    alert("段落被点击了。");
  });
});
//on() 方法在被选元素及子元素上添加一个或多个事件处理程序。自 jQuery 版本 1.7 起，on() 方法是 bind()、live() 和 delegate() 方法的新的替代品。该方法给 API 带来很多便利，我们推荐使用该方法，它简化了 jQuery 代码库。注意：使用 on() 方法添加的事件处理程序适用于当前及未来的元素（比如由脚本创建的新元素）。提示：如需移除事件处理程序，请使用 off() 方法。提示：如需添加只运行一次的事件然后移除，请使用 one() 方法。
//$(selector).on(event,childSelector,data,function)
//event	必需。规定要从被选元素添加的一个或多个事件或命名空间。由空格分隔多个事件值，也可以是数组。必须是有效的事件。
//childSelector	可选。规定只能添加到指定的子元素上的事件处理程序（且不是选择器本身，比如已废弃的 delegate() 方法）。
//data		可选。规定传递到函数的额外数据。
//function	可选。规定当事件发生时运行的函数。
//注意：定义的on事件可以重复，而且不会被覆盖
```

##### one( )	

```javascript
//为元素的事件添加处理函数。处理函数在每个元素上每种事件类型最多执行一次。
//当点击 <p> 元素时，增加该元素的文本大小（每个 <p> 元素只能触发一次事件）
$("p").one("click",function(){
$(this).animate({fontSize:"+=6px"});
});
//one() 方法为被选元素添加一个或多个事件处理程序，并规定当事件发生时运行的函数。当使用 one() 方法时，每个元素只能运行一次事件处理程序函数。
//$(selector).one(event,data,function)
//event	必需。规定添加到元素的一个或多个事件。由空格分隔多个事件值。必须是有效的事件。
//data	可选。规定传递到函数的额外数据。
//function	必需。规定当事件发生时运行的函数。

//通过改变元素的高度，对元素应用动画
$("button").click(function(){
    $("#box").animate({height:"300px"});
});
//animate() 方法执行 CSS 属性集的自定义动画。该方法通过 CSS 样式将元素从一个状态改变为另一个状态。CSS属性值是逐渐改变的，这样就可以创建动画效果。只有数字值可创建动画（比如 "margin:30px"）。字符串值无法创建动画（比如 "background-color:red"）。提示：请使用 "+=" 或 "-=" 来创建相对动画。
//(selector).animate({styles},speed,easing,callback)
//styles	必需。规定产生动画效果的一个或多个 CSS 属性/值。注意： 当与 animate() 方法一起使用时，该属性名称必须是驼峰写法： 您必须使用 paddingLeft 代替 padding-left，marginRight 代替 margin-right，依此类推
//speed		可选。规定动画的速度。可能的值：毫秒"slow""fast"
//easing		可选。规定在动画的不同点中元素的速度。默认值是 "swing"。可能的值："swing" - 在开头/结尾移动慢，在中间移动快"linear" - 匀速移动 提示：扩展插件中提供更多可用的 easing 函数。
//callback		可选。animate 函数执行完之后，要执行的函数。

//(selector).animate({styles},{options})
//styles		必需。规定产生动画效果的一个或多个 CSS 属性/值（同上）。
//options		可选。规定动画的额外选项。可能的值：speed - 设置动画的速度;easing - 规定要使用的 ;easing 函数callback - 规定动画完成之后要执行的函数;step - 规定动画的每一步完成之后要执行的函数;queue - 布尔值。指示是否在效果队列中放置动画。如果为 false，则动画将立即开始。;specialEasing - 来自 styles 参数的一个或多个 CSS 属性的映射，以及它们的对应 easing 函数
```

##### focus( )	

```javascript
//触发或将函数绑定到指定元素的focus事件	获得焦点
//添加函数到 focus 事件。当 <input> 字段获得焦点时发生 focus 事件
$("input").focus(function(){
    $("span").css("display","inline").fadeOut(2000);
});
//当元素获得焦点时（当通过鼠标点击选中元素或通过 tab 键定位到元素时），发生 focus 事件。focus() 方法触发 focus 事件，或规定当发生 focus 事件时运行的函数。提示：该方法通常与 blur() 方法一起使用。
//$(selector).focus(function)
//function	可选。规定当 focus 事件发生时运行的函数。

//使用淡出效果显示所有 <p> 元素
$("button").click(function(){
    $("p").fadeOut();
});
//fadeOut() 方法逐渐改变被选元素的不透明度，从可见到隐藏（褪色效果）。注释：隐藏的元素不会被完全显示（不再影响页面的布局）。提示：该方法通常与 fadeIn() 方法一起使用。
//$(selector).fadeOut(speed,easing,callback)
//speed	可选。规定褪色效果的速度。可能的值：毫秒"slow""fast"
//easing	可选。规定在动画的不同点上元素的速度。默认值为 "swing"。可能的值："swing" - 在开头/结尾移动慢，在中间移动快;"linear" - 匀速移动提示：扩展插件中提供更多可用的 easing 函数。
//callback	可选。fadeOut() 方法执行完之后，要执行的函数。
```

##### change( )	

```javascript
//触发或将函数绑定到指定元素的change事件	元素的值改变时
//当 <input> 字段改变时警报文本
$("input").change(function(){
    alert("文本已被修改");
});
//当元素的值改变时发生 change 事件（仅适用于表单字段）。change() 方法触发 change 事件，或规定当发生 change 事件时运行的函数。注意：当用于 select 元素时，change 事件会在选择某个选项时发生。当用于 text field 或 text area 时，change 事件会在元素失去焦点时发生。
```

##### keyup( )	

```javascript
//触发或将函数绑定到指定元素的keyup事件	释放按键时
//当键盘键被松开时，设置 <input> 字段的背景颜色为黄色，当键盘被松开时，设置<input>字段的背景颜色为粉色
$(document).ready(function(){
  $("input").keydown(function(){
    $("input").css("background-color","yellow");
  });
  $("input").keyup(function(){
    $("input").css("background-color","pink");
  });
});
//与 keyup 事件相关的事件顺序：keydown - 键按下的过程;keypress - 键被按下;keyup - 键被松开;当键盘键被松开时发生 keyup 事件。keyup() 方法触发 keyup 事件，或规定当发生 keyup 事件时运行的函数。提示：请使用 event.which 属性来返回哪个键被按下。
```

#####  unbind( )	

```javascript
//从元素上删除一个以前附加事件处理程序
//移除所有<p>的滑出效果
$(document).ready(function(){
  $("p").click(function(){
    $(this).slideToggle();
  });
  $("button").click(function(){
    $("p").unbind();
  });
});
//unbind() 方法移除被选元素的事件处理程序。该方法能够移除所有的或被选的事件处理程序，或者当事件发生时终止指定函数的运行。该方法也可以通过 event 对象取消绑定的事件处理程序。该方法也用于对自身内部的事件取消绑定（比如当事件已被触发一定次数之后，删除事件处理程序）。注意：如果未规定参数，则 unbind() 方法会删除指定元素的所有事件处理程序。注意：unbind() 方法适用于任意由 jQuery 添加的事件处理程序。自 jQuery 版本 1.7 起，on() 和 off() 方法是在元素上添加和移除事件处理程序的首选方法。
//$(selector).unbind(event,function,eventObj)
//event		可选。规定一个或多个要从元素上移除的事件。由空格分隔多个事件值。如果只规定了该参数，则会删除绑定到指定事件的所有函数。
//function	可选。规定从元素上指定事件取消绑定的函数名称。
//eventObj	可选。规定要使用的移除的 event 对象。这个 eventObj 参数来自事件绑定函数。
```

##### show( )	

```javascript
//隐藏匹配的元素
//隐藏或显示<p>
$(document).ready(function(){
	$(".btn1").click(function(){
		$("p").hide();
	});
	$(".btn2").click(function(){
		$("p").show();
	});
});
//show() 方法显示隐藏的被选元素。注意：show() 适用于通过 jQuery 方法和 CSS 中 display:none 隐藏的元素（不适用于通过 visibility:hidden 隐藏的元素）。提示：如需隐藏元素，请查看 hide() 方法。
//$(selector).show(speed,easing,callback)
//speed	可选。规定显示效果的速度。可能的值：毫秒"slow""fast"
//easing	可选。规定在动画的不同点上元素的速度。默认值为 "swing"。可能的值："swing" - 在开头/结尾移动慢，在中间移动快;"linear" - 匀速移动提示：扩展插件中提供更多可用的 easing 函数。
//callback	可选。show() 方法执行完之后，要执行的函数。
```

##### off()	

```javascript
//常用于移除通过 on() 方法添加的事件处理程序
//移除<p>的事件
$(document).ready(function(){
  $("p").on("click",function(){
    $(this).css("background-color","pink");
  });
  $("button").click(function(){
    $("p").off("click");
  });
});
//off() 方法通常用于移除通过 on() 方法添加的事件处理程序。自 jQuery 版本 1.7 起，off() 方法是 unbind()、die() 和 undelegate() 方法的新的替代品。该方法给 API 带来很多便利，我们推荐使用该方法，它简化了 jQuery 代码库。注意：如需移除指定的事件处理程序，当事件处理程序被添加时，选择器字符串必须匹配 on() 方法传递的参数。提示：如需添加只运行一次的事件然后移除，请使用 one() 方法。
//$(selector).off(event,selector,function(eventObj),map)
//event		必需。规定要从被选元素移除的一个或多个事件或命名空间。由空格分隔多个事件值。必须是有效的事件。
//selector	可选。规定添加事件处理程序时最初传递给 on() 方法的选择器。
//function(eventObj)	可选。规定当事件发生时运行的函数。
//map	规定事件映射 ({event:function, event:function, ...})，包含要添加到元素的一个或多个事件，以及当事件发生时运行的函数。
```

##### toggle()

```javascript
//切换 <p> 元素的显示与隐藏状态：
$(document).ready(function(){
  $(".btn1").click(function(){
  $("p").toggle();
  });
});
//toggle() 方法切换元素的可见状态。如果被选元素可见，则隐藏这些元素，如果被选元素隐藏，则显示这些元素。
//$(selector).toggle(speed,callback,switch)
//speed:可选。规定元素从可见到隐藏的速度（或者相反）。默认为 "0"。可能的值：毫秒 （比如 1500）"slow""normal""fast"在设置速度的情况下，元素从可见到隐藏的过程中，会逐渐地改变其高度、宽度、外边距、内边距和透明度。如果设置此参数，则无法使用 switch 参数。
//callback:可选。toggle 函数执行完之后，要执行的函数。除非设置了 speed 参数，否则不能设置该参数。
//switch:	可选。布尔值。规定 toggle 是否隐藏或显示所有被选元素。True - 显示所有元素;False - 隐藏所有元素如果设置此参数，则无法使用 speed 和 callback 参数。
//注释：该效果适用于通过 jQuery 隐藏的元素，或在 CSS 中声明 display:none 的元素（但不适用于 visibility:hidden 的元素）。
```

##### slideToggle()

```javascript
//通过使用滑动效果，在显示和隐藏状态之间切换 <p> 元素
$(document).ready(function(){
  $(".btn1").click(function(){
  $("p").slideToggle();
  });
});
//slideToggle() 方法通过使用滑动效果（高度变化）来切换元素的可见状态。如果被选元素是可见的，则隐藏这些元素，如果被选元素是隐藏的，则显示这些元素。
//$(selector).slideToggle(speed,callback)
//可选。规定元素从隐藏到可见的速度（或者相反）。默认为 "normal"。可能的值：毫秒 （比如 1500）"slow""normal""fast"在设置速度的情况下，元素在切换的过程中，会逐渐地改变其高度（这样会创造滑动效果）。
//可选。toggle 函数执行完之后，要执行的函数。除非设置了 speed 参数，否则不能设置该参数。
//如果元素已经隐藏，则该效果不产生任何变化，除非规定了 callback 函数。
//toggle和slideToggle的区别:   toggle：动态效果为从右至左。横向动作。slideToggle：动态效果从下至上。竖向动作；两者都能实现对一个元素的显示和隐藏。
```

##### mouseenter()

```javascript
//移入
//  mouseover/mouseout事件，子元素被移入移出也会触发父元素的事件
//  mouseenter/mouseleave事件，子元素被移入移出不会触发父元素的事件（推荐使用）
$(".father").hover(function(){
    console.log("father被移入了");
},function(){//前一个方法是移入，后一个方法是移出
    console.log("father被移出了");
})//hover方法是调用了mouseenter和mouseleave方法
//当hover只有一个方法时，移出时调用一次，移入时调用一次
```

##### mouseover()

```javascript
//触发或将函数绑定到指定元素的mouse over事件	鼠标移过时
//当鼠标指针位于 <p> 元素上方时，设置背景色为黄色,当鼠标指针移开时，设置背景位灰色
$(document).ready(function(){
  $("p").mouseover(function(){
    $("p").css("background-color","yellow");
  });
  $("p").mouseout(function(){
    $("p").css("background-color","lightgray");
  });
});
//当鼠标指针位于元素上方时，会发生 mouseover 事件。mouseover() 方法触发 mouseover 事件，或添加当发生 mouseover 事件时运行的函数。注意：与 mouseenter 事件不同，mouseover 事件在鼠标指针进入被选元素或任意子元素时都会被触发，mouseenter 事件只有在鼠标指针进入被选元素时被触发。参见页面底部演示实例。提示：该事件通常与 mouseout 事件一起使用。
```

##### mouseleave()

```
//移出
```

##### mouseout( )	

```
//触发或将函数绑定到指定元素的mouse out事件	鼠标移出时
```

##### hide( )	

```
//显示匹配的元素
```

##### click( )	

```
//触发或将函数绑定到指定元素的click事件	单击鼠标时
```

##### dbclick( )	

```
//触发或将函数绑定到指定元素的dbclick事件	双击鼠标时
```

##### keydown( )	

```
//触发或将函数绑定到指定元素的keydown事件	按下键盘时
```

##### blur( )	

```
//触发或将函数绑定到指定元素的blur事件	失去焦点
```

------

### 																遍历方法

##### siblings

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20200925105211586.png" alt="image-20200925105211586" style="zoom: 50%;" />

```javascript
//返回带有类名 "start" 的每个 <li> 元素的所有同级元素：
<div style="width:500px;" class="siblings">
	<ul>ul (父节点)  
		<li>li (类名为"star"的上一个兄弟节点)</li>
		<li>li (类名为"star"的上一个兄弟节点)</li>
		<li class="start">li (兄弟节点)</li>
		<li>li (类名为"star"的下一个兄弟节点)</li>
		<li>li (类名为"star"的下一个兄弟节点)</li>
	</ul>   
</div>
<script>
$(document).ready(function(){
	$("li.start").siblings().css({"color":"red","border":"2px solid red"});
});
</script>
```

------

### 																																	静态方法

##### 静态方法each

```javascript
//js使用each静态方法
var arr=[1,3,5,7];
arr.forEach(function(value,index){
   console.log(index,value); 
});
//value 遍历到的元素
//index 当前遍历到的索引
//js使用forEach方法只能遍历数组，不能遍历伪数组 var obj={0:1,1:3,2:5,3:7,4:9,length:5}
//jQuery使用each静态方法
var arr=[1,3,5,7];
$.each(arr,function(index,value){
   console.log(index,value); 
});
//jQuery使用each方法能遍历数组和伪数组
```

##### 静态方法map

```javascript
var arr=[1,3,5,7];
arr.map(function(value,index,array){
   console.log(index,value,array); 
});
//value 遍历到的元素
//index 当前遍历到的索引
//array 当前遍历到的数组
//和js使用forEach方法只能遍历数组一样，js使用map方法也不能遍历伪数组
var arr=[1,3,5,7];
$.map(arr,function(index,value){
   console.log(index,value); 
});
//jQuery使用map方法能遍历数组和伪数组

//jQuery中的each静态方法和map静态方法的区别：1.each静态方法默认的返回值就是，遍历谁就返回谁；map静态方法默认是返回值是一个空数组   2.each静态方法不支持在回调函数中对遍历的数组进行处理;map静态方法可以在回调函数中通过return对遍历的数组进行处理，然后生成一个新的数组返回
var res=$.map(obj,function(index,value){
   console.log(index,value); 
    return value+index;
});
```

##### 静态方法trim

```javascript
var str="   lnj   "
var res=$.trim(str);
console.log("---"+str"---");
console.log("---"+res"---");
//作用: 去除字符串两端的空格
//参数: 需要去除的字符串
//返回值: 去除空格之后的字符串
```

##### 静态方法isWindow

```javascript
//真数组
var arr=[1,3,5,7,9];
//伪数组（有0-length下标,有length）
var arrlike={0:1,1:3,2:5,3:7,4:9,length:5}
//对象
var obj={"name":"lnj",age:"33"};
//函数（方法）
var fn=function(){};
//window对象
var w=window;

$.isWindow(arr);
console.log(w);
//判断传入的对象是否是window对象
//返回值true/false
```

##### 静态方法isArray

```javascript
$.isArray(arr);
console.log(w);
//判断传入的对象是否是真数组
//返回值true/false
```

##### 静态方法isFunction

```javascript
$.isFunction(arr);
console.log(w);
//判断传入的对象是否是真函数（方法）
//返回值true/false
//注意:jQuery本质是一个函数
```

##### 静态方法holdReady

```javascript
$.holdReady(true);
$(document).ready(function(){
   alert("ready"); 
});
//作用:暂停ready执行
```



------

### 										操控属性和属性节点的方法

##### attr

```javascript
<span class="span1" name="it666"></span>
<span class="span2" name="lnj"></span>
console.log($("span").attr("class"));
$("span").attr("class","box");
//作用:获取或者设置属性节点的值
//可以传递一个参数，也可以传递两个参数。如果传递一个参数，代表获取属性节点的值；如果传递两个参数，代表设置属性节点的值
//注意:     如果是获取：无论找到多少个元素，都只会返回第一个元素指定的属性节点的值
//          如果是设置：找到多少个元素就好设置多少个元素；如果设置的属性节点不存在，那么系统会自动新增
```

##### remove

```javascript
console.log($("span").removeAttr("class name"));
//注意：会删除所有找到元素指定的属性节点
```

##### prop

```javascript
<span class="span1" name="it666"></span>
<span class="span2" name="lnj"></span>
$("span").eq(0).prop("demo","it666");
$("span").prop("demo");
//特点和attr一样
//prop不仅能操作属性，还能操作属性节点
console.log($("span").prop("class"));
console.log($("span").prop("class","box"));
//prop和attr区别
<input type="checkbox" checked="checked"></input>
console.log($("input").prop("checked"));//true/false
console.log($("input").attr("checked"));//checked/undefined
//官方推荐在操作属性节点时，具有true和false两个属性的属性节点，如checked、selected或者disabled使用prop(),其他的使用attr()

//用input输入框中输入的图片地址更换下面图片
var btn=document.getElementsByTagName("button")[0];
//给按钮添加点击事件
btn.onclick=function(){
    //获取输入框输入的内容 
    var input=document.getElementsByTagName("input")[0];
    var text=input.value;
    //修改img的src属性节点的值
    $("img").attr("src",text);
    //因为src的返回值不是true/false，所有不适合用prop()
}
```

##### removeprop

```javascript
$("span").removeProp("demo");
//特点和removeAttr方法一样
```

------

### 														操作class类属性节点

##### addClass、removeClasss、toggleClass

```javascript
$("div").addClass("class1 class2")
//作用：添加一个类，如果要添加多个类，多个类名之间用空格隔开即可
$("div").removeClass("class1 class2")
//作用：删除一个类，如果要删除多个类，多个类名之间用空格隔开即可
$("div").toggleClass("class1 class2")
//作用：切换一个类，有就删除，没有就切换
```

------

### 														操作文本值

##### html、text、val

```javascript
$("div").html("<p>我是段落<span>我是span</span></p>");
console.log($("div").html());
//和原生js中的innerHTML一模一样
$("div").text("<p>我是段落<span>我是span</span></p>");
console.log($("div").text());
//和原生js中的innerText一模一样
$("input").val("请输入内容");
//和原生js中的value一模一样
```

------

### 															操作CSS

```javascript
//逐个设置
$("div").css("width","100px")
$("div").css("height","100px")
$("div").css("background","red")
//链式设置    链式操作如果大于3步，建议分开
$("div").css("width","100px").css("height","100px").css("background","red")
//批量设置
$("div").css({
    width:"100px",
    height:"100px",
    background:"red"
});
//获取CSS样式值
console.log($("div").css("width"));
```

------

### 														位置和尺寸操作

```javascript
//offset 作用：获取元素距离窗口的偏移位
console.log($(".son").offset().left);
//设置元素距离窗口的偏移位
$(".son").offset({
    left:10
});
//position 作用:获取元素距离定位元素的偏移位
console.log($(".son").postion().left);
$(".son").postion({
    left:10
});//没有效果
//注意position方法只能获取不能设置

//scrollTop 作用:获取滚动的偏移位
console.log($(".scroll").scrollTop());
//设置滚动的偏移位
$(".scroll").scrollTop(300);
```

------

### 															事件绑定

```javascript
//1.eventName(fn) 编码效率略高/部分事件jQuery没有实现，所以不能添加
$("button").click(function(){
   alert("hello lnj"); 
});
//2.on(eventName,fn) 编码效率略低/所有事件都可以添加
$("button").on("click",function(){
   alert("hello lnj"); 
});
```



------

### 														事件冒泡和默认行为

```javascript
//事件冒泡 return false/event.stopPropagation()
<div class="father">
    <div class="son"></div>
</div>
<a href="http://www.baidu.com">我是百度</a>
<form action="http://www.taobao.com">
    <input type="text"/>
    <input type="submit"/>
</form>
$(".son").click(function(event){
    alert("son");
    return false;//第一种方法阻止冒泡，只会弹出son
    event.stopPropagation();//第二种方法阻止冒泡，只会弹出son
})//点击son区域，father也会弹出
$(".father").click(function(){
    alert("father");
})

//默认行为
$("a").click(function(event){
    alert("弹出注册框");
    return false;//第一种方法阻止网页跳转默认行为
    event.preventDefault();//第二种方法阻止网页跳转默认行为
})
```

------

### 														事件自动触发

```javascript
//    trigger/triggerHandler
<div class="father">
    <div class="son"></div>
</div>
$(".son").click(function(event){
    alert("son");
});
$(".father").click(function(){
    alert("father");
});
$(".father").trigger("click");//自动触发事件，不需要经过监听鼠标点击，会触发事件冒泡
$(".son").triggerHandler("click");//自动触发事件，并且不会触发事件冒泡，只弹出son
$("input[type='submit']").click(function(){
   alert("submit"); 
});//点击按钮以后会跳转页面
$("input[type='submit']").trigger("click");//点击按钮以后也会跳转页面
$("input[type='submit']").triggerHandler("click");//点击按钮以后不会跳转页面
//总结：使用trigger会触发默认事件，也会触发事件冒泡；使用triggerHandler不会触发默认行为，也不会触发事件冒泡
//trigger和triggerHandler对<a>都不会触发默认行为，都无法跳转页面；可以在<a>中再添加一个标签，让trigger或triggerHandler设置里面的标签
```

------

### 																自定义事件

```javascript
$(".son").on("myClick",function(){
   alert("son"); 
});//myClick为自定义事件，trigger自动调用
$(".son").trigger("myClick");
//自定义事件必须满足两个条件：1.事件必须通过on绑定的 2.事件必须通过trigger来触发
```

------

### 															事件命名空间

```javascript
//命名空间的使用是为了区分不同人写的同样事件的代码
$(".son").on("click.zs",function(){
   alert("click1"); 
});
$(".son").on("click.ls",function(){
   alert("click2"); 
});
$(".son").trigger("click.ls");
//想要事件命名空间有效，必须满足两个条件：1.事件是通过on来绑定的 2.通过trigger触发事件

$(".father").on("click.zs",function(){
   alert("click1"); 
});
$(".father").on("click",function(){
   alert("click2"); 
});
$(".son").on("click.zs",function(){
   alert("click2"); 
});
$(".son").trigger("click.zs");
//利用trigger触发子元素带命名空间的事件，那么父元素带相同命名空间的事件也会被触发，而父元素没有命名空间的事件不会被触发
$(".son").trigger("click");
//利用trigger触发子元素不带命名空间的事件，那么子元素所有相同类型的事件和父元素所有相同类型的事件都会被触发
```

------

### 															事件委托

```javascript
//事件委托：请别人帮忙做事情，然后将做完的结果反馈给我们
<ul>
    <li>我是第1个li</li>
	<li>我是第1个li</li>
	<li>我是第1个li</li>
</ul>
<button>新增一个li</button>
$("button").click(function(){
   $("ul").append("<li>我是新增的li</li>") ;
});
$("button").click(function(){
   console.log($(this).html());
});//这样查询出来的li只能查询页面加载之前的li,页面加载之后新增的li无法查询出来，即入口函数没有加载之前新增的li无法载入
//在jQuery中，通过核心函数找到的元素不止一个，那么在添加事件的时候，jQuery会遍历所有找到的元素，给所有找到的元素添加事件

$("ul").delegate("li","click",function(){
    console.log($(this).html());
});//把li的click事件委托给ul，因为在入口函数加载之前ul就一直存在，所以新增的li也能查询到，冒泡传递
```



------



### 														代码实现

------

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20200923101541124.png" alt="image-20200923101541124" style="zoom: 67%;" />

```javascript
		<script src="scripts/jquery.js" type="text/javascript"></script>
		<script type="text/javascript">
			//导航栏效果
			$(function() {
				$("#nav li").hover(function() {
					$(this).find(".jnNav").show();
				}, function() {
					$(this).find(".jnNav").hide();
				});
			})
		</script>
```

------

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20200923101558110.png" alt="image-20200923101558110" style="zoom: 50%;" />

```javascript
<script type="text/javascript" src="./js/jquery-1.9.1.js" ></script>
//用jQuery实现checkbox的全选和反选
		<script>
			//全选事件
            function selectAll(checkObj){
                $(".itemSelect").prop("checked", $(checkObj).prop("checked"));
			}
            $(".itemSelect").click();//反选即为每个复选框执行依次点击事件，即可实现反选
            //所有.itemSelect注册点击事件，注册点击事件的目的是在多选框被全选中的情况下，也能使多选按钮被选中。
            $(function() {
                $(".itemSelect").click(function(){
                    $(".itemSelect:checked").length == $(".itemSelect").length ? $("#allCheck").prop("checked", true) : $("#allCheck").prop("checked", false);
                });
            });
            //反选事件：
            function reverseBtn(){
            }
		</script>
```

------

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20200923101739809.png" alt="image-20200923101739809" style="zoom: 50%;" />

```javascript
<script language="javascript" type="text/javascript" src="js/jquery-1.9.1.js"></script>
//用jQuery实现列表的隐藏和显示
		<script type="text/javascript">
			$(function() { //页面加载事件
				$(".clsHead").click(function() { //图片点击事件
					if($(".clsContent").is(":visible")) { //如果内容可见
						$(".clsHead span img").attr("src", "Images/a1.gif"); //改变图片
						$(".clsContent").css("display", "none"); //隐藏内容 
					} else {
						$(".clsHead span img").attr("src", "Images/a2.gif"); //改变图片
						$(".clsContent").css("display", "block"); //显示内容
					}
				});

				$(".clsBot > a").click(function() { //热点链接点击事件
					if($(".clsBot > a").text() == "简化") { //如果内容为'简化'字样
						$("ul li:gt(4):not(:last)").hide(); //隐藏index号大于4且不是最后一项的元素
						$(".clsBot > a").text("更多"); //将字符内容更改为"更多"
					} else {
						$("ul li:gt(4):not(:last)").show().addClass("GetFocus"); //显示所选元素且增加样式
						$(".clsBot > a").text("简化"); //将字符内容更改为"简化"
					}
				});
			});
		</script>
```

------

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20200923101747269.png" alt="image-20200923101747269" style="zoom:50%;" />

```javascript
<script type="text/javascript" src="js/jquery-1.9.1.js" ></script>
<script type="text/javascript">
			//隔行变色
			$(function(){
				//使用过滤选择器中的偶数选择器
				$("dl:even").css("background-color","blue");
			})
		</script>
```

------

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20200923101831262.png" alt="image-20200923101831262" style="zoom:50%;" />

```javascript
<script src="js/jquery.min.js" type="text/javascript"></script>
//用jQuery实现提示框跟随鼠标位置进行移动
	<script type="text/javascript"> 
	$(function () {
	        var x = 15;
	        var y = 10;
	        var newtitle = '';
	        $('.mytooltip').mouseover(function (e) {
	            newtitle = this.title;
	            this.title = '';
	            $('body').append('<div id="mytitle" >' + newtitle + '</div>');
	            $('#mytitle').css({
	                'left': (e.pageX + x) + 'px',
	                'top': (e.pageY + y)  + 'px'
	            }).show();
	        }).mouseout(function () {
	            this.title = newtitle;
	            $('#mytitle').remove();
	        }).mousemove(function (e) {
                //此处e是鼠标  e.pageX是水平距离  e.pageY是垂直距离
	            $('#mytitle').css({
	                'left': (e.pageX + x )+ 'px',
	                'top': (e.pageY + y ) + 'px'
	            }).show();
	        });
	    });		
		</script>
```

------

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20200923101900254.png" alt="image-20200923101900254" style="zoom:50%;" />

```javascript
<script src="js/jquery.min.js" type="text/javascript"></script>
//用jQuery实现在获取焦点和失去焦点时input输入框颜色的变化
	<script>
		$(function(){
			$("#name").focus(function(){
				$("#name").css("border","red 2px solid");
			});
			$("#name").blur(function(){
				$("#name").css("border-width","1px").css("border-color","gray");
			});
			$("#password").focus(function(){
				$("#password").css("border-style","solid").css("border-width","2px").css("border-color","#c9302c");
			});
			$("#password").blur(function(){
				$("#password").css("border-width","1px").css("border-color","gray");
			});
			$("#submit").focus(function(){
				$("#submit").css("border","red 2px solid");
			});
			$("#submit").click(function(){
				$("#submit").css("border","red 2px solid");
			});
			$("#register").click(function(){
				$("#register").css("border","red 2px solid");
			});
		});
	</script>
```

------

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20200923102008846.png" alt="image-20200923102008846" style="zoom:67%;" />

```javascript
<script type="text/javascript" src="js/jquery.min.js"></script>
<script type="text/javascript">
    //用jQuery实现信息的动态增加和删除操作
			$(function() {
				//定位"增加"按钮，同时添加单击事件
				$("#addID").click(function() {
					//获取姓名和联系方式的值
					var name = $("#name").val();
					var phone = $("#phone").val();
					//去掉二边的空格
					name = $.trim(name);
					phone = $.trim(phone);
					//如果姓名和联系方式没有填
					if(name.length == 0 || phone.length == 0) {
						//提示用户                  
						alert("姓名或联系方式没有填");
					} else {
						//创建1个tr标签
						var $tr = $("<tr></tr>");
						//创建3个td标签
						var $td1 = $("<td><input type='checkbox' /></td>");
						var $td2 = $("<td>" + name + "</td>");
						var $td3 = $("<td>" + phone + "</td>");
						var $td4 = $("<td></td>");
						//创建input标签，设置为删除按钮
						var $del = $("<input type='button' value='删除'>");
						//为删除按钮动态添加单击事件
						$del.click(function() {
							//删除按钮所有的行，即$tr对象
							$tr.remove();
						});
						//将删除按钮添加到td3标签中
						$td4.append($del);
						//将3个td标签依次添加到tr标签中
						$tr.append($td1);
						$tr.append($td2);
						$tr.append($td3);
						$tr.append($td4);
						//将tr标签添加到tbody标签中
						$("#tbodyID").append($tr);
						//清空用户名和联系方式文本框中的内容
						$("#name").val("");
						$("#phone").val("");
					}
				});
				//全选中和全取消
				//定位tfoot中的全选复选框，同时添加单击事件
				$("#checkboxAll").click(function() {
					//获取该全选复选框的状态
					var flag = this.checked;
					//如果选中
					if(flag) {
						//将tbody中的所有复选框选中
						$("tbody input:checkbox").prop("checked", "checked");
						//如果未选中
					} else {
						//将tbody中的所有复选框取消
						$("tbody input:checkbox").removeAttr("checked");
					}
				});
				$("#btnDel").click(function() {
					var checkbox = $("td input:checkbox");
					for(var i = 0; i < checkbox.length; i++) {
						var flag = checkbox[i].checked;
						//如果选中
						if(flag) {
							//删除
						checkbox[i].parentNode.parentNode.remove();
						}
					}
				});
			});
		</script>
```

------

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20200923115106911.png" alt="image-20200923115106911" style="zoom:50%;" />

```javascript
<script type="text/javascript">
			$(function() {
				//流程
				$("#tab_bg > div").hover(function() {			
					$(this).siblings('div').removeClass('tab_current').addClass('tab_common');	
					$(this).removeClass('tab_common').addClass('tab_current');
					$(this).parents('#tab_bg').removeClass('tab_bg0 tab_bg1 tab_bg2 tab_bg3 tab_bg4').addClass('tab_bg' + $(this).index());
				});

			});
		</script>
```

------

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20200923115153769.png" alt="image-20200923115153769" style="zoom:50%;" />

```javascript
 <script>
		$(function(){
			var arrs = [];
			arrs[1] = ["济南","临沂","青岛","威海","济宁","聊城","德州"];
			arrs[2] = ["南京","苏州","徐州","连云港","宿迁","淮安","盐城","扬州"];
			$("#province").change(function(){
				var province= $("#province").find("option:selected").index();
				var citys=arrs[province];
				var $op0=$("<option>请选择城市</option>");
				$("#city").find("option").remove();
				$("#city").append($op0);
				for(var i=0;i<citys.length;i++){
					var $op=$("<option>"+citys[i]+"</option>");
					$("#city").append($op);
				}
			});
		});
	</script>
```

------

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20200923115404598.png" alt="image-20200923115404598" style="zoom:50%;" />

```javascript
<script>
		$(function(){
				$("#xianshi").click(function(){
					if($("#xianshi a").text()=="精简显示品牌"){
						$("ul li:gt(5):not(:last)").hide();
						$("#xianshi a").text("显示全部品牌");
						$(".gaoliang").css("color","blue");
					}
					else{
						$("ul li:gt(5):not(:last)").show();
						$(".gaoliang").css("color","#D2691E");
						$("#xianshi a").text("精简显示品牌");
					}
				}
				)
		}
		);
	</script>
```

