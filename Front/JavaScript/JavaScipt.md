## DOM元素



```javascript
addEventListener（”input“，function（）{
    //事件内容
}）
```

### 连接

对象的连接

```js
obj = {x:1,y:2,z:3}
obj1 = Object.assign({m:23},obj,{z:4,a:5})
//or
obj2 = {...obj,z:4,a:5}
//x: 1, y: 2, z: 4, a: 5
```

数组的连接

```js
arr.push()
//or
[...arr,32,4]
```

### 指针函数

```js
var add = (x,y)=>x+y
add(3,6)//9
```

# 你不知道的JavaScript 笔记

## 作用域闭包

### 闭包和循环

```javascript
for (var i = 1; i <= 5; i++) {
    setTimeout(function () {
        console.log(i);
    }, i*1000)
}
```

```javascript
for (var i = 1; i <= 5; i++) {
    (function () {
        setTimeout(function () {
            console.log(i);
        }, i * 1000)
    })()
}
```

```javascript
for (var i = 1; i <= 5; i++) {
    (function () {
        var j = i;
        setTimeout(function () {
            console.log(j);
        }, j * 1000)
    })()
}
```

```javascript
for (let i = 1; i <= 5; i++) {
    setTimeout(function () {
        console.log(i);
    }, i * 1000)
}
```



### 模块

#### 现代的模块机制

```javascript
var MyModules = (function () {
    var modules = {}
    function define(name, deps, impl) { // 初始化函数名，引用函数名(用于当作方法的参数)，方法
        for (var i = 0; i < deps.length; i++) {
            deps[i] = modules[deps[i]];//检查方法是否存在，额。。。把函数名变为真正的函数
        }
        modules[name] = impl.apply(impl, deps) // apply() 方法调用一个具有给定this值的函数，以及以一个数组（或类数组对象）的形式提供的参数。
        //func.apply(thisArg, [argsArray]) 
        // thiArg在 func 函数运行时使用的 this 值
        // argArray一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 func 函数
    }
    function get(name) {
        return modules[name];
    }
    return {
        define: define,//初始化方法
        get: get//拿出方法
    }
})()

MyModules.define("bar", [], function () {
    function hello(who) {
        return "Let me introduce: " + who
    }
    return {
        hello: hello
    }
})

MyModules.define("foo", ["bar"], function (bar) {
    var hungry = "hippo"
    function awesome() {
        console.log(bar.hello(hungry).toUpperCase());
    }
    return {
        awesome: awesome
    }
})

var bar = MyModules.get("bar")
var foo = MyModules.get("foo")

console.log(
    bar.hello("hippo")
);

foo.awesome()
```

# this

## 关于this

### 误解

1. this是本身

2. this的作用域

   一个精华的代码

   ```javascript
   function foo() {
       var a = 2;
       this.bar()
   }
   function bar() {
       console.log(this.a);
   }
   
   foo() //undefined
   ```

### 正题

#### 绑定方式

1. ##### 默认绑定

   

2. ##### 隐式绑定

   ###### 绑定丢失

   ```javascript
   function foo() {
       console.log(this.a);
   }
   var obj = {
       a:2,
       foo:foo
   }
   var bar = obj.foo
   
   var a = "opps,global"
   bar()// “opps,global”
   ```

   因为ba引用了obj.foo本身，因此此时的bar()没有任何的修饰的函数调用

3. ##### 显式绑定

```javascript
function foo() {
    console.log(this.a);
  }
var obj = {
    a:2,
    foo:foo
}
var bar = obj.foo
var a = "opps,global"
bar()
```

