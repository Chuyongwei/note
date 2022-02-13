# Vue

## 理论

### MVVM

![MVVM框架](./image/bVbeQ94.png)

MVVM框架三要素:响应式,模板引擎及其渲染

响应式: vue如何监听数据变化

模板: vue的模板如何编写和解析

渲染: vue如何将模板转换为html



### 生命周期

<img src="https://cn.vuejs.org/images/lifecycle.png" alt="vue的生命周期" style="zoom: 50%;" />

参考https://juejin.cn/post/6844903602356502542

| 生命周期钩子  | 组件状态                                                     | 最佳实践                                                    |
| ------------- | ------------------------------------------------------------ | ----------------------------------------------------------- |
| beforeCreate  | 实例初始化之后，this指向创建的实例，不能访问到data、computed、watch、methods上的方法和数据 | 常用于初始化非响应式变量                                    |
| created       | 实例创建完成，可访问data、computed、watch、methods上的方法和数据，未挂载到DOM，不能访问到$el属性，$ref属性内容为空数组 | 常用于简单的ajax请求，页面的初始化                          |
| beforeMount   | 在挂载开始之前被调用，beforeMount之前，会找到对应的template，并编译成render函数 | –                                                           |
| mounted       | 实例挂载到DOM上，此时可以通过DOM API获取到DOM节点，$ref属性可以访问 | 常用于获取VNode信息和操作，ajax请求                         |
| beforeupdate  | 响应式数据更新时调用，发生在虚拟DOM打补丁之前                | 适合在更新之前访问现有的DOM，比如手动移除已添加的事件监听器 |
| updated       | 虚拟 DOM 重新渲染和打补丁之后调用，组件DOM已经更新，可执行依赖于DOM的操作 | 避免在这个钩子函数中操作数据，可能陷入死循环                |
| beforeDestroy | 实例销毁之前调用。这一步，实例仍然完全可用，this仍能获取到实例 | 常用于销毁定时器、解绑全局事件、销毁插件对象等操作          |
| destroyed     | 实例销毁后调用，调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁 | –                                                           |

### 模板引擎

我们学习了vue的使用,css的嵌套.条件语句,函数的使用做了一个网页

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>购物车</title>
    <style>
        .active{
            background-color: beige;
        }
    </style>
</head>
<body>
    <div id="app">
        <h2 v-bind:title="title">
            {{title}}
        </h2>
        <p>
            <input v-model="course" type="text" v-on:keydown.enter="addCourse"/>
            <button @click="addCourse">新增</button>
        </p>
        <!-- 列表渲染 -->
        <!-- <div v-for="c in courses" :key="c" :class="{active:selectedCourse===c}"
        @click="selectedCourse=c">
            {{ c }}
        </div> -->

        <!-- 条件渲染 -->
        <p v-if="courses.length==0">没有任何信息</p>
        <!-- style -->
        <div class="course-list" v-else>

            <div v-for="c in courses" :key="c" 
            :style="{backgroundColor:selectedCourse===c?'#ddd':'transparent'}"
            @click="selectedCourse=c">
                {{ c }}
            </div> 
        </div>
    </div>
    <script src="./vue.js"></script>
    <script>
        const app= new Vue({
            el: "#app",
            data() {
                return {
                    title:'开课吧购物车',
                    courses:["web","web2"],
                    course:"",
                    selectedCourse:"",
                    active:true
                }
            },
            methods: {
                addCourse() {
                    if(this.course!=""){
                        this.courses.push(this.course)
                    this.course=""
                    }

                }
            },
        })
        console.log(app.$options.render);
    </script>
