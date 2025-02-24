vue-study-web16

## ts部分

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

### 接口和泛型

types/index.ts

```ts
// 定义一个接口，用于限制person对象属性
export interface PersonInter {
    id:string,
    name: string,
    age:number
}

export type Persons = Array<PersonInter>
```

使用

```js
    // 引入接口
    import { type PersonInter,type Persons } from '@/types';
   

    // let person:PersonInter = {id: 'sfd123',name:"sfds",age:16}
    let personList:Array<PersonInter> = [
        {id: 'sfd123',name:"sfds",age:16},
        {id: 'sfd123',name:"sfds",age:16},
        {id: 'sfd123',name:"sfds",age:16}
    ]

    let personList2:Persons = [
        {id: 'sfd123',name:"sfds",age:16},
        {id: 'sfd123',name:"sfds",age:16},
        {id: 'sfd123',name:"sfds",age:16}
    ]

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



## 选项式API

### 弊端

>  如果我们要修改一个需求那么我们就要将`data`,`methods`，`computed`这些都要做修改

## [组合式API](https://cn.vuejs.org/guide/extras/composition-api-faq.html)



通过组合式 API，我们可以使用导入的 API 函数来描述组件逻辑。在单文件组件中，组合式 API 通常会与 [``](https://cn.vuejs.org/api/sfc-script-setup.html) 搭配使用。这个 `setup` attribute 是一个标识，告诉 Vue 需要在编译时进行一些处理，让我们可以更简洁地使用组合式 API。比如，`<script setup>` 中的导入和顶层变量/函数都能够在模板中直接使用。

## setup

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

### 语法糖

> 使用语法糖后就可以不用集中return数据了

使用前

```vue
<script lang="ts">
export default {
    name: 'Preson3',
    beforeCreate(){
        // alert("存在beforecreate");
        
    },
    data(){
        return {
            a:100,
            // 此时setup是存在的
            c:this.name
        }
    },
    setup(){
        // setup中没有this
        let name = "李四"
        let age = 10 //此时的变量不是响应式的，因此改了值是不会注入的
        let tel = "1234423"

        function changeAge(){
            age = 20
            console.log("修改年龄",age);
        }

        function showTel(){
            alert(tel)
        }

        return {name,age,tel,
            changeAge,showTel}
    }
}
</script>
```

使用后

```vue
<script lang="ts">
export default {
    name: 'Preson3',
    data(){
        return{
            a:100,
            // 此时setup是存在的
            c:this.name
        }
    }
}
</script>

<script setup lang="ts">
    // setup中没有this
    let name = "李四"
    let age = 10 //此时的变量不是响应式的，因此改了值是不会注入的
    let tel = "1234423"
    function changeAge(){
        age = 20
        console.log("修改年龄",age);
    }
    function showTel(){
        alert(tel)
    }
    
</script>
```

为了方便我们在浏览器中获取数据，我们需要安装一个插件`vite`

然后在根目录`vite.config.ts`

``

### 响应

```javascript
data(){
    return{
        obj:{},
        msg:""
    }
}
```

#### ref

> 将变量变成了响应式的对象，修改变量就要修改`value`

```javascript
import {ref} from 'vue'       
let counter = ref(0) // ref返回带有vlue属性的对象
        function changeCounter(){
            counter.value++
        }
```

也可以处理对象

```js
    let couter = ref([
        {
            name: 'sdf'
        }
        ])
    function changeCounter(){
            counter.value[0] = {name:'sfs'}
        }
```



#### reactive

> 用于处理对象的响应式，不能处理基本类型

```javascript
import {reactive} from 'vue'   
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

如果要整体修改的话建议这样

```js
Object.assign(obj,{...});
Object.assign(car,{bind:"捷达"})
```

#### toRefs

> 将对象的内容变为响应式（ref形式），形成一个来自原对象的新对象

```javascript
import {reactive,toRefs} from 'vue'   
let obj = {
    name:"sfd",
    value: 432
}
let name,value = toRefs(obj)
```

#### toRef

> 将对象的单个对象取出

```ts
import {reactive,toRef} from 'vue'   
let obj = {
    name:"sfd",
    value: 432
}
let name = toRefs(obj,'name')
```



### 监听

#### 选项式Api

```javascript
watch:{
    counter(newVal,oldVal){
        
    }
}
```

