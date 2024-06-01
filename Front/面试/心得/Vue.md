## 方法

### Vue.extend

创建Vue组件

```javascript
Vue.extend( options )
  参数：{Object} options
  用法：使用基础 Vue 构造器，创建一个“子类”。参数是一个包含组件选项的对象；
  	   data 选项是特例，需要注意： 在 Vue.extend() 中它必须是函数；
```

```javascript
<div id="mount-point"></div>
// 创建构造器
var Profile = Vue.extend({
  template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
  data: function () {
    return {
      firstName: 'Walter',
      lastName: 'White',
      alias: 'Heisenberg'
    }
  }
})
// 创建 Profile 实例，并挂载到一个元素上。
new Profile().$mount('#mount-point')

// 结果如下：
<p>Walter White aka Heisenberg</p>

```

第二种写法

```javascript
// 1. 定义一个vue模版 
let  tem ={
    template:'{{firstName}} {{lastName}} aka {{alias}}',
    data:function(){    
    return{    
	    firstName:'Walter',   
	    lastName:'White',    
	    alias:'Heisenberg'
    }
}

// 2. 调用
const TemConstructor = Vue.extend(tem) 
const intance = new TemConstructor({el:"#app"}) // 生成一个实例，并且挂载在 #app 上
```



### Vue.component

创建Vue组件并且装载

```javascript
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```

- 使用组件

  ```javascript
  new Vue({
      components: {
          
      }
  })
  ```

  

### v-director

```javascript
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})

<input type="text" v-focus/>
```

局部指令

```javascript
directives: {
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}
```



## 双向绑定

### 父组件向子组件通信

属性绑定，数据拦截

父组件

```vue
<template>
	<comp-a :msg="msg"></comp-a>
</template>
<script>
export default {
  data() {
    return {
      msg: "",
    };
  },
    components:{
        CompA
    }
};
</script>
```

子组件

```vue
<template>
<div>
    {{msg}}
    </div>
</template>
<script>
    export default{
        props:[
            msg
        ]
    }
</script>
```

### 子组件向父组件通信

事件绑定+事件触发

```vue
<template>

	<comp-a  @click="click"></comp-a>
</template>
<script>
export default {
  data() {
    return {
      msg: "",
    };
  },
    components:{
        CompA
    },
    method:{
        click(msg){
            console.log(msg)
        }
    }
};
</script>
```

```vue
<template>
  <div>
    <input type="text" v-model="msg" />
  </div>
</template>
<script>
export default {
  data() {
    return {
      msg: "",
    };
  },
  method:{
      click(){
          this.emit("click",msg)
      }
  }
};
</script>
```

### 全局通信

```javascript
this.$root.$on("aaa",function(){})

this.$root.$emit("aaa",arg1)
```

# 番外

## vue3

## vue2和vue3区别

### 1、双向数据绑定原理不同

**vue2**：vue2的双向数据绑定是利用**ES5的一个APIObject.definePropert()** 对数据进行劫持，结合发布订阅模式的方式来实现的。

**vue3**：vue3中使用了**ES6的Proxy API**对数据代理。相比vue2.x，使用proxy的优势如下：

- defineProperty只能监听某个属性，不能对全对象监听
- 可以省去for in，闭包等内容来提升效率(直接绑定整个对象即可)
- 可以监听数组，不用再去单独的对数组做特异性操作vue3.x可以检测到数组内部数据的变化。

### 2、是否支持碎片

**vue2**：vue2**不支持**碎片。

**vue3**：vue3**支持碎片（Fragments）**，就是说可以拥有多个根节点。

### 3、API类型不同

**vue2**：vue2使用**选项类型api**，选项型api在代码里分割了不同的属性：data,computed,methods等。

**vue3**：vue3使用**合成型api**，新的合成型api能让我们使用方法来分割，相比于旧的api使用属性来分组，这样代码会更加简便和整洁。

### 4、定义数据变量和方法不同

**vue2**：vue2是把数据放入data中，在vue2中定义数据变量是**data(){}**，创建的方法要在**methods:{}**中。

**vue3**：，vue3就需要使用一个新的setup()方法，此方法在组件初始化构造的时候触发。使用以下三个步骤来建立反应性数据： 

- 从vue引入**reactive**；
- 使用**reactive()** 方法来声明数据为响应性数据；
- 使用setup()方法来返回我们的响应性数据，从而**template**可以获取这些响应性数据。

### 5、生命周期钩子函数不同

**vue2**：**vue2中的生命周期**：

- beforeCreate 组件创建之前
- created 组件创建之后
- beforeMount 组价挂载到页面之前执行
- mounted 组件挂载到页面之后执行
- beforeUpdate 组件更新之前
- updated 组件更新之后

**vue3**：**vue3中的生命周期**：

- setup 开始创建组件
- onBeforeMount 组价挂载到页面之前执行
- onMounted 组件挂载到页面之后执行
- onBeforeUpdate 组件更新之前
- onUpdated 组件更新之后

而且vue3.x 生命周期在调用前需要先进行引入。除了这些钩子函数外，vue3.x还增加了onRenderTracked 和onRenderTriggered函数。

### 6、父子传参不同

**vue2**：父传子，用props,子传父用事件 Emitting Events。在vue2中，会**调用this$emit**然后传入事件名和对象。

**vue3**：父传子，用props,子传父用事件 Emitting Events。在vue3中的setup()中的第二个参数content对象中就有emit，那么我们只要在setup()接收**第二个参数中使用分解对象法取出emit**就可以在setup方法中随意使用了。

### 7、指令与插槽不同

**vue2**：vue2中使用slot可以**直接使用slot**；v-for与v-if在vue2中优先级高的是**v-for指令**，而且不建议一起使用。

**vue3**：vue3中必须使用**v-slot的形式**；vue3中v-for与v-if,只会把当前v-if当做v-for中的一个判断语句，**不会相互冲突**；vue3中移除keyCode作为v-on的修饰符，当然也不支持config.keyCodes；vue3中**移除v-on.native修饰符**；vue3中**移除过滤器filter**。

### 8、main.js文件不同

**vue2**：vue2中我们可以使用**pototype(原型)**的形式去进行操作，引入的是**构造函数**。

**vue3**：vue3中需要使用**结构**的形式进行操作，引入的是**工厂函数**；vue3中app组件中可以**没有根标签**。

### 拓展阅读

### setup()函数特性

- setup()函数接收两个参数：props、context(包含attrs、slots、emit)。
- setup函数是处于生命周期beforeCreated和created俩个钩子函数之前。
- 执行setup时，组件实例尚未被创建（在setup()内部，this不会是该活跃实例得引用，即不指向vue实例，Vue为了避免我们错误得使用，直接将setup函数中得this修改成了undefined）。
- 与模板一起使用时，需要返回一个对象。
- 因为setup函数中，props是响应式得，当传入新的prop时，它将会被更新，所以不能使用es6解构，因为它会消除prop得响应性，如需解构prop，可以通过使用setup函数中得toRefs来完成此操作。
- 在setup()内使用响应式数据时，需要通过 .value 获取。
- 从setup() 中返回得对象上得property 返回并可以在模板中被访问时，它将自动展开为内部值。不需要在模板中追加.value。
- setup函数只能是同步的不能是异步的。
