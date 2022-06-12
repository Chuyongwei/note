解决跨域

```powershell
server
{
   listen 3003;
   server_name localhost;
   ##  = /表示精确匹配路径为/的url，真实访问为http://localhost:5500
   location = / {
       proxy_pass http://localhost:5500;
   }
   ##  /no 表示以/no开头的url，包括/no1,no/son，或者no/son/grandson
   ##  真实访问为http://localhost:5500/no开头的url
   ##  若 proxy_pass最后为/ 如http://localhost:3000/;匹配/no/son，则真实匹配为http://localhost:3000/son
   location /no {
       proxy_pass http://localhost:3000;
   }
   ##  /ok/表示精确匹配以ok开头的url，/ok2是匹配不到的，/ok/son则可以
   location /ok/ {
       proxy_pass http://localhost:3000;
   }
}
```



解决vue的router问题

- hash模式

  ```powershell
       location /patient-user {
        root /usr/share/nginx/html;
        index  index.html;
        try_files $uri $uri/ /index.html;
      }
  ```

- history模式

  1. router

     ```js
     const router = new VueRouter({
       mode: 'history',
       base: '/HackerAC/',
       scrollBehavior (to, from, savedPosition) {
         return { x: 0, y: 0 }
       },
       routes
     })
     ```

  2. vue.config.js

     ```powershell
     module.exports = {
       publicPath: process.env.NODE_ENV === 'production' ? '/HackerAC/' : '/',
       outputDir: 'dist/HackerAC',
     }
     ```

  3. nginx的配置
  
     ```js
     http {
         include       mime.types;
         default_type  application/octet-stream;
         sendfile        on;
         keepalive_timeout  65;
        
         server {
             listen       8085;
             server_name  localhost;
             
             location ^~ /HackerAC {
                 index  /HackerAC/index.html;
                 try_files $uri $uri/ /HackerAC/index.html;
             }
             
             error_page   500 502 503 504  /50x.html;
             location = /50x.html {
                 root   html;
             }
         }
     }
     ```
  
     
