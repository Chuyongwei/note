# 开始

参考文档https://www.tslang.cn/docs/home.html

> typescript是一个开源的，渐进是包含类型的javascript超集

安装

```shell
npm i typescript -g
```

使用

```shell
tsc --init
```

监听

``` shell
tsc -p ./tsconfig.json --watch
```



# 使用

## 变量

### 字符

### 数组

1. 原始方法

   ```typescript
   let arr:number[]=[1,2,3,4]
   console.log(arr)
   
   let arr:string[]=["php","js","golang"]
   console.log(arr)
   ```

2. array方法

   ```typescript
   let arr:Array<string> = ["php","js","golang"]
   
   ```

### 枚举

```typescript
enum Flag {success=1,error=-1}
enum Color {red,bule,orange}
var c:Color = Color.orange
console.log(c)
```

### any类

#### 用例

控制元素

```typescript
var box:any = document.getElementById("box")
box.style.color = 'red';
```

### null和undefined值

```typescript
// var num:number;
// console.log(num) // undefined 报错(新版本不报错)

// var num:undefined
// console.log(num) // undefined 正确

//为了解决这个问题设成以下方式
// var num:number|undefined;
// num=1231
// console.log(num)

var num:number|undefined|null;
num=null
console.log(num)
```

## 函数

```typescript
// function run(){
//     console.log('run')
// }

//没有返回任意类型
// function run():void{
//     console.log('run')
// }

function run():number{
    return 123
}
console.log(run())

var a:never;

a=(()=>{
    throw new Error('错误')
})()
```



## 类

### es5定义

```js
    function Person() {
        this.name = '张三'
        this.age = 20
        this.run = function () {
            alert(this.name + '在运动')
        }
    }
    Person.getInfo = function () {
        alert('我是静态方法')
    }
    Person.prototype.sex = "男"
    Person.prototype.work = function () {
        alert(this.name + "在工作")
    }
    var p = new Person();
    p.work() 
```

```js
        // Web类 继承Person
        function Web(){
            Person.call(this) /*对象冒充继承*/
        }

        var w = new Web();
        w.run()//对象冒充可以继承构造函数里面的属性和方法 
        w.work()//但是没法继承原型链的继承
```

继承原型链方法

```js
        function Web(){

        }
        Web.prototype = new Person();
        var w= new Web() //实例化子类时无法给父类传参
        w.run()
```

### ts方法

### 定义类方法

```typescript
class Person{
    name:string
    constructor(n:string){
        this.name =n
    }

    run():void{
        alert(this.name)
    }
    getName():string{
        return this.name
    }
    setName(name:string):void{
        this.name= name
    }
}

var p = new Person('张三')
p.run()
alert(p.getName())
p.setName("123")
alert(p.getName())
```

### 实现继承

```typescript
class Person{
    name:string
    constructor(name:string){
        this.name = name
    }
    run():string{
        return `${this.name}在运动`
    }
}

class Web extends Person{
    constructor(name:string){
        super(name)
    }
    run():string{
         return `${this.name}在工作`
    }
}
var w = new Web("lisi") 
alert(w.run()) //lisi在工作
```

### 类里的修饰符

> public：公有 类里面,子类,类外面可以访问
> protected：保护类型 类里面,子类可以访问
> private：私有 类里可以访问 子类,类外部不能访问

```typescript
class Person{
    protected name:string
    constructor(name:string){
        this.name = name
    }
    run():string{
        return `${this.name}在运动`
    }
}

class Web extends Person{
    constructor(name:string){
        super(name)
    }
    run():string{
         return `${this.name}在运动子类`
    }
    work():string{
        return `${this.name}在工作`
   }
}

var w = new Web("lisi")
alert(w.run())
var p = new Person("哈哈哈")
// alert(p.name)
```

### 静态方法

> 无法直接调用类的属性，但是可以使用静态属性

```typescript
class Person {
    public name: string
    public age:number=20

    // 静态属性
    static sex = "男"
    constructor(name: string) {
        this.name = name
    }
    run(): string {
        return `${this.name}在运动子类`
    }
    work(): string {
        return `${this.name}在工作`
    }

    static print(){
        alert('静态方法---'+this.sex) //静态方法不能用变量，可以用静态变量
    }
}

var p = new Person("张三");

p.run();
Person.print() //静态方法可以直接使用
```

