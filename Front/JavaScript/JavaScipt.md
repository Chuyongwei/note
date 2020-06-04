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

