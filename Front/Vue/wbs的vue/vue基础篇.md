# Vue基础篇

***陈华旺-chenhuawang@itany.com***

[TOC]

## 1、设计模式

### 1.1、SPA 

- SPA：single page application  单页应用程序
  - SPA应用将所有的活动局限于一个 Web 页面中，仅在该 Web 页面初始化时加载相应的 HTML 、js 、CSS 
  - SPA应用一旦页面加载完成， SPA 不会因为用户的操作而进行页面的重新加载或跳转
  - SPA应用利用 JavaScript 动态的变换 HTML，从而实现UI与用户的交互![1552360436609](./assets/1552360436609.png)

### 1.2、MVVM 

- mvvm：model-view -viewModel  模型 视图 视图模型
  - 模型：指的是构成页面内容的相关数据（包含：前端定义的数据，后端传递的数据）
  - 视图：指的是呈现给开发这和用户查看的展示数据的页面
  - 视图模型：mvvm设计模式的核心思想，它是连接view和model的桥梁。

![1564541437944](assets/1564541437944.png)

> Tips：前端实现MVVM设计思想的框架（Vue,React,Angular,微信小程序……），其目的都是为了高度封装view-model 的交互过程，让开发这只用关心页面构成和数据构成，无需花费大量时间关心数据和页面的状态关系

## 2、Vue简介

+ Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**
+ Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合
+ Vue借鉴React和Angular的部分代码设计，并提高了易用性和轻量化

## 3、Vue的页面基本使用

- 获取vue的核心语法库

  - 通过地址 <https://cdn.jsdelivr.net/npm/vue/dist/vue.js> 下载vue核心语法包
  - 使用  npm 进行Vue语法库的下载 

- 页面在Vue库

  ```html
  <script src="../js/vue.js"></script>
  ```

  - 页面装载Vue核心语法后，会在浏览器window对象中提供一个全局的构造方法**Vue**![1564542961030](assets/1564542961030.png)![1564543042694](assets/1564543042694.png)
  - **Vue函数为一个JS对象构造器，使用时需要通过 new 关键字进行 Vue 对象创建**

- 页面基本关联和应用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="../js/vue.js"></script>
    <script>
        window.onload = function(){
            var vue = new Vue({
                el:"#app",
                data:{
                    info:"测试页面数据展示"
                }
            });
        }
    </script>
</head>
<body>
    <div id="app">
        <h1>{{ info }}</h1>
        <input type="text" v-model="info">
    </div>
</body>
</html>
```

+ Vue官方开发调试工具 `vue devtools`：工具在开发环境下可以实现浏览器对vue功能的基本监控

  ![1564543554047](assets/1564543554047.png)

![1564543757382](assets/1564543757382.png)

## 4、Vue的全局环境配置

+ 在vue项目运行启动前，对vue的运行环境进行相关功能设置
  + 开启关闭调试工具，关闭开启控制台日志和警告，关闭开启调试工具……
  + 所有的Vue全局环境设置依赖于Vue的全局配置对象**`Vue.config`**

```js
// 取消 Vue 所有的日志与警告 , 取值类型：boolean 默认值：false
Vue.config.silent = true; 

//配置是否允许 vue-devtools 检查代码 , 取值类型：boolean
//开发版本默认为 true，生产版本默认为 false。生产版本设为 true 可以启用检查。
Vue.config.devtools = true

