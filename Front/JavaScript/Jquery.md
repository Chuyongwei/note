# JQuery

### 不同

1.jquery的ready是在图片加载前运行，js的onload在图片加载后运行

```js
  window.onload = function (ev) {
            var img = document.getElementsByTagName("img")[0];
            console.log(img);

            var width = window.getComputedStyle(img).width;
            console.log("onload", width)
        }
        $(document).ready(function () {
            var $img = $("img")
            console.log($img)
            var $width = $img.width();
            console.log("ready", $width)
        })
```

### 写法

```js
        1.$(document).ready(function(){        });
        2.jQuery(document).ready(function(){        })
        3.$(function(){        })
        4.jQuery(function(){        })
```

### 解决冲突

```
jQery.noConflict();
```

## **元素**

### 对象的类

构造：

```js
        function AClass() {}
```

​	静态方法：

```js
AClass.staticMethod = function(){
            alert("sdaf")
        }
        AClass.staticMethod();
```

​	实例方法：

```js
        AClass.prototype.hh = function(){
            alert("sdf")
        }
        var pro = new AClass();
        pro.hh();
```

### 数组：

​	原生的forEach只能遍例数组，不能遍历伪数组

```js
        var arr=[1,2,3,4];
        arr.forEach(function(value,index){
            console.log(index,value)
        })
	arr.map(function(value,index,array){
      console.log(index,value,array)

   })
```

jQuery可以遍历伪数组

map可以返回新结构的数组

each不行

```js
        var obj={0:1,1:3,2:5,3:7,4:9, length:5};
        var res2 = $.each(obj,function(index,value){
            console.log(index,value)
        })

        console.log(res2)
        $.map(obj,function(index,value){
            console.log(index,value)
            return value+index
        })
```

### 字符

切割数组

```
        var str = "  wcy    ";
        var res = $.trim(str)
        console.log(str)
        console.log(res)
```

判断类型

```
		$.isWindow(sa)
		$.isObject()
		$.isFunction()
```

#### 暂停

` $.holdReady(true);`暂停script

### 元素选择器

```js
var $div = $("div:parent")
```

`:empty`找没有子元素没有文本的

`:parent`找有子元素或有内容的

`:contains('')`找包含指定文本的内容的元素

`:has(‘span’)`找包含指定子元素的元素

`$("input[type='submit']")`submit类型的input

`$(this).index`批处理的索引

`$("ul").eq(0)`第一个ul

`$( ).siblings()`获取除了自己其他的选中元素

`$("url").children.children("div")`所有的div子标签

### attr方法

```js
$("span").attr("class","box")//设置值
$("span").attr("class")//查看值
```

- 设置：
  - 得到多少改多少
  - 没有就创建
- 获取
  -  无论获取多少个元素，只返回一个

```js
$("span").removeAttr("a b")//同时删除a和b
```



<!-- 2020.01.13 -->

### prop方法

```js
 $("span").eq(0).prop("demo","in")//设置属性
 $("span").eq(0).prop("demo")//查看属性
 $("span").removeProp("dome")//删除获取的元素的节点
```

### 类操作

addClass添加类

deleteClass删除类

removeClass

toggleClass修改类

```js
span.addClass("a b")
```

### 文本操作

```js
//innerHTML
$("div").html("<p>我是段落<span>我是span</span></p>")//添加html文本
$("div").html()//
//innerText
$("div").text("")
$("div").text()
//value
$("input").val("请输入内容")
$("input").val()
```

### 设置css

1.逐步操作

```js
 $("div").css("width","100px")
 $("div").css("height","100px")
 $("div").css("background","red")
```

2.链式操作

```js
$("div").css("width","100px").css("height","100px").css("background","red")
```

3.对象操作

```js
$("div").css({
    width: "100px",
    height: "100px",
    background: "yellow"
})
```



### 设置长宽

offset().left获取距离窗口的偏移位

修改：

```css
offset({
	left:10
})
```

postion().获取定位元素的偏移位

postion不能修改

### 滚动

滚动监听

```js
$(window).scroll(function(){
    //获取滚动位置
    var offset = $("html,body").scrollTop();
}
```

获取滚动距离

```js
$(".scroll").scrollTop()
```

获取网页滚动距离

```js
$("html").scrollTop()+$("body").scrollTop
```

```js
$.("html","body").scrollTop(300)
```

## 事件

> 可以使用多个事件

1.快捷操作

```js
$("button").click(function(){
    alert("hello inj")
})
```

2.可以使用所有js

```js
$("button").on("click",function(){

})
```

**删除**

`$("button").off()`取消所有事件 

`$("button").off("click")`取消指定类型的事件

`$("button").off("click",test)`取消指定类型的指定事件

### 事件冒泡

父亲和儿子的事件都会发生

