 

# **学生管理系统Web开发**

Python之Django笔记

作者:bug智造者-小刘

我把程序代码运行在云服务器上了，这里献上网址

项目地址：[首页](http://124.221.71.6:8080)

注意：点击”欢迎使用学生信息管理系统“才能够进入登陆页面

进入登陆页面需要输入账号密码

可以自行创建普通账号进行登录

普通账号只能查看所有信息，不可进行增删改查操作，只有root管理员用户才可以，需要root管理员账号和密码的可以私信我索要，免费提供学习交流

背景：

在学习掌握了一定的Python基础后，并初步学习了解了Django的web开发框架，以及MySQL数据库的基本用法，通过Django的学习，完成简单的学生管理系统的web开发实践，并记录学习Django的过程以及遇到的问题.

**环境准备**

**安装****python****环境**

已安装，此处忽略

**安装****Django**

> pip install django

**检验安装****Django****版本**

```python
import django
print("Django版本为",django.get_version())
# 输出结果为:Django版本为 3.2.7
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**构建项目**

**创建****Django****项目**

> django-admin startproject project_name

**创建****app**

> django-admin startapp app_name

**终端创建****app**

> python manage.py startapp app

**启动服务器，运行程序**

> python manage.py runserver

**配置****MySQL**

**安装****python****环境**

> pip install pymysql

**修改****setting.py****文件**

将创建的app配置到setting.py文件中去

```python
INSTALLED_APPS = [
   'django.contrib.admin',
   'django.contrib.auth',
   'django.contrib.contenttypes',
   'django.contrib.sessions',
   'django.contrib.messages',
   'django.contrib.staticfiles',
   'app', #app名称
]
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**配置****MySQL****数据库的信息**

```python
#需要修改自己的数据库文件
#其中host为你的本机地址,port为你安装数据库的端口,password是登录数据库密码
DATABASES = {
   'default': {
       'ENGINE': 'django.db.backends.mysql',
       'NAME': 'demo',
       'USER': 'root',
       'PASSWORD': 'root',
       'HOST': '127.0.0.1',
       'POST': '3306'
  }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**配置** ***init\*.py****文件**

```python
from pymysql import install_as_MySQLdb
install_as_MySQLdb()
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



启动MySQL服务

> net start mysql

登录MySQL数据库

> mysql -uroot -p

创建数据库

> create database database_name charset utf8

**创建模型类****Model**

```python
from django.db import models

# Create your models here.

# 1.创建模型类
# 创建用户类
class UserInfo(models.Model):
# 2.定义字段 属性
   UserId = models.AutoField(primary_key=True)
   username = models.CharField(max_length=16, blank=False, verbose_name="用户名")
   password = models.CharField(max_length=16, blank=False, verbose_name="密码")

# 创建学生信息类
class StuInfo(models.Model):
   id = models.AutoField(primary_key=True)
   stuname = models.CharField(max_length=16, verbose_name="学生姓名")
   stuphone = models.CharField(max_length=16, verbose_name="学生电话")
   stuaddress = models.CharField(max_length=16, verbose_name="学生地址")
   stucollege = models.CharField(max_length=16, verbose_name="学生院系")
   stumajor = models.CharField(max_length=16, verbose_name="学生专业")
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)


 ​

在终端上进行数据迁移(每操作Models.py相当于对数据库表进行操作，都需要执行数据迁移)

> python manage.py makemigrations
>  ​
>  python manage.py migrate

**admin****站点创建**

配置admin后台站点的信息，注册模型为UserInfo,StuInfo

```python
from django.contrib import admin
from .models import UserInfo, StuInfo
# Register your models here.
class UserInfoAdmin(admin.ModelAdmin):
    list_display = ['UserId', 'username', 'password']

class StuInfoAdmin(admin.ModelAdmin):
    list_display = ['id', 'stuname', 'stuphone', 'stuaddress', 'stucollege', 'stumajor']

admin.site.register(UserInfo, UserInfoAdmin)
admin.site.register(StuInfo, StuInfoAdmin)
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**创建管理员用户**

> python manage.py createsuperuser

**首页**

**配置路由**

**一级路由**

添加一个app下的路由

```python
path('', include('app.urls')),

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
   path('admin/', admin.site.urls),
   path('', include('app.urls')),
]
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**创建二级路由**

在APP下创建一个路由文件urls.py文件

```python
from django.urls import path
from . import views

urlpatterns = [
   path('', views.home, name="主页"),
]
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**创建模板**

在APP下创建一个模板文件templates，用于存放HTML页面模板

新建一个home.html文件，用于展示主页信息内容

```html
home.html

<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <title>首页</title>
<style>
   button{
      color: brown;
       border: 0px;
       font-size: 40px;
       line-height: 100px;
       background-color: transparent
  }

   body{
       text-align: center;
       background-size: 100%
  }
</style>
</head>

<body background="/static/1.jpg">
   <a href="login">
   <button type="submit">欢迎使用学生信息管理系统</button>
   </a>
</body>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**配置静态文件夹**

创建静态文件夹，在app下创建一个static文件夹，存放静态文件，比如图片，HTML页面展示的背景图片，在setting.py进行配置如下

```python
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/3.2/howto/static-files/
STATIC_URL = '/static/'
STATICFILES_DIRS = [
   os.path.join(BASE_DIR, 'static'),
]
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



编写视图模块

```python
from django.shortcuts import render

# Create your views here.

def home(request):
   return render(request, "home.html")

def login(request):
   return render(request, "login.html")
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20211004022939757.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



**登录**

**完善登录模块，实现登录功能**

前端部分login

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>登录</title>
</head>
<body background="/static/1.jpg" style="background: 100%">
<div  style="text-align: center; margin-top: 150px">
<form action="home" method="post" class="form-signin">
  {% csrf_token %}
   <h1>&nbsp&nbsp&nbsp登&nbsp&nbsp录</h1>
   <label>账号:</label>
   <input placeholder="🐕账号"><br><br>
   <label>密码:</label>
   <input placeholder="🔑密码"><br><br>
   <button type="submit">登录</button>
</form>
</div>
</body>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**完善前端代码，修改登录页面**

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>登录</title>
   <style>
       input,button{
   width: 320px;
   height: 45px;
   margin: 0px 8px;
   border-radius: 10px; /*圆角矩形*/
   text-indent: 10px; /*里面隐形的字体首行缩进*/
   margin: 10px auto;
}
       body{
   margin: 0px;
   padding: 0px;
   height: 180px;
   background-image: url(/static/1.jpg);   /*背景图片*/
   background-size:100% 100%;   /*图片自由伸缩*/
}
   </style>

</head>
<body background="/static/1.jpg" style="background-size: cover">
<div  style="text-align: center; margin-top: 60px">
<form action="/login/" method="post" class="form-signin" >
  {% csrf_token %}
   <h1 style="color: white; font-size: 60px">欢迎登录本系统</h1><br><br><br>
   <label style="color: white; font-size: 20px">账号：&nbsp</label>
   <input name="username"  type="text" style="width: 150px;height: 25px" placeholder="🐕账号"required autofocus><br><br>
   <label style="color: white; font-size: 20px">密码：&nbsp</label>
   <input name="password" type="password" style="width: 150px;height: 25px"  placeholder="🔑密码"required><br><br>
   <div style="padding-left: 70px">
       <button type="reset"style="width: 70px;height: 30px;font-size: 15px;color: palevioletred" >重置</button>&nbsp
       <button type="submit"style="width: 70px;height: 30px;font-size: 15px;color: palevioletred" >登录</button>
   </div>
   <p style="color: brown; padding-left: 70px">{{msg}}</p>
</form>

</div>
</body>

</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**后端部分**

```python
LoginId = 0
def login(request):
    #判断请求类型
    if request.method =="GET":
        return render(request, "login.html")
    else:
        #从前端表单中获取输入的数据，即账号和密码
        name = request.POST.get("username", None)
        pwd = request.POST.get("password", None)
        #获取数据库中的账号密码数据
        emp = models.UserInfo.objects.values("username", "password", "UserId").filter(username=name)
        #判断根据账号筛选前端输入的数据是否存在于数据库中
        if emp.count() == 0:
            return render(request, "login.html", {"msg": "登录失败,账号不存在"})
        else:
            if emp[0]['password'] == pwd:
                globals()["LoginId"] = emp[0]['UserId']

                return render(request, "welcome.html")
            else:
                return render(request, "login.html", {'msg': '登陆失败,密码错误'})
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20211004020844826.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

![img](https://img-blog.csdnimg.cn/20211004023224227.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

 ![img](https://img-blog.csdnimg.cn/20211004022846406.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



**主界面**

登录之后进入主界面，即welcome界面

在templates模板创建一个welcome的HTML模板

**前端部分****welcome**



```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>welcome</title>
   <style>
       body{
           background: pink;
      }
       div{
           text-align: center;
           padding-top: 50px;
      }
       h1{
           font-size: 50px;
           color: blue;
           text-align: center;
           padding-top: 50px;
      }
       button {
           width: 300px;
           height: 45px;
           font-size: 20px;
           color: brown;/*字体颜色*/
           border-color: blue;/*边框颜色*/
           border-radius: 20px;/*圆角矩形*/

      }

   </style>
</head>
<body>

<h1>欢迎进入学生管理系统</h1>
<div>
   <button type="submit">增加学生信息</button><br><br>
   <button type="submit">删除学生信息</button><br><br>
   <button type="submit">修改学生信息</button><br><br>
   <button type="submit">查看学生信息</button><br><br>
   <a href="/login">
   <button type="button">回到登录页面</button>
   </a>
</div>
</body>
</html>

实现回到登录页面功能

使用a标签的href跳转链接

<a href="/login">
<button type="button">回到登录页面</button>
</a>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



![img](https://img-blog.csdnimg.cn/2021100402312329.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



**实现添加学生信息功能**

**前端部分**

创建一个add.html模板

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>添加信息</title>
<style>
   body{
       background: lightblue;
       text-align: center;
  }
   h1{
       font-size: 30px;
       padding-top: 30px;
  }
   label,input{
       font-size: 20px;
      /* border-top: 20px;             padding-top: 20px;内边距*/
       margin-top: 15px;/*外边距*/
       width: 180px;   /*输入框长度*/
  }
   button{         /*控制按钮位置大小及间距*/
       font-size: 25px;
       margin-left: 60px;
       margin-right: -20px;
       /*border-radius: 10px;*/
       color: deepskyblue;
  }

</style>
</head>
<body>
<h1>请在下列信息栏填入学生的基本信息</h1><br>
<div>
<form id="StuInfo" action="/add/" method="post">
  {% csrf_token %}
   <label>学生学号:</label>
   <input type="text"name="id"required autofocus><br>
   <label>学生姓名:</label>
   <input type="text"name="stuname"required autofocus><br>
   <label>学生电话:</label>
   <input type="text"name="stuphone"required autofocus><br>
   <label>学生地址:</label>
   <input type="text"name="stuaddress"required autofocus><br>
   <label>学生院系:</label>
   <input type="text"name="stucollege"required autofocus><br>
   <label>学生专业:</label>
   <input type="text"name="stumajor"required autofocus><br>

</form>
<p style="font-size: 20px; color: red; padding-left: 60px">
      {% if err %}
          {{ err }}
      {% endif %}
</p>
<p style="padding-left: 90px;font-size: 20px">
      {% if success %}
          {{ success }}
      {% endif %}
</p>
</div>
<div>

   </p>
   <button type="reset" form="StuInfo">清空</button>
   <button type="submit" form="StuInfo">提交</button><br><br>
   <a href="/welcome">
       <button style="width: 180px">返回主页面</button>
   </a>
   </div>
</body>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**后端部分****(views****视图****)**

```python
def add(request):
   if request.method == 'GET':
       return render(request, 'add.html')
   elif request.method =='POST':
       list = ['id', 'stuname', 'stuphone', 'stuaddress', 'stucollege', 'stumajor']
       info = []
       for li in list:
           info.append(request.POST.get(li))
       if globals()['LoginId'] != 1:
           return HttpResponse("非root用户，没有权限添加用户！")
       s = models.StuInfo.objects.filter(id=info[0])
       if s.count != 0:
           return render(request, 'add.html', {'err': '学生信息已存在，请勿重复添加'})
       stu = models.StuInfo()
       stu.id = info[0]
       stu.stuname = info[1]
       stu.stuphone = info[2]
       stu.stuaddress = info[3]
       stu.stucollege = info[4]
       stu.stumajor = info[5]
       stu.save()  #保存数据
       return render(request, 'add.html', {'success': '学生信息添加成功!!'})def add(request):
   if request.method == 'GET':
       return render(request, 'add.html')
   elif request.method =='POST':
       list = ['id', 'stuname', 'stuphone', 'stuaddress', 'stucollege', 'stumajor']
       info = []
       for li in list:
           info.append(request.POST.get(li))
       if globals()['LoginId'] != 1:
           return HttpResponse("非root用户，没有权限添加用户！")
       s = models.StuInfo.objects.filter(id=info[0])
       if s.count != 0:
           return render(request, 'add.html', {'err': '学生信息已存在，请勿重复添加'})
       stu = models.StuInfo()
       stu.id = info[0]
       stu.stuname = info[1]
       stu.stuphone = info[2]
       stu.stuaddress = info[3]
       stu.stucollege = info[4]
       stu.stumajor = info[5]
       stu.save()  #保存数据
       return render(request, 'add.html', {'success': '学生信息添加成功!!'})
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)




   ![img](https://img-blog.csdnimg.cn/20211004020913206.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​编辑







**实现删除学生信息功能**

**前端部分**

创建模板delete.html,添加路由映射

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>删除信息</title>

<style>
   body{
       background: lightblue;
       text-align: center;
  }
   h1{
       font-size: 30px;
       padding-top: 30px;
  }
   label,input{
       font-size: 20px;
      /* border-top: 20px;             padding-top: 20px;内边距*/
       margin-top: 15px;/*外边距*/
       width: 180px;   /*输入框长度*/
  }
   button{         /*控制按钮位置大小及间距*/
       font-size: 18px;
       margin-left: 50px;
       margin-right: -20px;
       /*border-radius: 10px;*/
       color: deepskyblue;
  }
   div{
       font-size: 20px;
  }

</style>
</head>
<body>

<h1>请根据学生学号进行删除信息</h1><br>
<form action="/delete/" method="post">
  {% csrf_token %}
<label>学生学号:</label>
<input type="text" name="id" placeholder="请输入学号">
<button type="submit" value="submit">确定</button>
</form>
<br>
<a href="/welcome">
   <button>回到主界面</button>
</a>

<div style="color: red"><br>
{% if err %}
{{ err }}
{% endif %}
</div>
<div>
{% if success %}
{{ success }}
{% endif %}
</div>
</body>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**后端部分**

```python
def delete(request):
   if request.method == 'GET':
       return render(request, 'delete.html')

   id = request.POST.get('id', None)
   print(id)
   if id.isspace() == True:
       return render(request, 'delete.html', {'err': '学号不能由空格组成,请重新输入！！！'})
   if len(id) == 0:
       return render(request, 'delete.html', {'err': '学号不能为空,请重新输入！！！'})
   emp = models.StuInfo.objects.filter(id=id)
   if emp.count() == 0:
       return render(request, 'delete.html', {'err': '该用户不存在,请重新输入！！！'})
       models.StuInfo.objects.filter(id=id).delete()
       return render(request, 'delete.html', {'success': '删除成功'})
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



 ![img](https://img-blog.csdnimg.cn/20211004021103290.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑







**实现修改学生信息功能**

**前端部分**

同上，创建update模板，添加路由

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>更新信息</title>
   <style>
   body{
       background: lightblue;
       text-align: center;
  }
   h1{
       font-size: 30px;
       padding-top: 30px;
  }
   label,input{
       font-size: 20px;
      /* border-top: 20px;             padding-top: 20px;内边距*/
       margin-top: 15px;/*外边距*/
       width: 180px;   /*输入框长度*/
  }
   button{         /*控制按钮位置大小及间距*/
       font-size: 25px;
       margin-left: 40px;
       margin-right: -30px;
       /*border-radius: 10px;*/
       color: deepskyblue;
       margin-top: 10px;
  }

</style>
</head>
<body>
<h1>请根据学生学号进行修改学生信息操作</h1>
<form id="StuInfo" action="/update/"method="post">
  {% csrf_token %}
   <label>学号:</label>
   <input name='id'placeholder="请输入学生学号"required autofocus="autofocus"><br>
   <label>姓名:</label>
   <input name="stuname" placeholder="请输入学生姓名"><br>
   <label>电话:</label>
   <input name="stuphone" placeholder="请输入学生电话"><br>
   <label>地址:</label>
   <input name="stuaddress" placeholder="请输入学生地址"><br>
   <label>院系:</label>
   <input name="stucollege" placeholder="请输入学生院系"><br>
   <label>专业:</label>
   <input name="stumajor" placeholder="请输入学生专业"><br>

</form>
<p style="font-size: 25px">
{% if success %}
{{ success }}
{% endif %}
</p>
<p style="font-size: 25px; color: red">
{% if err %}
{{ err }}
{% endif %}
</p>

<a href="/welcome/">
   <button type="submit" style="font-size: 20px">返回</button>
</a>
<button form="StuInfo" type="submit" style="font-size: 20px">确定</button>

</body>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**后端部分**

```python
def update(request):
   if request.method =='GET':
       return render(request, 'update.html')
   list = ['id', 'stuname', 'stuphone', 'stuaddress', 'stucollege', 'stumajor']
   info = []
   for li in list:
       info.append(request.POST.get(li))
   if globals()['LoginId'] != 1:
       return render(request, 'update.html', {'err': '权限不够，请切换为 root 用户重试'})
   id = request.POST.get('id', None)
   if id.isspace() == True:
       return render(request, 'update.html', {'err': '学号不能为空格组成,请重新输入！'})
   s = models.StuInfo.objects.filter(id=info[0])
   if s.count() == 0:
       return render(request, 'update.html', {'err': '没有此学生信息，无法修改'})
   stu = models.StuInfo()
   stu.id = info[0]
   stu.stuname = info[1]
   stu.stuphone = info[2]
   stu.stuaddress = info[3]
   stu.stucollege = info[4]
   stu.stumajor = info[5]
   stu.save()
   return render(request, 'update.html', {'success': '学生信息修改成功！'})
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



 ![img](https://img-blog.csdnimg.cn/2021100402095661.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑







![img](https://img-blog.csdnimg.cn/20211004021103290.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑





**实现查询学生信息功能**

**前端部分**

查询页面select

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>查询信息</title>
   <style>
   body{
       background: lightblue;
       text-align: center;
  }
   h1{
       font-size: 30px;
       padding-top: 20px;
  }
   label,input{
       font-size: 20px;
      /* border-top: 20px;             padding-top: 20px;内边距*/
       margin-top: 15px;/*外边距*/
       width: 180px;   /*输入框长度*/
       
  }
   button{         /*控制按钮位置大小及间距*/
       font-size: 20px;
       margin-left: 40px;
       margin-right: -20px;
       /*border-radius: 10px;*/
       color: deepskyblue;
  }

       
</style>
</head>
<body>
<div>
<h1>查询学生信息</h1>
<form action="/select/" method="post">
  {% csrf_token %}
   <label style="margin-left: 100px; ">请根据学生学号进行查询:</label>
   <input name="id" placeholder="请输入学生学号" required>
   <button type="submit">查询</button>
</form>
<br>
<a href="/login">
<button style="background-color: transparent;border-style: hidden;color: red">
  {% if error %}
      {{ error }}
  {% endif %}<br>
</button>
</a>
<label style="color: red">
  {% if err %}
      {{ err }}
  {% endif %}
  {% if success %}
      {{ success }}
  {% endif %}<br>
</label>
</div>
<div>
   <h1 style="margin-top: -20px">查询结果如下</h1>
   <div style="font-size: 30px; margin-left: 700px; width:400px; text-align: left">
       <p> 学生学号:{{ id }}</p>
       <p>学生姓名:{{ stuname }}</p>
       <p>学生电话:{{ stuphone }}</p>
       <p>学生地址:{{ stuaddress }}</p>
       <p>学生院系:{{ stucollege }}</p>
       <p>学生专业:{{ stumajor }}</p>
   </div>

<p>
<a href="/welcome">
   <button>回到主菜单</button>
</a>
</p>
</div>
</body>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**后端部分**

```python
def select(request):
   if request.method =='GET':
       return render(request, 'select.html')
   if globals()['LoginId'] != 1:
       return render(request, 'select.html', {'err': '非 root 用户，无法查看！'})
   #从表单中获取id值
   id = request.POST.get('id', None)
   #判断id不能为空的字符串类型
   if id.isspace() == True:
       return render(request, 'select.html', {'err': "不能为空值,请重新输入！"})
   #从数据库根据id值将对应信息赋值给stu
   stu = models.StuInfo.objects.filter(id=id)
   if stu.count() == 0:
       return render(request, 'select.html', {'err': '没有查询到此学生信息，请确定是否录入系统'})
   info = models.StuInfo.objects.values('id', 'stuname', 'stuphone', 'stuaddress', 'stucollege', 'stumajor').filter(id=id)[0]
   print("info=", info)
   return render(request, 'select.html', info)
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



 ![img](https://img-blog.csdnimg.cn/2021100402112012.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑







**查询所有学生信息**

**前端**

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>所有信息</title>
   <style>
       body{
           text-align: center;
      }
       label{
           align: center;
           font-size: 30px;
      }
       label2{
           font-size: 25px;
      }

   </style>
</head>
<body>
<h1>所有学生信息一览</h1>

<table border="1" align="center"style="border-collapse: collapse; width: 1000px">
   <tr>
<th id="id">学生学号</th>
<th>学生姓名</th>
<th>学生电话</th>
<th>学生地址</th>
<th>学生院系</th>
<th>学生专业</th>
   </tr>

  {% for s in info %}
<tr>
<td id="id"> {{ s.id }}  </td>
<td> {{ s.stuname }}  </td>
<td> {{ s.stuphone }}  </td>
<td> {{ s.stuaddress }}  </td>
<td> {{ s.stucollege }}  </td>
<td> {{ s.stumajor }}  </td>
</tr>
  {% endfor %}

</table>
<br>
<a href="/welcome/">
   <button>返回</button>
</a>
</body>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**后端代码**

```python
def info(request):
   if request.method == 'POST':
       return render(request, 'info.html')
   info = models.StuInfo.objects.all()
   print(info)
   return render(request, 'info.html', {"info": info})
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





 ![img](https://img-blog.csdnimg.cn/20211004021126409.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑







# 效果展示



![img](https://img-blog.csdnimg.cn/20211004021416502.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



![img](https://img-blog.csdnimg.cn/20211004021438503.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑![img](https://img-blog.csdnimg.cn/20211004021344677.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

 

电脑端测试结果截图：

![img](https://img-blog.csdnimg.cn/37b4ca0c113d4e5ebc56d8c81bebce21.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



![img](https://img-blog.csdnimg.cn/20211004015121323.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

 ![img](https://img-blog.csdnimg.cn/20211004020730174.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



![img](https://img-blog.csdnimg.cn/20211004020743953.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



 ![img](https://img-blog.csdnimg.cn/2021100402123793.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑





 ![img](https://img-blog.csdnimg.cn/20211004021243584.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



 ![img](https://img-blog.csdnimg.cn/20211004021148213.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



 ![img](https://img-blog.csdnimg.cn/20211004021302821.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

iPad端web测试结果截图

![img](https://img-blog.csdnimg.cn/20211004032030894.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

![img](https://img-blog.csdnimg.cn/20211004032042259.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

 ![img](https://img-blog.csdnimg.cn/20211004032045936.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

 ![img](https://img-blog.csdnimg.cn/20211004032051138.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



![img](https://img-blog.csdnimg.cn/20211004032055463.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



![img](https://img-blog.csdnimg.cn/20211004032058440.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



![img](https://img-blog.csdnimg.cn/20211004032102334.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

 ![img](https://img-blog.csdnimg.cn/20211004032109764.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

 ![img](https://img-blog.csdnimg.cn/20211004032113331.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



![img](https://img-blog.csdnimg.cn/20211004032118183.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

 手机移动端测试结果截图

![img](https://img-blog.csdnimg.cn/20211004032404376.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



![img](https://img-blog.csdnimg.cn/20211004032412569.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

 ![img](https://img-blog.csdnimg.cn/20211004032417422.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



![img](https://img-blog.csdnimg.cn/20211004032422925.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

 ![img](https://img-blog.csdnimg.cn/20211004032429797.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



![img](https://img-blog.csdnimg.cn/20211004032512747.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



![img](https://img-blog.csdnimg.cn/20211004032523687.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAYnVn5pm66YCg,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



源码文件下载：[https://download.csdn.net/download/weixin_45971950/27882607![ ](https://csdnimg.cn/release/blog_editor_html/release1.9.1/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=L892)https://download.csdn.net/download/weixin_45971950/27882607](https://download.csdn.net/download/weixin_45971950/27882607)

[Index of /download/![ ](https://csdnimg.cn/release/blog_editor_html/release2.2.3/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=N176)http://124.221.71.6/download/](http://124.221.71.6/download/)