// 设置为 false 以阻止 vue 在启动时生成生产提示 , 取值类型：boolean 默认值：true
Vue.config.productionTip = false;
```

## 5、基本交互

### 5.1、插值表达式

+ 语法：`Mustache语法`  `{{  }}`

+ 功能：取决于JS框架为其赋予的 功能特性 == **在不同的框架下 语法功能能不同**

+ **特性：响应式数据功能：**

  + HTML标签通过插值表达式绑定Vue的数据变量，当变量发生变化时该标签会重新渲染加载。
  + 插值表达式只能对 Vue 数据变量变化作为响应，而无法修改变量

+ 对于Vue框架而言：只能被用于 html 标签的主体内容中，不能用于标签的其它位置

+ 对于Vue框架而言：只能绑定对应 Vue对象数据仓库中定义的变量，或者 **简单的 JS 表达式** 和 **JS 内置对象**

  ```html
  <标签>{{ Vue对象数据仓库变量|JS表达式|JS内置对象 }}</标签>
  ```

  > Tips:当Vue数据仓库中变量名称和JS内置对象名称相同时，Vue将优先使用仓库中变量

+ 对于不同类型的数据 ，为保证页面输出正确结果，vue插值表达式 对所有 变量调用了自定义的**toString()**方法

  ```js
  var _toString = Object.prototype.toString;
  
  function isPlainObject (obj) {
      return _toString.call(obj) === '[object Object]'
  }
  
  function toString (val) {
      return val == null 
      ?'': Array.isArray(val) || (isPlainObject(val) && val.toString === _toString)
          ? JSON.stringify(val, null, 2)
      	: String(val)
  }
  ```

+ 值表达式 底层调用的 是 DOM 对象的  textContent 属性 进行值得写入操作

  - **html格式字符串将不被解析**
  - js 转义符将不被识别

### 5.2、基础指令

- 为开发者 提供 在页面中进行 特殊功能的属性描述方法

  - 语法：Vue指令以 `v-名称` 结构定义
  - 位置：**指令只能被用于 html 容器的标签属性上**   `<标签 v-指令="" ></标签>`
  - 实现：指令本身实际上就是一个JS方法的特殊封装，页面定义的指令只是对方法的调用和触发
  - 功能：通过指令可实现 HTML标签写入，标签判断、标签循环、标签事件绑定、标签属性绑定……

- 完整语法：

  - `v-指令名[:参数][.修饰符][=取值]`

  - `v-指令名[:参数][.修饰符][="取值"]`

    > Tips：
    >
    > ​	1、普通指令取值范围和插值表达式基本一致，可取Vue数据仓库中定义的变量，可取匿名变量，可取JS内置对象、可进行简单的四则运算；
    >
    > ​	2、对于特殊指令`v-on`只能绑定Vue方法仓库中的自定义方法，或绑定简单JS表达式

- **指令特性：无痕迹特性==代码开发时标签上的vue语法表达式，在项目运行时会被删除，不会保留**

  > vue对象和容器完成语法解析后，不会在浏览器上保留语法定义规范

#### 1、v-text

- 取值：`string`
- 功能：更新元素的 `textContent`。如果要更新部分的 `textContent` ，需要使用 `{{ Mustache }}` 插值。
- 示例：`<span v-text="msg"></span>`

#### 2、v-html

- 取值：`string`
- 功能：更新元素的 `innerHTML` 
- 示例：`<div v-html="html"></div>`

#### 3、v-pre

- 取值：**不需要表达式**，该指令为boolean类型属性
  - 写表示 true（启用功能） 不写表示 false（不启用功能）
- 功能：跳过这个元素和它的子元素的编译过程。可以用来显示原始 Mustache 标签。跳过大量没有指令的节点会加快编译。
- 示例：`<span v-pre>{{ 该语法会直接显示在页面 }}</span>`

#### 4、v-once

- 取值：**不需要表达式**，该指令为boolean类型属性
  - 写表示 true（启用功能） 不写表示 false（不启用功能）
- 功能：对当前元素和内部元素vue功能执行**一次**，程序执行过程不在对该元素范围内的vue功能进行重新执行
- 示例：`<span v-once>该区域vue功能只在初始化时执行一次 {{msg}}</span>`

#### 5、v-cloak

- 取值：**不需要表达式**，该指令为boolean类型属性

- 功能：实现在vue功能构建完成前，隐藏浏览上vue语法表达式，**该指令本身不具有特殊功能，需配合css样式实现效果**

- 示例：

  ```css
  [v-cloak] {
    display: none;
  }
  ```

  ```html
  <div v-cloak>
    {{ message }}
  </div>
  ```

> Tips：v-cloak指令功能主要利用了 **指令特性** + css 样式实现，因此所有的vue指令都可以实现该功能

#### 6、v-on

- 缩写：`@` ，项目中可用 `@` 替代 `v-on`

- 语法：

  ```html
  <button v-on[:参数][.修饰符]="取值"></button>
  <button @[:参数][.修饰符]="取值"></button>
  ```

- 取值：`Function | Inline Statement | Object | Array`

- 参数：eventName（事件名称）

- 功能：绑定元素事件监听器，事件类型由参数指定

- v-on 绑定的事件必须是vue对象 **方法仓库** 中的一个自定义方法

  ```js
  new Vue({
      // 当前vue实例对应的方法仓库
      methods:{
  		
      }
  })
  ```

- **修饰符**：

  - `.stop` - 调用 `event.stopPropagation()`。

  - `.prevent` - 调用 `event.preventDefault()`。

  - `.capture` - 添加事件侦听器时使用 capture 模式（事件捕获模式）

  - `.self` - 只当事件是从侦听器绑定的元素本身触发时才触发回调。

  - `.{keyCode | keyAlias}` - 只当事件是从特定键触发时才触发回调。

  - `.native` - 监听组件根元素的原生事件。

  - `.once` - 只触发一次回调。

  - `.left` - (2.2.0) 只当点击鼠标左键时触发。

  - `.right` - (2.2.0) 只当点击鼠标右键时触发。

  - `.middle` - (2.2.0) 只当点击鼠标中键时触发。

  - `.passive` - (2.3.0) 以 `{ passive: true }` 模式添加侦听器（启用默认事件功能）

    > Tips：addEventListener() 事件绑定中的 passive 属性和 preventDefault 功能的关系
    >
    > ​	元素事件每次被触发，浏览器都会去查询被执行行为是否有preventDefault阻止该次事件的默认动作。
    >
    > ​	加上**passive就是为了告诉浏览器，不用查询了，执行 方法中没用preventDefault阻止默认动作。**
    >
    > ​        这里一般用在滚动监听，@scoll，@touchmove 中。因为滚动监听过程中，移动每个像素都会产生一次事件，每次都使用都进行查询prevent会使滑动卡顿。通过passive将内核线程查询跳过，可以大大提升滑动的流畅度。
    >
    > **注：Vue时间绑定时，passive和prevent冲突，不能同时绑定在一个监听器上。**

#### 7、v-show

- 取值：`any`
- 功能：根据表达式的boolean结果，**切换元素的 `display` CSS 属性，控制元素显示隐藏**
- 示例：`<p v-show=" flag "></p>`

#### 8、v-if、v-else-if、v-else

- 取值：

  - v-if：`any`
  - v-else-if：`any`
  - v-else：**不需要表达式**，该指令为boolean类型属性

- 用法：根据表达式的boolean结果，**执行元素的创建和删除操作**

- 示例：

  ```html
  <div v-if="type === 'A'">A</div>
  <div v-else-if="type === 'B'">B</div>
  <div v-else>Not A/B/C</div>
  ```

  - `v-else` 指令的上一个元素 必须使用了 `v-if` 或者 `v-else-if`
  - `v-else-if` 指令的上一个元素 必须使用了 `v-if` 

#### 9、v-for

- 取值：`Array | Object | number | string `

- 功能：基于数据多次渲染元素或模板块

- 语法：`<标签 v-for=" 取值表达式 in 待循环值 "></标签>`

  - 取值表达式：可直接定义临时变量；

  ```html
  <div v-for="item in items">
    {{ item.text }}
  </div>
  ```

  + 取值表达式：也可以为数组索引指定别名 (或者用于对象的键)：

  ```html
  <div v-for="(item, index) in items"></div>
  <div v-for="(val, key) in object"></div>
  <div v-for="(val, name, index) in object"></div>
  ```

- **辅助渲染**：`v-for` 指令为提高性能采用部分标签渲染操作（**只针对与需要添加和删除的标签进行渲染操作**）

  - `v-for` 默认不改变整体，而是**重复替换使用已有元素**，该方式会导致页面排序功能展示出现问题。
  - 为迫使其`v-for`重新排序元素，需要提供一个 `key` 的特殊属性，为每个元素提供唯一key值

```html
<div v-for="item in items" :key="item.id">
  {{ item.text }}
