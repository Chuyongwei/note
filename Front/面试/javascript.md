## js加载时机

- 普通的js脚本`js\sync`，在html加载的过程中到达该脚本时候进行加载和执行

- `async`脚本采用异步的方法进行加载

- `defer`延迟加载，在html加载完后执行

- 总结：`async`：==加载完后执行==不依赖于dom中的元素，也不产生与其他脚本有关的数据

  `defer`==加载完后等html渲染完成后执行==依赖dom中产生的元素，或者被其他脚本依赖（因为一个页面并不只有这一个脚本）

### dom与CSSOM

DOM的渲染 是需要等js，css都解析完成后才进行的

### 添加事件

flase具体标签优先

 布尔值参数是true，表示在**捕获阶段**调用事件处理程序；就是**最不具体的节点先接收事件，最具体的节点最后接收事件**

  如果是false，在**冒泡阶段**调用事件处理程序;则是先寻找指定的位置，**由最具体的元素接收，然后逐级向上传播至最不具体的元素的节点（文档）**

```js
function bodyScroll(event){
    event.preventDefault();
}
document.body.addEventListener('touchmove',bodyScroll,false);
document.body.removeEventListener('touchmove',bodyScroll,false);
```

1. true的触发顺序总是在false前面
2. 如果多个均为true   则外层触发先于内层
3. 如果多个均为false  则内层触发先于外层

### BOM

Browser Object Model浏览器对象模型，是JavaScript的组成之一，它提供了独立于内容与浏览器窗口进行交互的对象，使用浏览器对象模型可以实现与HTML的交互。它的作用是将相关的元素组织包装起来，提供给程序设计人员使用，从而降低开发人员的劳动量，提高设计Web页面的能力。
window : alert() , prompt() , confirm() , setInterval() , clearInterval() , setTimeout() , clearTimeout() ;↳

history : go(参数) , back() , foward() ;

location : herf属性.

