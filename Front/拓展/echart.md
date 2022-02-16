## 基本使用

1. 导入echarts组件

2. 创建一个新的echart组件并将div导入echart组件中

   ```js
   var dom = document.getElementById("container");
   var myChart = echarts.init(dom);
   ```

3. 给组件导入数据

   ```js
   option = {
       xAxis: {
           type: 'category',
           data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
       },
       yAxis: {
           type: 'value'
       },
       series: [{
           data: [820, 932, 901, 934, 1290, 1330, 1320],
           type: 'line'
       }]
   };
   myChart.setOption(option, true);
   ```

   

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Echart</title>
</head>

<body>
    <div style="width: 80px; background-color: red; height: 80px;"></div>
    <div id="container" style="height: 800px;width: 800px;"></div>
    <script src="https://cdn.bootcdn.net/ajax/libs/echarts/5.3.0/echarts.min.js"></script>
    <script>
        var dom = document.getElementById("container");
        var myChart = echarts.init(dom);
        option = {
            legend: {}, // 图标
            tooltip: {}, // 节点介绍
            xAxis: {  // 横坐标
                type: 'category', // 类型
                data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
            },
            yAxis: { // 纵坐标
                type: 'value' 
            },
            series: [{
                name: "fdsaf",
                color : "blue",
                data: [820, 932, 901, 934, 1290, 1330, 1320],//数据
                type: 'line' // 图形样式
            },
            {
                name: "sadhg",
                color : "red",
                data: [802, 732, 1001, 930, 190, 1430, 1390],//数据
                type: 'line' // 图形样式
            }]
        };
        myChart.setOption(option, true);
    </script>
</body>