</body>
</html>
```

我们做的一个vue网页的样子 `console.log(app.$options.render);`

```js
(function anonymous(
) {
with(this){return _c('div',{attrs:{"id":"app"}},[_c('h2',{attrs:{"title":title}},[_v("\n            "+_s(title)+"\n        ")]),_v(" "),_c('p',[_c('input',{directives:[{name:"model",rawName:"v-model",value:(course),expression:"course"}],attrs:{"type":"text"},domProps:{"value":(course)},on:{"keydown":function($event){if(!$event.type.indexOf('key')&&_k($event.keyCode,"enter",13,$event.key,"Enter"))return null;return addCourse($event)},"input":function($event){if($event.target.composing)return;course=$event.target.value}}}),_v(" "),_c('button',{on:{"click":addCourse}},[_v("新增")])]),_v(" "),(courses.length==0)?_c('p',[_v("没有任何信息")]):_c('div',{staticClass:"course-list"},_l((courses),function(c){return _c('div',{key:c,style:({backgroundColor:selectedCourse===c?'#ddd':'transparent'}),on:{"click":function($event){selectedCourse=c}}},[_v("\n                "+_s(c)+"\n            ")])}),0)])}
})
```



所以我们能不做html部分的网页...

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>购物车</title>
    <style>
        .active{
            background-color: beige;
        }
    </style>
</head>
<body>
    <div id="app">
        <!-- 这里我们没有代码 -->
    </div>
    <script src="./vue.js"></script>
    <script>
        new Vue({
            el: "#app",
            data() {
                return {
                    title:'开课吧购物车',
                    courses:["web","web2"],
                    course:"",
                    selectedCourse:"",
                    active:true
                }
            },
            methods: {
                addCourse() {
                    if(this.course!=""){
                        this.courses.push(this.course)
                    this.course=""
                    }

                }
            },
            render() {
                with(this){return _c('div',{attrs:{"id":"app"}},[_c('h2',{attrs:{"title":title}},[_v("\n            "+_s(title)+"\n        ")]),_v(" "),_c('p',[_c('input',{directives:[{name:"model",rawName:"v-model",value:(course),expression:"course"}],attrs:{"type":"text"},domProps:{"value":(course)},on:{"keydown":function($event){if(!$event.type.indexOf('key')&&_k($event.keyCode,"enter",13,$event.key,"Enter"))return null;return addCourse($event)},"input":function($event){if($event.target.composing)return;course=$event.target.value}}}),_v(" "),_c('button',{on:{"click":addCourse}},[_v("新增")])]),_v(" "),(courses.length==0)?_c('p',[_v("没有任何信息")]):_c('div',{staticClass:"course-list"},_l((courses),function(c){return _c('div',{key:c,style:({backgroundColor:selectedCourse===c?'#ddd':'transparent'}),on:{"click":function($event){selectedCourse=c}}},[_v("\n                "+_s(c)+"\n            ")])}),0)])}
            }
        })
    </script>
</body>
</html>
```

### 计算属性

```js
            computed: {
                total() {
                    // 计算属性有缓存性: 如果值没变化,则页面不重新渲染
                    return this.courses.length + '门'
                }
            },
```



### 监听器

**这里的courses是data上的**

普通情况下

```js
            // vwatcher
            // 默认情况下watch初始化时不执行
            watch: {
                courses(newValue, oldValue) {
                    this.totalCount = newValue.length + "门"
                }
            },
```

有选项的

```js
            // vwatcher-option
            watch: {
                courses: {
                    immediate: true,  // 立即执行一次
                    // deep: true, // 是否进行深度检测?在数组内部复杂时使用
                    handler(newValue, oldValue) {
                        this.totalCount = newValue.length + "门"
                    }
                }
            },
```

### 计算属性vs监听器

- 语境上的差异

  `watch`是一值影响多值

  `computed`是多值影响一值

- **计算属性**有缓存的性质,可以避免渲染

- 



## 组件化管理

### 使用

#### 组件前

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>购物车</title>
    <style>
        .active {
            background-color: beige;
        }
    </style>
</head>