### 多态

>  多态：父类定义方法不去实现，让继承它的子类去实现，每一个子类有不同的实现

```typescript
class Animal{
    name:string;
    constructor(name:string){
        this.name = name
    }
    eat(){
        console.log("吃的方法")
    }
}

class Dog extends Animal{
    constructor(name:string){
        super(name)
    }
    eat(){
        return this.name+"吃粮食"
    }
}

class Cat extends Animal{
    constructor(name:string){
        super(name)
    }
    eat(){
        return this.name+"吃老鼠"
    }
}
```

### 抽象类

>抽象类：它是提供其他类继承的基类，不能直接被实例化

```typescript
abstract class Animal {
    public name:string
    constructor(name:string) {
        this.name =name
    }

    abstract eat():any; //抽象方法不包含具体实现必须在派生类中实现
}

class Dog extends Animal {
    // 抽象类的子类必须实现抽象类里面的抽象方法
    eat() {
        console.log(this.name+"吃粮食")
    }
}

var d = new Dog('小花花');
d.eat()
```

### 接口

```typescript
interface FullName{
    firstName:string;
    secondName:string;
}
function printName(name:FullName){
    //必须传入对象 FristName
    console.log(name.firstName+"--------"+name.secondName)
}
function printInfo(info:FullName){
    console.log(info.firstName+info.secondName);
}

var obj ={
    age:20,
    firstName:'张',
    secondName:'三'
}

printName(obj)

var info ={
    age:20,
    firstName:'李',
    secondName:'四'
}

printInfo(info)
```

可选属性

> 在属性前加问号

```typescript

interface FullName{
    firstName:string;
    secondName?:string;
}

function getName(name:FullName){
    //必须传入对象 FristName
    console.log(name)
}

getName({
    // secondName:'213',
    firstName: '123'
})
```

例子：ts封装ajax

```typescript
interface Config{
    type:string;
    url:string;
    data?:string
    dataType:string
}

function ajax(config:Config){
    var xhr = new XMLHttpRequest()
    xhr.open(config.type,config.url,true)
    xhr.send(config.data)
    xhr.onreadystatechange = function(){
        if(xhr.readyState==4 && xhr.status==200){
            console.log('chenggong')
            if(config.dataType=='json'){
                console.log(JSON.parse(xhr.responseText))
            }else{
                console.log(xhr.responseText)
            }
        }
    }
}

ajax({
    type:'get',
    data:'name=zhansgan',
    url:'http://a.itying.com/api/productlist',
    dataType:'json'
})
```

### 可索引接口：数组，对象约束（不常用）

对数组的约束

```typescript

interface UserArr{
    [index:number]:string
}

var arr:UserArr=['23332','233234'];

console.log(arr[0])
```

对对象的约束

```typescript
interface UserObj{
    [index:string]:string
}

var obj:UserObj={name:'张三',}
```

对类的约束

```typescript
interface Animal{
    name:string
    eat(str:string):void
}

class Dog implements Animal{
    name:string
    constructor(name:string){
        this.name=name
    }
    eat(): void {
        console.log(this.name+'吃粮食');       
    }
}

var d= new Dog('324')
d.eat()

class Cat implements Animal{
    name:string
    constructor(name:string){
        this.name=name
    }
    eat(food:string): void {
        console.log(this.name+food);       
    }
}

var c1= new Cat('小花')
c1.eat("吃老鼠")
```

### 接口的扩展

```typescript
interface Animal{
    eat():void
}

interface Person extends Animal{
    work():void
}

class Web implements Person{
    public name:string;
    constructor(name:string){
        this.name = name
    }
    eat(){
        console.log(this.name+"喜欢吃馒头");
        
    }
    work(){
        console.log(this.name+"写代码");
    }
}

var w = new Web('小李')

w.work()
```



```typescript
interface Animal{
    eat():void
}

interface Person extends Animal{
    work():void
}

class Programmer{
    public name:string
    constructor(name:string){
        this.name = name;
    }
    coding(code:string){
        console.log(this.name+code);
        
    }
}

class Web extends Programmer implements Person{
    // public name:string;
    constructor(name:string){
        super(name)
    }
    eat(){
        console.log(this.name+"喜欢吃馒头");
        
    }
    work(){
        console.log(this.name+"写代码");
    }
}

var w = new Web('小李')

w.work()
w.coding("写ts")
```