阻止事件冒泡

```js
$(".son").click(function(event){
    alert("son")
    //return false;
    event.stopPropagation()
})
```

默认行为

​	阻止默认行为

```js
$(".son").click(function(event){
    alert("son")
    //return false;
    event.preventDefault()
})
```

### 自动触发

```
trigger(事件名)
```

`$(".father").trigger("click")`和`$(".father").triggerHandler("click")`自动触发

**区别**：

如果使用trigger自动触发会事件冒泡和默认事件

triggerHandler不会触发事件冒泡

### 自定义事件

条件：

1.事件必须通过on绑定

2.事件通过trigger触发

```js
$(".son").on("myClick",function(){
    alert("s")
})
$(".son")。trigger("myClick")
```

事件命名空间

我们可以将事件名变为`action.usename`

```js
$(".son").trigger("click.ls")//选择触发事件
```

> 带命名空间的子元素会触发带有相同命名空间的相同事件的父元素
>
> 不带命名空间的子元素会触发所有相同事件的父元素事件

### 事件委托

请别人帮忙做事情，然后将做完的事情结果反馈给我

在jQuery种当找到很多个元素添加事件时会遍历

找一个从入口函数就有的元素来完成其他元素的事件新增

```js
$("ul").delegate("li","click",function(){
    console.log($(this).html())
})
```

### 移入和移出

```js
$(function(){
    //子元素的移入和移出也会触发方法
    // $(".father").mouseover(function(){
    //     console.log("father被移入")
    // })
    // $(".father").mouseout(function(){
    //     console.log("father被移出")
    // })

    //子元素的移入和移出不会触发方法
    // $(".father").mouseenter(function(){
    //     console.log("father被移入")
    // })
    // $(".father").mouseleave(function(){
    //     console.log("father被移出")
    // })

    //若参数一个方法移动都触发，两个（移入，移出）
    $(".father").hover(function(){
        console.log("father移入")
    },function(){
        console.log("father移出")
    })
})
```

## 动画效果

```js
//弹出动画
$("div").show(1000,function(){
})
//消失动画
$("div").hide(1000,function(){
})
//切换动画
$("div").toggle(1000,function(){
})
//拉起
 $("div").slideUp(1000, function () {
 })
//拉下
$("div").slideDown(1000, function () {
})
//淡入
$("div").fadeIn(1000,function(){
})
//淡出
$("div").fadeOut(1000,function(){
})
//切换
$("div").fadeToggle(1000,function(){
})
//淡入到0.2
$("div").fadeTo(1000,0.2,function(){
})
```

使用连续动画

```js
$(".ad").stop(1000).slideDown(1000).fadeOut(1000).fadeIn(1000)
```

stop用于规避动画

### 自定义动画

animate
1.接受一个对像
2.指定时长
3.指定节奏默认“swing”
4.动画执行完后的回调函数

```js
$(".one").animate({
    width:500//变为宽度500
},1000,function(){
})
$(".one").animate({
    marginLeft:500//右移500
},1000,function(){
})
$(".one").animate({
    width: "+=100"//自增100
},1000,function(){
})
 $(".one").animate({
     width: "toggle"
 },1000,function(){
 })
```

`animate().delay(2000)`等待两秒

stop

```js
// 立即停止当前动画, 继续执行后续的动画
$("div").stop();
$("div").stop(false);
$("div").stop(false, false);
// 立即停止当前和后续所有的动画
$("div").stop(true);
$("div").stop(true, false);
// 立即完成当前的, 继续执行后续动画
$("div").stop(false, true);
// 立即完成当前的, 并且停止后续所有的
$("div").stop(true, true);
```

## 节点

### 添加

内部插入

```js
var $li = $("<li>新增的li</li>")
$li.appendTo("ul")
$("ul").append($li)//尾插
$li.prependTo("ul")
$("ul").prepend($li)//头插
```

外部插入

```js
 $("ul").after($li)//在ul的后面插入
 $li.insertAfter("ul")
 $("ul").before($li)//ul前面插入
```

### 删除

```js
$("li").empty() //清空内容
$("li").remove(".item") //删除.item的div
```

### 替换

```js
var $h6 = $("<h6>我是标题6</h6>") 
$h6.replaceAll("div")
$("div").replaceWith($h6)
```

### 复制

> 浅复制只会复制元素不会复制事件
> 深复制会将元素和事件都复制

```js
$("button").eq(0).click(function () {//深复制
   var $li = $("li:first").clone(false);
   $("ul").append($li)
})
$("button").eq(1).click(function () {//浅复制
    var $li = $("li:first").clone(true);
   $("ul").append($li)
})
$("li").click(function(){
    alert("弹了")
})
```



<!--江南老师的p49-p51 p56-p134-->