<body>
    <div id="app">
        <h2 v-bind:title="title">
            {{title}}
        </h2>
        <p>
            <input v-model="course" type="text" v-on:keydown.enter="addCourse" />
            <button @click="addCourse">新增</button>
        </p>
        <!-- 条件渲染 -->
        <p v-if="courses.length==0">没有任何信息</p>
        <!-- 列表渲染 -->
        <!-- <div v-for="c in courses" :key="c" :class="{active:selectedCourse===c}"
        @click="selectedCourse=c">
            {{ c }}
        </div> -->

        <!-- style -->
        <div class="course-list" v-else>
            <div v-for="c in courses" :key="c" :style="{backgroundColor:selectedCourse===c?'#ddd':'transparent'}"
                @click="selectedCourse=c">
                {{ c }}
            </div>
        </div>

        <!--  商品总数 -->
        <p>
            <!-- 通过表达式 -->
            <!-- 课程总数:{{courses.length+"门"}} -->
            <!-- 通过计算表达式 -->
            <!-- 课程总数: {{total}} -->
            <!-- 监听器 -->
            课程总数: {{totalCount}}
        </p>
    </div>
    <script src="./vue.js"></script>
    <script>

        // 模拟异步数据调用
        function getCourses(){
            return new Promise(resolve => {
                setTimeout(() => {
                    resolve(["web全栈",'web高级'])
                }, 2000);
            })
        }

        // 1.创建爱你vue实例
        const app = new Vue({
            el: "#app",
            data() {
                return {
                    title: '开课吧购物车',
                    courses: [],
                    course: "",
                    selectedCourse: "",
                    active: true,
                    totalCount: 0
                }
            },
            async created () {
                const courses = await getCourses()
                this.courses = courses
            },
            mounted () {
                // const sourses = await getCourses()
                // this.courses = courses
            },
            // 在这里使用vue内的变量都要加this
            methods: {
                addCourse() {
                    if (this.course != "") {
                        this.courses.push(this.course)
                        this.course = ""
                    }

                }
            },
            computed: {
                total() {
                    // 计算属性有缓存性: 如果值没变化,则页面不重新渲染
                    return this.courses.length + '门'
                }
            },
            // vwatcher
            // 默认情况下watch初始化时不执行
            // watch: {
            //     courses(newValue, oldValue) {
            //         this.totalCount = newValue.length + "门"
            //     }
            // },
            // vwatcher-option
            watch: {
                courses: {
                    immediate: true,  // 立即执行一次
                    // deep: true, // 是否进行深度检测?在数组内部复杂时使用
                    handler(newValue, oldValue) {
                        this.totalCount = newValue.length + "门"
                    }
                }
            },
        })
        console.log(app.$options.render);
    </script>
</body>

</html>
```

#### 组件后

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>购物车</title>
    <style>
        .active {
            background-color: beige;
        }
    </style>
</head>

<body>
    <div id="app">
        <h2 v-bind:title="title">
            {{title}}
        </h2>
        <p>
            <input v-model="course" type="text" v-on:keydown.enter="addCourse" />
            <button @click="addCourse">新增</button>
        </p>
        <!-- 列表组件 -->
        <course-list :courses="courses"></course-list>


        <!--  商品总数 -->
        <p>
            <!-- 通过表达式 -->
            <!-- 课程总数:{{courses.length+"门"}} -->
            <!-- 通过计算表达式 -->
            <!-- 课程总数: {{total}} -->
            <!-- 监听器 -->
            课程总数: {{totalCount}}
        </p>
    </div>
    <script src="./vue.js"></script>
    <script>

        Vue.component('course-list', {
            data() {
                return {
                    selectedCourse: "",
                }
            },
            props: {
                courses: {
                    type: Array,
                    default: []
                },

            },
            template: `
            <div>
                <!-- 条件渲染 -->
            <p v-if="courses.length==0">没有任何信息</p>
            <!-- <div v-for="c in courses" :key="c" :class="{active:selectedCourse===c}"
            @click="selectedCourse=c">
                {{ c }}
            </div> -->
    
            <!-- style -->
            <div class="course-list" v-else>
                <div v-for="c in courses" :key="c" :style="{backgroundColor:selectedCourse===c?'#ddd':'transparent'}"
                    @click="selectedCourse=c">
                    {{ c }}
                </div>
            </div>
                </div>
            `
        })

        // 模拟异步数据调用
        function getCourses() {
            return new Promise(resolve => {
                setTimeout(() => {
                    resolve(["web全栈", 'web高级'])
                }, 2000);
            })
        }

        // 1.创建爱你vue实例
        const app = new Vue({
            el: "#app",
            data() {
                return {
                    title: '开课吧购物车',
                    courses: [],
                    course: "",
                    active: true,
                    totalCount: 0
                }
            },
            async created() {
                const courses = await getCourses()
                this.courses = courses
            },
            mounted() {
                // const sourses = await getCourses()
                // this.courses = courses
            },
            // 在这里使用vue内的变量都要加this
            methods: {
                addCourse() {
                    if (this.course != "") {
                        this.courses.push(this.course)
                        this.course = ""
                    }

                }
            },
            computed: {
                total() {
                    // 计算属性有缓存性: 如果值没变化,则页面不重新渲染
                    return this.courses.length + '门'
                }
            },
            // vwatcher
            // 默认情况下watch初始化时不执行
            // watch: {
            //     courses(newValue, oldValue) {
            //         this.totalCount = newValue.length + "门"
            //     }
            // },
            // vwatcher-option
            watch: {
                courses: {
                    immediate: true,  // 立即执行一次
                    // deep: true, // 是否进行深度检测?在数组内部复杂时使用
                    handler(newValue, oldValue) {
                        this.totalCount = newValue.length + "门"
                    }
                }
            },
        })
        console.log(app.$options.render);
    </script>
</body>

</html>
```

