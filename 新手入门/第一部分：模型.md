

Let’s learn by example.

让我们通过例子来学习

Throughout this tutorial, we’ll walk you through the creation of a basic poll application.

通过本教程，我们将带着你创建一个基本的投票应用



It’ll consist of two parts:

A public site that lets people view polls and vote in them.

An admin site that lets you add, change, and delete polls.

它将包含两个部分：

一个公开的网站，可以让人们查看投票结果以及让他们投票

一个后台管理系统，可以添加，更新，和删除投票



We’ll assume you have [Django installed](https://docs.djangoproject.com/en/1.10/intro/install/) already. You can tell Django is installed and which version by running the following command:

假设你已经安装好了Django，你可以输入以下命令来验证和查看你所安装的版本：

```
python -m django --version
```

If Django is installed, you should see the version of your installation. If it isn’t, you’ll get an error telling “No module named django”.

如果已经安装Django，你将看到你安装的版本好。如果没有，将会报一个“No module named django”的错误



This tutorial is written for Django 1.10 and Python 3.4 or later. If the Django version doesn’t match, you can refer to the tutorial for your version of Django by using the version switcher at the bottom right corner of this page, or update Django to the newest version. If you are still using Python 2.7, you will need to adjust the code samples slightly, as described in comments.

本教程基于Django 1.10 以及 Python 3.4+版本编写。如果你的Django版本不符， 如果Django版本不符，可以通过当前页面右下角的版本转换器查看适用于你所使用的版本的Django教程，或者把Django升级到最新的版本。 如果你还在使用2.7版本的Python，你将需要按照注释中的内容稍微调整一下示例代码。

See [How to install Django](https://docs.djangoproject.com/en/1.10/topics/install/) for advice on how to remove older versions of Django and install a newer one.

关于如何卸载旧版本并安装新版本的Django，请参考[How to install Django](https://docs.djangoproject.com/en/1.10/topics/install/) 



+ Where to get help:

If you’re having trouble going through this tutorial, please post a message to [django-users](https://docs.djangoproject.com/en/1.10/internals/mailing-lists/#django-users-mailing-list) or drop by [#django on irc.freenode.net](irc://irc.freenode.net/django) to chat with other Django users who might be able to help.

+ 如何获得帮助：

在本教程中，你有任何问题，都可以发消息给[django 开发人员](https://docs.djangoproject.com/en/1.10/internals/mailing-lists/#django-users-mailing-list) 或者访问 [#django on irc.freenode.net](irc://irc.freenode.net/django)网站，与其他Django开发者交流寻求帮助。


### Creating a project

### 创建项目

If this is your first time using Django, you’ll have to take care of some initial setup. Namely, you’ll need to auto-generate some code that establishes a Django [project](https://docs.djangoproject.com/en/1.10/glossary/#term-project) – a collection of settings for an instance of Django, including database configuration, Django-specific options and application-specific settings.

From the command line, cd

 into a directory where you’d like to store your code, then run the following command:

如果这是你第一次使用Django，你需要注意一下初始化设置。换句话说，你需要使用命令行来自动创建一个Django[项目](https://docs.djangoproject.com/en/1.10/glossary/#term-project) --一个Django框架开发的网站，包括数据库设置，针对Django的配置以及针对app应用的配置。

在命令行中，cd到你想要报错你项目代码的目录，并执行一下命令：

```
django-admin startproject mysite
```



This will create a mysite

 directory in your current directory. If it didn’t work, see [Problems running django-admin](https://docs.djangoproject.com/en/1.10/faq/troubleshooting/#troubleshooting-django-admin).

这将会在你的当前目录下创建一个mysite目录。如果没有，请查看[Problems running django-admin](https://docs.djangoproject.com/en/1.10/faq/troubleshooting/#troubleshooting-django-admin).



+ **NOTE**

You’ll need to avoid naming projects after built-in Python or Django components. In particular, this means you should avoid using names like django (which will conflict with Django itself) or test (which conflicts with a built-in Python package).



+ **注意**

你应该避免使用Python或Django的内建变量来给项目起名。特别地，这意味着你不能用django（与Django自身冲突）或test（与内建的Python模块冲突）这类项目名



+ **Where should this code live?**

If your background is in plain old PHP (with no use of modern frameworks), you’re probably used to putting code under the Web server’s document root (in a place such as /var/www). With Django, you don’t do that. It’s not a good idea to put any of this Python code within your Web server’s document root, because it risks the possibility that people may be able to view your code over the Web. That’s not good for security.

Put your code in some directory outside of the document root, such as /home/mycode.



+ **代码应该存在哪里**

如果你有过普通老久的PHP开发背景（没有使用过现代流行框架），你可能会把代码保存到web服务器的文档根目录下（如/var/www）。但在Django里，请不要这样做。将任何的python代码放在web服务器根目录是有很大风险的，因为别人可能通过网站查看到你的源代码。这违背了安全原则。

将你的代码放置在Web服务器根目录以外的地方，例如/home/mycode。

Let’s look at what [startproject](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-startproject) created:

让我们看一下[startproject](http://www.usyiyi.cn/documents/django_182/ref/django-admin.html#django-admin-startproject)命令创建了哪些文件和目录：

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```



These files are:

+ The outer mysite/ root directory is just a container for your project. Its name doesn’t matter to Django; you can rename it to anything you like.

+ manage.py: A command-line utility that lets you interact with this Django project in various ways. You can read all the details about manage.py in [django-admin and manage.py](https://docs.djangoproject.com/en/1.10/ref/django-admin/).

+ The inner mysite/ directory is the actual Python package for your project. Its name is the Python package name you’ll need to use to import anything inside it (e.g. mysite.urls).

+ mysite/__init__.py: An empty file that tells Python that this directory should be considered a Python package. If you’re a Python beginner, read [more about packages](https://docs.python.org/3/tutorial/modules.html#tut-packages) in the official Python docs.

+ mysite/settings.py: Settings/configuration for this Django project. [Django settings](https://docs.djangoproject.com/en/1.10/topics/settings/) will tell you all about how settings work.

+ mysite/urls.py: The URL declarations for this Django project; a “table of contents” of your Django-powered site. You can read more about URLs in [URL dispatcher](https://docs.djangoproject.com/en/1.10/topics/http/urls/).

+ mysite/wsgi.py: An entry-point for WSGI-compatible web servers to serve your project. See [How to deploy with WSGI](https://docs.djangoproject.com/en/1.10/howto/deployment/wsgi/)for more details.



这些文件是：

+ 外层的mysite/根目录仅仅是一个项目容器。它的命名对Django无关，你可以把它重命名为任何你喜欢的名字。

+ manage.py：一个命令行工具，可以使你用多种方式对Django项目进行交互。你可以在[*django-admin和manage.py*](https://docs.djangoproject.com/en/1.10/ref/django-admin/)中读到关于manage.py的所有细节。

+ 内层的mysite/目录是你的项目的真正的Python包。它是你导入任何东西时将需要使用的Python包的名字（例如 mysite.urls。

+ mysite/__init__.py：一个空文件，它告诉Python这个目录应该被看做一个Python包。如果你是一个Python初学者，[关于包的更多内容](https://docs.python.org/tutorial/modules.html#packages)请阅读Python的官方文档）。

+ mysite/settings.py：该Django 项目的设置/配置。[*Django 设置*](https://docs.djangoproject.com/en/1.10/topics/settings/) 将告诉你这些设置如何工作。

+ mysite/urls.py：该Django项目的URL声明；你的Django站点的“目录”。 

你可以在[*URL 转发器*](https://docs.djangoproject.com/en/1.10/topics/http/urls/) 中阅读到关于URL的更多内容。

+ mysite/wsgi.py：用于你的项目的与WSGI兼容的Web服务器入口。 

更多细节请参见[*如何利用WSGI进行部署*](https://docs.djangoproject.com/en/1.10/howto/deployment/wsgi/)。

**
The development server**
**开发服务器**
Let’s verify your Django project works. Change into the outer mysite directory, if you haven’t already, and run the following commands:
让我们来验证一下你的django项目是否正常。进入外层的mysite目录，如果你还


```
python manage.py runserver
```


You’ll see the following output on the command line:

你将在命令行看到如下输出：

```
Performing system checks...
System check identified no issues (0 silenced).
You have unapplied migrations; your app may not work properly until they are applied.Run 'python manage.py migrate' to apply them.
March 01, 2017 - 15:50:53
Django version 1.10, using settings 'mysite.settings'Starting development server at [http://127.0.0.1:8000/](http://127.0.0.1:8000/)
Quit the server with CONTROL-C.
```

     
+ Note

Ignore the warning about unapplied database migrations for now; we’ll deal with the database shortly.

+ 注意

暂时忽略关于未应用的数据库迁移，我们很快会讲到database

You’ve started the Django development server, a lightweight Web server written purely in Python. We’ve included this with Django so you can develop things rapidly, without having to deal with configuring a production server – such as Apache – until you’re ready for production.

你已经启动了一个Django开发服务器，一个轻量级的纯python写的web服务器。我们集成在了Django内，这样你就可以快速的开发产品，直到你准备好上线前都无需配置生产服务器--比如Apache

Now’s a good time to note: **don’t** use this server in anything resembling a production environment. It’s intended only for use while developing. (We’re in the business of making Web frameworks, not Web servers.)

Now that the server’s running, visit [http://127.0.0.1:8000/](http://127.0.0.1:8000/) with your Web browser. You’ll see a “Welcome to Django” page, in pleasant, light-blue pastel. It worked!

友情提示：千万不要在类生产服务器使用，它仅能在开发中使用。（我们重点是开发web框架，而不是web服务器）

服务器运行后，浏览器访问[http://127.0.0.1:8000/](http://127.0.0.1:8000/) ，你可以看到令人赏星悦目的淡蓝色的Django欢迎页面，它工作了。

+ **Changing the port**

By default, the [runserver](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-runserver) command starts the development server on the internal IP at port 8000.

If you want to change the server’s port, pass it as a command-line argument. For instance, this command starts the server on port 8080:

+ **更改端口**

默认默认情况下，[runserver](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-runserver) 命令启动的开发服务器运行在内部IP的8000端口。如果你想更改端口，需要加个参数，比如说，下面这个命令开启了8080端口

```
python manage.py runserver 8080
```

If you want to change the server’s IP, pass it along with the port. So to listen on all public IPs (useful if you want to show off your work on other computers on your network), use:

如果你想更改绑定的服务IP，可把IP和端口一起作为参数。因此若要监听所有外网IP（如果你想在局域网内与其他人开发同一站点的话，该功能非常有用），输入命令：

```
python manage.py runserver 0.0.0.0:8000
```

Full docs for the development server can be found in the [runserver](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-runserver) reference.

开发服务器的所有文档可以在[runserver](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-runserver)的参考手册中找到。

+ **Automatic reloading of [runserver](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-runserver)**

The development server automatically reloads Python code for each request as needed. You don’t need to restart the server for code changes to take effect. However, some actions like adding files don’t trigger a restart, so you’ll have to restart the server in these cases.

+ **[runserver](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-runserver)的自动重载**

开发服务器会根据需要自动重新载入Python代码。 你不必为了使更改的代码生效而重启服务器。 然而，一些行为比如添加文件，不会触发服务器的重启，所以在这种情况下你需要手动重启服务器。


### 创建投票应用

Now that your environment – a “project” – is set up, you’re set to start doing work.

现在你已经在你的开发环境新建了一个项目，你可以在上面开始工作了

Each application you write in Django consists of a Python package that follows a certain convention. Django comes with a utility that automatically generates the basic directory structure of an app, so you can focus on writing code rather than creating directories.

你创建的每一个django应用，都必须包含一个约定俗成的python包。Django自带了一个自动创建app目录结构的工具，这样你可以专心码代码而不是去创建那些目录

+ **Projects vs. apps**

What’s the difference between a project and an app? An app is a Web application that does something – e.g., a Weblog system, a database of public records or a simple poll app. A project is a collection of configuration and apps for a particular website. A project can contain multiple apps. An app can be in multiple projects.

项目和应用之间有什么区别呢？一个应用是一个web程序，它负责特定的工作--比如说一个博客系统，一个存储公共文档的数据库系统或者一个简单的投票应用。而项目是一个网站配置和应用的集合。项目可以保护多个应用，一个应用也可以从属于多个项目

Your apps can live anywhere on your [Python path](https://docs.python.org/3/tutorial/modules.html#tut-searchpath). In this tutorial, we’ll create our poll app right next to your manage.py file so that it can be imported as its own top-level module, rather than a submodule of mysite.
To create your app, make sure you’re in the same directory as manage.py and type this command:

你的应用可以放在[Python path](https://docs.python.org/3/tutorial/modules.html#tut-searchpath)的任何位置。在本教程，我们将投票应用放在manage.py文件同级目录下，这样就可以作为顶层模块导入，而不是mysite的子模块。进入到manage.py的同级目录下，输入一下命令：


```
python manage.py startapp polls
```

That’ll create a directory polls, which is laid out like this:

命令将会创建一个polls目录，结构如下：
```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```


This directory structure will house the poll application.

这个目录就结构是投票应用的框架了

**Write your first view**

Let’s write the first view. Open the file polls/views.py and put the following Python code in it:

让我们开始写第一个view函数，打开polls/views.py,输入以下代码：


```
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```


This is the simplest view possible in Django. To call the view, we need to map it to a URL - and for this we need a URLconf.

To create a URLconf in the polls directory, create a file called urls.py. Your app directory should now look like:

这是Django里最简单的view函数。为了调用该函数，我们需要编辑URLconf.在polls目录创建URLconf，也就是创建一个名为urls.py文件。这时你的目录结构看起来像这样：


```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    urls.py
    views.py
```

In the polls/urls.py file include the following code:


```
from django.conf.urls import url

from . import views

urlpatterns = [
    url(r'^$', views.index, name='index'),
]
```

The next step is to point the root URLconf at the polls.urls module. In mysite/urls.py, add an import for django.conf.urls.include and insert an include() in the urlpatterns list, so you have:

下一步就是让项目主urls文件指向polls.urls这个模块。在mysite/urls.py，导入django.conf.urls.include模块，并且添加到urlpatterns列表，所以mysite/urls.py如下：


```

# mysite/urls.py
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^polls/', include('polls.urls')),
    url(r'^admin/', admin.site.urls),
]
```


The include() function allows referencing other URLconfs. Note that the regular expressions for the include() function doesn’t have a $ (end-of-string match character) but rather a trailing slash. Whenever Django encounters include(), it chops off whatever part of the URL matched up to that point and sends the remaining string to the included URLconf for further processing.

include()函数可以关联其他的URLconfs。注意include()函数前的正则表达式没有$符(末尾匹配)，取而代之的是斜杠。Django在遇到include()函数后，会截断正则表达式匹配到的字符串，然后将剩下的url传递到所incloude的URLconf进一步处理。

The idea behind include() is to make it easy to plug-and-play URLs. Since polls are in their own URLconf (polls/urls.py), they can be placed under “/polls/”, or under “/fun_polls/”, or under “/content/polls/”, or any other path root, and the app will still work.

include()的设计思想是方便即插即用。由于polls在它自己的URLconf(polls/urls.py)，你可以用“/polls/”, “/fun_polls/”, “/content/polls/”或者其他任意主目录随意替换，它仍可以正常工作。


> **When to use include()**

> You should always use include() when you include other URL patterns.admin.site.urls is the only exception to this.

> 当你需要引入其他url时，尽量使用include()，唯一的特例是patterns.admin.site.urls

> **Doesn’t match what you see?**

> If you’re seeing include(admin.site.urls) instead of just admin.site.urls, you’re probably using a version of Django that doesn’t match this tutorial version. You’ll want to either switch to the older tutorial or the newer Django version.

> 如果你看到的是include(admin.site.urls)，而不是admin.site.urls，那你的Django版本可能与本教程不符。你应该切换到旧的教程教程或

You have now wired an index view into the URLconf. Lets verify it’s working, run the following command:

你已经将view函数index()映射到URLconf，让我们来验证它能否正常工作，运行如下命令：

```
python manage.py runserver

```


Go to [http://localhost:8000/polls/](http://localhost:8000/polls/) in your browser, and you should see the text “*Hello, world. You’re at the polls index.*”, which you defined in the index
 view.

The [url()](https://docs.djangoproject.com/en/1.10/ref/urls/#django.conf.urls.url) function is passed four arguments, two required: regex and view, and two optional: kwargs, and name. At this point, it’s worth reviewing what these arguments are for.

在你的浏览器访问[http://localhost:8000/polls/](http://localhost:8000/polls/)，你将看到“*Hello, world. You’re at the polls index.*”，这就是我们刚刚在index view函数定义的。

[url()](https://docs.djangoproject.com/en/1.10/ref/urls/#django.conf.urls.url)函数需要四个参数，两个必需参数：**regex**和**view**，两个可选参数：**Kwargs**和**name**。我们有必要详细了解下这些参数。

**
[url()](https://docs.djangoproject.com/en/1.10/ref/urls/#django.conf.urls.url) argument: regex**

The term “regex” is a commonly used short form meaning “regular expression”, which is a syntax for matching patterns in strings, or in this case, url patterns. Django starts at the first regular expression and makes its way down the list, comparing the requested URL against each regular expression until it finds one that matches.

“正则”是正则表达式的通用缩写，它是一种匹配字符串或url地址的语法。在这里就是指url patterns。Django拿着用户请求的url地址，在urls.py文件中对urlpatterns列表中的每一项条目从头开始进行逐一对比，直到匹配为止。

Note that these regular expressions do not search GET and POST parameters, or the domain name. For example, in a request to https://www.example.com/myapp/, the URLconf will look for myapp/. In a request to https://www.example.com/myapp/?page=3, the URLconf will also look for myapp/.

需要注意的是，regex不会去匹配GET或POST参数或域名，例如对于https://www.example.com/myapp/， regex只尝试匹配myapp/。对于https://www.example.com/myapp/?page=3,  regex也只尝试匹配myapp/。

If you need help with regular expressions, see [Wikipedia’s entry](https://en.wikipedia.org/wiki/Regular_expression) and the documentation of the [re](https://docs.python.org/3/library/re.html#module-re) module. Also, the O’Reilly book “Mastering Regular Expressions” by Jeffrey Friedl is fantastic. In practice, however, you don’t need to be an expert on regular expressions, as you really only need to know how to capture simple patterns. In fact, complex regexes can have poor lookup performance, so you probably shouldn’t rely on the full power of regexes.

如果你想深入研究正则表达式，可以参考[Wikipedia’s entry](https://en.wikipedia.org/wiki/Regular_expression) 以及 [re](https://docs.python.org/3/library/re.html#module-re) 模块的文档，由作者Jeffrey Friedl出版的O’Reilly “Mastering Regular Expressions”书籍也非常不错。但是实践中，你不需要多高深的正则表达式知识，因为你只需要知道如何匹配简单的规则。事实上，复杂的正则表达式查询性能会比较差，所以尽可能不用正则的复杂功能


Finally, a performance note: these regular expressions are compiled the first time the URLconf module is loaded. They’re super fast (as long as the lookups aren’t too complex as noted above).

最后，关于性能问题：正则表达式会进行预先编译当URLconf模块加载的时候，因此它的匹配搜索速度非常快，你通常感觉不到。


**[url()](https://docs.djangoproject.com/en/1.10/ref/urls/#django.conf.urls.url) argument: view**

When Django finds a regular expression match, Django calls the specified view function, with an [HttpRequest](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpRequest) object as the first argument and any “captured” values from the regular expression as other arguments. If the regex uses simple captures, values are passed as positional arguments; if it uses named captures, values are passed as keyword arguments. We’ll give an example of this in a bit.

当正则表达式匹配到某个条目时，自动将封装的HttpRequest对象作为第一个参数，正则表达式“捕获”到的值作为其他参数，传递给该条目指定的视图。如果是简单捕获，那么捕获值将作为一个位置参数进行传递，如果是命名捕获，那么将作为关键字参数进行传递。我们会举例说明这点

[url()](https://docs.djangoproject.com/en/1.10/ref/urls/#django.conf.urls.url) argument: kwargs

Arbitrary keyword arguments can be passed in a dictionary to the target view. We aren’t going to use this feature of Django in the tutorial.

任意数量的关键字参数可以作为一个字典传递给目标视图。在本教程中，我们不会用到它

**[url()](https://docs.djangoproject.com/en/1.10/ref/urls/#django.conf.urls.url) argument: name**

Naming your URL lets you refer to it unambiguously from elsewhere in Django, especially from within templates. This powerful feature allows you to make global changes to the URL patterns of your project while only touching a single file.
When you’re comfortable with the basic request and response flow, read [part 2 of this tutorial](https://docs.djangoproject.com/en/1.10/intro/tutorial02/)to start working with the database.

对你的URL进行命名，可以让你能够在Django的任意处，尤其是模板内显式地引用它。这个强大的功能，仅仅通过创建一个文件就可以让你在项目里任意修改URL patterns这个全局变量的值。如果你已经掌握了基本的request和response的过程，那就阅读[教程第二部分](https://docs.djangoproject.com/en/1.10/intro/tutorial02/)数据库相关