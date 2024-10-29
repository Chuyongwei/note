## 安装

### django

```powershell
pip install django
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

## 数据库

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

### 执行

二维表转换成模型

```py
python manage.py inspectdb > polls/models.py
```

模型转二维表

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

   

## Seesion



导入session

在创建Django项目时，默认的配置文件`settings.py`文件中已经激活了一个名为`SessionMiddleware`的中间件（关于中间件的知识我们在后面的章节做详细讲解，这里只需要知道它的存在即可），因为这个中间件的存在，我们可以直接通过请求对象的`session`属性来操作会话对象。前面我们说过，`session`属性是一个像字典一样可以读写数据的容器对象，因此我们可以使用“键值对”的方式来保留用户数据。与此同时，`SessionMiddleware`中间件还封装了对cookie的操作，在cookie中保存了sessionid，这一点我们在上面已经提到过了。

在默认情况下，Django将session的数据序列化后保存在关系型数据库中，在Django 1.6以后的版本中，默认的序列化数据的方式是JSON序列化，而在此之前一直使用Pickle序列化。JSON序列化和Pickle序列化的差别在于前者将对象序列化为字符串（字符形式），而后者将对象序列化为字节串（二进制形式），因为安全方面的原因，JSON序列化成为了目前Django框架默认序列化数据的方式，这就要求在我们保存在session中的数据必须是能够JSON序列化的，否则就会引发异常。还有一点需要说明的是，使用关系型数据库保存session中的数据在大多数时候并不是最好的选择，因为数据库可能会承受巨大的压力而成为系统性能的瓶颈，在后面的章节中我们会告诉大家如何将session保存到缓存服务中以提升系统的性能。

### 在视图函数中读写cookie

下面我们对如何使用cookie做一个更为细致的说明以便帮助大家在Web项目中更好的使用这项技术。Django封装的`HttpRequest`和`HttpResponse`对象分别提供了读写cookie的操作。

HttpRequest封装的属性和方法：

1. `COOKIES`属性 - 该属性包含了HTTP请求携带的所有cookie。
2. `get_signed_cookie`方法 - 获取带签名的cookie，如果签名验证失败，会产生`BadSignature`异常。

HttpResponse封装的方法：

1. `set_cookie`方法 - 该方法可以设置一组键值对并将其最终将写入浏览器。
2. `set_signed_cookie`方法 - 跟上面的方法作用相似，但是会对cookie进行签名来达到防篡改的作用。因为如果篡改了cookie中的数据，在不知道[密钥](<https://zh.wikipedia.org/wiki/%E5%AF%86%E9%92%A5>)和[盐](<https://zh.wikipedia.org/wiki/%E7%9B%90_(%E5%AF%86%E7%A0%81%E5%AD%A6)>)的情况下是无法生成有效的签名，这样服务器在读取cookie时会发现数据与签名不一致从而产生`BadSignature`异常。需要说明的是，这里所说的密钥就是我们在Django项目配置文件中指定的`SECRET_KEY`，而盐是程序中设定的一个字符串，你愿意设定为什么都可以，只要是一个有效的字符串。

上面提到的方法，如果不清楚它们的具体用法，可以自己查阅一下Django的[官方文档](<https://docs.djangoproject.com/en/2.1/ref/request-response/>)，没有什么资料比官方文档能够更清楚的告诉你这些方法到底如何使用。

刚才我们说过了，激活`SessionMiddleware`之后，每个`HttpRequest`对象都会绑定一个session属性，它是一个类似字典的对象，除了保存用户数据之外还提供了检测浏览器是否支持cookie的方法，包括：

1. `set_test_cookie`方法 - 设置用于测试的cookie。
2. `test_cookie_worked`方法 - 检测测试cookie是否工作。
3. `delete_test_cookie`方法 - 删除用于测试的cookie。
4. `set_expiry`方法 - 设置会话的过期时间。
5. `get_expire_age`/`get_expire_date`方法 - 获取会话的过期时间。
6. `clear_expired`方法 - 清理过期的会话。

下面是在执行登录之前检查浏览器是否支持cookie的代码。通常情况下，浏览器默认开启了对cookie的支持，但是可能因为某种原因，用户禁用了浏览器的cookie功能，遇到这种情况我们可以在视图函数中提供一个检查功能，如果检查到用户浏览器不支持cookie，可以给出相应的提示。

```py
def login(request):
    if request.method == 'POST':
        if request.session.test_cookie_worked():
            request.session.delete_test_cookie()
            # Add your code to perform login process here
        else:
            return HttpResponse("Please enable cookies and try again.")
    request.session.set_test_cookie()
    return render_to_response('login.html')
```

