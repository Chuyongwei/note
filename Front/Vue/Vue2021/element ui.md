# 布局

## 栅格

`el-col`和`el-row`

> 高亮是el-row的属性

- span

  一共24个栅格

- ==gutter==：间隔

- offset：偏移栏数

- 将 ==type== 属性赋值为 'flex'，可以启用 flex 布局，并可通过 `justify` 属性来指定 start, center, end, space-between, space-around 其中的值来定义子元素的排版方式。

- 响应式布局

  预设了五个响应尺寸：`xs`、`sm`、`md`、`lg` 和 `xl`。

  ```html
  <el-row :gutter="10">
    <el-col :xs="8" :sm="6" :md="4" :lg="3" :xl="1"><div class="grid-content bg-purple"></div></el-col>
    <el-col :xs="4" :sm="6" :md="8" :lg="9" :xl="11"><div class="grid-content bg-purple-light"></div></el-col>
    <el-col :xs="4" :sm="6" :md="8" :lg="9" :xl="11"><div class="grid-content bg-purple"></div></el-col>
    <el-col :xs="8" :sm="6" :md="4" :lg="3" :xl="1"><div class="grid-content bg-purple-light"></div></el-col>
  </el-row>
  ```


## 布局容器

用于布局的容器组件，方便快速搭建页面的基本结构：

`<el-container>`：外层容器。当子元素中包含 `<el-header>` 或 `<el-footer>` 时，全部子元素会垂直上下排列，否则会水平左右排列。

`<el-header>`：顶栏容器。

`<el-aside>`：侧边栏容器。

`<el-main>`：主要区域容器。

`<el-footer>`：底栏容器。

> 以上组件采用了 flex 布局，使用前请确定目标浏览器是否兼容。此外，`<el-container>` 的子元素只能是后四者，后四者的父元素也只能是 `<el-container>`。

# 组件

## 图标

```vue
<span icon="" ></span>
```



## 按钮

### 事件类型

- 主要按钮（primary）
- 成功按钮（success）
- 信息按钮（info）
- 警告按钮(warning)
- 危险按钮（danger）

### 禁用状态

标签加上`disabled`

### 图标按钮

```html
<el-button type="primary" icon="el-icon-edit"></el-button>
```

### 按钮组

`el-button-group`

```html
<el-button-group>
  <el-button type="primary" icon="el-icon-arrow-left">上一页</el-button>
  <el-button type="primary">下一页<i class="el-icon-arrow-right el-icon--right"></i></el-button>
</el-button-group>
```



# 问题

## vue项目sass-loader安装失败

1. 安装

   ```powershell
   npm install node-sass --save-dev //安装
   node-sass npm install sass-loader --save-dev //安装sass-loader
   ```

   整理 node-sass 安装失败的原因及解决办法

   npm 安装 node-sass 依赖时， 由于国内网络环境的问题，有时会失败。

2. 解决方法：使用淘宝镜像

   ```powershell
   npm i node-sass --sass_binary_site=[https://npm.taobao.org/mirrors/node-sass
   ```

   cnpm有点坑不推荐

3. 在vue2.x中sass

   ```powershell
   cnpm install node-sass@4.13.1 --save-dev       //安装node-sass`
   cnpm install sass-loader@7.3.1 --save-dev         //安装依赖包sass-loader`
   ```

   



zhuan
