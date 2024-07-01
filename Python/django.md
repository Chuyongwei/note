## 安装

### diango

```powershell
pip install diango
```

### 创建项目

```powershell
django startproject [项目名称]
```

运行项目

```powershell
django startproject 
```

自动生成了django项目

### 文件介绍

```
manage.py 运行和管理项目
site：
asgi.py 接受网路i请求
wsgi.py  接收网络请求
urls.py url和函数的对新关系
settings.py 项目配置
app01:
admin.py 默认提供admin后台管理
apps. 
```

### APP

创建app

```powershell
python manage.py startapp app01
```

快速启动

1. 确保注册

   > 在settings.py中注册

   ```python
   INSTALLED_APPS = [
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       'app01.apps.App01Config',
   ]
   ```

2. 创建连接

   url.py

   ```python
   from django.contrib import admin
   from django.urls import path
   from app01 import views
   
   urlpatterns = [
       path('admin/', admin.site.urls),
       # xxx.com/index
       path('index', views.index, ),
       path('user', views.user_list ),
       path('user/add', views.user_add ),
   ]
   ```

   

3. 启动

   ```powershell
   python manage.py runserver
   ```


### 静态文件

url.py

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    # xxx.com/index
    path('index', views.index, ),
]+ static(settings.STATIC_URL)

```



网页

```html
{% load static %}

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>测试网页</title>
    <style type="text/css">
        #head {
            background: blue;
            border: 6px plum solid;
            width: 100px;
        }
    </style>
    <script src="{% static 'js/jquery.js' %}"></script>
    <script src="static/jquery.js"></script>
</head>

<body>

    <div id="head">
        <img src = {% staic 'aaa.png' %}/>
    </div>
</body>

</html>
```

> 使用该方法可以方便后期的项目部署时的路径迁移

#### 模板语法

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>测试网页</title>

</head>

<body>
    <h1>模板语法学习</h1>
<div>{{n1}}</div>
<div>{{n2.1}}</div>
<div>
    {% for item in n2 %}
    <span>{{item}}</span>
    {% endfor %}
</div>
    <hr/>
<div>
    {% for k,v in n3.items %}
    <span>{{k}}:{{v}}</span>
    <br/>
    {% endfor %}
</div>
    <hr/>
{% if n1 == "ceo" %}
<h1>lalal</h1>
    {% elif n1 == "huhu" %}
{% else %}
<h1>呼呼</h1>
{% endif %}
</body>

</html>
```

### 请求与相应

```py
def something(req):
    # req是一个对象，封装了用户通过过浏览器发送过来的所有数据
    #传输方法
    print(req.method)

    #http://127.0.0.1:8000/something?hh=312&lele=31
    print(req.GET["hh"])

    print(req.POST)
    return HttpResponse("hello")
```

拦截

```python
@csrf_exempt

```

### 数据库

```powershell
pip install mysqlclient
```

### orm

1. 创建数据库

2. setttings.py

   ```python
   DATABASES = {
       "default": {
           "ENGINE": "django.db.backends.mysql",
           "NAME": "graduate_system",
           "USER": "root",
           "PASSWORD": "654321",
           "HOST": "127.0.0.1",
           "PORT": "3306",
       }
   }
   ```

3. 创建表

   model.py

   ```python
   from django.db import models
   
   # Create your models here.
   
   # 自动创建表
   class UserInfo(models.Model):
       id = models.AutoField(primary_key=True,)
       name = models.CharField(max_length=10,blank=True)
       pwd = models.CharField(max_length=100)
       # 允许为空
       # pwd = models.CharField(NULL=True,blank=True )
       age = models.IntegerField()
   ```

   执行

   ```sh
   python manage.py makemigrations
   python manage.py migrate
   ```

   修改删除重复执行即可

   > 注意添加原有列就会需要选择操作，除非是有设置默认值

   **注意**

   `python manage.py makemigrations`是在migration文件中创建了修改文件

   因此他就是跟据自己存储的数据作为记录的，不建议从数据库修改表结构

4. 处理

   ```python
   from app01.models import Department,UserInfo
   def orm(req):
       # 1.新建
       # Department.objects.create(title="销售部")
       # Department.objects.create(title = "IT部")
       # UserInfo.objects.create(name="五七七",age=19,pwd="6541321")
       # UserInfo.objects.create(name="王伍尔", age=33, pwd="6541321")
   
       # 2.删除
       # UserInfo.objects.filter(id=1001).delete()
       # Department.objects.all().delete()
   
       # 3.查找
       data = UserInfo.objects.filter(id=1002).first()
       #对象
       # data = UserInfo.objects.filter(id=1002).first()
       # print(data)
       # for i in data:
       #     print(i.name,i.age)
   
       #     更新
       UserInfo.objects.filter(id=1002).update(age=90)
   
       return HttpResponse("成功")
   ```

   