### 传值

```html
        <!-- 新增组件 -->
        <course-add v-model="course"  @add-course="addCourse"></course-add>
        <!-- <course-add :course="course" @input="course=$event"  @add-course="addCourse"></course-add> -->

        <!-- 列表组件 -->
        <course-list :courses="courses"></course-list>
```

```js
    Vue.component('course-list', {
            data() {
                return {
                    selectedCourse: "",
                }
            },
            props: {
                courses: {
                    type: Array,
                    default: []
                },
            },
    )
```



### 事件通知

```js
                methods: {
                    addCourse() {
                        // 派发事件通知父组件
                        /*
                        this.$emit(父组件方法名字,参数1,参数2...)
                        */
                        // this.$emit('add-course',this.course)
                        this.$emit('add-course',this.value)
                        console.log(this.value);
                    },
                    onInput(e){
                        console.log(e.target);
                        this.$emit('input',e.target.value)
                    }
                },
```



### 插槽

#### 默认插槽和具名插槽

父组件

```html
        <message :show.sync="show">
            <template v-slot:title="slotProps">
                <!-- 组件化的作用域:我们想让插槽访问组件上的值 -->
                <strong>{{slotProps.title}}</strong>
            </template>
            <template>
                新增课程成功!
            </template>
        </message>
```

子组件

```html
                <div class="message-box" v-if="show">
                    <!-- 通过具名slot获取传入内容 -->
                    <slot name="title" title="来自message的标题"></slot>
                    <!-- 通过slot获取传入内容 -->
                    <slot></slot>
                    <span class="message-box-close" 
                        @click="$emit('update:show',false)">X</span>
                </div>
```

#### 作用域插槽

```vue
<!-- comp3 -->
<div>
    <slot :foo="foo"></slot>
</div>
<!-- parent -->
<Comp3>
    <!-- 把v-solt的值制定为作用域上下文对象 -->
    <template v-slot:default="slotProps">
		来自子组件的数据: {{soltProps.foo}}
    </template>
</Comp3>
```



### Vue组件化的理解

组件化是Vue的精髓，Vue应用就是由一个个组件构成的。Vue的组件化涉及到的内容非常多，当面试时
被问到：谈一下你对Vue组件化的理解。这时候有可能无从下手，可以从以下几点进行阐述：
**定义**：组件是可复用的 Vue 实例，准确讲它们是VueComponent的实例，继承自Vue。
**优点**：从上面案例可以看出组件化可以增加代码的复用性、可维护性和可测试性。
**使用场景**：什么时候使用组件？以下分类可作为参考：
**通用组件**：实现最基本的功能，具有通用性、复用性，例如按钮组件、输入框组件、布局组件等。
**业务组件**：它们完成具体业务，具有一定的复用性，例如登录组件、轮播图组件。
**页面组件**：组织应用各部分独立内容，需要时在不同页面组件间切换，例如列表页、详情页组件
**如何使用组件**

- 定义：Vue.component()，components选项，sfc
- 分类：有状态组件，functional，abstract
- 通信：props，$emit()/$on()，provide/inject，$children/$parent/$root/$attrs/$listeners
- 内容分发：\<slot>，\<template>，v-slot
- 使用及优化：is，keep-alive，异步组件

**组件的本质**
vue中的组件经历如下过程
组件配置 => VueComponent实例 => render() => Virtual DOM=> DOM
所以组件的本质是产生虚拟DOM

