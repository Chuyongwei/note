# Class

## 原型链

- 对象有`__proto__`
- 函数用prototype

1. 类的操作在原型链上进行修改，否则类在创建的时候就无法创建到对象中
2. 如果对象修改了父类相同的值，会创建一个新的值，在引用时使用新创建的值；如果没有修改则使用父类的值



继承的方法

```javascript
// 定义一个动物类
    function Animal(name) {
        // 属性
        this.name = name || 'Animal';
        // 实例方法
        this.sleep = function () {
            console.log(this.name + '正在睡觉！');
        }
    }
    // 原型方法
    Animal.prototype.eat = function (food) {
        console.log(this.name + '正在吃：' + food);
    };
```



1. **原型链继承**

   **核心:** 将父类的实例作为子类的原型

   ```javascript
   //定义动物猫
   // 
   function Cat() {
   }
   // 原型变成父类的对象，因此构造器中的内容变得无意义了！！！
   Cat.prototype = new Animal();
   // 补充子类内容~_~
   Cat.prototype.name = 'cat';
    
   //　Test Code
   var cat = new Cat();
   console.log(cat.name); //cat
   cat.eat('fish'); //cat正在吃:fish
   cat.sleep(); //cat正在睡觉
   console.log(cat instanceof Animal); //true
   console.log(cat instanceof Cat); //true
   ```

   特点:

   1.非常纯粹的继承关系，实例是子类的实例，也是父类的实例；

   2.父类新增原型方法/原型属性，子类都能访问到；

   3.简单，易于实现；

   缺点:

   1.要想为子类新增属性和方法，必须要在new Animal()这样的语句之后执行，不能放到构造器中

   2.无法实现多继承；

   3.创建子类时，无法向父类构造函数传参；

   4.来自原型对象的属性是所有实例所共享；

2. **构造继承**

   **核心**：使用父类的构造函数来增强子类实例，等于是复制父类的实例属性给子类（没用到原型）

   ```javascript
   function Cat(name) {
       Animal.call(this);
       this.name = name || 'Tom';
   }
    
   // Test Code
   var cat = new Cat();
   console.log(cat.name);
   console.log(cat.sleep());
   console.log(cat instanceof Animal); // false
   console.log(cat instanceof Cat); // true
   ```

   特点:

   1. 解决了1中，子类实例共享父类引用属性的问题；
   2. 创建子类实例时，可以向父类传递参数；
   3. 可以实现多继承（call多个父类对象）；

   缺点:

   1. 实例并不是父类的实例，只是子类的实例；
   2. 只能继承父类的实例属性和方法，**不能继承原型属性/方法**；
   3. 无法实现函数复用，每个子类都有父类实例函数的副本，影响性能；

3. **实例继承**

   **核心**：为父类实例添加新特性，作为子类实例返回

   > Cat装饰父类，不算是类了

   ```javascript
   function Cat(name) {
       var instance = new Animal();
       instance.name = name || 'Tom';
       return instance;
   }
    
   // Test Code
   var cat = new Cat();
   console.log(cat.name);
   console.log(cat.sleep());
   console.log(cat instanceof Animal); // true
   console.log(cat instanceof Cat); // false
   ```

   特点:

   1. 不限制调用方式，不管是new 子类()还是子类(),返回的对象具有相同的效果；

   缺点:

   2.无法实现多继承；

4. 拷贝继承

   将父类的原型的值一点一点的移过去，属于一种原始的继承（拷贝父类的原型就可以复制了父类的原型的原型）

   ```javascript
   function Cat(name) {
       var animal = new Animal();
       for (var p in animal) {
           Cat.prototype[p] = animal[p];
       }
       Cat.prototype.name = name || 'Tom';
   }
    
   // Test Code
   var cat = new Cat();
   console.log(cat.name);
   console.log(cat.sleep());
   console.log(cat instanceof Animal); // false
   console.log(cat instanceof Cat); // true
   ```

   特点:

   1. 支持多继承；

   缺点:

   1. 效率较低，内存占用高（因为要拷贝父类的属性）；
   2. 无法获取父类不可枚举的方法（不可枚举方法，不能使用for in 访问到）；

5. **组合继承**

   > 以构造继承为基础解决原型链的问题，补充了原型链的继承
   >
   > 使用了原型链继承的方法，不同的是修改了原型的构造器

   **核心**：通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用

   子类的继承了父类，子类的原型是父类的构造的新对象：即子类使用父类原型的原型是用的子类的原型的原型     `Animal.prototype==Cat.prototype.prtotype`

   ```javascript
   function Cat(name) {
       Animal.call(this);
       this.name = name || 'Tom';
   }
   Cat.prototype = new Animal();
   Cat.prototype.constructor = Cat;
    
   // Test Code
   var cat = new Cat();
   console.log(cat.name);
   console.log(cat.sleep());
   console.log(cat instanceof Animal); // true
   console.log(cat instanceof Cat); // true
   ```

   特点:

   1.弥补了方式2的缺陷，可以继承实例属性/方法，也可以继承原型属性/方法；

   2.既是子类的实例，也是父类的实例；

   3.不存在引用属性共享问题；

   4.可传参；

   5.函数可复用；

   缺点:

   1. 调用了两次父类构造函数，生成了两份实例（子类实例将子类原型上的那份屏蔽了）

6. **寄生组合继承**

   > 以构造继承为基础解决原型链的问题，原型链使用原型为父类的空类

   **核心**：通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免的组合继承的缺点

   子类的继承了父类，子类的原型是有着父类原型的空类：子类使用有着父类原型的空类`Animal.prototype==Cat.prototype`

   ```javascript
   function Cat(name){
       Animal.call(this);
       this.name = name || 'Tom';
   }
   (function(){
       // 创建一个没有实例方法的类
       var Super = function(){};
       Super.prototype = Animal.prototype;
       //将实例作为子类的原型
       Cat.prototype = new Super();
       Cat.prototype.constructor = Cat; // 需要修复下构造函数
   })();
   
   /*
   
   
   */
    
   // Test Code
   var cat = new Cat();
   console.log(cat.name);
   console.log(cat.sleep());
   console.log(cat instanceof Animal); // true
   console.log(cat instanceof Cat); //true
   ```

   特点:

   1. 堪称完美；

   缺点:

   1.实现较为复杂；





