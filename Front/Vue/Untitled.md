# 基于vue-cli@3.x的移动webapp项目搭建

1. 执行`vue create app-name`创建一个项目

2. 进入到项目目录，如果没有安装router，就执行`npm install vue-router -S`安装vue-router并写入依赖

3. 清除无用的文件，让`src`目录保持最简单的一个`main.js`和`App.vue`即可

4. 创建一个名为`views`的目录, 在里面分别创建`Home.vue`,`Mall.vue`,`Cart.vue`,`Mine.vue`这四个页面级的视图组件

5. 创建一个名为`router`的目录，分别创建`index.js`和`routes.js`

   ```js
   // src/router/routes.js
      
   // 非页面级别的组件，一般不需要做成异步组件
   import SqFooter from '@/components/SqFooter'
   // 以下为页面级别的组件，需要做成异步组件
   const Home = () => import('@/views/Home')
   const Mall = () => import('@/views/Mall')
   const Cart = () => import('@/views/Cart')
   const Mine = () => import('@/views/Mine')
   // 导出的配置项
   export default [
     // 根目录访问，直接重定向到/home
     {
       path: '/',
       redirect: '/home',
       meta: {
         isTabItem: false // 由于tabbar的数据和routes的数据是共享的，但是又不是全部的routes数据，所以需要加一个标记来确定该路由是否需要显示在tabbar
       }
     },
     // 首页的组件和路由
     {
       path: '/home', // 访问的路径
       name: 'home', // 路由的名字
       components: {
         default: Home, // 默认的router-view
         footer: SqFooter // 带有name="footer"属性的router-view
       },
       meta: {
         isTabItem: true, 
         title: '首页', // 用于显示在tabbar上的文字
         icon: '&#xe60b;' // tabbar的icon，注意这里unicode,渲染的时候需要使用v-html，而不是插值表达式
       }
     },
     {
       path: '/mall',
       name: 'mall',
       components: {
         default: Mall,
         footer: SqFooter
       },
       meta: {
         isTabItem: true,
         title: '商城',
         icon: '&#xe603;'
       }
     },
     {
       path: '/cart',
       name: 'cart',
       components: {
         default: Cart,
         footer: SqFooter
       },
       meta: {
         isTabItem: true,
         title: '购物车',
         icon: '&#xe607;'
       }
     },
     {
       path: '/mine',
       name: 'mine',
       components: {
         default: Mine,
         footer: SqFooter
       },
       meta: {
         isTabItem: true,
         title: '我的',
         icon: '&#xe606;'
       }
     }
   ]
   ```

   ```js
   // src/router/index.js
   import Vue from 'vue'
   import VueRouter from 'vue-router'
      
   import routes from './routes'
      
   Vue.use(VueRouter)
      
   export default new VueRouter({
     routes
   })
   ```

6. 开始你的布局。在App.vue里，把头部、主体和底部的布局先写好, 样式什么样，就自已定, 注意在写布局的时候，最好加入css-reset

   ```html
   <template>
     <div class="sq-app-container">
       <div class="sq-app-header">
        头部
       </div>
       <div class="sq-app-main">
         主体
       </div>
       <div class="sq-app-tabbar">
         底部
       </div>
     </div>
   </template>
   ```

7. 下一步可以在头部直接加入一个组件，在主体部分加入router-view， 在底部也加一个带有命名的router-view

   ```html
   <template>
     <div class="sq-app-container">
       <div class="sq-app-header">
      		// 这个组件是不需要使用router-view渲染的   
         <SqHeader></SqHeader>
       </div>
       <div class="sq-app-main">
         <!-- 这里就是用于渲染default组件 -->
         <router-view></router-view>
       </div>
       <div class="sq-app-tabbar">
         <!-- 用于渲染底部的tabbar -->
         <router-view name="footer"></router-view>
       </div>
     </div>
   </template>
      
   <script>
   import SqHeader from '@/components/SqHeader'
   export default {
     components: {
       SqHeader
     }
   }
   </script>
      
   <style lang="scss">
   html,
   body,
   div,
   span,
   h1,
   h2,
   h3,
   h4,
   h5,
   h6,
   p,
   a,
   address,
   em,
   img,
   b,
   u,
   i,
   center,
   dl,
   dt,
   dd,
   ol,
   ul,
   li,
   form,
   label,
   legend,
   table,
   caption,
   tbody,
   tfoot,
   thead,
   tr,
   th,
   td,
   article,
   aside,
   canvas,
   details,
   embed,
   footer,
   header,
   hgroup,
   menu,
   nav,
   output,
   ruby,
   section,
   summary,
   time,
   mark,
   audio,
   video {
     margin: 0;
     padding: 0;
     border: 0;
     font-size: 100%;
     font: inherit;
     vertical-align: baseline;
   }
   // HTML5 display-role reset for older browsers
   article,
   aside,
   details,
   figcaption,
   figure,
   footer,
   header,
   hgroup,
   menu,
   nav,
   section {
     display: block;
   }
   body {
     line-height: 1;
   }
   ol,
   ul {
     list-style: none;
   }
   table {
     border-collapse: collapse;
     border-spacing: 0;
   }
   html,
   body {
     height: 100%;
     overflow: hidden;
   }
   .sq-app {
     &-container {
       display: flex;
       flex-direction: column;
       height: 100%;
     }
     &-main {
       flex: 1;
       overflow-x: hidden;
     }
   }
      
   @font-face {
     font-family: 'sqicon';  /* project id 1169064 */
     src: url('//at.alicdn.com/t/font_1169064_c2nrugsecol.eot');
     src: url('//at.alicdn.com/t/font_1169064_c2nrugsecol.eot?#iefix') format('embedded-opentype'),
     url('//at.alicdn.com/t/font_1169064_c2nrugsecol.woff2') format('woff2'),
     url('//at.alicdn.com/t/font_1169064_c2nrugsecol.woff') format('woff'),
     url('//at.alicdn.com/t/font_1169064_c2nrugsecol.ttf') format('truetype'),
     url('//at.alicdn.com/t/font_1169064_c2nrugsecol.svg#iconfont') format('svg');
   }
   </style>
   ```

接下来就是实际业务逻辑的开发，以及后面还要讲的一些ajax的配置和vuex状态管理