### API组件

#### 事件相关

```js
Vue.set(对象,属性,值) //更新值
Vue.delete(对象,属性)
```



```js
vm.$on("",方法)
vm.$once("",方法)
vm.$off("")
```

### 动画

```html
<transition enter-active-class="animated bounceIn"
leave-active-class="animated bounceOut">
```



### 过滤器



```js
// {{value | currency(symbol)}}
// {{ c.name }} -  {{c.price| currency('SS')}}
            filters:{
                currency(value,symbol='$'){ //
                    return symbol +value
                }
            }
```

### 自定义指令

```js
    //自定义指令
    //<input  v-focus/> 自动获取焦点
    Vue.directive('focus', {
        inserted(el){
            el.focus()
        }
    });
    const role = "user"
    //<button @click="batchUpdate" v-permission="'admin'" >批量更新价格</button>
    Vue.directive("permission",{
        inserted(el,binding) {
            console.log(binding);
            
            if(role !== binding.value){
                el.parentElement.removeChild(el)
            }
        }
    })
```



## VUE Cli

### 快速原型开发

1. 安装`@vue/cli-service-global`

   ```shell
   npm install @vue/cli-service-global -g
   ```

单个文件编译

```
vue serve App.vue
```

### 设置

> 设置文件我们将在vue.config.js中进行设置

使用public文件夹的注意事项

- shdfj

  ```js
  // vue.config.js
  module.exports = {
      publicPath: process.env.NODE_ENV === 'production'
          ? '/cart/'
          : '/'
  }
  ```

- 在 public/index.html 等通过 html-webpack-plugin 用作模板的 HTML 文件中，你需要通过`<%= BASE_URL %>` 设置链接前缀：

  ```html
  <link rel="icon" href="<%= BASE_URL %>favicon.ico">
  ```

- 在模板中，先向组件传入BASE_URL

  ```js
  data() {
      return {
          publicPath: process.env.BASE_URL
      }
  }
  ```

  然后

  ```html
  <img :src="`${publicPath}my-image.png`">
  ```

  



#### 代理

```js
devServer:{
	proxy:'http://localhost:3000'
}
```

```js
// node ./api.js
// npm i express

const express = require('express')
const app = express()

app.get('/api/courses', (req, res) => {
    setTimeout(() => {
        res.json([{ name: 'web全栈', price: 8999 }, { name: 'web高级', price: 8999 }])
    }, 1000)
})

app.listen(3000)
```



## 路由

### 使用

```powershell
vue add router
```

在router/index.js中

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from "../view/Home"
Vue.use(VueRouter)

const routes=[
    {
        path: "/",
        name: 'home',
        component: Home
    },
    {
        path: "/admin",
        name: 'admin',
        // meta: {title:"admin"},
        component: ()=>import('../views/Admin.vue')
    },
    {
        path:'*',
        name:"404报错"
    }
]

export default new VueRouter({routes})
```

然后我们在main.js中

```js
import Vue from 'vue';
import App from './App.vue';
import router from './router';

new Vue({
    router,
    render: h => h(App)
}).$mount('#app');

```

```vue
<template>
    <div id = "app">
        <nav>
            <router-link to="/">首页</router-link>
            <router-link to="/admin">管理</router-link>
        </nav>

        <!-- 路由出口 -->
        <router-view></router-view>
    </div>
</template>

<script>
    export default {
        el: "#app",
        data() {
            return {
                key: value
            }
        },
    }
</script>

<style lang="scss" scoped>

</style>
```

### 动态路由匹配

路由

```js
    {
        path: "/course/:name",
        name: 'course',
        component: ()=>import('../views/Detail.vue')
    }
```

本页

```vue
                <div v-for="c in courses" :key="c.name" :style="{backgroundColor:selectedCourse===c?'#ddd':'transparent'}"
                    @click="selectedCourse=c">
                    <router-link to="`/course/${c.name}`">
                        {{ c.name }} -  {{c.price| currency('SS')}}
                    </router-link>
                </div>
```

跳转目标页

```html
<div>{{$route.params.name}}</div>
```



### 子路由

```js
    {
        path: "/course/:name",
        name: 'course',
        component: ()=>import('../views/Detail.vue'),
        chldren:[
            {
                path :'',
                name:'',
                component: null
            }
        ]
    }
