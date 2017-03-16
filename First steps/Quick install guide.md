### **Quick install guide**

Before you can use Django, you’ll need to get it installed. We have a complete installation guide that covers all the possibilities; this guide will guide you to a simple, minimal installation that’ll work while you walk through the introduction.

### **快速安装指导**

在你开始使用Django之前，你需要先安装它。我们有一个完整安装指南，它涵盖了有的安装步骤和可能遇到的问题。通过阅读本指南，它会引导你安装一个简单，且最小化的Django。


### **Install Python**

### **安装Python**

Being a Python Web framework, Django requires Python. See What Python version can I use with Django? for details. Python includes a lightweight database called SQLite so you won’t need to set up a database just yet.

作为一个python版的Web框架，Django需要使用Python环境。具体信息请看[Django所使用的python版本](https://docs.djangoproject.com/en/1.10/faq/install/#faq-python-version-support)所有Python版本都包含一个轻量级的数据库名叫SQLite。因此你暂时不需要建立一个数据库。 

Get the latest version of Python at https://www.python.org/download/ or with your operating system’s package manager.

你可以在[https://www.python.org/download/](https://www.python.org/download/)找到最新的python或用你操作系统带的包管理工具。

> Django on Jython

> If you use Jython (a Python implementation for the Java platform), you’ll need to follow a few additional steps. See Running Django on Jython for details.
> 
> Django 与 Jython
> 如果你使用的是[Jython](http://www.jython.org/)（一个用java写的python），那你需要完成一些其他的步骤。具体请看[Jython版的Django](https://docs.djangoproject.com/en/1.10/howto/jython/)


You can verify that Python is installed by typing python from your shell; you should see something like:

你可以通过在shell上执行以下命令来验证Python是否安装；你应该可以看到如下：

```
Python 3.4.x
[GCC 4.x] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

#### ** Set up a database**

#### **配置数据库**

This step is only necessary if you’d like to work with a “large” database engine like PostgreSQL, MySQL, or Oracle. To install such a database, consult the [database installation information](https://docs.djangoproject.com/en/1.10/topics/install/#database-installation).


这一步仅当你需要安装一个大型数据库PostgreSQL, MySQL, or Oracle。安装这些数据库请看[database installation information](https://docs.djangoproject.com/en/1.10/topics/install/#database-installation).

#### **Remove any old versions of Django**

#### **卸载旧版本Django**

If you are upgrading your installation of Django from a previous version, you will need to [uninstall the old Django version before installing the new version](https://docs.djangoproject.com/en/1.10/topics/install/#removing-old-versions-of-django).

如果你要升级Django版本，请注意查看[安装新Django之前请先卸载旧版本](https://docs.djangoproject.com/en/1.10/topics/install/#removing-old-versions-of-django).

#### **Install Django**


You’ve got three easy options to install Django:
+ [Install an official release](https://docs.djangoproject.com/en/1.10/topics/install/#installing-official-release). This is the best approach for most users.
+ Install a version of Django [provided by your operating system distribution](https://docs.djangoproject.com/en/1.10/topics/install/#installing-distribution-package).
+ [Install the latest development version](https://docs.djangoproject.com/en/1.10/topics/install/#installing-development-version). This option is for enthusiasts who want the latest-and-greatest features and aren’t afraid of running brand new code. You might encounter new bugs in the development version, but reporting them helps the development of Django. Also, releases of third-party packages are less likely to be compatible with the development version than with the latest stable release.

#### **安装 Django**

你可以通过以下三种简单的方式来安装Django：

+ [安装官方发布版本](https://docs.djangoproject.com/en/1.10/topics/install/#installing-official-release)，对于大多数人来说，这是最好的方法

+ 安装[你的操作系统定制的发行包](https://docs.djangoproject.com/en/1.10/topics/install/#installing-development-version)，

+ [安装最新的研发版本](https://docs.djangoproject.com/en/1.10/topics/install/#installing-official-release)，这是为那些想要尝试最新最棒的特性而不担心运行崭新代码的Django发烧友准备的，用开发版本你可能会遇到新bug，请上报这些bug来以帮助Django更好的开发。而且，第三方包的兼容性可能会比 旧版本的要差一些。


> Always refer to the documentation that corresponds to the version of Django you’re using!

>If you do either of the first two steps, keep an eye out for parts of the documentation marked **new in development version**. That phrase flags features that are only available in development versions of Django, and they likely won’t work with an official release.

> **总是参考你所使用的对应版本的Django文档
> 如果你采用的是前两种方式安装的，请注意在文档中**new in development version**标记。这个标记表明这个特性仅适用开发版的Django，在官方版本中可能不适用。


#### **Verifying**
To verify that Django can be seen by Python, type python
 from your shell. Then at the Python prompt, try to import Django:

```
>>> import django
>>> print(django.get_version())1.10
You may have another version of Django installed.
```

#### **That’s it!**

安装就到这了 – 现在请移步至[入门教程](https://docs.djangoproject.com/en/1.10/intro/tutorial01/).


