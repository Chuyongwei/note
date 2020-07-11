

## 作用域

### 全局变量

> 不应该在函数中var定义的，可以在整个项目中使用的

```js
var abc = 1234 //全局变量
abcd = 2345 //全局(即windows)属性
```

结果

```shell
> abcd
< 1453
> delete window.abcd
< true
> delete window.abc
< false
> abcd
  VM608:1 Uncaught ReferenceError: abcd is not defined
    at <anonymous>:1:1
```

### 函数定义

```js
function test(){
    var ab = 45
}
test() //其他地方可以看到ab的值


function test(){
    var a = 3
    return a+3
}
console.log(test()); //6
console.log(a) //undefind
```

### 块状作用域（ES6新特性）

> 在{}中的就是一个块

```js
function test(){
    var a = 3
    function test2(){
        var b = 2
        return a+b //test2没找到a就向上找a
    }
    return test2 //给外界提供访问test2的口子
}
```

>  在ES6以前是没有块状作用域的概念,所以块内部的是不会被封装

```js
function test(){
    var a = 3
    if(a===3){
        var b = 4
        console.log('abc')
    }else{
        console.log('abcd')
    }
    return a+b //可以找到b
}
```

### var，const

let

```js
function test(){
    var a = 3
    if(a===3){
        let b = 4
        console.log('abc')
    }else{
        console.log('abcd')
    }
    return a+b
}
```

> this是一个动态的作用域

var和let的区别

1. window可以访问var
2. var可以重复定义
3. let不能进行变量提升

const

1. 不能给常量复制

