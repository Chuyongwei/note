

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



## 类Class

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

###  生成器函数function * (es6)

> function *  生成器函数(*generator function*) 它返回一个  [`Generator`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator) 对象。使用yield打断点 next()方法继续进行

```js
function * loop () {
  for (let i = 0; i < 5; i++) {
    yield console.log(i)
  }
}

const l = loop()

l.next()// 0
l.next()// 1
l.next()// 2
```

- 赋值

  ```js
  function * gen () {
    let val
    val = yield 1 // 因为是个数不是输出所以没反应
    console.log(val)
  }
  
  const l = gen()
  l.next()// 找业务和函数结尾
  l.next()// 意思就是业务的返回值是undefined（不是next()）
  ```

- 数组

  ```js
  function * gen () {
    let val
    val = yield [1, 2, 3] //{value: Array(3), done: false}
    console.log(val)
  }
  const l = gen()
  console.log(l.next())
  /*-------加星号后---------*/
  function * gen () {
    let val
    val = yield * [1, 2, 3] // {value: Array(3), done: false}
    console.log(val)
  }
  const l = gen()
  console.log(l.next()) // {value: 1, done: false}
  ```

- 赋值

  ```js
  function * gen () {
    let val
    val = yield [1, 2, 3]
    console.log(val)
  }
  
  const l = gen()
  console.log(l.next(10)) // 到了yield停止
  // l.return() 业务提前结束
  // // console.log(l.return(100)) //返回value为100
  console.log(l.next(20)) // 业务的返回值为20 外部传到yield的点给val值
  ```

- 报错

  ```js
  function * gen () {
    while (true) {
      try {
        yield 1
      } catch (e) {
        console.log(e.message)
      }
    }
  }
  const g = gen()
  console.log(g.next())
  console.log(g.next())
  g.throw(new Error('ss')) // 报错继续运行
  console.log(g.next())
  ```

- 实例

  抽奖问题

  > 不能将数据一次性输出希望主持人按一次出一个数字

  ```js
  function * draw (first = 1, second = 3, third = 5) {
    let firstPrize = ['1A', '1B', '1C', '1D']
    let secondPrize = ['2A', '2B', '2C', '2D', '2E', '2F']
    let thirdPrize = ['3A', '3B', '3C', '3D', '3E', '3F', '3E', '3F']
    let count = 0
    let random
    while (1) {
      if (count < first) {
        random = Math.floor(Math.random() * firstPrize.length)
        yield firstPrize[random]
        count++
        firstPrize.splice(random, 1)
      } else if (count < first + second) {
        random = Math.floor(Math.random() * secondPrize.length)
        yield secondPrize[random]
        count++
        secondPrize.splice(random, 1)
      } else if (count < first + second + third) {
        random = Math.floor(Math.random() * thirdPrize.length)
        yield thirdPrize[random]
        count++
        thirdPrize.splice(random, 1)
      }
    }
  }
  
  let d = draw()
  
  console.log(d.next().value)
  console.log(d.next().value)
  console.log(d.next().value)
  console.log(d.next().value) 
  ```

