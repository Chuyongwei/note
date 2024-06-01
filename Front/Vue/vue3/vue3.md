# vue-study-web16

> 开课吧web全栈的typescript的学习

### Ts加装

在已存在的项目装ts

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
+ 添加tsconfig.json文件(<span style="color:red">注意</span>: 因为是从原始项目转换的会有很多的警告却并不影响使用,在修改完全前我们可以加配置)
	```json
	{
		"noImplicitAny": false,//无视警告
	}
	```

### Ts学习

vue3模板

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

# vue3

[不是官方！！不是官方！！](https://vue3js.cn/)

### 在前面

#### 去除ESLint限制

找到package.josn 文件
删除掉 eslint:recommended 这一行

### 去除引用限制

```json
"eslintConfig": {
  "rules": {
    "vue/no-unused-components": "off"
  }
}
```



## [组合式API](https://cn.vuejs.org/guide/extras/composition-api-faq.html)



通过组合式 API，我们可以使用导入的 API 函数来描述组件逻辑。在单文件组件中，组合式 API 通常会与 [``](https://cn.vuejs.org/api/sfc-script-setup.html) 搭配使用。这个 `setup` attribute 是一个标识，告诉 Vue 需要在编译时进行一些处理，让我们可以更简洁地使用组合式 API。比如，`<script setup>` 中的导入和顶层变量/函数都能够在模板中直接使用。

### setup

```vue
<template>
	<div>
        {{msg}}
    </div>
</template>
<script>
    export default {
    setup () {
        console.log("setup");
        const msg = "hello"
        return {
            msg
        }
    },
    beforeCreate(){
        console.log("beforeCreate");
    },
    created(){
        console.log("created");
    }
    
}
</script>
```

### 响应

```javascript
data(){
    return{
        obj:{},
        msg:""
    }
}
```



### ref

> 变量为有value值的响应式

```javascript
        let counter = ref(0) // ref返回带有vlue属性的对象
        function changeCounter(){
            counter.value++
        }
```

### reactive

```javascript
    const obj = reactive({
      name: "张三",
      age: 123,
      children: {
        name: "儿子",
      },
    });
    function changeObj() {
      obj.age = 43;
    }
```



### toRefs

> 将对象的内容变为响应式（ref形式）

```javascript
let obj = {
    name:"sfd",
    value: 432
}
let name,value = toRefs(obj)
```

### 监听

### 选项式Api

```javascript
watch:{
    counter(newVal,oldVal){
        
    }
}
```

### watch

> 监听变量

```javascript
watch(msg,(newVal,oldVal)=>{
    consolt.log(newVal);
    consolt.log(oldVal)
})
```

### watcheffect

>不用设定监听属性，自动收集依赖,引用的变量修改时改变

```javascript
watchEffect(()=>{
  console.log("监听effect",obj.age);
  console.log(counter);
})
```

watch与watcheffect的区别：

- watchEffect不需要指定监听的属性，自动收集依赖，只要在回调中引用到了响应式的属性，只要这些属性发生改变，回调就会执行

  watch只能监听指定的属性。做出回调函数的执行，可可以监听多个vue3开始后

- watch可以获取到新值和旧值wathcEffect拿不到

- watchEffect在组建初始化的时候就会自动执行一次，用来收集依赖，watch不需要

### 使用计算属性

```javascript
computer:{
    a(){
        return ...
    }
}
```

### computer

```javascript
    let msg = ref("hello");
    let reverseMsg = computed(() => {
      return msg.value.split("").reverse().join("");
    });
    const obj = reactive({
      name: "张三",
      reverseMsg: computed(() => {
        return msg.value.split("").reverse().join("");
      }),
      children: {
        name: "儿子",
      },
    });
    return {
      msg,
      reverseMsg,
      obj,
    };
```

### 生命周期



