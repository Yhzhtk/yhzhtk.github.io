---
layout: post
title: Django 学习笔记
tags: [Django, python]
---
[Django](https://www.djangoproject.com/) 是一个高效开发实用设计的Python Web框架，是一个开放源码项目，源码托管在[Github](https://github.com/django/django)。其核心组件有：

*	用于创建模型的对象关系映射
*	为最终用户设计的完美管理界面
*	一流的 URL 设计
*	设计者友好的模板语言
*	缓存系统
<!--break-->
我学习是在Windows上，如果在Linux上类似，首先是安装，[下载Django](https://www.djangoproject.com/download/)压缩包并解压，使用以下命令安装。

	tar xzvf Django-1.5.2.tar.gz
	cd Django-1.5.2
	sudo python setup.py install

安装完成之后，在命令行输入：

	python -c "import django; print(django.get_version())"

如果提示当前安装的版本号则说明安装成功了。接下来创建一个Django项目，将当前路径移动到你想要建立项目的地方，使用命令：


	python D:\Python27\Lib\site-packages\django\bin\django-admin.py startproject mysite

注意django-admin.py在你Python路径下。创建好之后，使用tree /F(Windows下)可以查看项目文件树：

	mysite
	    │  manage.py
	    │
	    └─mysite
	            settings.py
	            urls.py
	            wsgi.py
	            __init__.py

这里面各种文件看名称可以大致猜出用途，具体使用，我们下面再说，先启动这个项目，进去第一层mysite文件夹，运行命令：

	python manage.py runserver
	python manage.py runserver 8080 # 指定端口
	python manage.py runserver 0.0.0.0:8000 # 指定绑定的IP和端口

便启动了项目，默认端口是8000，在浏览器输入地址： http://localhost:8000/ 就可以看到 **“It worked!”**，项目就运行了。

这就是最基本的创建一个Django项目的流程。

{% include syntax-python.html %}
<pre class="brush: python;">
</pre>

