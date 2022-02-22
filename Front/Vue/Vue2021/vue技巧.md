# vue的配置方面的问题

> 在目录中的vue.config.js文件我们进行vue的配置问题

## 部署

```js
module.exports = {
    devServer:{
        host: "localhost",
        port: 3000,
        open: true
    }
}
```

## 跨域

vue.config.js中

```js
amodule.exports = {
    devServer: {
        host: '127.0.0.1',
        port: 8084,
        open: true,// vue项目启动时自动打开浏览器
        proxy: {
            '/api': { // '/api'是代理标识，用于告诉node，url前面是/api的就是使用代理的
                target: "http://xxx.xxx.xx.xx:8080", //目标地址，一般是指后台服务器地址
                changeOrigin: true, //是否跨域
                pathRewrite: { // pathRewrite 的作用是把实际Request Url中的'/api'用""代替
                    '^/api': "" 
                }
            }
        }
    }
}
```

然后我们将axios加上基础路径

```js
axios.defaults.baseURL = '/api'
```

## 去除引用未使用组件限制

package.json

```js
"eslintConfig": {
  "rules": {
    "vue/no-unused-components": "off"
  }
}

```



# 编程问题

# 使用引号

```vue
<div :style="item+''">
    
</div>
```

