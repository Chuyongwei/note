# 开始

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