### 泛型

#### 引入

同时返回string和number类型

1. 分开写

   > 会造成代码的冗余

   ```
   function getData(value:number):number{
       return value
   }
   function getData(value:string):string{
       return value
   }
   ```

   

2. 使用any类型

   > // any放弃了类型检查，传入什么 返回什么 
   > // 返回可以不一致

   ```typescript
   function getData(value:any):any{
       return value
   }
   ```

3. 使用泛型

   ```typescript
   function getData<T>(value:T):T{
       return value;
   }
   getData<string>('sdf');
   getData<number>(123);
   ```

   

实例：最小对算法

```typescript
//最小堆算法
class MinClass{
    public list:number[] = []
    add(num:number){
        this.list.push(num)
    }
    min(){
        var minNum = this.list[0];


        for (let i = 0; i < this.list.length; i++) {
            if(minNum>this.list[i]){
                minNum = this.list[i]
            }
        }
        return minNum
    }
}

var m = new MinClass();
m.add(2);
m.add(3);
console.log(m.min());
```

泛型后

```typescript
class MinClass<T>{
    public list:T[] = []
    add(value:T){
        this.list.push(value)
    }
    min(){
        var minNum = this.list[0];


        for (let i = 0; i < this.list.length; i++) {
            if(minNum>this.list[i]){
                minNum = this.list[i]
            }
        }
        return minNum
    }
}

var m = new MinClass<number>();
m.add(2);
m.add(3);
console.log(m.min());
```



#### 泛型接口

1. 普通方法

   ```typescript
   interface ConfigFn{
       <T>(value:T):T;
   }
   
   var getData:ConfigFn = function<T>(value:T):T{
       return value
   }
   
   getData<string>('张三');
   ```

2. 通过将函数传入到带有接口的变量中

   ```typescript
   interface ConfigFn<T>{
       (value:T):T;
   }
   
   function getData<T>(value:T):T{
       return value
   }
   
   var myGetData:ConfigFn<string>=getData;
   
   myGetData('20')
   ```


#### 把类当作参数类型的泛型类 

```typescript
class User{
    username:string|undefined;
    password:string|undefined
}

class MysqlDb{
    add(user:User):boolean{
        console.log(user);
        
        return true
    }
}

var u = new User()
u.username='张三'
u.password= "1233"
var Db = new MysqlDb();
Db.add(u)
```

##### 泛化

封装对象

```typescript
class MysqlDb<T>{
    add(info:T):boolean{
        console.log(info);       
        return true
    }
}

class User{
    username:string|undefined;
    password:string|undefined
}

var u = new User()
u.username='张三'
u.password= "1233"
var Db = new MysqlDb<User>(); //为了添加限制我们加上泛型类型
Db.add(u)
```



```tsx
class ArticleCate {
    title: string | undefined
    desc: string | undefined
    status: string | undefined
    constructor(params: {
        title: string | undefined
        desc: string | undefined
        status?: string | undefined
    }) {
        this.title = params.title
        this.desc = params.desc
        this.status = params.status
    }
}

var a = new ArticleCate({
    title:"分类",
    desc:'1111'
})

var Db = new MysqlDb<ArticleCate>()
Db.add(a)
```



## 模块

