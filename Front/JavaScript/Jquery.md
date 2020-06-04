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

```
        1.$(document).ready(function(){        });
        2.jQuery(document).ready(function(){        })
        3.$(function(){        })
        4.jQuery(function(){        })
```

### 解决冲突

```
jQery.noConflict();
```

## 元素

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

类操作

addClass添加类

deleteClass删除类

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
                width:"100px",
                height:"100px",
                background:"yellow"
            })
```



### 设置长宽

offset().left获取距离窗口的偏移位

修改：

```js
offset({
	left:10
})
```

postion().获取定位元素的偏移位

postion不能修改

### 滚动

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

