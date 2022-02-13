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