```

### 编程导航

借助router的实例方法,可编写代码来实现编程式导航



```js
this.$router.push(location, onComplete?, onAbort?)
```



```js
// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

### 组件的复用

> 如果组件被创建了就不会再次创建

如果我们需要监视他的生成我们可以

```js
watch:{
	$router(){
		console.log("获取详情")
	}
}
```

### 路由守卫

正如其名，vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。有多种机会植入路由导航过程中：全局的, 单个路由独享的, 或者组件级的。

记住**参数或查询的改变并不会触发进入/离开的导航守卫**。你可以通过[观察 `$route` 对象](https://router.vuejs.org/zh/guide/essentials/dynamic-matching.html#响应路由参数的变化)来应对这些变化，或使用 `beforeRouteUpdate` 的组件内守卫。

#### 路由前置守卫



```js
router.beforeEach((to, from, next) => {
    // 判断路由是否需要守卫
    //路由的meta中进行设置auth的值
    if(to.meta.auth) {
        if(window.isLogin){
            next()
        }else{
            next("/login?refirect="+to.fullpath)
        }
    }else{
        next()
    }
})
```



####  路由独享的守卫

你可以在路由配置上直接定义 `beforeEnter` 守卫：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

这些守卫与全局前置守卫的方法参数是一样的。

#### 组件内的守卫

最后，你可以在路由组件内直接定义以下路由导航守卫：

- `beforeRouteEnter`
- `beforeRouteUpdate` (2.2 新增)
- `beforeRouteLeave`

```js
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

`beforeRouteEnter` 守卫 **不能** 访问 `this`，因为守卫在导航确认前被调用，因此即将登场的新组件还没被创建。

不过，你可以通过传一个回调给 `next`来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。

```js
beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
```

注意 `beforeRouteEnter` 是支持给 `next` 传递回调的唯一守卫。对于 `beforeRouteUpdate` 和 `beforeRouteLeave` 来说，`this` 已经可用了，所以**不支持**传递回调，因为没有必要了。

```js
beforeRouteUpdate (to, from, next) {
  // just use `this`
  this.name = to.params.name
  next()
}
```

这个离开守卫通常用来禁止用户在还未保存修改前突然离开。该导航可以通过 `next(false)` 来取消。

```js
beforeRouteLeave (to, from, next) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (answer) {
    next()
  } else {
    next(false)
  }
}
```

### 数据获取

https://router.vuejs.org/zh/guide/advanced/data-fetching.html

有时候，进入某个路由后，需要从服务器获取数据。例如，在渲染用户信息时，你需要从服务器获取用户的数据。我们可以通过两种方式来实现：

- **导航完成之后获取**：先完成导航，然后在接下来的组件生命周期钩子中获取数据。在数据获取期间显示“加载中”之类的指示。
- **导航完成之前获取**：导航完成前，在路由进入的守卫中获取数据，在数据获取成功后执行导航。

从技术角度讲，两种方式都不错 —— 就看你想要的用户体验是哪种。

```js
export default {
  data () {
    return {
      post: null,
      error: null
    }
  },
  beforeRouteEnter (to, from, next) {
    getPost(to.params.id, (err, post) => {
      next(vm => vm.setData(err, post))
    })
  },
  // 路由改变前，组件就已经渲染完了
  // 逻辑稍稍不同
  beforeRouteUpdate (to, from, next) {
    this.post = null
    getPost(to.params.id, (err, post) => {
      this.setData(err, post)
      next()
    })
  },
  methods: {
    setData (err, post) {
      if (err) {
        this.error = err.toString()
      } else {
        this.post = post
      }
    }
  }
}
```

### 动态路由

```js
this.$router.addRoutes([
    {
        path:"/admin",
        name:"",
        component:()=>{import()}
    }
])
```



## 参考

https://ustbhuangyi.github.io/vue-analysis/v2/components/lifecycle.html



# VUE实例

## 属性

- $listerners和$attrs

  获取父元素的使用

- $ref

  获取子元素的使用

- provide/inject



## vueX

https://www.processon.com/view/link/5e146d6be4b0da16bb15aa2a#map





## 自定义vue

1. Observer 劫持
2. complier 加载
3. watcher 监视
4. dep：依赖



## 手写vue



> - runtime:仅包含运行时
> - common： cjs规范，用于webpack1
> - esm：ES模块，用于webpack2
> - umd:unversal module definition ,兼容cjs和es用于浏览器

- `src/platforms/web/entry-compiler.js`

  入口文件，覆盖$mount，执行模板解析和编译工作

- `src/platforms/web/runtime/index.js`

  定义$mount

- `src/core/index.js`

  定义全局api

- `src/core/instance/index.js`

  定义构造函数

  ```js
  initMixin(Vue)  //通过该方法给Vue添加_init方法
  stateMixin(Vue)  // $set,$delete,$watch
  eventsMixin(Vue) //加入方法
  lifecycleMixin(Vue) // 挂载生命周期
  renderMixin(Vue)
  ```

  

- `src/core/instance/init.js`

  初始化方法_init
  
  52行
  
  ```js
      initLifecycle(vm) // 初始化生命周期
      initEvents(vm)
      initRender(vm)  //声明$slots,$root,$children,$refs
      callHook(vm, 'beforeCreate')
      initInjections(vm) // resolve injections before data/props
      initState(vm)  //重要：数据初始化，响应式
      initProvide(vm) // resolve provide after data/props
      callHook(vm, 'created')
  ```
  
- `src\core\observer\index.js`

  ```js
  obj = {foo:'foo'}
  obj,bar='aaa'
  Vue.set(obj,'bar','aaa') //ok
  items = [1,3]
  Vue.set(items,0,'abc')
  items.length=0
  items[0]='aaa'
  ```

  



### 初始化过程 



**缺点**

1. 递归遍历，性能会受影响
2. api性能不统一

### 更新过程

`/core/util/next-tick.js`实现更新

> 所有任务放入更新队列中，如果哦后面有相同的任务就忽略，然后一步一步的更新

### 虚拟dom树



## NUXT

构建

```powershell
npx create-nuxt-app <name>
```



## Ts加装

### 在已存在的项目装ts

```powershell
$ vue add @vue/typescript
# 是否使用ts类
? Use class-style component syntax? Yes
#是否在使用TypeScript的同时使用Babel(
? Use Babel alongside TypeScript (required for modern mode, auto-detected polyfills, transpi
ling JSX)? No
# 是否将所有js转为ts
? Convert all .js files to .ts? No
允许js文件变异
? Allow .js files to be compiled? No
? Skip type checking of all declaration files (recommended for apps)? Yes
```

之后
+ main.js变为main.ts
+ 修改App.vue
+ 添加tsconfig.json文件(<font style="color: red;">注意</font>: 因为是从原始项目转换的会有很多的警告却并不影响使用,在修改完全前我们可以加配置)
	```json
	{
		"noImplicitAny": false,//无视警告
	}
	```


### Ts学习

#### vue3模板

```vue
<template>
	<div class="hello">