</html>
```



## 数据集

而数据设置在 `数据集`（`dataset`）中，会有这些好处：

- 能够贴近数据可视化常见思维方式：（I）提供数据，（II）指定数据到视觉的映射，从而形成图表。
- 数据和其他配置可以被分离开来。数据常变，其他配置常不变。分开易于分别管理。
- 数据可以被多个系列或者组件复用，对于大数据量的场景，不必为每个系列创建一份数据。
- 支持更多的数据的常用格式，例如二维数组、对象数组等，一定程度上避免使用者为了数据格式而进行转换。

一般形式的

```js
option = {
  legend: {},
  tooltip: {},
  dataset: {
    // 提供一份数据。
    source: [
      ['product', '2015', '2016', '2017'],
      ['Matcha Latte', 43.3, 85.8, 93.7],
      ['Milk Tea', 83.1, 73.4, 55.1],
      ['Cheese Cocoa', 86.4, 65.2, 82.5],
      ['Walnut Brownie', 72.4, 53.9, 39.1]
    ]
  },
  // 声明一个 X 轴，类目轴（category）。默认情况下，类目轴对应到 dataset 第一列。
  xAxis: { type: 'category' },
  // 声明一个 Y 轴，数值轴。
  yAxis: {},
  // 声明多个 bar 系列，默认情况下，每个系列会自动对应到 dataset 的每一列。
  series: [{ type: 'bar' }, { type: 'bar' }, { type: 'bar' }]
};
```

也可以是对象形式

```js
option = {
  legend: {},
  tooltip: {},
  dataset: {
    // 用 dimensions 指定了维度的顺序。直角坐标系中，如果 X 轴 type 为 category，
    // 默认把第一个维度映射到 X 轴上，后面维度映射到 Y 轴上。
    // 如果不指定 dimensions，也可以通过指定 series.encode
    // 完成映射，参见后文。
    dimensions: ['product', '2015', '2016', '2017'],
    source: [
      { product: 'Matcha Latte', '2015': 43.3, '2016': 85.8, '2017': 93.7 },
      { product: 'Milk Tea', '2015': 83.1, '2016': 73.4, '2017': 55.1 },
      { product: 'Cheese Cocoa', '2015': 86.4, '2016': 65.2, '2017': 82.5 },
      { product: 'Walnut Brownie', '2015': 72.4, '2016': 53.9, '2017': 39.1 }
    ]
  },
  xAxis: { type: 'category' },
  yAxis: {},
  series: [{ type: 'bar' }, { type: 'bar' }, { type: 'bar' }]
};
```

### 数据到图形的映射

### 把数据集（ dataset ）的行或列映射为系列（series）

有了数据表之后，使用者可以灵活地配置：数据如何对应到轴和图形系列。

用户可以使用 `seriesLayoutBy` 配置项，改变图表对于行列的理解。`seriesLayoutBy` 可取值：

- 'column': 默认值。系列被安放到 `dataset` 的列上面。
- 'row': 系列被安放到 `dataset` 的行上面。

```js
option = {
  legend: {},
  tooltip: {},
  dataset: {
    source: [
      ['product', '2012', '2013', '2014', '2015'],
      ['Matcha Latte', 41.1, 30.4, 65.1, 53.3],
      ['Milk Tea', 86.5, 92.1, 85.7, 83.1],
      ['Cheese Cocoa', 24.1, 67.2, 79.5, 86.4]
    ]
  },
  xAxis: [
    { type: 'category', gridIndex: 0 },
    { type: 'category', gridIndex: 1 }
  ],
  yAxis: [{ gridIndex: 0 }, { gridIndex: 1 }],
  grid: [{ bottom: '55%' }, { top: '55%' }],
  series: [
    // 这几个系列会出现在第一个直角坐标系中，每个系列对应到 dataset 的每一行。
    { type: 'bar', seriesLayoutBy: 'row' },
    { type: 'bar', seriesLayoutBy: 'row' },
    { type: 'bar', seriesLayoutBy: 'row' },
    // 这几个系列会出现在第二个直角坐标系中，每个系列对应到 dataset 的每一列。
    { type: 'bar', xAxisIndex: 1, yAxisIndex: 1 },
    { type: 'bar', xAxisIndex: 1, yAxisIndex: 1 },
    { type: 'bar', xAxisIndex: 1, yAxisIndex: 1 },
    { type: 'bar', xAxisIndex: 1, yAxisIndex: 1 }
  ]
};
```



## 图形样式

### 折线图（line）

#### 基础版

```js
option = {
  xAxis: {
    type: 'category',
    data: ['A', 'B', 'C']
  },
  yAxis: {
    type: 'value'
  },
  series: [
    {
      data: [120, 200, 150],
      type: 'line'
    }
  ]
};
```

#### 完整版

```js
option = {
    xAxis: {
        type: 'category',
        data: ['A', 'B','b','c','C']
    },
    yAxis: {
        type: 'value'
    },
    series: [
        {
            data: [120, 200,'-', 90, 150], // '-'表示空数
            type: 'line',
            lineStyle:{ // 线
                color: "red",
                widh: 4,
                type:"dashed" // dashed 虚线 dotted 点线 solid 实线
            },
            label:{ // 标签
                show:false,
                position:"bottom", // 支持|top | left | right | bottom | inside | insideLeft | insideRight | insideTop | insideBottom | insideTopLeft | insideBottomLeft | insideTopRight | insideBottomRight
                distance: 80, // 离线的距离 top和insideRight有效
                offset: [80,90], //标签的偏移
                rotate: 13, //角度
                formatter: '{b}: {@score}',//内容编辑器 详见笔记
                color: "#fff000",//字体颜色
                textStyle:{
                    fontSize:90
                }
            },
            emphasis:{
                label:{show:"true"} //设置鼠标移动时显示
            } 
        }
    ]
};
```



#### 标签



[series-line.](https://echarts.apache.org/zh/option.html#series-line)[label.](https://echarts.apache.org/zh/option.html#series-line.label) [formatter](https://echarts.apache.org/zh/option.html#series-line.label.formatter)

> 标签内容格式器，支持字符串模板和回调函数两种形式，字符串模板与回调函数返回的字符串均支持用 `\n` 换行。

**字符串模板** 模板变量有：

- `{a}`：系列名。
- `{b}`：数据名。
- `{c}`：数据值。
- `{@xxx}`：数据中名为 `'xxx'` 的维度的值，如 `{@product}` 表示名为 `'product'` 的维度的值。
- `{@[n]}`：数据中维度 `n` 的值，如 `{@[3]}` 表示维度 3 的值，从 0 开始计数。

**示例：**

```ts
formatter: '{b}: {@score}'
```

**回调函数**

回调函数格式：

```ts
(params: Object|Array) => string
```

参数 `params` 是 formatter 需要的单个数据集。格式如下：

```ts
{
    componentType: 'series',
    // 系列类型
    seriesType: string,
    // 系列在传入的 option.series 中的 index
    seriesIndex: number,
    // 系列名称
    seriesName: string,
    // 数据名，类目名
    name: string,
    // 数据在传入的 data 数组中的 index
    dataIndex: number,
    // 传入的原始数据项
    data: Object,
    // 传入的数据值。在多数系列下它和 data 相同。在一些系列下是 data 中的分量（如 map、radar 中）
    value: number|Array|Object,
    // 坐标轴 encode 映射信息，
    // key 为坐标轴（如 'x' 'y' 'radius' 'angle' 等）
    // value 必然为数组，不会为 null/undefied，表示 dimension index 。
    // 其内容如：
    // {
    //     x: [2] // dimension index 为 2 的数据映射到 x 轴
    //     y: [0] // dimension index 为 0 的数据映射到 y 轴
    // }
    encode: Object,
    // 维度名列表
    dimensionNames: Array<String>,
    // 数据的维度 index，如 0 或 1 或 2 ...
    // 仅在雷达图中使用。
    dimensionIndex: number,
    // 数据图形的颜色
    color: string
}
```

注：encode 和 dimensionNames 的使用方式，例如：

如果数据为：

```ts
dataset: {
    source: [
        ['Matcha Latte', 43.3, 85.8, 93.7],
        ['Milk Tea', 83.1, 73.4, 55.1],
        ['Cheese Cocoa', 86.4, 65.2, 82.5],
        ['Walnut Brownie', 72.4, 53.9, 39.1]
    ]
}
```

则可这样得到 y 轴对应的 value：

```ts
params.value[params.encode.y[0]]
```

如果数据为：

```ts
dataset: {
    dimensions: ['product', '2015', '2016', '2017'],
    source: [
        {product: 'Matcha Latte', '2015': 43.3, '2016': 85.8, '2017': 93.7},
        {product: 'Milk Tea', '2015': 83.1, '2016': 73.4, '2017': 55.1},
        {product: 'Cheese Cocoa', '2015': 86.4, '2016': 65.2, '2017': 82.5},
        {product: 'Walnut Brownie', '2015': 72.4, '2016': 53.9, '2017': 39.1}
    ]
}
```

则可这样得到 y 轴对应的 value：

```ts
params.value[params.dimensionNames[params.encode.y[0]]]
```



### 扇形图（pie）



# 整合问题

## vue整合echarts

### 安装

由于目前新版的echart没有兼容vue2，所以我们要用旧版的

```powershell
npm install echarts@4.9.0 --save
```

正常情况下

```powershell
npm install echart --save
```

### 配置

在main.js中进行全局的访问

```javascript
import echarts from 'echarts'
Vue.prototype.$echarts = echarts
```

### 使用

```vue
//在Echarts.vue文件中
<template>
  <div class="Echarts">
    <div id="main" style="width: 600px; height: 400px"></div>
  </div>
</template>
 
<script>
export default {
  name: "Echarts",
  methods: {
    myEcharts() {
      var myChart = this.$echarts.init(document.getElementById("main"));
      //配置图表
      var option = {
        title: {
          text: "echarts入门示例",
        },
        tooltip: {},
        legend: {
          data: ["销量"],
        },
        xAxis: {
          data: ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"],
        },
        yAxis: {},
        series: [
          {
            name: "销量",
            type: "bar",
            data: [5, 20, 36, 10, 10, 20],
          },
        ],
      };
      myChart.setOption(option);
    },
  },
  mounted() {
    this.myEcharts();
  },
};
</script>
 
<style>
</style>
```