#### watch

> 监视ref基本属性
>
> 监视ref对象是监视的是对象的地址，如果要监视属性需要设置deep:true
>
> 监视reactive是默认深度监视,且不可关闭

```javascript
watch(msg,(newVal,oldVal)=>{
    consolt.log(newVal);
    consolt.log(oldVal)
})
```

如果是监听某个属性,并且这个属性是基本类型

```js
watch(()=>person.name,(newVal,oldVal)=>{
    consolt.log(newVal);
    consolt.log(oldVal)
})
```



#### watcheffect

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

#### computer

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

vue3

```javascript
import {ref,computed} from 'vue'
    let fristname = ref("张")
    let nextname = ref("三")
    
    // 计算属性，但是是只读的
    /*
    let fullname = computed(()=>{
        return fristname.value + nextname.value
    })*/

    let fullname = computed({
        get(){
            return fristname.value +"-" + nextname.value
        },
        set(val){
            console.log(val);
            
            const [str1,str2] = val.split('-')
            fristname.value = str1
            nextname.value = str2
        }
    })
```

### ref属性

> 用于获取标签的属性

```vue
<template>
    <div class="preson">
        <h2 ref="title2">我是ref</h2>
        <button @click="showref">点我显示ref</button>
    </div>
</template>

<script setup lang="ts" name="Person">
import {ref,computed,watch,defineExpose} from 'vue'
   

    let title2 = ref()

    function showref(){
        console.log(title2.value);
        
    }
    // TAG 只有这里边的属性才能被非组件看到
    defineExpose({title2,fristname})

</script>
```

父组件

```vue
<template>
  <div>
    你好
    <Preson/>
    <preson-3 ref="ren3"/>
    <person-object/>
    <button @click="showref">显示pernson的内容</button>
  </div>
</template>

<script lang="ts">
import Preson from './components/Preson.vue';
import Preson3 from './components/Preson3.vue';
import PersonObject from './components/Person-object.vue';
import { ref } from 'vue';
export default {
  name:"App",
  components:{
    Preson,
    Preson3,
    PersonObject
  }
}
</script>

<script lang="ts" setup>


// 获取子组件的属性
let ren3 = ref()
function showref(){
  console.log(ren3.value);
  
}
</script>

<style scoped>

</style>
```

### props

父组件

```vue
<template>
  <div>
    <preson-3 ref="ren3" :a="personList2"/>
  </div>
</template>

<script lang="ts">
import Preson3 from './components/Preson3.vue';

import {type Persons } from '@/types';
export default {
  name:"App",
  components:{
    Preson,
    Preson3,
    PersonObject
  }
}
</script>

<script lang="ts" setup>
// props
let personList2:Persons = [
        {id: 'sfd123',name:"sfds",age:16},
        {id: 'sfd123',name:"sfds",age:16},
        {id: 'sfd123',name:"sfds",age:16}
    ]
</script>

<style scoped>

</style>
```



子组件

```js
    let pro = defineProps(['a'])
    console.log("props属性：",pro.a);
```



### 生命周期



# 面试题

## setup和data的关系

> setup要比data快的，因此data可以使用setup的数据，但是反过来就不行了，`时间周期`并且setup是不使用`this`的

```vue
<template>
    <div class="preson">
        人人{{ name }} -- {{ age }} -- {{c}}
        <button @click="showTel">查看联系方式</button>
        <button @click="changeAge">修改年龄</button>
    </div>
</template>
// TAG vue3的组合式API
<script lang="ts">
export default {
    name: 'Preson3',
    beforeCreate(){
        // alert("存在beforecreate");
        
    },
    data(){
        return {
            a:100,
            c:this.name
        }
    },
    setup(){
        // setup中没有this
        let name = "李四"
        let age = 10 //此时的变量不是响应式的，因此改了值是不会注入的
        let tel = "1234423"

        function changeAge(){
            age = 20
            console.log("修改年龄",age);
        }

        function showTel(){
            alert(tel)
        }

        return {name,age,tel,
            changeAge,showTel}
    }
}
</script>

<style scoped>
.preson{
    background-color: skyblue;
    box-shadow: 0 0 10px;
    border-radius: 10px;
    padding: 20px;
}
button{
    margin: 0 5px;
}
</style>
```

## vite插件



