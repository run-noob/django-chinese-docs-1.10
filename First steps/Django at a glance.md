### **Django at a glance**

Because Django was developed in a fast-paced newsroom environment, it was designed to make common Web-development tasks fast and easy. Here’s an informal overview of how to write a database-driven Web app with Django.

Django诞生在稍纵即逝的新闻行业背景下，它的设计初衷是为了使常见的web开发工作变得快捷和简单。以下简单介绍了下如何用Django编写一个数据驱动的web应用。

The goal of this document is to give you enough technical specifics to understand how Django works, but this isn’t intended to be a tutorial or reference – but we’ve got both! When you’re ready to start a project, you can [start with the tutorial](https://docs.djangoproject.com/en/1.10/intro/tutorial01/) or [dive right into more detailed documentation](https://docs.djangoproject.com/en/1.10/topics/).

本文档通过足够的技术细节来让你理解Django是如何工作的，但它并不表示是一个新手指南或参考 -- 我们两者兼顾！当你准备好新建一个项目，你可以从[新手指南](https://docs.djangoproject.com/en/1.10/intro/tutorial01/) 开始，具体细节请看[详细文档](https://docs.djangoproject.com/en/1.10/topics/)。

#### **Design your model**

#### **设计你的模型**

Although you can use Django without a database, it comes with an [object-relational mapper](https://en.wikipedia.org/wiki/Object-relational_mapping) in which you describe your database layout in Python code.

尽管在Django中你可以不用数据库，但它提供了一个[object-relational mapper](https://en.wikipedia.org/wiki/Object-relational_mapping) 系统，通过ORM你可以用python代码来描述你的数据库结构

The [data-model syntax](https://docs.djangoproject.com/en/1.10/topics/db/models/) offers many rich ways of representing your models – so far, it’s been solving many years’ worth of database-schema problems. Here’s a quick example:

[数据库模型语法](https://docs.djangoproject.com/en/1.10/topics/db/models/) 给你的模型提供了丰富的API -- 到目前为止，它解决了多年来数据库表问题。下面是一个简单例子：

mysite/news/models.py
```
from django.db import models

class Reporter(models.Model):
    full_name = models.CharField(max_length=70)

    def __str__(self):              # __unicode__ on Python 2
        return self.full_name

class Article(models.Model):
    pub_date = models.DateField()
    headline = models.CharField(max_length=200)
    content = models.TextField()
    reporter = models.ForeignKey(Reporter, on_delete=models.CASCADE)

    def __str__(self):              # __unicode__ on Python 2
        return self.headline
```

#### **Install it**

#### **安装Django**

Next, run the Django command-line utility to create the database tables automatically:

接下来，运行以下Django命令工具来自动创建数据库表：

```
python manage.py migrate
```

The [migrate](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-migrate) command looks at all your available models and creates tables in your database for whichever tables don’t already exist, as well as optionally providing [much richer schema control](https://docs.djangoproject.com/en/1.10/topics/migrations/).

[migrate](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-migrate) 命令会查找你所有可用的模块然后在你的数据库中创建还不存在的数据库表，同时还提供[非常丰富的表操作](https://docs.djangoproject.com/en/1.10/topics/migrations/).。

#### **Enjoy the free API**

#### **感受API的魅力**

With that, you’ve got a free, and rich, [Python API](https://docs.djangoproject.com/en/1.10/topics/db/queries/) to access your data. The API is created on the fly, no code generation necessary: 

接着，你就可以使用一个便捷且功能丰富的[*Python API*](https://docs.djangoproject.com/en/1.10/topics/db/queries/)来访问你的数据。这些API是即时创建的，不需要代码生成：

```
# Import the models we created from our "news" app
>>> from news.models import Reporter, Article

# No reporters are in the system yet.
>>> Reporter.objects.all()
[]

# Create a new Reporter.
>>> r = Reporter(full_name='John Smith')

# Save the object into the database. You have to call save() explicitly.
>>> r.save()

# Now it has an ID.
>>> r.id
1

# Now the new reporter is in the database.
>>> Reporter.objects.all()
[<Reporter: John Smith>]

# Fields are represented as attributes on the Python object.
>>> r.full_name
'John Smith'

# Django provides a rich database lookup API.
>>> Reporter.objects.get(id=1)
<Reporter: John Smith>
>>> Reporter.objects.get(full_name__startswith='John')
<Reporter: John Smith>
>>> Reporter.objects.get(full_name__contains='mith')
<Reporter: John Smith>
>>> Reporter.objects.get(id=2)
Traceback (most recent call last):
    ...
DoesNotExist: Reporter matching query does not exist.

# Create an article.
>>> from datetime import date
>>> a = Article(pub_date=date.today(), headline='Django is cool',
...     content='Yeah.', reporter=r)
>>> a.save()

# Now the article is in the database.
>>> Article.objects.all()
[<Article: Django is cool>]

# Article objects get API access to related Reporter objects.
>>> r = a.reporter
>>> r.full_name
'John Smith'

# And vice versa: Reporter objects get API access to Article objects.
>>> r.article_set.all()
[<Article: Django is cool>]

# The API follows relationships as far as you need, performing efficient
# JOINs for you behind the scenes.
# This finds all articles by a reporter whose name starts with "John".
>>> Article.objects.filter(reporter__full_name__startswith='John')
[<Article: Django is cool>]

# Change an object by altering its attributes and calling save().
>>> r.full_name = 'Billy Goat'
>>> r.save()

# Delete an object with delete().
>>> r.delete()
```

#### **A dynamic admin interface: it’s not just scaffolding – it’s the whole house**

#### **动态管理界面：它并不只是脚手架，而是一整栋房子**

Once your models are defined, Django can automatically create a professional, production ready [administrative interface](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/) – a website that lets authenticated users add, change and delete objects. It’s as easy as registering your model in the admin site:

一旦你的模型定义之后，Django能自动创建一个专业的、可以用于生产环境的 [*管理界面*](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/)– 可以让认证的用户添加、修改和删除对象的一个站点。
只需简单地在admin系统中注册你的模型即可：

mysite/news/models.py
```
from django.db import models

class Article(models.Model):
    pub_date = models.DateField()
    headline = models.CharField(max_length=200)
    content = models.TextField()
    reporter = models.ForeignKey(Reporter, on_delete=models.CASCADE)
```

mysite/news/admin.py
```
from django.contrib import admin

from . import models

admin.site.register(models.Article)
```

The philosophy here is that your site is edited by a staff, or a client, or maybe just you – and you don’t want to have to deal with creating backend interfaces just to manage content.

这里的设计理念是你的网站由一个员工、客户或者由你自己去编辑 —— 而且你也不想仅仅为了管理内容而去创建后台界面。

One typical workflow in creating Django apps is to create models and get the admin sites up and running as fast as possible, so your staff (or clients) can start populating data. Then, develop the way data is presented to the public.

创建Django应用的一个典型工作流程是创建模型然后尽快地让admin sites启动和运行起来， 这样您的员工（或客户）能够开始录入数据。 然后才开发展现数据给公众的界面。

#### **Design your URLs**

#### **设计你的URLs**

A clean, elegant URL scheme is an important detail in a high-quality Web application. Django encourages beautiful URL design and doesn’t put any cruft in URLs, like .php
 or .asp.

对于高质量的Web 应用来说，使用简洁、优雅的URL 模式是一个非常值得重视的细节。Django崇尚漂亮的URL设计且不会像.php或.asp一样把乱七八糟东西放到URLs里面.

To design URLs for an app, you create a Python module called a [URLconf](https://docs.djangoproject.com/en/1.10/topics/http/urls/). A table of contents for your app, it contains a simple mapping between URL patterns and Python callback functions. URLconfs also serve to decouple URLs from Python code.

为了给一个应用设计URLs，你需要创建一个叫做 [URLconf](https://docs.djangoproject.com/en/1.10/topics/http/urls/)的Python模块。
这其实是你应用的目录，它包含URL模式与Python回调函数间的一个简单映射。 URLconfs 还将Python代码与URLs解耦。

Here’s what a URLconf might look like for the Reporter
/Article example above:

上面提到的Reporter/Article例子，它的URLconf 可能像以下这样：

mysite/news/urls.py
```
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^articles/([0-9]{4})/$', views.year_archive),
    url(r'^articles/([0-9]{4})/([0-9]{2})/$', iews.month_archive),
    url(r'^articles/([0-9]{4})/([0-9]{2})/([0-9]+)/$', views.article_detail),
]
```

The code above maps URLs, as simple [regular expressions](https://docs.python.org/3/howto/regex.html#regex-howto), to the location of Python callback functions (“views”). The regular expressions use parenthesis to “capture” values from the URLs. When a user requests a page, Django runs through each pattern, in order, and stops at the first one that matches the requested URL. (If none of them atches, Django calls a special-case 404 view.) This is blazingly fast, because the regular expressions are compiled at load time.

上面的代码将简单的[正则表达式](https://docs.python.org/howto/regex.html)组成的URLs，映射到Python的回调函数（视图）。正则表达式通过圆括号来“捕获”URLs中的值。 当一个用户请求一个页面时，Django将按照顺序去匹配每一个模式，直到匹配到第一个为止。 
（如果没有匹配到， Django将返回404状态码。）整个过程是极快的，因为正则表达式在加载时就已经编译好了。

Once one of the regexes matches, Django imports and calls the given view, which is a simple Python function. Each view gets passed a request object – which contains request metadata – and the values captured in the regex. For example, if a user requested the URL “/articles/2005/05/39323/”, Django would call the function news.views.article_detail(request, '2005', '05', '39323').

一旦有一个正则表达式匹配上了，Django 将导入和调用对应的视图，它其实就是一个简单的Python函数。 每个视图将得到一个request对象 —— 它包含了request 的metadata(元数据) —— 和正则表达式所捕获到的值。例如，如果一个用户请求了URL “/articles/2005/05/39323/”，Django将调用函数news.views.article_detail(request, '2005', '05', '39323')。

#### **Write your views**

#### **编写你的view函数**
Each view is responsible for doing one of two things: Returning an [HttpResponse](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponse) object containing the content for the requested page, or raising an exception such as [Http404](https://docs.djangoproject.com/en/1.10/topics/http/views/#django.http.Http404). The rest is up to you.

每个视图只负责两件事中的一件：返回一个包含请求的页面内容的 [HttpResponse
](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponse)对象， 或抛出一个异常如[Http404](https://docs.djangoproject.com/en/1.10/topics/http/views/#django.http.Http404)。 剩下的则取决于你。

Generally, a view retrieves data according to the parameters, loads a template and renders the template with the retrieved data. Here’s an example view for year_archive
 from above:

通常，一个视图会根据参数来检索数据、加载一个模板然后使用检索出来的数据渲染模板。下面是上文提到的year_archive的视图例子：
mysite/news/views.py
```
from django.shortcuts import render

from .models import Article

def year_archive(request, year):
    a_list = Article.objects.filter(pub_date__year=year)
    context = {'year': year, 'article_list': a_list}
    return render(request, 'news/year_archive.html', context)
```

This example uses Django’s [template system](https://docs.djangoproject.com/en/1.10/topics/templates/), which has several powerful features but strives to stay simple enough for non-programmers to use.

这个例子使用了Django的[模板系统](https://docs.djangoproject.com/en/1.10/topics/templates/)，它功能强大，但即使没有编程经验，使用起来也足够简单。

#### **Design your templates**

#### **设计你的模板**

The code above loads the news/year_archive.html
 template.

上面的代码加载了news/year_archive.html模板

Django has a template search path, which allows you to minimize redundancy among templates. In your Django settings, you specify a list of directories to check for templates with [DIRS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-TEMPLATES-DIRS). If a template doesn’t exist in the first directory, it checks the second, and so on.

Django有一个模板搜索路径，它让你最小化模板之间的冗余代码。 在Django的settings设置中，你可以通过[DIRS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-TEMPLATES-DIRS)指定一个查找模板的目录列表。
如果这个模板没有在第一个目录中，那么它会去查找第二个，以此类推。

Let’s say the news/year_archive.html template was found. Here’s what that might look like:

我们假设news/year_archive.html模板已经找到。它看起来可能是下面这个样子：

mysite/news/templates/news/year_archive.html

```
{% extends "base.html" %}

{% block title %}Articles for {{ year }}{% endblock %}

{% block content %}
<h1>Articles for {{ year }}</h1>

{% for article in article_list %}
    <p>{{ article.headline }}</p>
    <p>By {{ article.reporter.full_name }}</p>
    <p>Published {{ article.pub_date|date:"F j, Y" }}</p>
{% endfor %}
{% endblock %}

```

Variables are surrounded by double-curly braces.  {% raw %}{{ article.headline }}{% endraw %} means “Output the value of the article’s headline attribute.” But dots aren’t used only for attribute lookup. They also can do dictionary-key lookup, index lookup and function calls.

变量使用两对大括号包围。  {% raw %}{{ article.headline }}{% endraw %}表示“输出 article的headline属性”。但是点符号不仅用于属性查找。
它们还用于字典的键值查找、索引查找和函数调用。

Note  {% raw %}{{ article.pub_date|date:"F j, Y" }}{% endraw %} uses a Unix-style “pipe” (the “|” character). This is called a template filter, and it’s a way to filter the value of a variable. In this case, the date filter formats a Python datetime object in the given format (as found in PHP’s date function).

注意 {% raw %}{{ article.pub_date|date:"F j, Y" }}{% endraw %}使用Unix风格的“管道”（“|”字符）。这叫做模板过滤器，它是过滤变量值的一种方式。在本例中，date过滤器用于格式化Python的datetime对象（正如在PHP中日期函数）。

You can chain together as many filters as you’d like. You can write [custom template filters](https://docs.djangoproject.com/en/1.10/howto/custom-template-tags/#howto-writing-custom-template-filters). You can write [custom template tags](https://docs.djangoproject.com/en/1.10/howto/custom-template-tags/), which run custom Python code behind the scenes.

你可以无限制地串联使用多个过滤器。 你也可以编写[*自定义的目标过滤器*](https://docs.djangoproject.com/en/1.10/howto/custom-template-tags/#howto-writing-custom-template-filters)，你也可以编写[自定义tag](https://docs.djangoproject.com/en/1.10/howto/custom-template-tags/)，tag可以模板里执行定制化的python代码。

Finally, Django uses the concept of “template inheritance”. That’s what the {% raw %}{% extends "base.html" %}{% endraw %} does. It means “First load the template called ‘base’, which has defined a bunch of blocks, and fill the blocks with the following blocks.” In short, that lets you dramatically cut down on redundancy in templates: each template has to define only what’s unique to that template.

最后，Django秉承“模板继承”的理念。这就是{% raw %}{% extends "base.html" %}{% endraw %}所做的事。它表示“首先载入‘base’ 模板，该模板中定义了一系列block，然后使用接下来的（存在于继承模板）blocks填充这些blocks”。简而言之，模板继承让你大大减少模板间的冗余代码：每个模板只需要定义它单独的部分。

Here’s what the “base.html” template, including the use of [static files](https://docs.djangoproject.com/en/1.10/howto/static-files/), might look like:

下面是“base.html”模板可能的样子，它使用了[*静态文件*](https://docs.djangoproject.com/en/1.10/howto/static-files/)

mysite/templates/base.html
```
{% load static %}
<html>
<head>
    <title>{% block title %}{% endblock %}</title>
</head>
<body>
    ![]({% static )
    {% block content %}{% endblock %}
</body>
</html>
```

Simplistically, it defines the look-and-feel of the site (with the site’s logo), and provides “holes” for child templates to fill. This makes a site redesign as easy as changing a single file – the base template.

简单地说，它定义网站的外观（包括网站的logo ），并提供“空缺”让子模板填充。这使站点的重构变得非常容易，只需改变一个文件 —— base模板。

It also lets you create multiple versions of a site, with different base templates, while reusing child templates. Django’s creators have used this technique to create strikingly different mobile versions of sites – simply by creating a new base template.

它还可以让你利用不同的基础模板并重用子模板创建一个网站的多个版本。Django 的创建者已经利用这一技术来创造了显著不同的手机版本的网站 —— 只需创建一个新的基础模板。

Note that you don’t have to use Django’s template system if you prefer another system. While Django’s template system is particularly well-integrated with Django’s model layer, nothing forces you to use it. For that matter, you don’t have to use Django’s database API, either. You can use another database abstraction layer, you can read XML files, you can read files off disk, or anything you want. Each piece of Django – models, views, templates – is decoupled from the next.

请注意，如果你喜欢其它模板系统，你可以不使用Django的模板系统。  虽然Django的模板系统与Django的模型层集成得特别好，但并没有强制你使用它。同理，你也可以不使用Django的数据库API。 你可以使用其它数据库抽象层，你可以读取 XML 文件，你可以从磁盘中读取文件，或任何你想要的方法去操作数据。Django的每个组成部分 —— 模型、视图和模板都可以解耦。

#### **This is just the surface**

This has been only a quick overview of Django’s functionality. Some more useful features:

+ A [caching framework](https://docs.djangoproject.com/en/1.10/topics/cache/) that integrates with memcached or other backends.
+ A [syndication framework](https://docs.djangoproject.com/en/1.10/ref/contrib/syndication/) that makes creating RSS and Atom feeds as easy as writing a small Python class.
+ More sexy automatically-generated admin features – this overview barely scratched the surface.

The next obvious steps are for you to [download Django](https://www.djangoproject.com/download/), read [the tutorial](https://docs.djangoproject.com/en/1.10/intro/tutorial01/) and join [the community](https://www.djangoproject.com/community/). Thanks for your interest!

#### **这仅仅是入门知识**

这里只是Django功能的一个快速概览。以下是一些更有用的功能：

+ [缓存框架](https://docs.djangoproject.com/en/1.10/topics/cache/) 可以与memcached或其它后端集成。

+ [聚合框架](https://docs.djangoproject.com/en/1.10/ref/contrib/syndication/)可以让创建RSS和Atom的 feeds 像写一个小小的 Python 类一样容易。

+ 更令人激动的是自动创建的站点管理功能 —— 本文仅仅触及了点皮毛。

很明显，下一步你应该做的是[下载Django](https://docs.djangoproject.com/en/1.10/ref/contrib/syndication/)、阅读[教程](https://docs.djangoproject.com/en/1.10/intro/tutorial01/)，并加入[社区](https://www.djangoproject.com/community/)。感谢您的关注！