<p><input type="text" name="a" id="a" @keydown.enter="addFeature"></p>

		<ul>
			<li v-for="feature in features" :key="feature">{{feature}}</li>
			<li>特性总数:{{count}}</li>
		</ul>
		
	</div>
</template>

<script lang="ts">
	import { Component, Vue } from 'vue-property-decorator';
@Component({
	//vue2混合
})
	export default class Hello extends Vue{
		features: string[]= []
		addFeature(e:KeyboardEvent){
			let inp=e.target as HTMLInputElement
			this.features.push(inp.value)
			console.log("Dsaf",this.features);
			inp.value = ' '
		}

		created(){
			this.features = ["类型注解","变异语言"]
			console.log("create")
		}
		//存取器用于计算属性
		get count(){
			return this.features.length
		}
	}
</script>

<style scoped>

</style>
```

## vue的最佳实践

安装

```powershell
npm i svg-sprite-loader -D
```



```js
const port = 7070
const title = "webpack的服务"
const path = require("path")

function resolve(dir){
	return path.join(__dirname,dir)
}

module.exports = {
	publicPath: '/best',

	devServer:{
		port,
		configureWebpack:{
			name:title
		}
	},
	chainWebpack(config){
		config.module.rule('svg').exclude.add(resolve("./src/icons"))
		config.module.rule('icons')
      .test(/\.svg$/)
      .include.add(resolve('./src/icons')).end()
      .use('svg-sprite-loader')
      .loader('svg-sprite-loader')
      .options({ symbolId: 'icon-[name]' })
	},
}
```

检查配置

```powershell
vue inspect --rule svg
```



# Vue测试

###  v-if和v-for哪个优先级高

```vue
<!DOCTYPE html>
<html>

