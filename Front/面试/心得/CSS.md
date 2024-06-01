### 回流

元素改变 尺寸，宽高，边框，内容，位置 都会引起重排，导致需要重新构建页面的时候

- 增删可见的 DOM 元素的时候
- 元素的位置发生改变
- 元素的尺寸发生改变
- 内容改变
- 页面第一次渲染的时候

### 盒子模型

width = content宽度 + padding + border

- `border`
- `margin`
- `content`
- `padding`

### 选择器

#### 特殊符号选择器

- 群组选择器（’,’）

- 子元素选择器（空格）

  `div li`div下所有的li标签

- 直接子元素选择器（’>’）

  `div>li`div下以div为直接父元素的li标签

- 相邻兄弟选择器（’+’）

  `div+li`div后面首个li标签

- 兄弟选择器（’~’）

  `div~li`与div相同父元素的li元素

#### 属性选择器

- [div]

#### 伪类选择器

- `div:hover`：鼠标经过过的元素
- `div:active`：点击的元素
- `div:link`:鼠标未访问的元素
- `div:visited `：鼠标访问的元素

- `nth-child(n)`：子元素选择器

### 嵌入

#### html内

```html
<head>
 
<title></title>
 
<style type="text/css">
 
p{
 
background-color:yellow;
 
}
 
</style>
 
</head>
```

#### 导入

```html
 
<head>
 
<title></title>
 
<link href="xxx.css" rel="stylesheet" type="text/css"/>
 
</head>
```

### 布局

#### position

- `relative`
- `absolution`
- `fixed`
- `inherit`

#### flex布局

```html
display:felx
```



### [BFC规范](https://juejin.cn/post/6950082193632788493)

> 用于避免浮动元素的影响的元素使用bfc

`BFC`是一个完全独立的空间（布局环境），让空间里的子元素不会影响到外面的布局。那么怎么使用`BFC`呢，`BFC`可以看做是一个`CSS`元素属性

这里简单列举几个触发`BFC`使用的`CSS`属性

- overflow: hidden
- display: inline-block
- position: absolute
- position: fixed
- display: table-cell
- display: flex

#### BFC的规则

- `BFC`就是一个块级元素，块级元素会在垂直方向一个接一个的排列
- `BFC`就是页面中的一个隔离的独立容器，容器里的标签不会影响到外部标签
- 垂直方向的距离由margin决定， 属于同一个`BFC`的两个相邻的标签外边距会发生重叠
- 计算`BFC`的高度时，浮动元素也参与计算





### Float

#### 三栏方式

- flex

  ```html
  <!DOCTYPE html>
  <html lang="en">
   
  <head>
      <title>flex布局</title>
      <style>
          .main{
              height: 60px;
              display: flex;
          }
   
          .left,
          .right{
              height: 100%;
              width: 200px;
              background-color: #ccc;
          }
   
          .content{
              flex: 1;
              background-color: #eee;
          }
      </style>
  </head>
   
  <body>
      <div class="main">
          <div class="left"></div>
          <div class="content"></div>
          <div class="right"></div>
      </div>
  </body>
   
  </html>
  ```

- 圣杯布局

  ```html
  <!DOCTYPE html>
  <html>
      <head>
          <meta charset=utf-8>
          <style type="text/css">
          * {
                  margin: 0;
                  padding: 0;
          }
          .container {
              border: 1px solid black;
              /* 防止容器盒子高度塌陷和给之后的左、右浮动元素预留位置 */
              overflow: hidden;
              padding: 0px 100px;
              min-width: 100px;
          }
   
          .left {
              background-color: greenyellow;
              /* 保证之后的"margin-left"属性可以将自身拉到上一行 */
              float: left;
              /* 固定宽度 */
              width: 100px;
              /* 将元素向左移动属性值的单位，100%相对于父容器计算 */
              margin-left: -100%;
              /* 相对定位，需要将自身再向左移动自身的宽度，进入容器的"padding-left"区域 */
              position: relative;
              /* 自身的宽度，刚好进入容器的"padding-left"区域 */
              left: -100px;
          }
   
          .center {
              background-color: darkorange;
              float: left;
              width: 100%;
          }
   
          .right {
              background-color: darkgreen;
              float: left;
              width: 100px;
              margin-left: -100px;
              position: relative;
              left: 100px;
          }
          </style>
      </head>
      <body>
      	<section class="container">
              <article class="center"><br /><br /><br /></article>
              <article class="left"><br /><br /><br /></article>
              <article class="right"><br /><br /><br /></article>
          </section>
      </body>
  </html>
  ```

  

### 单位

1. 绝对长度单位：**px 像素**
2. 百分比: **%**
3. 相对**父**元素字体大小单位: **em**
4. 相对于**根**元素字体大小的单位: **rem**（默认**16px**）
5. 相对于视口*宽度的百分比（100vw即视窗宽度的100%）: **vw**
6. 相对于视口*高度的百分比（100vh即视窗高度的100%）: **vh**

### opacity: 0、visibility: hidden、display: none⭐⭐⭐

| 区别     | opacity: 0 | visibility: hidden | display: none |
| -------- | ---------- | ------------------ | ------------- |
| 页面布局 | 不改变     | 不改变             | 改变          |
| 触发事件 | 能触发     | 不能触发           | 不能触发      |

### 行内素、块级元素和行内块元素⭐⭐⭐

```css
display:inline;// 转换为行内元素
display:block;// 转换为块级元素
display:inline-block// 转换为行内块元素
```

块标签：div、h1~h6、ul、li、table、p、br、form。
特征：独占一行，换行显示，可以设置宽高，可以嵌套块和行


行标签：span、a、img、textarea、select、option、input。
特征：只有在行内显示，不会自动进行换行，内容撑开宽、高，不可以设置宽、高（img、input、textarea等除外）。（设置float后可以设置宽、高）

 

### 溢出

单行多行都要添加

```css
overflow: hinden
```

单行
定元素内的空白处理：white-space:nowrap; 文本不进行换行；默认值normal

```css
overflow: hidden;
text-overflow:ellipsis;  //ellipsis;省略
white-space: nowrap;  //nowrap 不换行
```


多行


1.-webkit-line-clamp用来限制在一个块元素显示的文本的行数。 为了实现该效果，它需要组合其他的WebKit属性。常见结合属性：
2.display: -webkit-box; 必须结合的属性 ，将对象作为弹性伸缩盒子模型显示 。
3.-webkit-box-orient 必须结合的属性 ，设置或检索伸缩盒对象的子元素的排列方式 。 

 IE不兼容

```html
<!DOCTYPE html>
<html>
<head> 
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
<style> 
.text2{
display: -webkit-box;    
-webkit-box-orient: vertical;    
-webkit-line-clamp: 3;    
overflow: hidden;
}
</style> 
	
</head>
<body>
 
<div class="text2">
    这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话
    这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话
    这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话这是一句话
</div>
 
</body>
</html>
```

### 布局

#### 静态布局

- 传统的设置固定大小，高度宽度

#### 弹性布局

- flex布局

- 百分比

- 媒体查询

  ```css
  /* 在 screen 类型 大于560px 小于 700px 加载 */
      @media screen and (min-width: 560px) and (max-width: 700px) {
        .box1 {
          background-color: burlywood;
        }
      }
  ```

  

### 导入

```css
@import url("http://....")
```