</div>
```

#### 10、v-bind

- 缩写：`:`，项目中可用 `：` 替代 `v-bind`

- 语法：

  ```html
  <p v-bind[:参数][.修饰符]="取值"></p>
  <p :[参数][.修饰符]="取值"></p>
  ```

- 取值：`any (对应属性取值) | Object (对应属性取值)`

- 参数：`attrOrProp (optional)`

- 修饰符：

  - `.prop` - 被用于绑定 DOM 属性 (property)。
  - `.camel` - (2.1.0+) 将 kebab-case 特性名转换为 camelCase. (从 2.1.0 开始支持)
  - `.sync` (2.3.0+) 语法糖，会扩展成一个更新父组件绑定值的 `v-on` 侦听器。

- 用法：

  动态地绑定一个或多个特性，或一个组件 prop 到表达式。

  在绑定 `class` 或 `style` 特性时，支持其它类型的值，如数组或对象。可以通过下面的教程链接查看详情。

  在绑定 prop 时，prop 必须在子组件中声明。可以用修饰符指定不同的绑定类型。

  没有参数时，可以绑定到一个包含键值对的对象。注意此时 `class` 和 `style` 绑定不支持数组和对象。

- 示例：

  ```html
  <!-- 绑定一个属性 -->
  <img v-bind:src="imageSrc">
  ```

#### 11、指令总结

+ **功能指令：`v-pre`、`v-cloak`、`v-on`、`v-once`**

+ **构成指令：`v-text`、`v-html`、`v-show`、`v-if`、`v-else`、`v-else-if`、`v-for`、`v-bind`**

+ Vue项目的构成指令和插值表达式具有相同的**响应式特性**

  + ==响应式功能：实际上就是所谓的数据状态同步操作；特指项目构成中，内存中变量数据变化，页面会及时做出响应（页面重构渲染）==

  > Tips：扩展阅读《现代 js 框架存在的根本原因》
  >
  > 英文原文：<https://medium.com/dailyjs/the-deepest-reason-why-modern-javascript-frameworks-exist-933b86ebc445>
  >
  > 中文翻译：<https://www.zcfy.cc/article/the-deepest-reason-why-modern-javascript-frameworks-exist>

### 5.3、响应式原理

+ 响应式原来实现基础：`Object.defineProperty(obj, prop, descriptor)`

  + `Object.defineProperty()` 功能也被称之为 javaScript 的 **数据劫持**
  + `Object.defineProperty()` 是JS语法中提供**可控变量定义方式**
  + ES6 为了加强该功能，提出了一个新的对象 proxy，实际是 Object.defineProperty() 强化型
  + **Vue3.0 核心语法中将全面使用 proxy 替代 Object.defineProperty()**


![1564625490878](assets/1564625490878.png)

+ 基本使用：

  + 语法：`Object.defineProperty(obj, prop, descriptor)`
  + 参数：

    + `obj`要在其上定义属性的对象。
    + `prop`要定义或修改的属性的名称。
    + `descriptor`将被定义或修改的属性描述符。

  + 返回：被传递给函数的对象。


### 5.4、双向数据绑定指令v-model

+ 基于响应式原理和元素事件监听实现响应式功能

![1564625789960](assets/1564625789960.png)

- 取值：随表单控件类型不同而不同。
- 限制：**仅限于HTML表单元素**，`<input>`、`<select>` 、`<textarea>`
- 语法：`<input type="text" v-model="name">`
- 修饰符：
  - `.lazy` 取代 `input` 监听 `change` 事件
  - `.number` 输入字符串转为有效的数字
  - `.trim`  输入首尾空格过滤

## 6、数据控制

### 6.1、计算属性 (computed)

+ 构建方式：

  ```js
  new Vue({
      data:{},
      computed:{
          // 构建计算属性
      }
      methods:{}
  })
  ```

- 类型：`computed:{ key:value }`
  - key：取值类型string，用于==定义计算属性变量名称==，**计算属性具有 vue普通数据仓库的变量功能，同时具有vue方法仓库中相同功能**
  - value：定义计算属性的相关取值
    - 取值为 Function 时，提供计算属性取值功能，此时该计算属性为**只读属性**
    - 取值为 Object { get:Function,setFunction } 时，提供计算属性取值和修改功能
- 功能：用于控制数据在页面输出前，对数据进行包装处理
- 特性：
  - **计算属性 在对数据进行处理包装时，需要依赖一个当前Vue对象中普通属性**
  - **计算属性的结果 ，会随着依赖的普通属性的变换发生变换（重新调用方法）**
  - **计算属性的变量名称不能和 vue实例中其它数据仓库的名称一样**

### 6.2、过滤器 (filters)

- 功能：将向页面写入的数据，经过特定的方法 进行 包装处理，将处理后的结果展示到页面中，**页面的最终结果上来说，过滤器和计算属性功能相似**

- 范围：滤器可以用在两个地方：**双花括号插值和 v-bind 表达式** (后者从 2.1.0+ 开始支持)

- 语法：在页面中通过管道符`|`，连接待处理变量和过滤器方法 

  ```html
  <!-- 在双花括号中 -->
  {{ 待处理变量 | 过滤器方法名 }}
  {{ 待处理变量 | 过滤器方法名() }}
  
  <!-- 在 `v-bind` 中 -->
  <div v-bind:id="待处理变量 | 过滤器方法名"></div>
  <div v-bind:id="待处理变量 | 过滤器方法名()"></div>
  ```

#### 1、局部过滤器

- **仅限于 定义过滤器时 所参考的 vue实例对象的 容器中**
  - 局部过滤器的定义方式:   **需要定义在一个 已经存在的 vue实例中**
  - 局部过滤器的使用范围：**在定义过滤器的 vue对象的 容器才可使用**
- 定义局部过滤器

```js
new Vue({
    filters:{
        // key 描述过滤器名称
        // value 描述过滤器 执行方法
        testFilter:function(){
            return "";  //过滤器而言，因为需要将结果展示在页面中，所有方法必须存在返回值
        }
    }
})
```

#### 2、全局过滤器

- **使用 Vue 装载 新的 过滤器方法，提供给当前项目中所有的vue实例使用**
  - 全局过滤器的定义方式:   **通过为 Vue 构造方法增加 新的方法 完成 过滤器的定义（全局配置）**
  - 全局过滤器的使用范围：**所有的vue实例的容器中都可以使用**
- 全局过滤器定义
  - 全局定义 需要依赖于 Vue构造器对象
  - ==全局过滤器 一定要在 vue实例构建之前定义==
  - 语法：Vue.filter( id, [definition\] )
    - id == name : 定义过滤器名称(过滤器的方法名) 取值为 string，具有**唯一性 **
    - definition ： 过滤器的执行方法（**和局部过滤的方法特性一样**），是一个匿名方法
  - **如果全局过滤器名称和局部过滤器名称相同**
    - **过滤器可以共存，但定义局部过滤器的实例中无法使用全局过滤器，因为局部过滤器优先级高于全局过滤器**
- 定义全局过滤器

```js
Vue.filter("formatDate",function(date){
    console.log("全局过滤器：",date);
    return "全局过滤器";
})
```

### 6.3、监视器 (watch)

+ 功能：构建一个对Vue实例中数据仓库中变量（data，computed）的监控方法，**实现当数据变化时执行额外扩展方法**

+ 组件内构建方式：

  ```js
  var vm = new Vue({
      watch: {
         	key:value
      },
  })
  ```
  + key（string）：被监视的数据变量名称==或对象路径表示方式==

    + **!注意! : 对象路径表示形式只能用于Vue的监视器定义时**

  + value（Function|Object|Array）: 定义监视器执行方式

    + 取值 Function : 定义基础的数据监控方法

    + 取值 Object : 定义可扩展数据监控配置

      ```js
      {
          handler: Function 定义监控方法
          deep: Boolean 是否开启深度监视
          immediate: Boolean 是否开启初始化触发
      }
      ```

+ 组件外构建

  ```js
  var vm = new Vue({……})；
  var unwatch = vm.$watch( expOrFn, callback, [options] )
  ```

  + expOrFn（string|Function）被监视的数据变量名称、==对象路径表示方式==、==多变量配置方法==

    + 取值为string ：被监视的数据变量名称==或对象路径表示方式==
    + 取值为Function：被监视的==多变量配置方法==

  + callback（Function）: 定义的监控处理方法

  + options （Objecy）：定义监控扩展功能

  + 返回值 unwatch （Function）: 返回一个用于关闭销毁销毁当前监控的方法

    ```js
    {
        deep: Boolean 是否开启深度监视
        immediate: Boolean 是否开启初始化触发
    }
    ```

### 6.4、计算属性、过滤器、监视器

+ 计算属性
  + 依赖于Vue实例,只能在实例中定义使用
  + 调用时不能接收额外参数，必须依赖vue实例中的 一个或 多个固定数据变量
  + 计算属性默认是只读属性，但是可以在定义时使用对象模式，开启可读写模式
  + 计算属性会对结果进行缓存操作
    1. 如果依赖变量没有变化，计算属性方法不被触发，直接从缓存中进行读取；
    2. 如果依赖变量发生变化，会重新执行一次方法
  + 计算属性是被作为一个类属性使用
+ 过滤器
  + 可以根据需要选择全局过滤器或局部过滤器
  + 调用时可以接收多个参数，其中包含待处理数据，因此可以不依赖于固定vue实例
  + 过滤器只能完成对于被过滤数据的读取操作，无法进行设置操作
  + 过滤不具有缓存特性，页面中定义调用一次必然会重新执行一次
  + 过滤是被作为一个特殊的处理方法使用
+ 监视器
  + 依赖于Vue实例的固定变量，可以在实例中定义，也可以在全局中通过实例对象进行定义
  + 监视器不能被调用，只能由Vue检测变量变化自动执行，方法默认自带两个参数 oldValue和newValue
  + 监视可以完成对固定变量的监控和修改操作
  + 监视器被作为Vue功能的扩展接口使用

## 7、页面模板和render函数

+ Vue对象构建时需要为对象指定页面构成，该构成称之为模板
+ 在基本构成vue对象时通过 el 指定**构成的页面模板**，**同时指定页面位置**
+ **用于构建模板的标签结构必须有且仅有一个根节点**

#### 1、模板属性 (template)

+ Vue实例添加 `template` 属性，可以独立定义页面构成模板
+ template 构成的模板 **最终会替换到 el 的指向位置**

+ 示例:

  ```js
  var vm = new Vue({
      el: '#app',
      template:StringDOM | StringEl
  });
  ```

  + StringDOM：HTML标签的字符串定义方式
  + StringEl：HTML元素选择器

#### 2、模板渲染函数（render）

+ Vue实例添加 `render` 属性函数，可以独立定义JS模板构成函数

+ render 构成的模板==具有最高优先权==， **最终会替换到 el 的指向位置**，且template属性失效

+ 示例:

  ```js
  var vm = new Vue({
      el: '#app',
      data:{
          title:"标题"
      },
      render: function (createElement) {
          return createElement('h4', 'render'+this.title);
      }
  });
  ```
  + render 属性取值为一个固定函数**`function (createElement) {}`**，该函数返回构成的模板
  + createElement 为渲染函数的固定参数，**该参数可用于以方法方式构建页面模板**

## 8、实例属性和方法

+ Vue中对部分特殊的属性和功能方法进行特殊指代定义，用于提供独立的执行和获取方式
+ Vue的实例属性和方法以统一规范以 **`$`** 开头
+ 相关实例属性和方法 
  + vm.$el：描述当前Vue实例使用的根 DOM 元素。
  + vm.$data：描述当前Vue实例观察的数据对象。
  + vm.$options：构建当前 Vue 实例的初始化选项。
  + vm.$refs：返回一个对象，记录当前Vue实例模板中，**定义了ref属性**的所有 DOM 元素。
  + vm.$watch：构建一个对Vue实例中数据仓库中变量（data，computed）的监控方法
  + vm.$set：等同于 `Vue.set` ，手动为实例中没有数据监听的变量添加监视功能
  + vm.$delete：等同于 `Vue.delete` ，手动为实例中的变量删除监视功能
  + vm.$mount：手动挂在Vue实例
  + vm.$destroy：收到销毁Vue实例
  + vm.$nextTick：等同于 `Vue.nextTick` ，将执行函数体延迟到页面DOM更新完成后执行





















