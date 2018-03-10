---
title: Django使用(Some Problems in Using Django)
date: 2017-11-13 11:32:00
tags: [Python,Django]
categories: [Web]
---
## python虚拟环境建立                
```              
# Windows:                 
<建立>python -m venv 环境名          
<进入>../Scripts/activate.bat             
<退出>deactivate        

# Ubuntu14:             
<建立>python3 -m venv --without-pip test4             
<进入>.source test4/bin/activate                   
<pip>curl https://bootstrap.pypa.io/get-pip.py | python               
<退出>deactivate                

<查看当前安装python库环境>pip freeze                   
```                     

## Django基本命令                              
```                           
<创建工程>django-admin.py startproject project_name                   
<创建应用>python manage.py startapp app_name              
<创建更改的文件-数据库>python manage.py makemigrations              
<将生成的py文件应用到数据库>python manage.py migrate                
<使用开发服务器>python manage.py runserver (ip:prot)              
<清空数据库>python manage.py flush              
<创建超级管理员>python manage.py createsuperuser          
<修改用户密码>python manage.py changepassword username               
<Django 项目环境终端>python manage.py shell               
<数据库命令行>python manage.py dbshell                       
<验证有无错误>python manage.py validate
```                                          

## Django视图(views.py)                       
* 视图的两个条件:                       
1. 第一个参数类型：HttpRequest            
2. 返回HttpRespense               
* view<-建立联系->url：                   

```                  
# views.py  
from django.http import HttpResponse
def hello(request):
    return HttpResponse("Hello Django")

# urls.py
from firstapp.views import *
urlpatterns = [
    path('hello/', hello),
]
```                       

## Django模板             
> 模板里面含有逻辑与变量，用于动态生成HTML页面                         
* 使用Django的最基本方式:                                
1. 用原始模板代码字符串创建一个Template对象              
2. 调用模板对象的render方法，并传入一套变量         

```                                
from django import template
t=template.Template("hello,{{name}}")
c=template.Context({"name":"Django"})
print(t.render(c))
```                      

* 使用模板步骤：                  
1. 创建模板HTML文件--一般放在app/templates/目录下                 
2. 在settings.py中指定模板路径                    
```                            
import os
TEMPALTE_DIRS=os.path.join(os.path.dirname(__file__),'templates')
```                          

3. 在view.py中返回模板（另一种简单方法）                                      
```                          
from django.shortcut import render_to_response
def index(request):
    return render_to_response('2.html',{'name':'hello'})
```                     

# 模型开发与数据库交互                   
可以在setting.py中配置数据库        
```                      
# model.py
from django.db import models
class mymodel(models.Model):
    ...
```                          

这样的类有一些操作数据库的方法，这相当于django对各种数据库提供了一个facade                             

# Django后台管理                       
将自定义类型加入后台管理：           
```                   
# admin.py
from django.contrib import admin
admin.site.register(Myclass)
```                              