[什么是作用域](https://juejin.im/post/5d13a5fce51d455a694f9560)

[JavaScript深入之词法作用域和动态作用域](https://segmentfault.com/a/1190000008972987)

[冴羽的博客](https://github.com/mqyqingfeng/Blog)

[深入理解JS中声明提升、作用域（链）和`this`关键字](https://github.com/creeperyang/blog)



## 数组

### 遍历

#### 一般方法

```js
const arr = [1,2,3,4,5]
for (let i = 0; i < arr.length; i++) {
    const element = arr[i];
    if(element===2){
        // break
        continue
    }
    console.log(element)
}
```

#### each方法

不能用break和continue

```js
arr.forEach(function(item){
    if(item===2){
        // break
        // continue
    }
    console.log(item)
})
```

#### every方法

```js
arr.every(function(item){
    if(item===2){
        return false
    }
    console.log(item)
    return true
})
```

#### for in

> 用于对象使用

```js
for(let index in arr){
    if(index == 2){
        continue
    }
    console.log(index,arr[index])
}
```

#### for of(ES6)

```js
for (let item of arr) {
  console.log(item)
}
```



### 伪数组

> 特性像数组
> 不能直接调用数组的方法

```js
let args = [].slice.call(arguments)
```

特征

1. 靠索引保存数据
2. 有length属性

```
a = { 0: 'a', 1: 'b', length: 2 }
```

通过为数组创建数组

```
let array = Array.from({ length: 5 }, function () { return 1 })
console.log(array)
```



### 创建数组

```js
 // Array.prototype.of
 let array = Array.of(1, 2, 3, 5, 5)
 console.log(array)
 // Array.prototype.fill
 let array = Array(5).fill(1)
 console.log(array)
 // Array.fill(value, start, end)
 console.log(array.fill(8, 2, 4))
```



### 查找数组

- filter

  ```js
  let array = [1, 2, 3, 4]
  let find = array.filter(function (item) {
    return item % 2 === 0
  })
  console.log(find)//返回数组
  ```

- find 找到后就停止(ES6)

  ```js
  let find = array.find(function (item) {
    return item % 2 === 0
  })
  console.log(find)//返回单个字符
  ```

- findIndex 查找第一次的位置

  ```js
  let find = array.findIndex(function (item) {
    return item % 2 === 0
  })
  console.log(find)//返回第一个位置
  ```

-   ES6 find 和 filter 的区别

  遇到个功能是要分类就想说在前端过滤，不要从查数据库的时候过滤了。然后就想说除了filter还有啥好用的
  发现有个find，测试一番之后发现

  ```js
  const list = [{'name':'1',index:1},{'name':'2'},{'name':'1'}]
  let list2 = list.find(i=>i.name==='1') 
  
  let list3 = list.filter(i=>i.name==='1')
  
  console.log(list); // [ { name: '1', index: 1 }, { name: '2' }, { name: '1' } ]
  console.log(list2); // { name: '1', index: 1 }
  
  console.log(list3); // [ { name: '1', index: 1 }, { name: '1' } ]
  ```

  find 和 filter 都是不改变原数组的方法

  > 但是find只查出第一个符合条件的结果像例子里是直接返回了一个对象而不是数组！而filter返回全部结果仍然是数组。

  

- [专栏:JavaScript：遍历](https://juejin.im/post/5ceb84afe51d45775d516f00) 



## 类

### 声明类

创建类

```js
let Animal = function (type) {
  this.type = type
  this.eat = function () {
  	console.log('I am eat food')
  }
}
let dog = new Animal('dog')
let monkey = new Animal('Monkey')
```

如果eat方法是静态方法在初始化时会发生占用大量空间，所以我们将方法放入原型链中

```js
Animal.prototype.eat = function () {
  console.log('I am eat food')
}
```

如果使用下面的方法，就只改dog的方法

```js
dog.eat = function () {
  console.log('error')
}
```

用下面方法会更改所有该类的方法（方法要在原型链上）

```js
dog.constructor.prototype.eat = function () {
  console.log('error')
}
```

ES6语法

```js
class Animal {
  constructor (type) {
    this.type = type
  }

  eat () {
    console.log('I am eat food')
  }
}

let dog = new Animal('dog')
let monkey = new Animal('Monkey')

console.log(dog) //Animal {type: "dog"}
console.log(monkey)
dog.eat()
monkey.eat()

console.log(typeof Animal) // function
```



### 读写属性的保护

es6

```js
let _age = 4
class Animal {
  constructor (type) {
    this.type = type
  }
  get age () {
    return _age
  }
  set age (val) {
    if (val < 7 && val > 4) {
      _age = val
    }
  }

  eat () {
    console.log('I am eat food')
  }
}
let dog = new Animal('dog')
console.log(dog.age)
dog.age = 6
console.log(dog.age)
```



### 静态方法和类方法

```js
let Animal = function (type) {
  this.type = type
}

Animal.prototype.eat = function () {
  Animal.walk()
  console.log('i am eat food hello')
}

Animal.walk = function () {
  console.log('i am walk')
}

let dog = new Animal('dog')
dog.eat()
```

es6

```js
class Animal {
  constructor (type) {
    this.type = type
  }
  eat () {
    Animal.walk()
    console.log('i am eat food')
  }
  static walk () { // 静态方法属于类的
    console.log('i am waling')
  }
}

let dog = new Animal('dog')
dog.eat()
```

> 静态方法不用调用实例的东西



### 继承

es5

```js
let Animal = function (type) {
  this.type = type
}

let Dog = function () {
  // 初始化父类的构造函数
  Animal.call(this, 'dog')
  this.run = function () {
    console.log('i can run')
  }
}
// 引用
Dog.prototype = Animal.prototype

let dog = new Dog()
dog.run()
console.log(dog.type)
```

es6

```js
class Animal {
  constructor (type) {
    this.type = type
  }
  eat () {
    Animal.walk()
    console.log('i am eat food')
  }
  static walk () { // 静态方法属于类的
    console.log('i am waling')
  }
}

class Dog extends Animal {
  constructor (type) {
    super('dog')// 不管如何参数要与父类的一样
    this.age = 2
  }
}
let dog = new Dog()
dog.eat()
console.log(dog.type)
```



## 函数

### 默认值

```js
function f (x, y, z) {
  if (y === undefined) {
    y = 7
  }
  if (z === undefined) {
    z = 42
  }
  return x + y + z
}

console.log(f(3))

```

es6

```js
function f6 (x, y = 7, z = 42) {
  return x + y + z
}
  
console.log(f6(1, undefined, 43))
```



```js
function f6 (x, y = 7, z = x+y) {
  return x *10 + z
}
console.log(f6(1))
```

### 不确定参数

```js
function sum (...nums) {
  // Rest paraameter
  let num = 0
  nums.forEach(function (item) {
    num += item * 1
  })
  return num
}

console.log(sum(1, 2, 3, 5))
```

```js
function sum (base, ...nums) {
  // Rest paraameter
  let num = 0
  nums.forEach(function (item) {
    num += item * 1
  })
  return base * 2 + num
}

console.log(sum(1, 2, 3, 5))
```

### 数组参数（三点）

```js
function sum (x, y, z) {
  return x + y + z
}

let data = [4, 5, 6]
console.log(sum.apply(this, data)) // es5
// spered
console.log(sum(...data)) // es
```

### 箭头函数

```js
let hello = (name, city) => {
  console.log('hello world', name, city)
}

hello('immoc', '背景')
```

只有一个函数可以省略括号

```
let hello = name => {
  console.log('hello world', name)
}
```

返回表达式省略花括号

```js
let sum = (x, y, z) => x + y + z
console.log(sum(1, 2, 4)) // 7
```

返回对象要加括号

```js
let sum = (x,y,z) => ({
  x:x,
  y:y,
  z:z
})
console.log(sum(1, 2, 4)) // {x: 1, y: 2, z: 4}
```

this指向写代码时原来的this

```js
let test = {
  name :'test',
  say : ()=>{
    console.log(this.name,this)
  }
}
test.say()
```

## Object

属性值支持表达式和变量（要加`[]`）

- 定义函数

  es5

  ```js
  hello: function () {
    console.log('hello')
  }
  ```

  es6

  ```js
  hello () {
    console.log('hello')
  }
  ```

- es5不支持异步方法

- 定义变量属性

  es5

  ```js
  let x = 1; let y = 2; let z = 3
  let obj = {
    x,
    y
  }
  obj[z + y] = 6
  ```



### 拷贝

可以进行

```js
const target = {}
// const source = { b: 4, c: 5 }
Object.assign(target, source) // 拷贝 现在
console.log(target, source) // 
```

> 弊端：属于浅拷贝，当值是一个对象时烤的是地址，会把原来的对象覆盖

## 容器

### set

```js
// Set
// 无序不重复
let s = new Set()
// let s = new Set([1, 2, 3, 4, 5])
// s.add('hello')
// s.add('goodbye')
s.add('hello').add('goodbye')
// s.delete('hello')
// s.clear()
console.log(s.has('hello3'), s.size)
// console.log(s.keys()) // 遍历器
// console.log(s.values())
// console.log(s.entries())
s.forEach(item => { // 遍历
  console.log(item)
})
```

### map

```js
let map = new Map([[1, 'value-3'], [3, 'value-4']])
// map.set(1,4) // 添加和修改
// map.delete(1)
console.log(map.get(1))// 获取键的值
console.log(map.has(2))// 检查是否存在该键

console.log(map)
console.log(map.keys(), map.values(), map.entries()) // 各种的集合

//遍历方法
// map.forEach((value, key) => {
//   console.log(value, key)
// })
for (let [key, value] of map) {
  console.log(key, value)
}

//键可以是function
let o = function () {
  console.log("0")
}
map.set(o, 4)
console.log(map.get(o))
```



## 字符

### 连接

```js
const a = 20
const b = 10
const c = 'javascript'

// const str = 'my age is ' + (a + b) + ' i love ' + c
const str = `my age is ${a + b} i love ${c}`
console.log(str)
```

### 换行

```js
let g = `我是第一行
换行了`
console.log(g) 
/*
我是第一行
换行了
*/
```

### 字符串-函数

```js
function Price (strings, type) {
  let s1 = strings[0]
  const retailPrice = 20
  const wholeSalePrice = 16
  let showTxt
  if (type === 'retail') {
    showTxt = '购买单价是：' + retailPrice
  } else {
    showTxt = '购买的批发价是：' + wholeSalePrice
  }
  // console.log(s1) // 您此次的
  return `${s1}${showTxt}`
}

let showTxt = Price`您此次的${'retail'}`
console.log(showTxt)
```

### 解构赋值



```js
let arr = ['hello', 'world']
let [firstName,surName] = arr // 解构赋值
console.log(firstName,surName) // hello world
```

- 选择

  ```js
  let arr = 'abcd'
  let [firstName, , thridName] = arr
  console.log(firstName, ,thridName) //a c
  ```

- 对象

  ```js
  let user = { name: 's', surname: 't' };// 对象要加分号
  [user.name, user.surname] = [1, 2]
  console.log(user) // 1  2
  ```

- 显示

  ```js
  let arr = [1, 2, 3, 4, 5, 6, 7, 8]
  let [firstName, curName, ...last] = arr
  console.log(firstName, curName, last)
  ```

  

- 在解构赋值时若没有参数则显示`undefnd`

- 若不想显示`undefind`则写成

  ```js
  let [firstName=‘hello’, curName, ...last] = arr
  ```

- 对象取值

  ```js
  let options = {
    title: 'menu',
    // width: 100,
    height: 200
  }
  let { title: title2, width = 130, height } = options
  console.log(title2, width, height)
  ```

  只想关注某些变量

  ```js
  let options = {
    title: 'menu',
    width: 100,
    height: 200
  }
  let { title, ...last } = options
  // menu {width: 100, height: 200}
  ```

  复杂点的

  ```js
  let options = {
    size: {
      width: 100,
      height: 200
    },
    items: ['Cake', 'Donut']
  }
  
  let { size: { width: width2, height }, items: [item1] } = options
  console.log(width2, height, item1)
  ```

- [解构赋值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

  [Destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

  [ES6 JavaScript Destructuring in Depth](https://ponyfoo.com/articles/es6-destructuring-in-depth)

## 正则

### y修饰符

```js
const s = 'aaa_aa_a'
const r1 = /a+/g // ^ $
const r2 = /a+/y // 粘连紧跟着查找

console.log(r1.exec(s)) // aaa
console.log(r2.exec(s)) // aaa

console.log(r1.exec(s)) // aaa
console.log(r2.exec(s)) // aaa
```

### u修饰符

[编码](http://www.fileformat.info/info/unicode/char/search.htm)

```js
let s2 = '\uD842\uDFB7'
console.log(/^\uD842/.test(s2))
console.log(/^\uD842/u.test(s2))
console.log(/^.$/u.test(s2))
// \u将编码装换成字符
console.log(/\u{61}/u.test('a'))
```

## 异步操作

### 默认异步

> js是单线程，先加载的1.js文件，然后发生异步操作不等待js文件的运行先让test运行

```js
function loadScript (src) {
  let script = document.createElement('script')
  script.src = src
  document.head.append(script)
}

function test () {
  console.log('test')
}
loadScript('./1.js', test())

loadScript('./1.js', function (script) {
  console.log(script)
  loadScript('./2.js', function (script) {
    console.log(script)
    loadScript('./3.js', function (script) {
      console.log(script)
    })
  })
})
```

> 为了解决多层嵌套的问题导致的代码复杂我们使用Promise

### Promise

<!-- 2020.07.11 -->

<!-- 将2.39，40看看 -->

```js
function loadScript (src) {
  return new Promise((resolve, reject) => {
    let script = document.createElement('script')
    script.src = src
    script.onload = () => resolve(src)
    script.onerror = (err) => reject(err)
    document.head.append(script)
  })
}
loadScript('./4.js')
  .then(() => {
    return loadScript('./2.js') // 如果不加return就将方法认为成表达式，无法阻止报错后的运行，加上后就返回了Promise实例
  }, err => {
    console.log(err)
  })
  .then(() => {
    loadScript('./3.js')
  }, err => {
    console.log(err)
  })
```

### Resolve&Reject

> Resolve静态方法
>
> Reject报错

```js
function test (bool) {
  if (bool) {
    return new Promise((resolve, reject) => {
      resolve(20)
    })
  } else {
    // return Promise.resolve(42) // 静态方法
    return Promise.reject(new Error('ss'))
  }
}

test(0).then(value => {
  console.log(value)
}, (err) => {
  console.log(err)
})
```

### 集体抓错

```js
loadScript('./1.js')
  .then(() => {
    return loadScript('./2.js')
  })
  .then(() => {
    return loadScript('./3.js')
  })
  .catch((err) => { // 实例方法不是静态，使用promise的reject方法不是throw new Error
    console.log(err)
  })
```

### all

```js
const p1 = Promise.resolve(1)
const p2 = Promise.resolve(2)
const p3 = Promise.resolve(3)

Promise.all([p1, p2, p3]).then((value) => {
console.log(value)
}) 
```

### race

```js
const p1 = () => {
  return new Promise((resolve, reject) => {
    setTimeout(function () { // 定时器
      resolve(1)
    }, 1000)
  })
}

const p2 = () => {
  return new Promise((resolve, reject) => {
    setTimeout(function () {
      resolve(2)
    }, 0)
  })
}
Promise.race([p1(), p2()]).then((value) => {
  console.log(value)
})
```



## 反射