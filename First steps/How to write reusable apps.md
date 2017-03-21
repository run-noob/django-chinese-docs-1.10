This advanced tutorial begins where [Tutorial 7](https://docs.djangoproject.com/en/1.10/intro/tutorial07/) left off. We’ll be turning our Web-poll into a standalone Python package you can reuse in new projects and share with other people.

紧接着[教程7](https://run-noob.gitbooks.io/django-chinese-docs-1-10/content/First%20steps/Writing%20your%20%EF%AC%81rst%20Django%20app%20part%207.html)，从这只后我们开始高级教程部分。我们将投票应用做成一个单独的python包，你可以在新项目使用，或分享给其他人。

If you haven’t recently completed Tutorials 1–7, we encourage you to review these so that your example project matches the one described below.

如果你还未完成教程1-7，我们建议你完善它们，使你的示例项目与下面描述的一致

#### **Reusability matters[¶](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/#reusability-matters)**

#### **可重用很重要[¶](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/#reusability-matters)**

It’s a lot of work to design, build, test and maintain a web application. Many Python and Django projects share common problems. Wouldn’t it be great if we could save some of this repeated work?

设计、构建、测试和维护一个网页应用需要做许多工作。许多Python 和 Django 项目都会遇到一些共性问题。如果我们能不重复的造轮子，岂不是很棒。

Reusability is the way of life in Python. [The Python Package Index (PyPI)](https://pypi.python.org/pypi) has a vast range of packages you can use in your own Python programs. Check out [Django Packages](https://www.djangopackages.com/) for existing reusable apps you could incorporate in your project. Django itself is also just a Python package. This means that you can take existing Python packages or Django apps and compose them into your own web project. You only need to write the parts that make your project unique.

可重用性在Python 中是一种常见的方式。
[Python包索引 (PyPI)](https://pypi.python.org/pypi)网站 具有大量的包，你可以在你自己的Python程序中使用。
查阅一下[Django Packages](https://www.djangopackages.com/)中已经存在的可重用的应用，你可以把它们放到你的项目中。
Django 自身也只是一个Python 包。
这意味着你可以获取已经存在的Python包和Django应用并将它们融合到你自己的网页项目。你只需要编写你项目的独有的部分代码。

Let’s say you were starting a new project that needed a polls app like the one we’ve been working on. How do you make this app reusable? Luckily, you’re well on the way already. In [Tutorial 3](https://docs.djangoproject.com/en/1.10/intro/tutorial03/), we saw how we could decouple polls from the project-level URLconf using an include. In this tutorial, we’ll take further steps to make the app easy to use in new projects and ready to publish for others to install and use.

假如说你正在开始一个新的项目，需要一个像我们正在编写的投票应用。你如何让该应用可重用？幸运的是，其实你已经在这条道路上了。在[*教程 3*](https://run-noob.gitbooks.io/django-chinese-docs-1-10/content/First%20steps/Writing%20your%20%EF%AC%81rst%20Django%20app%20part%203.html)中，我们看到我们可以如何使用include将投票应用从项目级别的URLconf 解耦。在本教程中，我们将进一步让你的应用更容易复用到新的项目中，而且可以发布给其它人安装和使用。

#### **Package? App?**

A Python [package](https://docs.python.org/3/glossary.html#term-package) provides a way of grouping related Python code for easy reuse. A package contains one or more files of Python code (also known as “modules”).

Python [package](https://docs.python.org/3/glossary.html#term-package)提供了可让一组相关性代码复用的方式，一个包包含一个或许多python文件（也就是我们常说的模块）

A package can be imported with import foo.bar or from foo import bar. For a directory (like polls) to form a package, it must contain a special file __init__.py, even if this file is empty.

一个包可以通过import foo.bar or from foo import bar方法导入。如果是一个目录（如polls）要做成一个包，必需要包含一个特殊文件__init__.py，即使这个文件是空。

A Django *application* is just a Python package that is specifically intended for use in a Django project. An application may use common Django conventions, such as having models, tests, urls, and views submodules.

一个Django应用就是仅仅用于Django项目中Python包。一个应用可以遵循常见的Django 约定，例如具有models、tests、urls和views 子模块。

Later on we use the term *packaging* to describe the process of making a Python package easy for others to install. It can be a little confusing, we know.

将python包变的让其他人更易于安装的过程，我们称之为打包。目前对于你来说可能有点困惑。

#### **Your project and your reusable app[¶](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/#your-project-and-your-reusable-app)**

#### **你的项目以及可复用的app[¶](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/#your-project-and-your-reusable-app)**

After the previous tutorials, our project should look like this:

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
    polls/
        __init__.py
        admin.py
        migrations/
            __init__.py
            0001_initial.py
        models.py
        static/
            polls/
                images/
                    background.gif
                style.css
        templates/
            polls/
                detail.html
                index.html
                results.html
        tests.py
        urls.py
        views.py
    templates/
        admin/
            base_site.html
```

You created mysite/templates
 in [Tutorial 7](https://docs.djangoproject.com/en/1.10/intro/tutorial07/), and polls/templates in [Tutorial 3](https://docs.djangoproject.com/en/1.10/intro/tutorial03/). Now perhaps it is clearer why we chose to have separate template directories for the project and application: everything that is part of the polls application is in polls. It makes the application self-contained and easier to drop into a new project.

你在[*教程 2*](https://run-noob.gitbooks.io/django-chinese-docs-1-10/content/First%20steps/Writing%20your%20%EF%AC%81rst%20Django%20app%20part%202.html)中创建了mysite/templates
 ，在[*教程 3*](https://run-noob.gitbooks.io/django-chinese-docs-1-10/content/First%20steps/Writing%20your%20%EF%AC%81rst%20Django%20app%20part%203.html)中创建了polls/templates。 
现在你可能更加清晰为什么我们为项目和应用选择单独的模板目录：属于投票应用的部分全部在polls目录中。这使得该应用只含有自己的东西，而且更加容易放到一个新的项目中。

The polls directory could now be copied into a new Django project and immediately reused. It’s not quite ready to be published though. For that, we need to package the app to make it easy for others to install.

现在可以拷贝polls目录到一个新的Django项目并立即重用。尽管它还不能充分准备好到可以立即发布。所以，我们需要打包这个应用来让它对其他人易于安装。

#### **Installing some prerequisites[¶](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/#installing-some-prerequisites)**

#### **安装一些需要的东西[¶](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/#installing-some-prerequisites)**

The current state of Python packaging is a bit muddled with various tools. For this tutorial, we’re going to use [setuptools](https://pypi.python.org/pypi/setuptools) to build our package. It’s the recommended packaging tool (merged with the distribute
 fork). We’ll also be using [pip](https://pypi.python.org/pypi/pip) to install and uninstall it. You should install these two packages now. If you need help, you can refer to [how to install Django with pip](https://docs.djangoproject.com/en/1.10/topics/install/#installing-official-release). You can install setuptools the same way.

Python 目前有众多打包工具，非常混乱不堪。在本教程中，我们打算使用[setuptools](https://pypi.python.org/pypi/setuptools)来构建我们的包。它是推荐的打包工具（已经与distribute
 分支合并）。我们还将使用[pip](https://pypi.python.org/pypi/pip)来安装和卸载它。现在你应该安装这两个包。如果你需要帮助，你可以参考[how to install Django with pip](https://docs.djangoproject.com/en/1.10/topics/install/#installing-official-release)。你可以使用同样的方法安装setuptools

#### **Packaging your app[¶](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/#packaging-your-app)**

#### **打包你的应用[¶](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/#packaging-your-app)**

Python *packaging* refers to preparing your app in a specific format that can be easily installed and used. Django itself is packaged very much like this. For a small app like polls, this process isn’t too difficult.

Python 打包 会将你的应用按一定规则进行处理， 使其更易安装和使用。Django 也是以非常相似的方式打包起来的。对于一个像polls这样的小应用，这个过程不是太难。

1. First, create a parent directory for polls, outside of your Django project. Call this directory django-polls.

 首先，创建一个polls的父目录，在Django项目目录外。目录名为django-polls

> **Choosing a name for your app**

> When choosing a name for your package, check resources like PyPI to avoid naming conflicts with existing packages. It’s often useful to prepend django- to your module name when creating a package to distribute. This helps others looking for Django apps identify your app as Django specific.

当为你的包选择一个名字时，检查一下PyPI中的资源以避免与已经存在的包冲突。当创建一个要发布的包时，在你的模块名字前面加上django-通常很有用。 这有助于其他正在查找Django应用的人区分你的应用是专门用于Django的。

Application labels (that is, the final part of the dotted path to application packages) *must* be unique in [INSTALLED_APPS
](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-INSTALLED_APPS). Avoid using the same label as any of the Django [contrib packages](https://docs.djangoproject.com/en/1.10/ref/contrib/), for example auth, admin, or messages.

应用的标签（应用的包的点分路径的最后部分）在 [INSTALLED_APPS
](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-INSTALLED_APPS)中*必须*唯一。避免使用与Django的[*contrib 包*](http://python.usyiyi.cn/documents/django_182/ref/contrib/index.html) 中任何一个使用相同的标签，例如auth、admin和messages。

2. Move the polls directory into the django-polls directory.

将polls目录移动到django-polls

3. Create a file django-polls/README.rst with the following contents:

创建一个包含以下内容的文件django-polls/README.rst：

django-polls/README.rst

```
=====
Polls
=====

Polls is a simple Django app to conduct Web-based polls. For each
question, visitors can choose between a fixed number of answers.

Detailed documentation is in the "docs" directory.

Quick start
-----------

1. Add "polls" to your INSTALLED_APPS setting like this::

    INSTALLED_APPS = [
        ...
        'polls',
    ]

2. Include the polls URLconf in your project urls.py like this::

    url(r'^polls/', include('polls.urls')),

3. Run `python manage.py migrate` to create the polls models.

4. Start the development server and visit http://127.0.0.1:8000/admin/
   to create a poll (you'll need the Admin app enabled).

5. Visit http://127.0.0.1:8000/polls/ to participate in the poll.

```

4. Create a django-polls/LICENSE file. Choosing a license is beyond the scope of this tutorial, but suffice it to say that code released publicly without a license is *useless*. Django and many Django-compatible apps are distributed under the BSD license; however, you’re free to pick your own license. Just be aware that your licensing choice will affect who is able to use your code.

创建一个django-polls/LICENSE文件。如何选择License超出本教程的范围，但值得一说的是, 公开发布的代码如果没有License是毫无用处的。Django和许多与Django兼容的应用以BSD License 发布；然而，你可以随便挑选自己的License。只需知道你所选择的License,将决定谁能使用你的代码.

5. Next we’ll create a setup.py file which provides details about how to build and install the app. A full explanation of this file is beyond the scope of this tutorial, but the [setuptools docs](https://setuptools.readthedocs.io/en/latest/) have a good explanation. Create a file django-polls/setup.py with the following contents:

下一步我们将创建一个setup.py 文件，它包含了如何构建和安装该应用的详细信息。该文件的详细解说超出了本教程的范围，[setuptools 文档](http://pythonhosted.org/setuptools/setuptools.html) 已详细说明。
创建一个文件django-polls/setup.py，其内容如下：

django-polls/setup.py
```
import os
from setuptools import setup

with open(os.path.join(os.path.dirname(__file__), 'README.rst')) as readme:
    README = readme.read()

# allow setup.py to be run from any path
os.chdir(os.path.normpath(os.path.join(os.path.abspath(__file__), os.pardir)))

setup(
    name='django-polls',
    version='0.1',
    packages=['polls'],
    include_package_data=True,
    license='BSD License',  # example license
    description='A simple Django app to conduct Web-based polls.',
    long_description=README,
    url='http://www.example.com/',
    author='Your Name',
    author_email='yourname@example.com',
    classifiers=[
        'Environment :: Web Environment',
        'Framework :: Django',
        'Intended Audience :: Developers',
        'License :: OSI Approved :: BSD License', # example license
        'Operating System :: OS Independent',
        'Programming Language :: Python',
        # Replace these appropriately if you are stuck on Python 2.
        'Programming Language :: Python :: 3',
        'Programming Language :: Python :: 3.2',
        'Programming Language :: Python :: 3.3',
        'Topic :: Internet :: WWW/HTTP',
        'Topic :: Internet :: WWW/HTTP :: Dynamic Content',
    ],
)

```

6. Only Python modules and packages are included in the package by default. To include additional files, we’ll need to create a MANIFEST.in file. The setuptools docs referred to in the previous step discuss this file in more details. To include the templates, the README.rst and our LICENSE file, create a file django-polls/MANIFEST.in with the following contents:

默认只有Python模块和包会包含进包中。如果需要包含额外的文件，我们需要创建一个MANIFEST.in文件。上一步提到的setuptools 文档对这个文件有更详细的讨论。如果要包含模板、README.rst和我们的LICENSE 文件，创建一个文件django-polls/MANIFEST.in，其内容如下：

django-polls/MANIFEST.in
```
include LICENSE
include README.rst
recursive-include polls/static *
recursive-include polls/templates *
```

7. It’s optional, but recommended, to include detailed documentation with your app. Create an empty directory django-polls/docs for future documentation. Add an additional line to django-polls/MANIFEST.in:

包含你的应用的详细文档，虽然这是可选的，但是推荐你这样做。创建django-polls/docs目录用于保存将来的文档。然后再django-polls/MANIFEST.in增加一行：

```
recursive-include docs *
```
Note that the docs directory won’t be included in your package unless you add some files to it. Many Django apps also provide their documentation online through sites like [readthedocs.org](https://readthedocs.org/).

注意**docs**不会包含进你的包中除非你添加一些文件到它下面。
许多Django应用还通过类似[readthedocs.org](https://readthedocs.org/)这样的站点提供它们的在线文档.

8. Try building your package with python setup.py sdist
 (run from inside django-polls). This creates a directory called dist
 and builds your new package, django-polls-0.1.tar.gz.

通过命令python setup.py sdist（在django-polls目录内执行）来打包，这会创建一个dist目录并构建一个新包：django-polls-0.1.tar.gz。

For more information on packaging, see Python’s [Tutorial on Packaging and Distributing Projects](https://packaging.python.org/en/latest/distributing.html).

更多打包相关的信息请看[Tutorial on Packaging and Distributing Projects](https://packaging.python.org/en/latest/distributing.html).

#### **Using your own package[¶](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/#using-your-own-package)**

#### **使用你自己的包[¶](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/#using-your-own-package)**

Since we moved the polls directory out of the project, it’s no longer working. We’ll now fix this by installing our new django-polls package.

由于我们将polls目录移出了项目，它已无法正常使用。我们现在通过安装我们新构建的django-polls包来修复这个问题。

> **Installing as a user library**

> **作为用户级别的库安装**

The following steps install django-polls as a user library. Per-user installs have a lot of advantages over installing the package system-wide, such as being usable on systems where you don’t have administrator access as well as preventing the package from affecting system services and other users of the machine.

以下的步骤将安装django-polls 成某个用户的库。用户级别的安装比系统级别的安装有许多优点，例如将包运行在普通用户级别上不但不会影响系统服务还不会影响其他用户

Note that per-user installations can still affect the behavior of system tools that run as that user, so virtualenv is a more robust solution (see below).

注意根据用户的安装仍然可以影响以该用户身份运行的系统工具，所以virtualenv 是更健壮的解决办法（见下文）。

1. To install the package, use pip (you already [installed it](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/#installing-reusable-apps-prerequisites), right?):

用pip命令来安装包（你已经安装了，对吧？）

```
pip install --user django-polls/dist/django-polls-0.1.tar.gz
```

2. With luck, your Django project should now work correctly again. 
Run the server again to confirm this.
幸运的是，你的Django项目又重新正常工作了。你可以启动服务器来验证。

3. To uninstall the package, use pip:

用以下命令卸载这个包：
```
pip uninstall django-polls
```

#### **Publishing your app[¶](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/#publishing-your-app)**

#### **发布你的应用[¶](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/#publishing-your-app)**

Now that we’ve packaged and tested django-polls, it’s ready to share with the world! If this wasn’t just an example, you could now:

现在你已经打包并测试了django-polls，它已经准备好公开发布到全世界了！如果这不仅仅是例子，你现在可以：

* Email the package to a friend.

* 邮件发送给你的朋友

* Upload the package on your website.

* 上传到你的站点

* Post the package on a public repository, such as [the Python Package Index (PyPI)](https://pypi.python.org/pypi). [packaging.python.org](https://packaging.python.org/) has [a good tutorial](https://packaging.python.org/en/latest/distributing.html#uploading-your-project-to-pypi) for doing this.

提交到一个公开的仓库源。 [the Python Package Index (PyPI)](https://pypi.python.org/pypi). [packaging.python.org](https://packaging.python.org/) 有一个非常好的教程 [a good tutorial](https://packaging.python.org/en/latest/distributing.html#uploading-your-project-to-pypi) 

#### **Installing Python packages with virtualenv[¶](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/#installing-python-packages-with-virtualenv)**

#### **用virtualenv安装Python包[¶](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/#installing-python-packages-with-virtualenv)**
Earlier, we installed the polls app as a user library. This has some disadvantages:

前面，我们以用户级别安装了polls应用。这有一些不利的地方：

* Modifying the user libraries can affect other Python software on your system.

* 更改用户的库可能会影响到系统里其他Python程序

* You won’t be able to run multiple versions of this package (or others with the same name).

* 你不可能同时跑这个包的多个版本（或者其他同名的包）

Typically, these situations only arise once you’re maintaining several Django projects. When they do, the best solution is to use [virtualenv](https://virtualenv.pypa.io/). This tool allows you to maintain multiple isolated Python environments, each with its own copy of the libraries and package namespace.

当你维护几个Django项目时，这种情况会进程出现。当你碰到了这个问题，最好的解决办法是使用[virtualenv](http://www.virtualenv.org/)。这个工具允许你维护多个分离的Python环境，每个都具有它自己的库和包的命名空间。