> 从ECMAScript 2015开始，JavaScript引入了模块的概念。TypeScript也沿用这个概念。
>
> 模块在其自身的作用域里执行，而不是在全局作用域里；这意味着定义在一个模块里的变量，函数，类等等在模块外部是不可见的，除非你明确地使用[`export`形式](https://www.tslang.cn/docs/handbook/modules.html#export)之一导出它们。 相反，如果想使用其它模块导出的变量，函数，类，接口等的时候，你必须要导入它们，可以使用 [`import`形式](https://www.tslang.cn/docs/handbook/modules.html#import)之一。
>
> 模块是自声明的；两个模块之间的关系是通过在文件级别上使用imports和exports建立的。
>
> 模块使用模块加载器去导入其它的模块。 在运行时，模块加载器的作用是在执行此模块代码前去查找并执行这个模块的所有依赖。 大家最熟知的JavaScript模块加载器是服务于Node.js的 [CommonJS](https://en.wikipedia.org/wiki/CommonJS)和服务于Web应用的[Require.js](http://requirejs.org/)。
>
> TypeScript与ECMAScript 2015一样，任何包含顶级`import`或者`export`的文件都被当成一个模块。相反地，如果一个文件不带有顶级的`import`或者`export`声明，那么它的内容被视为全局可见的（因此对模块也是可见的）。



```typescript
var dbUrl = 'xxxxxx'

export function getData():any[]{
    console.log("获取数据库的值");
    return[
        {
            title:'12312'
        },
        {
            title:'123213'
        }
    ]
}
```

或者

```typescript
var dbUrl = 'xxxxxx'

function getData():any[]{
    console.log("获取数据库的值");
    return[
        {
            title:'12312'
        },
        {
            title:'123213'
        }
    ]
}
// export default {getData,dbUrl}
export {getData,dbUrl}
```



```typescript
import {getData} from './modules/db';
// import {getData as get} from './modules/db';
getData();
```

## 命名空间

```typescript
namespace A{
    interface Amimal{
        name:string
        eat():void
    }
    export class Dog implements Amimal{
        name: string
        constructor(name:string){
            this.name = name
        }
        eat(): void {
            console.log(`${this.name}在吃狗粮`,`);
        }
    }
    export class Cat implements Amimal{
        name: string
        constructor(name:string){
            this.name = name
        }
        eat(): void {
            console.log(`${this.name}在吃猫粮`);
        }
    }
}

var d = new A.Dog('狼狗')
d.eat()
```

## 装饰器

### 类装饰器

```json
{
    "compilerOptions": {
        "target": "ES5",
        "experimentalDecorators": true
    }
}
```



```typescript
function logClass(params:any){
    console.log(params);   
    params.prototype.run = function(){
        console.log("我是一个run方法");
        
    }
}

@logClass
class HttpClient{
    constructor(){

    }
    getData(){

    }
}

var http:any = new HttpClient()
http.run()

/*
HttpClient() {
    }
*/
```





```typescript
function logClass(params:string){
    return function(target:any){
        console.log(target,params);
    }
}

@logClass('hello')
class HttpClient{
    constructor(){

    }
    getData(){

    }
}

var http:any = new HttpClient()
```



### 方法

#### 可以改值

```typescript
// 方法装饰器
function logMethod(params:any){
    return function(target:any,methodName:any,desc:any){
        console.log(target);//原型对象
        console.log(methodName);//方法名
        console.log(desc);//描述
        target.apiUrl='xxxx'
        target.run = function(){
            console.log('run');
            
        }
    }
}

// @logClass('hello')
class HttpClient{
    public url:any|undefined
    constructor(){

    }
    @logMethod('http://')
    getData(){
        console.log(this.url)
    }
}

var http:any = new HttpClient()
// http.run()
console.log(http.apiUrl);

http.run()

```

#### 修改方法

```typescript
function logMethod(params:any){
    return function(target:any,methodName:any,desc:any){
        console.log(target);//原型对象
        console.log(methodName);//方法名
        console.log(desc.value);//描述
        target.apiUrl='xxxx'
        var oMethod=desc.value;
        desc.value =function(...args:any[]){
            oMethod.apply(this,args) //我的方法
            args = args.map((value)=>{
                return String(value)
            })
            console.log(args);
        }
    }
}

// @logClass('hello')
class HttpClient{
    public url:any|undefined
    constructor(){

    }
    @logMethod('http://')
    getData(...args:any[]){
        console.log(args)
        console.log('我是getData的方法')
    }
}

var http:any = new HttpClient()
// http.run()
// console.log(http.apiUrl);

http.getData(123,'xxx');


```



### 方法参数

```typescript
function logParams(params:any){
    return function(target:any,MethodName:any,paramsIndex:any){
        console.log(params);
        console.log(target);//
        console.log(MethodName);
        console.log(paramsIndex);//参数索引        
        target.apiUrl = params
    }
}

class HttpClient{
    public url:any|undefined
    constructor(){
    }
    
    getData(@logParams('uuid')uuid:any){
        console.log('我是getData的方法')
    }
}
var http:any = new HttpClient()
http.getData(123);

console.log(http.apiUrl);
```

### 执行顺序

属性 方法 装饰器 方法参数 属性 类