- 文献

  [What are JavaScript Generators and how to use them](https://codeburst.io/what-are-javascript-generators-and-how-to-use-them-c6f2713fd12e)

  [The Basics of ES6 Generators](https://davidwalsh.name/es6-generators)

  [A Practical Introduction to ES6 Generator Functions](https://davidtang.io/2016/10/15/a-practical-introduction-to-es6-generator-functions.html)

  [Generator](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator)

  [迭代器和生成器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Iterators_and_Generators)

## Object

### 基本使用

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
  
  es6
  
  ```js
  let x = 1; let y = 2; let z = 3
  let obj = {
    x,
    y,
    [z + y]: 6
  }
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

### 连接/变量输出

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

<!--lesson2-9-->

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

- 文献

  [解构赋值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

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

<!--lesson2-9-->

> 异步处理不用阻塞当前[线程](https://baike.baidu.com/item/线程/103101)来等待处理完成，而是允许后续操作，直至其它线程将处理完成，并回调通知此线程。
>
> 做了一系列的操作后告诉主机

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



## 反射Reflect

[深入理解 ES6 中的反射](https://juejin.im/post/5a0cf3745188254dd935f342)

<!-- lesson2-10 -->

```js
/* console.log(Math.floor.apply(null, [3.72]))
console.log(Reflect.apply(Math.floor, null, [3.72]))
let price = 91.5
// if (price > 100) {
//   price = Math.floor.apply(null, [price]) // 先制定方法
// } else {
//   price = Math.ceil.apply(null, [price])
// }
price = price > 100 ? Math.floor.apply(null, [price]) : Math.ceil.apply(null, [price])
console.log(price)
console.log(Reflect.apply(price > 100 ? Math.floor : Math.ceil, null, [price])) // 执行时根据条件调用什么方法
 */

// let d = Reflect.construct(Date, [])
// console.log(d.getTime(), d instanceof Date)

// Reflact和object差不多，但是Reflact返回ture
const studen = {}
let r = Reflect.defineProperty(studen, 'name', { value: 'Mike' })// 修改属性
console.log(studen, r)
const obj = { x: 1, y: 2 }
// Reflect.deleteProperty(obj, 'x') // 删除属性
// console.log(obj)
// console.log(Reflect.get(obj, 'x'))

// console.log(Reflect.getOwnPropertyDescriptor(obj, 'x'))// 静态反射方法
// console.log(Reflect.has(obj, 'ds3')) // 该属性是否存在 objact方法没有
// Object.freeze(obj) // 冻结
console.log(Reflect.isExtensible(obj)) // 是否为可扩展

console.log(Reflect.ownKeys(obj)) // 返回所有的键

// Symbol es6没有用过
// Reflect.preventExtensions(obj) // 阻止扩展
// console.log(Reflect.isExtensible(obj))
Reflect.set(obj, 'z', 4)
console.log(obj)
const arr = ['duck', 'duck', 'duck']
// Reflect.set(arr, 2, 'goose')
// console.log(arr)
// console.log(Reflect.getPrototypeOf(arr)) // 获取原型对象
Reflect.setPrototypeOf(arr, String.prototype) // 修改原型对象
// arr.sort()
console.log(Reflect.getPrototypeOf(arr))

```

## 代理

<!-- lesson2-11 -->

```js
let o = {
  name: 'xiaoming',
  price: 190
}
let d = new Proxy(o, {
  get (target, key) {
    if (key === 'price') {
      return target[key] + 20
    } else {
      return target[key]
    }
  }
})
console.log(d.price, d.name)
```

[Object.defineProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty):添加更改方法

[读懂属性描述符](https://juejin.im/post/5c7ca26ae51d457cd40503d2)

值

```js
let o = {
  name: 'xiaoming',
  price: 190
}
```

### 只读

<!-- lesson2-11 -->

es5

```js
for (const [key] of Object.entries(o)) { // entries将对象变为键值对
  // 属性描述符
  Object.defineProperty(o, key, {
    writable: false // 是否可写
  })
}
o.price = 300
console.log(o.name, o.price)

```

es6

```js
let d = new Proxy(o, {
  get (target, key) {
    return target[key]
  },
  set (target, key, value) { // o price 300 类 属性 值
    return false
  }
})
d.price = 300 // 因为set为false无法改变
console.log(d.price, d.name)
```

### 校验

```js
let d = new Proxy(o, {
  get (target, key) {
    return target[key] || ''
  },
  set (target, key, value) {
    if (Reflect.has(target, key)) {
      if (key === 'price') {
        if (value > 300) {
          return false
        } else {
          target[key] = value
        }
      }
    } else {
      return false
    }
  }
})
d.price = 543 // price大于300的值赋值不了
d.age = 'fdsf'// 属性不存在，赋值失败
console.log(d.price, d.name，d.age) 
```

- 将set方法拿出


```js
let validator = (target, key, value) => {
  if (Reflect.has(target, key)) {
    if (key === 'price') {
      if (value > 300) {
        return false
      } else {
        target[key] = value
      }
    }
  } else {
    return false
  }
}
let d = new Proxy(o, {
  get (target, key) {
    return target[key] || ''
  },
  set: validator
})
```

### 代理和变量管理

```js
class Component {
  constructor () {
    this.proxy = new Proxy({
      id: Math.random().toString(36).slice(-8) // 36进制截取后8位
    }, {})
  }

  get id () {
    return this.proxy.id
  }
}

let com = new Component()
let com2 = new Component()
com.id = 43242
for (let i = 0; i < 10; i++) {
  console.log(com.id, com2.id)
}
```

### 撤销

>  使用revoke将代理撤销

```js
let d = Proxy.revocable(o, {
  get (target, key) {
    if (key === 'price') {
      return target[key] + 20
    } else {
      return target[key]
    }
  }
})
console.log(d.proxy.price, d)
setTimeout(function () {
  d.revoke()
  setTimeout(function () {
    console.log(d.proxy.price)
  }, 100)
}, 1000)
```

[**ES6 Proxies in Depth**](https://ponyfoo.com/articles/es6-proxies-in-depth)

[Meta Programming In JavaScript-Part Three: Proxies and Reflection](https://lucasfcosta.com/2016/11/15/Meta-Programming-in-JavaScript-Part-Three.html)

[10 Use Cases for Proxy](http://brianyang.com/es6-features-10-use-cases-for-proxy/)

How to Use Proxies

[ES6 Proxies in practice](https://habr.com/en/post/448214/)



## 迭代器

<!-- 2020.07.12 -->

<!-- lesson-2.13 -->

### 遍历

对象

```js
let authors = {
  allAuthors: {
    fiction: ['Agla', 'Skks', 'LP'],
    scienceFiction: ['Neal', 'Arthru', 'Ribert'],
    fantasy: ['J.R.Tole', 'J.M.R', 'Terry P.K']
  },
  Addres: []
}
```

遍历规范

```js
authors[Sysbol.iterator] = function () {
  return {
    return {
      done: false, // 是否遍历完成
      value: 1 // 遍历的值
    }
  }
}
```

遍历

```js
authors[Symbol.iterator] = function * () {
  let allAuthors = this.allAuthors
  let keys = Reflect.ownKeys(allAuthors)
  let values = []
  while (1) {
    if (!values.length) {
      if (keys.length) {
        values = allAuthors[keys[0]]
        keys.shift() // shift() 方法从数组中删除第一个元素，并返回该元素的值。此方法更改数组的长度。
        yield values.shift()
      } else {
        return false
      }
    } else {
      yield values.shift()
    }
  }
}

let r = []
for (let v of authors) {
  r.push(v)
}
console.log(r)
```



文献

[A Simple Guide to ES6 Iterators in JavaScript with Examples](https://codeburst.io/a-simple-guide-to-es6-iterators-in-javascript-with-examples-189d052c3d8e)

[ES6 迭代器：Iterator, Iterable 和 Generator](https://harttle.land/2018/09/29/es6-iterators.html)

[ES6 Iterators and Generators in Practice](http://www.zsoltnagy.eu/es6-iterators-and-generators-in-practice/)

[ES6 Generators and Iterators: a Developer’s Guide](https://www.sitepoint.com/ecmascript-2015-generators-and-iterators/)

## 模块

### 导出

```javascript
export const name = 'hello'
export let addr = 'Beijing'
export var list = [1, 2, 3, 4, 45]
```

一起

```javascript
const name = 'hello'
let addr = 'Beijing'
var list = [1, 2, 3, 4, 45]
export default name
export {
  addr,
  list
}
```

### 导入

```javascript
import name, { addr, list } from './lessson2-14-mod'
console.log(name, addr, list)
```

### 改名

```javascript
import { list as list2 } from  './lessson2-14-mod'
```

> 函数没有问题

### 对象

```js
const data = {
  code: 1,
  message: 'success'
}

const des = {
  age: 20,
  addr: 'Beijing'
}

export default {
  data,
  des
}
```

错误方式

```js
import { data, des } from './lessson2-14-mod'//不知道是对象内容还是对象
```

正确方式

```js
import obj from './lessson2-14-mod'
let { data, des } = obj
console.log(data, des)
```

### 类

输入

```javascript
export class Test {
  constructor () {
    this.id = 5
  }
}

export class Animal {
  constructor () {
    this.name = 'dog'
  }
}

export default class People {
  constructor () {
    this.id = '132'
  }
}
```

输出

```js
import { Test, Animal } from './lessson2-14-mod'
let test = new Test()
console.log(test.id)
let animal = new Animal()
console.log(animal.name)
```



```js
import * as Mod from './lessson2-14-mod'
let test = new Mod.Test()
console.log(test.id)
let animal = new Mod.Animal()
console.log(animal.name)
let people = new Mod.default()
console.log(people.id)
```



### 文献

[[es6] import, export, default cheatsheet](https://hackernoon.com/import-export-default-require-commandjs-javascript-nodejs-es6-vs-cheatsheet-different-tutorial-example-5a321738b50f)

[ECMAScript 6 modules: the final syntax](https://2ality.com/2014/09/es6-modules-final.html)



## ES7

<!-- 2020.07.14 -->

### 包含

```js
const arr = [1, 2, 3, 4, 5]
console.log(arr.includes(40)) // arr是否包含40
```

### 平方

```js
// console.log(Math.pow(2, 5)) // 2的5次平方

console.log(2 ** 5) // es7
```

## ES8

### async关键字

<!-- lesson4-1 -->

> 将方法变成promise对象

```js
async function firstAsunc () {
  return 27
}

console.log(firstAsunc()) // Promise{<resolved>: 27}
```

对象

```js
async function firstAsunc () {
  return 27 // 类似于Promise.resolve(27)
}

console.log(firstAsunc().then(val => {
  console.log(val)
}))
```

### await关键字

> 必须与async一起用

```js
async function firstAsync () {
  let promise = new Promise((resolve, reject) => {
    setTimeout(function () {
      resolve('now it is done')
    }, 1000)
  })

  promise.then(val => {
  	console.log(val)
  })
  console.log(2)
  return Promise.resolve(3)
}

firstAsync().then(val => {
  console.log(val)
})
/*
2
3
now it is done
*/
```

await方式

```js
async function firstAsync () {
  let promise = new Promise((resolve, reject) => {
    setTimeout(function () {
      resolve('now it is done')
    }, 1000)
  })

  let result = await promise
  console.log(result)
  console.log(2)
  return Promise.resolve(3)
}

firstAsync().then(val => {
  console.log(val)
})
/*
now it is done
2
3
*/
```

### 遍历对象

<!-- lesson4-2 -->

```js
let grade = {
  'lilei': 96,
  'hanmeimei': 99
}
console.log(Object.keys(grade))
console.log(Object.keys(grade).filter(item => item === 'lilei'))// lilei // 筛选key为lilei的键
console.log(Object.values(grade).filter(item => item > 96)) // 99
```



```js
let result = []
for (let [k, v] of Object.entries(grade)) { // Object.entries使得对象可遍历
  console.log(k, v)
  if (k === 'lilei') {
    result.push(k)
  }
}

console.log(result)
```

### 填补

<!-- lesson4-3 -->

```js
for (let i = 1; i < 32; i++) {
 console.log(i.toString().padStart(2, '0')) // 几位字符 填充字符
}
// 01 02 03 04 05 06 07 08 09 10...
```

末位补位

```js
console.log(i.toString().padEnd(5, 'X$')) // 后面补位
```

## ES9

### 关于多个异步操作运行

同时运行

```js
function Gen (time) {
  return new Promise((resolve, reject) => {
    setTimeout(function () {
      resolve(time)
    }, time)
  })
}
async function test () {
  let arr = [Gen(2000), Gen(1000), Gen(3000)]
  for (let item of arr) {
    console.log(Date.now(), item.then(console.log)) // 同时运行完成
  }
}
test()
```

同时运行，排队结束

```js
function Gen (time) {
  return new Promise((resolve, reject) => {
    setTimeout(function () {
      resolve(time)
    }, time)
  })
}
async function test () {
  let arr = [Gen(2000), Gen(1000), Gen(3000)]
  for (let item of arr) {
    console.log(Date.now(), await item.then(console.log)) // 同时运行但是执行完要等到前一个任务完成
  }
}
test() 
```

排队运行

```js
function Gen (time) {
  return new Promise((resolve, reject) => {
    setTimeout(function () {
      resolve(time)
    }, time)
  })
}
async function test () {
  let arr = [Gen(2000), Gen(1000), Gen(3000)]
  for await (let item of arr) {
    console.log(Date.now(), item) // 只有前一个任务完成才能运行后面的操作
  }
}
test()
```



### 异步方法

```js
const obj = {
  count: 0,
  Gen (time) {
    return new Promise((resolve, reject) => {
      setTimeout(function () {
        resolve({ done: false, value: time })
      }, time)
    })
  },
  [Symbol.asyncIterator] () {
    let self = this
    return {
      next () {
        self.count++
        if (self.count < 4) {
          return self.Gen(Math.random() * 1000)
        } else {
          return Promise.resolve({
            done: true,
            value: ''
          })
        }
      }
    }
  }
}
```



### 对象合并

<!-- 2020.07.16 -->

<!-- lesson5-3 -->

```js
const input = {
  a: 1,
  b: 2,
  c: 3,
  d: 5
}
const output = {
  ...input,
  c: 3
} // 数组嵌套
console.log(input, output)
const { a, b, ...rest } = input // 将必填项找到，其余的做成数组
console.log(a, b, rest)
```



### 正则的s模式

```js
// console.log(/foo.bar/us.test('foo\nbar')) // s匹配换行符，u匹配unicode编码
const re = /foo.bar/sugi
console.log(re.dotAll) // 验证正则是否有s模式
console.log(re.flags) // 显示开启的模式
```

###  [match](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/match)

```js
/*
// match
const t = '2019-06-07'.match(/(\d{4}-(\d){2})-(\d{2})/) // "2019-06-07", "2019-06", "6", "07", index: 0, input: "2019-06-07", group: undefine
console.log(t[1]) // 2019-06
console.log(t[2]) // 6
console.log(t[3]) // 07 */

// groups 是什么？
// const t = '2019-06-07'.match(/(?<year>\d{4}-(?<mouth>\d){2})-(?<day>\d{2})/) // "2019-06-07", "2019-06", "6", "07", index: 0, input: "2019-06-07",groups: {year: "2019-06", mouth: "6", day: "07"}
// console.log(t)
// console.log(t.groups.year) // 2019

// 先行断言
let test = 'hello world'
console.log(test.match(/hello(?=\sworld)/)) // 先行断言 先找到hello然后看后面是否有world
console.log(test.match(/(?<=hello\s)world/))// 后行断言 先找到world然后看前面面是否有hello
console.log(test.match(/(?<!hello\s)world/)) // 先找到world然后看前面面是否不是hello
```



### 文献

[ES2018: asynchronous iteration - 2ality](https://2ality.com/2016/10/asynchronous-iteration.html)

[Promise.prototype.finally() - MDN Web Docs - Mozilla](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally)

[Upcoming regular expression features ](https://developers.google.com/web/updates/2017/07/upcoming-regexp-features)

[JavaScript 正则表达式匹配汉字- 知乎](https://zhuanlan.zhihu.com/p/33335629)                                                                                                                                                                                                                                                                                                             

## ES10

### 扁平化（falt）

- flat扁平化处理 参数扁平化处理几层 默认是1

```js
let arr = [1, [2, 3], [4, 5, [6, 7, [8, 9]]]]
console.log(arr.flat(2)) // [1, 2, 3, 4, 5, Array(2)]
```

- map

  ```js
  let arr = [1, 2, 3]
  console.log(arr.map(item => item * 2)) // [2,4,6] 
  ```

- faltMap

  将一堆数组做遍历后合到一起

  ```js
  console.log(arr.flatMap(item => [item * 2]))
  // arr.map(item => [item * 2]).flat(2)
  ```

去除空格

```js
let str = '   foo  '
console.log(str.trimStart()) // trimLeft 去左边
console.log(str.trimEnd()) // trimRight trimEnd 去右边
console.log(str.trim()) // 都去掉
```



### 数组转对象

```js
const arr = [['foo', 1], ['bar', 2]]
const obj = Object.fromEntries(arr)
console.log(obj.bar)
```

### 挑选对象

```js
const obj = {
  abc: 1,
  def: 2,
  ghksks: 3
}

let res = Object.fromEntries(
  Object.entries(obj).filter(([key, val]) => key.length === 3)
)

console.log(res)
```



### try ...catch

> catch不再需要（e）



### bigint

>  11n bigint 2^53时使用 



文献

[Well-formed JSON.stringify · V8](https://v8.dev/features/well-formed-json-stringify)

[BigInt: arbitrary-precision integers in JavaScript · V8](https://v8.dev/features/bigint)





## vue



```powershell
D:\baidu\doc\VScode\Font\ESlearn\vue-lesson>eslint --init
√ How would you like to use ESLint? · style
√ What type of modules does your project use? · esm
√ Which framework does your project use? · vue
√ Does your project use TypeScript? · No / Yes
√ Where does your code run? · browser
√ How would you like to define a style for your project? · guide
√ Which style guide do you want to follow? · standard
√ What format do you want your config file to be in? · JavaScript
Checking peerDependencies of eslint-config-standard@latest
The config that you've selected requires the following dependencies:

eslint-plugin-vue@latest eslint-config-standard@latest eslint@>=6.2.2 eslint-plugin-import@>=2.18.0 eslint-plugin-node@>=9.1.0 eslint-plugin-promise@>=4.2.1 eslint-plugin-standard@>=4.0.0
√ Would you like to install them now with npm? · No / Yes
Installing eslint-plugin-vue@latest, eslint-config-standard@latest, eslint@>=6.2.2, eslint-plugin-import@>=2.18.0, eslint-plugin-node@>=9.1.0, eslint-plugin-promise@>=4.2.1, eslint-plugin-standard@>=4.0.0
[..................] \ rollbackFailedOptional: verb npm-session 424791964e0b55c1
```