1、window.location.href = '你所要跳转到的页面'; 2、window.open('你所要跳转到的页面’); 2、window.history.back(-1):返回上一页 4、window.history.go(-1/1):返回上一页或下一页五、 3、history.go("baidu.com")；
4、window.print() 直接掉用打印窗口可以用来拔面试题。

### DOM

DOM，全称Document Object Model 文档对象模型。JS中通过DOM来对HTML文档进行操作
文档是整个的HTML网页文档
将网页中的每一个部分都转换为了一个对象
使用模型来表示对象之间的关系，方便获取对象



### ES6新增

数据类型：基本数据类型Symbol，引用数据类型Set ，Map
运算符：变量的解构赋值，对象和数组新增了扩展运算符
字符串方法：${ }，需要配合单反引号好完成字符串拼接的功能，eg：`http://localhost:3000/search/users?q=${keyWord}`
块级作用域：let,const
原生提供 Proxy 构造函数，用来生成 Proxy 实例
定义类的语法糖(class)
模块化import/export
生成器(Generator)和遍历器(Iterator)

### 基本数据类型

ES5：Null，Undefined，Number，String，Boolean
ES6新增：Symbol（仅有目的：作为对象属性的标识符，表示唯一的值）

```javascript
var obj = {};

obj[Symbol("a")] = "a";

//会根据给定的键 key，来从运行时的 symbol 注册表中找到对应的 symbol，
//如果找到了，则返回它，
//否则，新建一个与该键关联的 symbol，并放入全局 symbol 注册表中。
Symbol.for(key);

Symbol.for("bar") === Symbol.for("bar"); // true，证明了上面说的
Symbol("bar") === Symbol("bar"); // false，Symbol() 函数每次都会返回新的一个 symbol
ES10新增：BigInt（表示任意的大整数）

let bnum=1684424684321231561n  //方式1：数组后加n
bunm=BigInt("1684424684321231561")//方式2：调用BigInt


```

存储在栈（大小固定，先进后出）

### 引用数据类型

Object，function，Array，Date，RegExp，ES6新增：Set，MAP

地址存储在栈，内容存储在堆（树形结构，队列，先进先出）

### 声明和定义

*变量声明不开辟内存,只是告诉编译器,要声明的部分存在,要预留部分的空间。var i;*

*变量定义开辟**内存***。 var i=123;

### [toString，valueOf](https://blog.csdn.net/qq_35577655/article/details/119685179?ops_request_misc=&request_id=&biz_id=102&utm_term=valueOf和toString&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-2-119685179.142^v74^control_1,201^v4^add_ask,239^v2^insert_chatgpt&spm=1018.2226.3001.4187)

valueOf偏向于运算，toString偏向于显示。
valueOf:除了date其他的都是返回数据本身



### ==，\=\==，Object.is()

==：自动数据类型转换
强制转换规则

1. string和number，string->number，

2. 其他类型和boolean，bool->number

3. 对象和非对象，对象先调用 ToPrimitive 抽象操作（调用valueOf()或toString()）

4. null==undefined值转为Boolean值false

5. NaN!=NaN

- ===：严格模式，不进行自动数据类型转换，比较的是栈中值（即基本数据类型的值，或者引用数据类型的地址）
- Object.is()：在===基础上特别处理了NaN,-0,+0,保证-0与+0不相等，但NaN与NaN相等
- Object.is(+0,-0) //false
- Object.is(NaN,NaN) //true

### 判断数据类型：typeof运算符，instance of运算符，`isPrototypeOf()` 方法，constructor，Object prototype

- typeof：判断 基本数据类型

  ```javascript
  typeof a
  ```

- instance of：判断 引用数据类型，在其原型链中能否找到该类型的原型

  ```javascript
  a instanceof Object
  ```

- isPrototypeOf() ：在表达式 "object instanceof AFunction"中，object 的原型链是针对 AFunction.prototype 进行检查的，而不是针对 AFunction 本身。

- **constructor**：判断 **所有数据类型**（**不包含**继承引用数据类型的**自定义类型**）

  (**数据**).constructor === **数据类型**

### instance of （[⭐](https://blog.csdn.net/Jet_Lover/article/details/115637795?ops_request_misc=%7B%22request%5Fid%22%3A%22167818286416800227464615%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=167818286416800227464615&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~hot_rank-2-115637795-null-null.142^v73^control_1,201^v4^add_ask,239^v2^insert_chatgpt&utm_term=面试React&spm=1018.2226.3001.4187#1_6)手写）

第一个实例参数是否在第二个函数参数的原型链上

1. 获取首个对象参数的原型对象
2. 获取Fn函数的原型对象
3. 进入死循环，当两个参数的原型对象相等时返回true
4. 当两个参数的原型对象不相等时获取首个对象参数原型的原型并且循环该步骤直到null时返回false

### new

"_new"函数，该函数会返回一个对象，

该对象的**构造函数为函数参数**、原型对象为**函数参数的原型**，核心步骤有：

1. 创建一个新对象
2. 获取函数参数
3. 将新对象的原型对象和函数参数的原型连接起来
4. 将新对象和参数传给构造器执行
5. 如果构造器返回的不是对象，那么就返回第一个新对象

```javascript
const _new = function() {
    const object1 = {}
    const Fn = [...arguments].shift()
    object1.__proto__ = Fn.prototype
    const object2 = Fn.apply(object1, arguments)
    return object2 instanceof Object ? object2 : object1
}
```

### 类型转换

#### 转换为数字：

- Number()：可以把任意值转换成数字，如果要转换的字符串中有不是数字的值，则会返回NaN
- parseInt(string,radix)：解析一个字符串并返回指定基数的十进制整数，radix是2-36之间的整数，表示被解析字符串的基数。
- parseFloat(string)：解析一个参数并返回一个浮点数

#### 隐式转换：

1. let str = '123'
2. let res = str - 1 //122
3. str+1 // '1231'
4. +str+1 // 124

- 转换为字符串

  使用toString方法（NAN，undefined不能用）

#### 转换布尔值

Boolean()：0, ''(空字符串), null, undefined, NaN会转成false，其它都是true

### type of null

typeof null 的结果是Object。

在 JavaScript 第一个版本中，所有值都存储在 32 位的单元中，每个单元包含一个小的 类型标签(1-3 bits) 

000: object   - 当前存储的数据指向一个对象。

null 的值是机器码 NULL 指针(null 指针的值全是 0)

那也就是说null的类型标签也是000，和Object的类型标签一样，所以会被判定为Object。

### 事件

文档和浏览器窗口中发生的特定交互

#### [DOM事件流（event flow ）](https://juejin.cn/post/7192584563799883832)

先捕获再冒泡。存在三个阶段：事件捕获阶段、处于目标阶段、事件冒泡阶段。

```javascript
element.addEventListener(event, function[, useCapture]);
//useCapture 默认为false，即冒泡阶段调用事件处理函数，
//为ture时，在事件捕获阶段调用处理函数
```



![](https://img-blog.csdnimg.cn/img_convert/85cbce660c1d62e71c502705ffa701d4.webp?x-oss-process=image/format,png)

![](https://img-blog.csdnimg.cn/img_convert/10d3a13107d0b9be01df9fa1db341735.webp?x-oss-process=image/format,png)

### 事件委托/代理（⭐手写）

事件委托就是利用事件冒泡，就是把子元素的事件都绑定到父元素上。

应用：

1000个button注册点击事件。如果循环给每个按钮添加点击事件，那么会增加内存损耗，影响性能

好处：

- 替代循环绑定事件的操作，减少内存消耗，提高性能。比如：ul上代理所有li的click事件。

- 简化了dom节点更新时，相应事件的更新。比如：

  不用在新添加的li上绑定click事件。
  当删除某个li时，不用解绑上面的click事件。

**缺点**：

1. 事件委托基于冒泡，对于**不冒泡**的事件不支持。
2. 层级过多，冒泡过程中，可能会被某层**阻止掉**。
3. 理论上委托会导致浏览器**频繁调用处理函数**，虽然很可能不需要处理。所以建议**就近委托**，比如在`table`上代理`td`，而不是在`document`上代理`td`。

### 发布订阅模式（[⭐](https://blog.csdn.net/Jet_Lover/article/details/115637795?ops_request_misc=%7B%22request%5Fid%22%3A%22167818286416800227464615%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=167818286416800227464615&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~hot_rank-2-115637795-null-null.142^v73^control_1,201^v4^add_ask,239^v2^insert_chatgpt&utm_term=面试React&spm=1018.2226.3001.4187#1_6)手写）

完成"EventEmitter"类实现发布订阅模式。

1. 同一名称事件可能有多个不同的执行函数：构造函数中创建”events“对象变量存放所有的事件
2. 通过"on"函数添加事件：订阅事件。当总事件中不存在此事件时创建新的事件数组，当存在时将”fn“函数添加在该事件对应数组中
3. 通过"emit"函数触发事件：发布事件，遍历该事件下的函数数组（2中的fn参数）并全部执行

```javascript
class EventEmitter {
    constructor() {
        this.events = {}//二维，events' funcs
    }
    //添加事件：订阅事件
    on(event, fn) {
        if(!this.events[event]) {//当总事件中不存在此事件时创建新的事件数组
            this.events[event] = [fn]
        } else {                 //当存在时将”fn“函数添加在该事件对应数组中
            this.events[event].push(fn)
        }
    }
   //触发事件：发布事件
    emit(event) {
        if(this.events[event]) {//遍历该事件下的函数数组并全部执行
            this.events[event].forEach(callback => callback())//callback就是fn
        }
    }
}
```

### 观察者模式（⭐手写）

"Observerd"类实现观察者模式。要求如下：
"Observer"为观察者，"Observerd"为被观察者

1. 被观察者构造函数声明三个属性分别为"name"用于保存被观察者姓名、"state"用于保存被观察者状态、"observers"用于保存观察者们
2. 被观察者创建"setObserver"函数，用于保存观察者们，该函数通过数组的push函数将观察者参数传入"observers"数组中
3. 被观察者创建"setState"函数，设置该观察者"state"并且通知所有观察者，该函数首先通过参数修改被观察者的"state"属性，然后通过遍历"observers"数组分别调用各个观察者的"update"函数并且将该被观察者作为参数传入
4. 观察者创建"update"函数，用于被观察者进行消息通知，该函数需要打印（console.log）数据，数据格式为：小明正在走路。其中"小明"为被观察者的"name"属性，"走路"为被观察者的"state"属性

```javascript
//被观察者
class Observerd {
    constructor(name) {
        this.name = name
        this.state = '走路'
        this.observers = []
    }
    setObserver(observer) {
        this.observers.push(observer)
    }
    setState(state) {
        this.state = state
        this.observers.forEach(observer => observer.update(this))
    }
}
//观察者
class Observer {
    constructor() {
        
    }
    update(observerd) {
        console.log(observerd.name + '正在' + observerd.state)
    }
}
```

### var，let 和 const关键字

在 ES6 之前，JavaScript 只有两种作用域： 全局变量 与 函数内的局部变量。

ES6新增，块级作用域（由大括号包裹，比如：if(){},for(){}等）

var：可以跨块访问, 不能跨函数访问，允许重复声明，变量提升
let、const：只能在块作用域里访问，不允许在相同作用域中重复声明，不存在变量提升
const ：声明一个只读的常量，使用时必须初始化(即必须赋值)，一旦声明，常量的值就不能改变，（即，栈中的值不能变，引用类型，内存地址不能修改，可以修改里面的值。）。

### 原型链

```javascript
console.log(Person.prototype);
// {constructor: ƒ}
// 		constructor: ƒ Person()
// 			arguments: null
// 			caller: null
// 			length: 0
// 			name: "Person"
// 			prototype: {constructor: ƒ}
// 			__proto__: ƒ ()
// 			[[FunctionLocation]]: 09-原型对象.html:8
// 			[[Scopes]]: Scopes[1]
// 		__proto__: Object
(引用类型统称为object类型)
```

- 所有引用类型都有一个_\_proto\_\_(隐式原型)属性，属性值是一个普通的对象
- 所有函数  都有一个prototype(原型)  属性，属性值是一个普通的对象

### 寄生组合式继承（⭐手写）

通过寄生组合式继承使"Chinese"构造函数继承于"Human"构造函数。要求如下：

1. 给"Human"构造函数的原型上添加"getName"函数，该函数返回调用该函数对象的"name"属性
2. 给"Chinese"构造函数的原型上添加"getAge"函数，该函数返回调用该函数对象的"age"属性
3. 在"Human"构造函数的原型上添加"getName"函数
   在”Chinese“构造函数中通过call函数借助”Human“的构造器来获得通用属性
4. Object.create函数返回一个对象，该对象的__proto__属性为对象参数的原型。此时将”Chinese“构造函数的原型和通过Object.create返回的实例对象联系起来
5. 最后修复"Chinese"构造函数的原型链，即自身的"constructor"属性需要指向自身
   在”Chinese“构造函数的原型上添加”getAge“函数

```javascript
function Human(name) {
    this.name = name
    this.kingdom = 'animal'
    this.color = ['yellow', 'white', 'brown', 'black']
}
Human.prototype.getName = function() {
    return this.name
}
 
function Chinese(name,age) {
    Human.call(this,name)//call函数借助”Human“的构造器来获得通用属性
    this.age = age
    this.color = 'yellow'
}
 
//返回的对象__proto__属性为对象参数的原型
Chinese.prototype = Object.create(Human.prototype)//使用现有的对象来作为新创建对象的原型
//修复"Chinese"构造函数的原型链，即自身的"constructor"属性需要指向自身
Chinese.prototype.constructor = Chinese
 
Chinese.prototype.getAge = function() {
    return this.age
}
```

[【原型和原型链】什么是原型和原型链_TowYingWang的博客-CSDN博客_原型和原型链](https://blog.csdn.net/xiaoermingn/article/details/80745117?ops_request_misc=%7B%22request%5Fid%22%3A%22165001256616780366560731%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=165001256616780366560731&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~hot_rank-1-80745117.142^v9^pc_search_result_cache,157^v4^control&utm_term=原型和原型链&spm=1018.2226.3001.4187)

### Iterator，for in，for of，forEach，map循环遍历

#### Iterator

一种接口，为各种不同的数据结构提供统一的访问机制

例如Array.prototype[@@iterator]()

Array 对象的 @@iterator 方法实现了迭代协议，并允许数组被大多数期望可迭代的语法所使用，例如展开语法和 for...of 循环。它返回一个迭代器，生成数组中每个索引的值。

####  for of

["a", "b", "c", "d"];for…of 循环读取键值// a b c d

支持迭代协议的数据结构（数组、字符串、Set、Map 等），不包括对象。

对于字符串，类数组，类型数组的迭代，循环内部调用的是数据结构的Symbol.iterator方法。

for...of 不能循环普通的对象，需要通过和 Object.keys()搭配使用

### for in

["a", "b", "c", "d"];for…in 循环读取**键名** // 0 1 2 3

适用于遍历**对象**的 **公有** **可枚举属性，无法遍历 symbol 属性** 

**hasOwnProperty()** 方法来判断属性是否来自对象本身，并避免遍历**原型链**上的属性。

```javascript
var triangle = {a: 1, b: 2, c: 3};
 
function ColoredTriangle() {
  this.color = 'red';
}
 
ColoredTriangle.prototype = triangle;
 
var obj = new ColoredTriangle();
 
for (var prop in obj) {
  if (obj.hasOwnProperty(prop)) {
    console.log(`obj.${prop} = ${obj[prop]}`);
  }
}
 
// Output:
// "obj.color = red"
 
```

### forEach

arr.forEach（value[，index，默认隐藏参数arr]）

适用于需要知道**索引值**的**数组****遍历**，但是**不能中断（** break 和 return **）**

如果需要跳出循环可以使用 some() 或 every() 方法

```javascript
 
const isBelowThreshold = (currentValue) => currentValue < 30;
 
const array1 = [1, 30, 39, 29, 10, 13];
 
array1.forEach(element => console.log(element));
 
console.log(array1.every(isBelowThreshold));
// Expected output: false
 
//是不是至少有 1 个元素
console.log(array1.some(isBelowThreshold));//空数组,则返回false。
// Expected output: true
```

### map

**map** 方法，基本用法与 forEach 一致

1. forEach()方法**不会返回**执行结果，而是undefined
2. map()方法会得到一个**新的数组**并返回
3. 同样的一组数组，map()的执行速度优于 forEach()（**map() 底层做了深度优化**）

### [匿名函数、箭头函数、构造函数](https://juejin.cn/post/6936938080095961125)

#### 匿名函数

```javascript
//声明匿名函数
let  myFun = function( a,b ){
    console.info( a+b);
};
//执行 
myFun( 10,30 );
 
//等同于  立即执行匿名函数
 
(function(a,b){
    console.info( a+b );
})(10,30);
```

### call、apply、bind改变this

call()和apply()唯一区别：

`call()`接受的是**一个参数列表**

`apply()` 方法接受的是**一个包含多个参数的数组**，[this,[arg1,arg2]]。

```javascript
var obj1 = {
    name: 1,
    getName: function (num = '') {
        return this.name + num;
    }
};
 
var obj2 = {
    name: 2,
};
// 可以理解成在 obj2的作用域下调用了 obj1.getName()函数
console.log(obj1.getName()); // 1
console.log(obj1.getName.call(obj2, 2)); // 2 + 2 = 4
console.log(obj1.getName.apply(obj2, [2])); // 2 + 2 = 4
```

bind：语法和call一样，区别在于call**立即执行**，bind**等待执行**，bind不兼容IE6~8

**bind()** 方法创建一个**新的函数**，在 bind() 被调用时，这个**新函数的 this** 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用

### call （[⭐](https://blog.csdn.net/Jet_Lover/article/details/115637795?ops_request_misc=%7B%22request%5Fid%22%3A%22167818286416800227464615%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=167818286416800227464615&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~hot_rank-2-115637795-null-null.142^v73^control_1,201^v4^add_ask,239^v2^insert_chatgpt&utm_term=面试React&spm=1018.2226.3001.4187#1_6)手写）

```javascript
    // 给function的原型上面添加一个 _call 方法
    Function.prototype._call = function (context) {
        //  判断调用者是否是一个函数  this 就是调用者
        if (typeof this !== 'function') {
                    throw new TypeError('what is to be a function')
        }
        // 如果有 context 传参就是传参者 没有就是window
        context = context || window
        // 保存当前调用的函数
        // context._this ==> context[0]._this
        context._this = this   
        // 截取传过来的参数
        /*
          arguments
                 a: 1
                 fn: ƒ fns()
        */
        // 通过 slice 来截取传过来的参数
        const local = [...arguments].slice(1)
        // 传入参数调用函数
        let result = context._this(...local)
        // 删属性
        delete context._this
        return result
    }
 
    let obj = { a: 1 }
    function fns(a, b) {
        console.log(a, b);
        console.log(this)
    }
    fns._call(obj, 23, 555)
```

### apply（[⭐](https://blog.csdn.net/Jet_Lover/article/details/115637795?ops_request_misc=%7B%22request%5Fid%22%3A%22167818286416800227464615%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=167818286416800227464615&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~hot_rank-2-115637795-null-null.142^v73^control_1,201^v4^add_ask,239^v2^insert_chatgpt&utm_term=面试React&spm=1018.2226.3001.4187#1_6)手写）

```javascript
    // 给function的原型上面添加一个 _apply 方法
    Function.prototype._apply= function (context) {
        //  判断调用者是否是一个函数  this 就是调用者
        if (typeof this !== 'function') {
                    throw new TypeError('what is to be a function')
        }
        // 如果有 context 传参就是传参者 没有就是window
        context = context || window
        // 保存当前调用的函数
        context._this = this   
        // 截取传过来的参数
        /*
          arguments
                 a: 1
                 fn: ƒ fns()
        */
        //！！！！！！！！！！！！！！与call的唯一区别！！！！！！！！！！！
        // 这里开始判断传入的参数是否存在，此时参数是一个数组形式[thisArg,[传参]]
        // 那么如果arguments[1]即传参存在的时候，就是需要传参调用保存的函数
        // 如果不存在就直接调用函数
        if (arguments[1]) {
            result = context._this(...arguments[1])//！！！！将数组展开！！！！
        } else {
            result = context._this()
        }
        //！！！！！！！！！！！！！！与call的唯一区别！！！！！！！！！！！
        // 删属性
        delete context._this
        return result
    }
 
    let obj = { a: 1 }
    function fns(a, b) {
        console.log(a, b);
        console.log(this)
    }
    fns._call(obj, 23, 555)
```

### bind(手写)

```javascript
Function.prototype._bind = function (context) {
   if (typeof this !== 'function') {
                    throw new TypeError('what is to be a function')
    }
    var _this = this; // 保存调用bind的函数
    var context = context || window; // 确定被指向的this，如果context为空，执行作用域的this就需要顶上喽
    return function(){
        return _this.apply(context, [...arguments].slice(1)); // 如果只传context，则[...arguments].slice(1)为空数组
    }
};
 
var obj = {
    name: 1,
    getName: function(){
        console.log(this.name)
    }
};
 
var func = function(){
    console.log(this.name);
}._bind(obj);
 
func(); // 1
```

### 闭包（closure）

类比背包，当一个函数被创建并传递或从另一个函数返回时，它会携带一个背包。背包中是函数声明时作用域内的所有变量。

```javascript
var name = '余光';
 
function foo() {
  console.log(name); // 余光
}
 
(function (func) {
    var name = '老王';
 
    func()
})(foo); // 余光
```

因为js作用域生命周期在于内部脚本是否全部执行完毕才会销毁，并且不会带到父级作用域；

当函数内部返回一个函数，子函数没在父级作用域内完成整个生命周期的话，父级函数是没办法完成一整个生命周期的，闭包正是利用这一点卡住了父级函数的作用域。

因为被下级作用域内引用，而没有被释放。就导致上级作用域内的变量，等到下级作用域执行完以后才正常得到释放。

面试的时候，直接回答函数嵌套函数，且内部函数调用父级作用域的变量就可以称之为闭包了。

```javascript
function createCounter() {
   let counter = 0
   const myFunction = function() {
     counter = counter + 1
     return counter
   }
   return myFunction
 }
 const increment = createCounter()
 const c1 = increment()
 const c2 = increment()
 const c3 = increment()
 console.log('example increment', c1, c2, c3)
```

- 闭包会使得函数中的变量都被保存在内存中，**内存消耗**很大，所以不能滥用闭包
- 滥用闭包容易**内存泄漏**。
- 使用场景 : **防抖、节流**、**函数套函数**避免**全局污染**

### 正则表达式Regular Expression(RegExp) 

**字符串搜索模式**。

RegExp 对象是一个预定义了**属性和方法**的正则表达式对象

regexp.test(str)返回Bool

regexp.exec(str)返回匹配的子串 或者 null





#### 常用字符

\标记下一个字符是特殊字符或文字。例如，"n”和字符"n”匹配。"\n"则和换行字符匹配。

^匹配输入的开头.
$匹配输入的末尾

·匹配除换行字符外的任何单个字符
*匹配前一个字符零或多次。例如，"zo*”与"z”或"zoo”匹配。
+匹配前一个字符一次或多次。例如，"zo+"与"zoo”匹配，但和"z”不匹配。
?匹配前一个字符零或一次。例如，"a?ve?”和"never"中的“"ve”匹配。
x|y 匹配x或y
{n}匹配n次。n是非负整数
{n,} n是一个非负整数。至少匹配n次。例如，"o{2,)"和"Bob”中的"o”不匹配，但和"foooood"中的所有o匹配。"o{1}”与"o+”等效。"o{0,}”和"o*”等效。
{n,m}m和n是非负整数。至少匹配n次而至多匹配 m次。例如，"o{1,3]"和"fooooood”中的前三个o匹配。"o{0,1}”和“o?”等效。
[xyz]匹配括号内的任一字符。例如，"[abc]"和"plain”中的"a”匹配。

[^xyz]匹配非括号内的任何字符。例如，"[^abc]"和“plain”中的"p”匹配。
[a-z]字符范围。和指定范围内的任一字符匹配。例如，"[a-z]”匹配"a"到"z"范围内的任一小写的字母表字符。
[^m-z]否定字符范围。匹配不在指定范围内的任何字符。例如，"[m-z]”匹配不在"m"到"z"范围内的任何字符。

助记：digital

\d匹配数字字符。等价于[0-9]。
\D匹配非数字字符。等价于[^0-9]。

助记：space

\s匹配任何空白，包括空格、制表、换页等。与"[ \fn\rlt\v]”等效。
\S匹配任何非空白字符。与"[^ \fn\rlt\v]”等效。


\w匹配包括下划线在内的任何字字符。与"[A-Za-z0-9_]”等效。

\W匹配任何非字字符。与"[^A-Za-z0-9_]”等效。



```javascript
https://www.bilibili.com/video/BV1F54y1N74E/?spm_id_from=333.337.search-card.all.click&vd_source=6fd32175adc98c97cd87300d3aed81ea
//开始:                     ^
//协议:                     http(s)?:/\/\
//域名:                     [A-z0-9]+-[A-z0-9]+|[A-z0-9]+
//顶级域名 如com cn,2-6位:   [A-z]{2,6}
//端口 数字:                (\d+)?
//路径 任意字符 如 /login:   (\/.+)?
//哈希 ? 和 # ，如?age=1:    (\?.+)?(#.+)?
//结束:                      $
//     https://           www.bilibili                com    /video/BV1F54y1N74E  ?spm..            
/^(http(s)?:\/\/)?(([a-zA-Z0-9]+-[a-zA-Z0-9]+|[a-zA-Z0-9]+)\.)+([a-zA-Z]{2,6})(:\d+)?(\/.+)?(\?.+)?(#.+)?$/.test(url)
```

### 函数

#### 函数的`length`属性

将返回没有指定默认值的参数个数。也就是说，指定了默认值后，`length`属性将失真。

#### 函数声明与函数表达式的区别

函数声明会将那个函数提升到最前面（即使你写代码的时候在代码块最后才写这个函数），成为全局函数。

函数声明要指定函数名，而函数表达式不用，可以用作匿名函数。

#### 立即执行函数（iife）

( function( ){ })( )
原理：括号内部不能包含语句，当解析器对代码进行解释的时候，先碰到了()， 然后碰到function关键字

就会自动将()里面的代码识别为函数表达式而不是函数声明。

作用：立即执行函数会形成一个单独的作用域，我们可以封装一些临时变量或者局部变量，避免污染全局变量。

### 常用方法

#### 异或运算^

按位异或，相同为0，不同为1



运算法则：

1.交换律（随便换像乘一样）：a ^ b ^ c === a ^ c ^ b

2.任何数于0异或为任何数 0 ^ n === n

3.相同的数异或为0: n ^ n === 0

#### Math

```javascript
//e=2.718281828459045
Math.E;
 
//绝对值
Math.abs()
 
//基数（base）的指数（exponent）次幂，即 base^exponent。
Math.pow(base, exponent)
 
 
//max,min不支持传递数组
Math.max(value0, value1, /* … ,*/ valueN)
Math.max.apply(null,array)
apply会将一个数组装换为一个参数接一个参数
null是因为没有对象去调用这个方法,只需要用这个方法运算
 
 
//取整
Math.floor()  向下取一个整数（floor地板）
Math.ceil(x)  向上取一个整数（ceil天花板）
 
Math.round() 返回一个四舍五入的值
 
Math.trunc() 直接去除小数点后面的值
```

#### Number

0B，0O为ES6新增

二进制：有前缀0b（或0B）的数值，出现0,1以外的数字会报错（b：binary）
八进制：有前缀0o（或0O）的数值，或者是以0后面再跟一个数字（0-7）。如果超出了前面所述的数值范围，则会忽略第一个数字0，视为十进制数（o：octonary）
注意：八进制字面量在严格模式下是无效的，会导致支持该模式的JavaScript引擎抛出错误
十六进制：有前缀0x，后跟任何十六进制数字（0~9及A~F），字母大小写都可以，超出范围会报错

##### 特殊值

- Number.MIN_VALUE：5e-324
- Number.MAX_VALUE：1.7976931348623157e+308
- Infinity ，代表无穷大，如果数字超过最大值，js会返回Infinity，这称为正向溢出(overflow)；
- -Infinity ，代表无穷小，小于任何数值，如果等于或超过最小负值-1023（即非常接近0），js会直接把这个数转为0，这称为负向溢出(underflow)
- NaN ，Not a number，代表一个非数值
- isNaN()：用来判断一个变量是否为非数字的类型，如果是数字返回false;如果不是数字返回true。
- isFinite()：数值是不是有穷的

```javascript
var result = Number.MAX_VALUE + Number.MAX_VALUE;
 
console.log(isFinite(result)); //false
```

- typeof NaN // 'number'  ---NaN不是独立的数据类型，而是一个特殊数值，它的数据类型依然属于Number
- NaN =\== NaN // false　　 ---NaN不等于任何值，包括它本身
- (1 / +0) === (1 / -0) // false　　---除以正零得到+Infinity，除以负零得到-Infinity，这两者是不相等的

##### **科学计数法**

对于那些极大极小的数值，可以用e表示法（即科学计数法）表示的浮点数值表示。

等于e前面的数值乘以10的指数次幂

```javascript
numObj.toFixed(digits)//用定点表示法来格式化一个数值
 
function financial(x) {
  return Number.parseFloat(x).toFixed(2);
}
 
console.log(financial(123.456));
// Expected output: "123.46"
 
console.log(financial(0.004));
// Expected output: "0.00"
 
console.log(financial('1.23e+5'));
// Expected output: "123000.00"
```

取余是数学中的概念，

取模是计算机中的概念，

两者都是求两数相除的余数

1.当两数符号相同时，结果相同，比如：7%4 与 7 Mod 4 结果都是3

2.当两数符号不同时，结果不同，比如   (-7)%4=-3和(-7)Mod4=1

取余运算，求商采用fix 函数，向0方向舍入，取 -1。因此 (-7) % 4 商 -1 余数为 -3
取模运算，求商采用 floor 函数，向无穷小方向舍入，取 -2。因此 (-7) Mod 4 商 -2 余数为 1

key：((n % m) + m) % m;

```javascript
Number.prototype.mod = function(n) {
	return ((this % n) + n) % n;
}
// 或 
function mod(n, m) {
  return ((n % m) + m) % m;
}
```

### Map

保存键值对，任何值（对象或者[基本类型](https://developer.mozilla.org/zh-CN/docs/Glossary/Primitive)）都可以作为一个键或一个值。

Map的键可以是任意值，包括函数、对象或任意基本类型。
object的键必须是一个String或是Symbol 。

```javascript
const contacts = new Map()
contacts.set('Jessie', {phone: "213-555-1234", address: "123 N 1st Ave"})
contacts.has('Jessie') // true
contacts.get('Hilary') // undefined
contacts.delete('Jessie') // true
console.log(contacts.size) // 1
 
function logMapElements(value, key, map) {
  console.log(`m[${key}] = ${value}`);
}
 
new Map([['foo', 3], ['bar', {}], ['baz', undefined]])
  .forEach(logMapElements);
 
// Expected output: "m[foo] = 3"
// Expected output: "m[bar] = [object Object]"
// Expected output: "m[baz] = undefined"
```

### Set

值的集合，且值唯一

虽然NaN !== NaN，但set中NaN 被认为是相同的

```javascript
let setPos = new Set(); 
setPos.add(value);//Boolean
setPos.has(value);
setPos.delete(value);
 
function logSetElements(value1, value2, set) {
  console.log(`s[${value1}] = ${value2}`);
}
 
new Set(['foo', 'bar', undefined]).forEach(logSetElements);
 
// Expected output: "s[foo] = foo"
// Expected output: "s[bar] = bar"
// Expected output: "s[undefined] = undefined"
```

#### set判断值相等的机制

```javascript
//Set用===判断是否相等
const set= new Set();
const obj1={ x: 10, y: 20 },obj2={ x: 10, y: 20 }
set.add(obj1).add(obj2);
 
console.log(obj1===obj2);//false
console.log(set.size);// 2
 
set.add(obj1);
console.log(obj1===obj1);//true
console.log(set.size);//2
```

### 数组去重 （[⭐](https://blog.csdn.net/Jet_Lover/article/details/115637795?ops_request_misc=%7B%22request%5Fid%22%3A%22167818286416800227464615%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=167818286416800227464615&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~hot_rank-2-115637795-null-null.142^v73^control_1,201^v4^add_ask,239^v2^insert_chatgpt&utm_term=面试React&spm=1018.2226.3001.4187#1_6)手写）

```javascript
// Use to remove duplicate elements from the array
const numbers = [2,3,4,4,2,3,3,4,4,5,5,6,6,7,5,32,3,4,5]
console.log([...new Set(numbers)])
// [2, 3, 4, 5, 6, 7, 32]
```

### Array

```javascript
//创建字符串
//join() 方法将一个数组（或一个类数组对象）的所有元素连接成一个字符串并返回这个字符串
//如果数组只有一个元素，那么将返回该元素而不使用分隔符。
Array.join()
Array.join(separator)
 
//################创建数组：
//伪数组转成数组
Array.from(arrayLike, mapFn)
console.log(Array.from('foo'));
// Expected output: Array ["f", "o", "o"]
 
console.log(Array.from([1, 2, 3], x => x + x));
// Expected output: Array [2, 4, 6]
 
console.log( Array.from({length:3},(item, index)=> index) );// 列的位置
// Expected output:Array [0, 1, 2]
 
 
//################原数组会改变：
 
arr.reverse()//返回翻转后的数组
 
// 无函数
arr.sort()//默认排序顺序是在将元素转换为字符串，然后比较它们的 UTF-16
// 比较函数
arr.sort(compareFn)
function compareFn(a, b) {
  if (在某些排序规则中，a 小于 b) {
    return -1;
  }
  if (在这一排序规则下，a 大于 b) {
    return 1;
  }
  // a 一定等于 b
  return 0;
}
//升序
function compareNumbers(a, b) {
  return a - b;
}
 
 
//固定值填充
arr.fill(value)
arr.fill(value, start)
arr.fill(value, start, end)
 
 
//去除
array.shift() //从数组中删除第一个元素，并返回该元素的值。
 
array.pop() //从数组中删除最后一个元素，并返回该元素的值。此方法会更改数组的长度。
array.push() //将一个或多个元素添加到数组的末尾，并返回该数组的新长度
 
//unshift() 方法将一个或多个元素添加到数组的开头，并返回该数组的新长度
array.unshift(element0, element1, /* … ,*/ elementN)
 
//粘接，通过删除或替换现有元素或者原地添加新的元素来修改数组，并以数组形式返回被修改的内容。
array.splice(start)
array.splice(start, deleteCount)
array.splice(start, deleteCount, item1)
array.splice(start, deleteCount, item1, item2...itemN)
 
//################原数组不会改变：
 
//切片，浅拷贝（包括 begin，不包括end）。
array.slice()
array.slice(start)
array.slice(start, end)
 
//展平，按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。
array.flat()//不写参数默认一维
array.flat(depth)
 
//过滤器，函数体 为 条件语句
// 箭头函数
filter((element) => { /* … */ } )
filter((element, index) => { /* … */ } )
filter((element, index, array) => { /* … */ } )
array.filter(str => str .length > 6) 
 
//遍历数组处理
// 箭头函数
map((element) => { /* … */ })
map((element, index) => { /* … */ })
map((element, index, array) => { /* … */ })
array.map(el => Math.pow(el,2))
//map和filter同参
 
//接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值。
// 箭头函数
reduce((previousValue, currentValue) => { /* … */ } )
reduce((previousValue, currentValue, currentIndex) => { /* … */ } )
reduce((previousValue, currentValue, currentIndex, array) => { /* … */ } )
reduce((previousValue, currentValue) => { /* … */ } , initialValue)
reduce((previousValue, currentValue, currentIndex) => { /* … */ } , initialValue)
array.reduce((previousValue, currentValue, currentIndex, array) => { /* … */ }, initialValue)
 
//一个“reducer”函数，包含四个参数：
//previousValue：上一次调用 callbackFn 时的返回值。
//在第一次调用时，若指定了初始值 initialValue，其值则为 initialValue，
//否则为数组索引为 0 的元素 array[0]。
 
//currentValue：数组中正在处理的元素。
//在第一次调用时，若指定了初始值 initialValue，其值则为数组索引为 0 的元素 array[0]，
//否则为 array[1]。
 
//currentIndex：数组中正在处理的元素的索引。
//若指定了初始值 initialValue，则起始索引号为 0，否则从索引 1 起始。
 
//array：用于遍历的数组。
 
//initialValue 可选
//作为第一次调用 callback 函数时参数 previousValue 的值。
//若指定了初始值 initialValue，则 currentValue 则将使用数组第一个元素；
//否则 previousValue 将使用数组第一个元素，而 currentValue 将使用数组第二个元素。
const array1 = [1, 2, 3, 4];
 
// 0 + 1 + 2 + 3 + 4
const initialValue = 0;
const sumWithInitial = array1.reduce(
  (accumulator, currentValue) => accumulator + currentValue,
  initialValue
);
 
console.log(sumWithInitial);
// Expected output: 10
```

#### Array.filter（[⭐](https://blog.csdn.net/Jet_Lover/article/details/115637795?ops_request_misc=%7B%22request%5Fid%22%3A%22167818286416800227464615%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=167818286416800227464615&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~hot_rank-2-115637795-null-null.142^v73^control_1,201^v4^add_ask,239^v2^insert_chatgpt&utm_term=面试React&spm=1018.2226.3001.4187#1_6)手写）

```javascript
Array.prototype._filter = function(Fn) {
    if (typeof Fn !== 'function') return
    const array = this
    const newArray = []
    for (let i=0; i<array.length; i++) {
        const result = Fn.call(null, array[i], i, array)
        result && newArray.push(array[i])
    }
return newArray
 }
```

#### Array.map（[⭐](https://blog.csdn.net/Jet_Lover/article/details/115637795?ops_request_misc=%7B%22request%5Fid%22%3A%22167818286416800227464615%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=167818286416800227464615&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~hot_rank-2-115637795-null-null.142^v73^control_1,201^v4^add_ask,239^v2^insert_chatgpt&utm_term=面试React&spm=1018.2226.3001.4187#1_6)手写）

```javascript
Array.prototype._map = function(Fn) {
    if (typeof Fn !== 'function') return
    const array = this
    const newArray = []
    for (let i=0; i<array.length; i++) {
        const result = Fn.call(null, array[i], i, array)
        //##########与filter的唯一不同
        newArray.push(result)
    }
return newArray
 }
```

### Array.reduce（[⭐](https://blog.csdn.net/Jet_Lover/article/details/115637795?ops_request_misc=%7B%22request%5Fid%22%3A%22167818286416800227464615%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=167818286416800227464615&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~hot_rank-2-115637795-null-null.142^v73^control_1,201^v4^add_ask,239^v2^insert_chatgpt&utm_term=面试React&spm=1018.2226.3001.4187#1_6)手写）

```javascript
Array.prototype._reduce = function(fn,initialValue = 0){
  if(typeof fn !== 'function') return;
  let res = initialValue
  this.forEach((value,index,arr)=>{
	res = fn(res,value,index,arr)
  })
  return res
}
```

### String

```javascript
str.charAt(index)//获取第n位字符  
str.charCodeAt(n)//获取第n位UTF-16字符编码 （Unicode）A是65，a是97
String.fromCharCode(num1[, ...[, numN]])//根据UTF编码创建字符串
 
String.fromCharCode('a'.charCodeAt(0))='a'
 
str.trim()//返回去掉首尾的空白字符后的新字符串
 
str.split(separator)//返回一个以指定分隔符出现位置分隔而成的一个数组，数组元素不包含分隔符
 
const str = 'The quick brown fox jumps over the lazy dog.';
 
const words = str.split(' ');
console.log(words[3]);
// Expected output: "fox"
 
 
str.toLowerCase( )//字符串转小写；
str.toUpperCase( )//字符串转大写；
 
str.concat(str2, [, ...strN])
 
 
str.substring(indexStart[, indexEnd])  //提取从 indexStart 到 indexEnd（不包括）之间的字符。
str.substr(start[, length]) //没有严格被废弃 (as in "removed from the Web standards"), 但它被认作是遗留的函数并且可以的话应该避免使用。它并非 JavaScript 核心语言的一部分，未来将可能会被移除掉。
 
str.indexOf(searchString[, position]) //在大于或等于position索引处的第一次出现。
str.match(regexp)//找到一个或多个正则表达式的匹配。
const paragraph = 'The quick brown fox jumps over the lazy dog. It barked.';
let regex = /[A-Z]/g;
let found = paragraph.match(regex);
console.log(found);
// Expected output: Array ["T", "I"]
regex = /[A-Z]/;
found = paragraph.match(regex);
console.log(found);
// Expected output: Array ["T"]
 
//match类似 indexOf() 和 lastIndexOf()，但是它返回指定的值，而不是字符串的位置。
var str = '123123000'
str.match(/\w{3}/g).join(',') // 123,123,000
 
str.search(regexp)//如果匹配成功，则 search() 返回正则表达式在字符串中首次匹配项的索引;否则，返回 -1
const paragraph = '? The quick';
 
// Any character that is not a word character or whitespace
const regex = /[^\w\s]/g;
 
console.log(paragraph.search(regex));
// Expected output: 0
 
str.repeat(count)//返回副本
str.replace(regexp|substr, newSubStr|function)//返回一个由替换值（replacement）替换部分或所有的模式（pattern）匹配项后的新字符串。
const p = 'lazy dog.Dog lazy';//如果pattern是字符串，则仅替换第一个匹配项。
console.log(p.replace('dog', 'monkey'));
// "lazy monkey.Dog lazy"
 
 
let regex = /dog/i;//如果非全局匹配，则仅替换第一个匹配项
console.log(p.replace(regex, 'ferret'));
//"lazy ferret.Dog lazy"
 
regex = /d|Dog/g;
console.log(p.replace(regex, 'ferret'));
//"lazy ferretog.ferret lazy"
 
//当使用一个 regex 时，您必须设置全局（“g”）标志， 否则，它将引发 TypeError：“必须使用全局 RegExp 调用 replaceAll”。
const p = 'lazy dog.dog lazy';//如果pattern是字符串，则仅替换第一个匹配项。
console.log(p.replaceAll('dog', 'monkey'));
// "lazy monkey.monkey lazy"
 
 
let regex = /dog/g;//如果非全局匹配，则仅替换第一个匹配项
console.log(p.replaceAll(regex, 'ferret'));
//"lazy ferret.ferret lazy"
```

### Class

#### [类声明](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes#类声明)

```javascript
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}
```

#### [类表达式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes#类表达式)

```javascript
// 未命名/匿名类
let Rectangle = class {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
console.log(Rectangle.name);
// output: "Rectangle"
 
// 命名类
let Rectangle = class Rectangle2 {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
console.log(Rectangle.name);
// 输出："Rectangle2"
```

#### [使用 super 调用超类](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes#使用_super_调用超类)

[super](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/super) 关键字用于调用对象的父对象上的函数。

```javascript
class Cat {
  constructor(name) {
    this.name = name;
  }
 
  speak() {
    console.log(this.name + ' makes a noise.');
  }
}
 
class Lion extends Cat {
  constructor(name,id) {
    super(name);
    this.id = id;
  }
  speak() {
    super.speak();
    console.log(this.name + ' roars.');
  }
}
```

#### private

类属性在默认情况下是[公有](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes/Public_class_fields)的，但可以使用增加哈希前缀 `#` 的方法来定义私有类字段

从类外部引用私有字段是错误的。它们只能在类里面中读取或写入。

```javascript
class ClassWithPrivateField {
  #privateField;
}
 
class ClassWithPrivateMethod {
  #privateMethod() {
    return 'hello world';
  }
}
 
class ClassWithPrivateStaticField {
  static #PRIVATE_STATIC_FIELD;
}
 
class ClassWithPrivateStaticMethod {
  static #privateStaticMethod() {
    return 'hello world';
  }
}
```

#### **static** 关键字

定义静态方法和值。不能在类的实例上调用静态方法，而应该通过类本身调用

静态方法调用同一个类中的其他静态方法，可使用 `this` 关键字

```javascript
class StaticMethodCall {
    static staticMethod() {
        return 'Static method has been called';
    }
    static anotherStaticMethod() {
        return this.staticMethod() + ' from another static method';
    }
}
StaticMethodCall.staticMethod();
// 'Static method has been called'
 
StaticMethodCall.anotherStaticMethod();
// 'Static method has been called from another static method'
```

非静态方法中，不能直接使用 [this](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this) 关键字来访问静态方法。

而是要用类名来调用：`CLASSNAME.STATIC_METHOD_NAME()` ，

或者用构造函数的属性来调用该方法： `this.constructor.STATIC_METHOD_NAME()`.

```javascript
class StaticMethodCall {
    constructor() {
        console.log(StaticMethodCall.staticMethod());
        // 'static method has been called.'
        console.log(this.constructor.staticMethod());
        // 'static method has been called.'
    }
    static staticMethod() {
        return 'static method has been called.';
    }
}
```

#### [创建对象的方式](https://blog.csdn.net/KANGCHUNHUANG/article/details/123819224)

1. `var obj = {}`

2. 使用`new Object`

   ```javascript
   var obj = new Object()
   ```

3. 工厂模式

   ```javascript
   'use strict';
    
   // 使用工厂模式创建对象
   // 定义一个工厂方法
   function createObject(name){
       var o = new Object();
       o.name = name;
       o.sayName = function(){
           alert(this.name);
       };
       return o;
   }
   
   var o1 = createObject('zhang');
   var o2 = createObject('li');
   
   //缺点：调用的还是不同的方法
   //优点：解决了前面的代码重复的问题
   alert(o1.sayName===o2.sayName);//false
   ```

4. 构造函数模式

   ```javascript
   function Person(name, age, job) {
     this.name = name;
     this.age = age;
     this.job = job;
     this.sayName = function () { alert(this.name); };
   }
   var person1 = new Person('Nike', 29, 'teacher');
   var person2 = new Person('Arvin', 20, 'student');
   ```

5. 原型模式

   如果往新建的对象中加入属性，那么这个属性是放在对象中，如果存在与原型同名的属性，也不会改变原型的值。但是访问这个属性，拿到的是对象的值。

   访问的顺序：对象本身>构造函数的prototype

   如果对象中没有该属性，则去访问prototype，如果prototype中没有，继续访问父类，直到Object，如果都没有找到，返回undefined

   > 如果原型father中有该属性item1就直接使用该属性，那么如果某个对象修改了该属性的值，所有的该原型创建的对象访问的值都会改变

### Object