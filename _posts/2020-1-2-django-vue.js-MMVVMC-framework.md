---
layout: post
title:  "Windows10环境下django-vue.js MMVVMC前后端框架安装步骤"
comments: true
categories: 教程
---

## 安装python及django环境
### 1、下载python3.7.6

https://www.python.org/ftp/python/3.7.6/python-3.7.6-amd64.exe

选择安装位置，默认安装（Administrator权限）

### 2、修改系统环境变量，使python3.7及pip3可用

安装位置\Python37\python.exe

安装位置\Python37\Scripts

### 3、使用pip3安装django2.2.9

```
pip3 install Django==2.2.9
```

如果提示pip3不存在，可使用以下代码安装django

```
python -m pip3 install Django==2.2.9
```

查询django版本

```
python -m django --version
```

检查django-admin是否可用

```
django-admin
```

### 4、选择项目位置，构建django项目

```
cd /
mkdir Project
cd Project
django-admin.py startproject test1
cd test1
python manage.py runserver
```

Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.

使用http://127.0.0.1:8000/即可访问django网页

### 5、创建后端

```
python manage.py startapp backend
```

## 安装Vue-Cli脚手架

### 1、下载Node.js

https://nodejs.org/dist/v12.14.0/node-v12.14.0-x64.msi

选择安装位置并安装（Administrator权限）

会默认安装Node.js v12.14.0与npm v6.13.4，建议安装到默认位置，会自动创建系统环境变量

### 2、使用npm安装Vue Cli脚手架

```bash
npm install -g @vue/cli
```

### 3、选择项目位置，构建vue项目

```
cd /Project/test1
vue-init webpack frontend
```

### 4、运行vue程序

```
cd frontend
npm run dev
```

使用http://127.0.0.1:8001/即可访问vue网页

### 5、使用Webpack打包Vue.js项目

```
$进入vue.js项目目录
cd frontend
npm install
npm run build
$如提示won't work
cd dist
npm install -g http-server
hs
$使用localhost:8001即可访问页面
```

## 修改项目文件，使其使用vue.js的视图层

### 1、修改项目根路由

```
$此时应在test1根目录下，进入test1目录，即项目设置路径
cd test1
```

找到项目根 urls.py，修改为如下所示

```
from django.contrib import admin
from django.urls import path
from django.conf.urls import url
from django.views.generic import TemplateView
from django.conf.urls import include

urlpatterns = [
    path('admin/', admin.site.urls),
    path(r'', TemplateView.as_view(template_name="index.html")),
    path('api/', include('backend.urls', namespace='api')),
]
```

### 2、修改项目根设置

找到项目根 settings.py，增加以下信息

```
INSTALLED_APPS = [
    'backend',  //增加backend
]

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': ['frontend/dist'],  //修改这里
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

$ 在文件最后增加
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "frontend/dist/static"),
]
```

### 3、修改backend中的路由

找到backend中的 urls.py，修改为如下所示

```
from . import views
from django.urls import path

urlpatterns = [
    path(r'', views.index, name='index'),
]

app_name='backend'
```

### 4、修改backend中的view.py

找到backend中的 view.py，修改为如下所示

```
from django.shortcuts import render
from django.http import HttpResponse #添加

# Create your views here.

def index(request): #添加示例API
    return HttpResponse("Hello API !")
```

### 5、运行项目

```
python manage.py runserver
```

此时，在浏览器页面输入，可正常访问vue.js构建的前端页面，django构建的api，与django自带的admin管理界面

```
127.0.0.1:8000
```

![vue](..\pictures\vue.png)

```
127.0.0.1:8000/api
```

![api](..\pictures\api.png)

```
127.0.0.1:8000/admin
```

![django-admin](..\pictures\django-admin.png)