<head>
    <title>Vue事件处理</title>
</head>

<body>
    <div id="demo">
        <h1>v-for和v-if谁的优先级高？应该如何正确使用避免性能问题？</h1>
        <!-- <p v-for="child in children" v-if="isFolder">{{child.title}}</p> -->
        <template v-if="isFolder">
            <p v-for="child in children">{{child.title}}</p>
        </template>
    </div>
    <script src="../vue.js"></script>
    <script>
        // 创建实例
        const app = new Vue({
            el: '#demo',
            data() {
                return {
                    children: [
                        { title: 'foo' },
                        { title: 'bar' },
                    ]
                }
            },
            computed: {
                isFolder() {
                    return this.children && this.children.length > 0
                }
            },
        });
        console.log(app.$options.render);
    </script>
</body>

</html>
```



源码

如果不同级别,要是isFolder为空就不会进行渲染

```js
(function anonymous(
) {
with(this){return _c('div',{attrs:{"id":"demo"}},[_c('h1',[_v("v-for和v-if谁的优先级高？应该如何正确使用避免性能问题？")]),_v(" "),(isFolder)?_l((children),function(child){return _c('p',[_v(_s(child.title))])}):_e()],2)}
})
```

> _l列表渲染

如果同级别不管isFolder是否为空都会执行列表渲染然后每个循环都会执行判断

```js
(function anonymous(
) {
with(this){return _c('div',{attrs:{"id":"demo"}},[_c('h1',[_v("v-for和v-if谁的优先级高？应该如何正确使用避免性能问题？")]),_v(" "),_l((children),function(child){return (isFolder)?_c('p',[_v(_s(child.title))]):_e()})],2)}
})
```

结论

1. 显然v-for优先于v-if被解析（把你是怎么知道的告诉面试官）
2. 如果同时出现，每次渲染都会先执行循环再判断条件，无论如何循环都不可避免，浪费了性能
3. 要避免出现这种情况，则在外层嵌套template，在这一层进行v-if判断，然后在内部进行v-for循环
4. 如果条件出现在循环内部，可通过计算属性提前过滤掉那些不需要显示的项

### Vue组件data为什么必须是个函数而Vue的根实例则没有此限制？



实例

```html
<!DOCTYPE html>
<html>

<head>
    <title>Vue事件处理</title>
</head>

<body>
    <div id="demo">
        <h1>vue组件data为什么必须是个函数? </h1>
        <comp></comp>
        <comp></comp>
    </div>
    <script src="../vue.js"></script>
    <script>
        Vue.component('comp', {
            template: '<div @click="counter++">{{counter}}</div>',
            data: { counter: 0 }
        })
        // 创建实例
        const app = new Vue({
            el: '#demo',
        });
    </script>
</body>

</html>
```

# SSR

## 前提

express

```powershell
npm i express -S
```

 `node 01-express-test.js`运行

使用`npm i nodemon -g`可以监视

```js
// node js代码
const express  = require('express')

//获取实例
const server = express()


//
server.get('/',(req,res)=>{
	res.send("<strong>hello world</strong>")
})

server.listen(  8080 ,()=>{
	console.log("server running");
})
```

vue的渲染

```powershell
npm install vue-server-renderer -D
```

> 确保vue，vue-server-renderer版本一致

```js
// 1.创建vue实例
const Vue = require('vue')
const app = new Vue({
	template:'<div>hello world</div>'
})
//2.获取渲染器实例
const {createRenderer} = require('vue-server-renderer')
const renderer = createRenderer()
//3.用渲染器渲染vue实例
renderer.renderToString(app).then(html=>{
	console.log(html);
}).catch(e=>{
	console.log(e);
})
```

## 开始

```powershell
npm i serve-favicon -S
```





```powershell
npm i webpack-node-exter你als lodash.merge -D
```