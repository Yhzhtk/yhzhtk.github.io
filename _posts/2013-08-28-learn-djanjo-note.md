---
layout: post
title: Django 学习笔记
description: Django是一个高效的Python Web框架，开发小型Web应用使用Django是个非常不错的选择，其核心有：用于创建模型的对象关系映射、为最终用户设计的完美管理界面、一流的 URL 设计、设计者友好的模板语言、缓存系统
tags: [Django, python]
---

#### Django介绍和安装

[Django](https://www.djangoproject.com/) 是一个高效开发实用设计的Python Web框架，是一个开放源码项目，源码托管在[Github](https://github.com/django/django)。使用Django可以很快速的开发Web项目，对于一些小的应用是相当不错的选择。其核心有：

	· 用于创建模型的对象关系映射
	· 为最终用户设计的完美管理界面
	· 一流的 URL 设计
	· 设计者友好的模板语言
	· 缓存系统

我学习是在Windows上，如果在Linux上类似，首先是安装，[下载Django](https://www.djangoproject.com/download/)压缩包并解压，使用以下命令安装。

	tar xzvf Django-1.5.2.tar.gz
	cd Django-1.5.2
	python setup.py install

<!--break-->
{% include syntax-python-sql.html %}

安装完成之后，在命令行输入：

	python -c "import django; print(django.get_version())"

如果提示当前安装的版本号则说明安装成功了。

#### 创建第一个Django项目并启动运行

接下来创建一个Django项目，将当前路径移动到你想要建立项目的地方，使用命令：

	python D:\Python27\Lib\site-packages\django\bin\django-admin.py startproject mysite

注意django-admin.py在你Python的安装路径下。创建好之后，使用`tree /F` (Windows下)可以查看项目文件树：

	mysite
	    │  manage.py
	    │
	    └─mysite
	            settings.py
	            urls.py
	            wsgi.py
	            __init__.py

这里面各种文件看名称可以大致猜出用途，具体使用，我们下面再说，先启动这个项目，进去第一层mysite文件夹，运行命令：

<pre class="brush: python;">

	python manage.py runserver # 默认端口8000
	python manage.py runserver 8080 # 指定端口
	python manage.py runserver 0.0.0.0:8000 # 指定绑定的IP和端口

</pre>

便启动了项目，默认端口是8000，在浏览器输入地址： http://localhost:8000/ 就可以看到 `“It worked!”`，项目就运行了。

#### 创建一个APP并使用Models，数据库关系对象映射

这就是最基本的创建一个Django项目的流程。下面创建一个app，也就是网站的一个应用：

	python manage.py startapp polls

查看路径，发现多了一个pools文件夹和几个文件：

	│  manage.py
	│
	├─mysite
	│      settings.py
	│      urls.py
	│      wsgi.py
	│      __init__.py
	│
	└─polls
	        models.py
	        tests.py
	        views.py
	        __init__.py

其中models.py是数据库的模式类，用于存储数据库数据。我们创建两个类Poll和Choice，Poll存储问题包括问题内容和发布时间，Choice存储选择，包括指示的内容和被选的次数。

`polls/models.py`

<pre class="brush: python;">

from django.db import models

class Poll(models.Model):
    def __unicode__(self):
        return self.question

    question = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

class Choice(models.Model):
    def __unicode__(self):
        return self.choice_text

    poll = models.ForeignKey(Poll)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)

</pre>

每个类都继承自`django.db.models.Model`,其中CharField和DateTimeField等表示数据库对应的数据类型。

Django能够完成创建数据库和完成数据库访问的功能，在使用数据库之前，我们需要先安装polls应用，编辑`settings.py`,设置好`DATABASES`（数据库引擎和连接信息），改变`INSTALLED_APPS`添加上polls:

`settings.py`

<pre class="brush: python;">

	INSTALLED_APPS = (
	    'django.contrib.auth',
	    'django.contrib.contenttypes',
	    'django.contrib.sessions',
	    'django.contrib.sites',
	    'django.contrib.messages',
	    'django.contrib.staticfiles',
	    # Uncomment the next line to enable the admin:
	    # 'django.contrib.admin',
	    # Uncomment the next line to enable admin documentation:
	    # 'django.contrib.admindocs',
		'polls'
	)

</pre>

现在运行命令根据modes创建数据库表：

	python manage.py sql polls

你将看到类似以下输出：

<pre class="brush: sql;">

	BEGIN;
	CREATE TABLE `polls_poll` (
	    `id` integer AUTO_INCREMENT NOT NULL PRIMARY KEY,
	    `question` varchar(200) NOT NULL,
	    `pub_date` datetime NOT NULL
	);
	CREATE TABLE `polls_choice` (
	    `id` integer AUTO_INCREMENT NOT NULL PRIMARY KEY,
	    `poll_id` integer NOT NULL,
	    `choice_text` varchar(200) NOT NULL,
	    `votes` integer NOT NULL
	);
	ALTER TABLE `polls_choice` ADD CONSTRAINT `poll_id_refs_id_3aa09835` FOREIGN KEY (`poll_id`) REFERENCES `polls_poll` (`id`);
	COMMIT;

</pre>

这就是将要创建的表SQL语句，如果你想要查看更多关于创建数据库表的信息，你可以使用以下命令：

- `python manage.py validate` – 检查models是否有错
- `python manage.py sqlcustom polls` – 输出自定义的SQL信息，如果没定义则为空
- `python manage.py sqlclear polls` – 如果你数据库存储polls信息，则输出删除语句
- `python manage.py sqlindexes polls` – 输出创建索引的SQL语句
- `python manage.py sqlall polls` – 输出所有要执行的SQL语句

检测以上命令之后都没有问题，就可以执行到数据库了，运行以下命令将上面创建数据库的信息同步到数据库：

	python manage.py syncdb

运行中可能需要创建Django的auth系列表，直接yes，输入用户名密码即可。创建结束之后，查看数据库表：

	auth_group
	auth_group_permissions
	auth_permission
	auth_user
	auth_user_groups
	auth_user_user_permissions
	django_content_type
	django_session
	django_site
	polls_choice
	polls_poll

如果需要查看数据库，可以通过Django内置的方法，用命令简易处理数据库信息，运行命令：

	python manage.py shell

会进入Shell处理程序（我安装了ipython就直接进入ipython），使用shell可以很方便的处理数据库，操作示例：

<pre class="brush: python;">

	&gt;&gt;&gt; from polls.models import Poll, Choice
	
	# 列出所有数据库的polls对象
	&gt;&gt;&gt; Poll.objects.all()
	[]
	
	# 创建一个 Poll 的对象. 对于时间，使用 timezone.now() 替代 datetime.datetime.now()
	&gt;&gt;&gt; from django.utils import timezone
	&gt;&gt;&gt; p = Poll(question="What's new?", pub_date=timezone.now())
	
	# 将对象保存进数据库
	&gt;&gt;&gt; p.save()
	
	# Now it has an ID. Note that this might say "1L" instead of "1", depending
	# on which database you're using. That's no biggie; it just means your
	# database backend prefers to return integers as Python long integer
	# objects.
	&gt;&gt;&gt; p.id
	1
	
	# 通过python直接访问数据库字段
	&gt;&gt;&gt; p.question
	"What's new?"
	&gt;&gt;&gt; p.pub_date
	datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=&lt;UTC&gt;)
	
	# 改变数据库字段，然后保存
	&gt;&gt;&gt; p.question = "What's up?"
	&gt;&gt;&gt; p.save()
	
	&gt;&gt;&gt; Poll.objects.all()
	[&lt;Poll: Poll object&gt;]
	# 针对类设置__unicode__方法之后，输出就变成以下内容
	&gt;&gt;&gt; Poll.objects.all()
	[&lt;Poll: What's up?&gt;]
	# Make sure our __unicode__() addition worked.
	
	# Django 提供丰富的查询API，可以通过关键字参数查询
	&gt;&gt;&gt; Poll.objects.filter(id=1)
	[&lt;Poll: What's up?&gt;]
	&gt;&gt;&gt; Poll.objects.filter(question__startswith='What')
	[&lt;Poll: What's up?&gt;]
	
	# 查询发布时间的年
	&gt;&gt;&gt; from django.utils import timezone
	&gt;&gt;&gt; current_year = timezone.now().year
	&gt;&gt;&gt; Poll.objects.get(pub_date__year=current_year)
	&lt;Poll: What's up?&gt;
	
	# 如果ID不存在则会抛出异常
	&gt;&gt;&gt; Poll.objects.get(id=2)
	Traceback (most recent call last):
	...
	DoesNotExist: Poll matching query does not exist. Lookup parameters were {'id': 2}
	
	# Django提供一个缩写pk表示 primary-key， 如下查询等同于Poll.objects.get(id=1).
	&gt;&gt;&gt; Poll.objects.get(pk=1)
	&lt;Poll: What's up?&gt;
	
	# Django访问关系也非常简单，获取一个Poll
	&gt;&gt;&gt; p = Poll.objects.get(pk=1)
	
	# 获取外键关联choice，默认是一个set
	&gt;&gt;&gt; p.choice_set.all()
	[]
	
	#  创建三个Choice
	&gt;&gt;&gt; p.choice_set.create(choice_text='Not much', votes=0)
	&lt;Choice: Not much&gt;
	&gt;&gt;&gt; p.choice_set.create(choice_text='The sky', votes=0)
	&lt;Choice: The sky&gt;
	&gt;&gt;&gt; c = p.choice_set.create(choice_text='Just hacking again', votes=0)
	
	# Choice的访问Poll
	&gt;&gt;&gt; c.poll
	&lt;Poll: What's up?&gt;
	
	# Poll获取Choice
	&gt;&gt;&gt; p.choice_set.all()
	[&lt;Choice: Not much&gt;, &lt;Choice: The sky&gt;, &lt;Choice: Just hacking again&gt;]
	&gt;&gt;&gt; p.choice_set.count()
	3
	
	# API自动查找关系，通过Choice查找父类满足指定条件的结果
	# 其中current_year可以事先指定
	&gt;&gt;&gt; Choice.objects.filter(poll__pub_date__year=current_year)
	[&lt;Choice: Not much&gt;, &lt;Choice: The sky&gt;, &lt;Choice: Just hacking again&gt;]
	
	# 删除一个Choice
	&gt;&gt;&gt; c = p.choice_set.filter(choice_text__startswith='Just hacking')
	&gt;&gt;&gt; c.delete()

</pre>

#### 系统自带的Admin管理

启用Django自带的管理系统，只需要做以下三个操作：

1. setting.py 文件中对`INSTALLED_APPS`项取消注释 `"django.contrib.admin"`
1. 运行 `python manage.py syncdb`更新数据库
1. 编辑`mysite/urls.py`文件，取消以下两个注释：

<pre class="brush: python;">

from django.contrib import admin
admin.autodiscover()

url(r'^admin/', include(admin.site.urls)),

</pre>

运行`python manage.py runserver`启动后，输入地址 [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/)即可登录管理。

登录后发现并有没有poll表和choice表，这只是系统用户的管理，要设置数据表的管理也很简单，只需要在polls下面创建一个文件`admin.py`,输入以下内容：

<pre class="brush: python;">

	from django.contrib import admin
	from polls.models import Poll， Choice
	
	admin.site.register(Poll)
	admin.site.register(Choice)

</pre>

重新启动，就可以看到Poll和Choice的管理界面了。管理可以完成增删改查的一些列数据库操作，方便简单。




