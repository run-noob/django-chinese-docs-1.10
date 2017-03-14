### **Writing your first Django app, part 3**[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial03/#writing-your-first-django-app-part-3)

This tutorial begins where [Tutorial 2](https://docs.djangoproject.com/en/1.10/intro/tutorial02/) left off. We’re continuing the Web-poll application and will focus on creating the public interface – “views.”

紧接着[教程二](https://run-noob.gitbooks.io/django1-10/content/%E6%96%B0%E6%89%8B%E5%85%A5%E9%97%A8/%E7%AC%AC%E4%BA%8C%E9%83%A8%E5%88%86%EF%BC%9Amodels%E5%92%8Cadmin.html)，我们继续开发投票这个web应用，并将注意力集中在创建对外访问的“视图”界面上。 

#### **Overview**[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial03/#overview)

#### **概览**[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial03/#overview)

A view is a “type” of Web page in your Django application that generally serves a specific function and has a specific template. For example, in a blog application, you might have the following views:

+ Blog homepage – displays the latest few entries.
+ Entry “detail” page – permalink page for a single entry.
+ Year-based archive page – displays all months with entries in the given year.
+ Month-based archive page – displays all days with entries in the given month.
+ Day-based archive page – displays all entries in the given day.
+ Comment action – handles posting comments to a given entry.

视图是Django应用中的一“类”网页，它通常有一个特定的函数以及一个特定的模板。例如，在博客应用中，可能有以下视图：

+ 博客首页 —— 显示最新发表的文章链接。
+ 博客“详细”页面 —— 单篇文章的详细页面。
+ 基于年份的归档页面 —— 显示某给定年份里所有月份发表过的博客。
+ 基于月份的归档页面 —— 显示在给定月份中发表过所有文章。
+ 基于日期的归档页面 —— 显示在给定日期中发表过的所有文章链接。
+ 评论 —— 评论某博客

In our poll application, we’ll have the following four views:

+ Question “index” page – displays the latest few questions.
+ Question “detail” page – displays a question text, with no results but with a form to vote.
+ Question “results” page – displays results for a particular question.
+ Vote action – handles voting for a particular choice in a particular question.

在我们的投票应用中，将有以下四个视图：

+ Question首页 —— 显示最新发布的几个Question。
+ Question“详细”页面 —— 显示单个Question的具体内容，有一个投票的表单，但没有投票结果。
+ Question“结果”页面 —— 显示某Question的投票结果。
+ 投票功能 —— 可对Question中某个Choice的进行投票。

In Django, web pages and other content are delivered by views. Each view is represented by a simple Python function (or method, in the case of class-based views). Django will choose a view by examining the URL that's requested (to be precise, the part of the URL after the domain name).

在Django中，网页的页面和其他内容都是由视图来传递的（视图对WEB请求进行回应）。 每个视图都是由一个简单的Python函数(或者是基于类的视图的方法)。Django通过检查请求的URL（准确地说，是URL里域名之后的那部分）来选择使用哪个视图。

Now in your time on the web you may have come across such beauties as “ME2/Sites/dirmod.asp?sid=&type=gen&mod=Core+Pages&gid=A6CD4967199A42D9B65B1B”. You will be pleased to know that Django allows us much more elegant *URL patterns* than that.

平日你上网时，可能会遇到像 “ME2/Sites/dirmod.asp?sid=&type=gen&mod=Core+Pages&gid=A6CD4967199A42D9B65B1B”这样优美的URL。 你将会愉快地了解到，Django允许我们使用更加优雅的URL模式。 

A URL pattern is simply the general form of a URL - for example: /newsarchive/<year>/<month>/.

URL模式就是一个URL的通用形式 —— 例如： /newsarchive/<year>/<month>/。

To get from a URL to a view, Django uses what are known as ‘URLconfs’. A URLconf maps URL patterns (described as regular expressions) to views.

Django使用叫做‘URLconfs’的配置来为URL匹配视图。 一个URLconf负责将URL模式（由正则表达式编写）匹配到视图。

This tutorial provides basic instruction in the use of URLconfs, and you can refer to [django.urls](https://docs.djangoproject.com/en/1.10/ref/urlresolvers/#module-django.urls) for more information.

本教程有URLconfs的基本使用方法，你可以在 [django.urls](https://docs.djangoproject.com/en/1.10/ref/urlresolvers/#module-django.urls)看到更详细的信息 。

Writing more views[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial03/#writing-more-views)
Now let’s add a few more views to polls/views.py
. These views are slightly different, because they take an argument:

#### **编写更多的视图**

现在让我们给polls/views.py添加一些更多的视图。这些视图和之前的略有不同，因为它们另带了一个参数：

polls/views.py
```
def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)

def results(request, question_id):
    response = "You're looking at the results of question %s."
    return HttpResponse(response % question_id)

def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)
```

Wire these new views into the polls.urls
 module by adding the following [url()](https://docs.djangoproject.com/en/1.10/ref/urls/#django.conf.urls.url) calls:

通过下面的[url()](https://docs.djangoproject.com/en/1.10/ref/urls/#django.conf.urls.url) 调用将这些新的视图和polls.urls模块关联起来：

polls/urls.py
```
from django.conf.urls import url

from . import views

urlpatterns = [
    # ex: /polls/
    url(r'^$', views.index, name='index'),
    # ex: /polls/5/
    url(r'^(?P<question_id>[0-9]+)/$', views.detail, name='detail'),
    # ex: /polls/5/results/
    url(r'^(?P<question_id>[0-9]+)/results/$', views.results, name='results'),
    # ex: /polls/5/vote/
    url(r'^(?P<question_id>[0-9]+)/vote/$', views.vote, name='vote'),
]
```

Take a look in your browser, at “/polls/34/”. It’ll run the detail()
 method and display whatever ID you provide in the URL. Try “/polls/34/results/” and “/polls/34/vote/” too – these will display the placeholder results and voting pages.

看看你的浏览器，输入“/polls/34/”它将运行detail()方法并显示你在URL中提供的ID。 再试一下“/polls/34/results/”和“/polls/34/vote/” —— 它们将显示出对应的结果界面和投票界面。

When somebody requests a page from your website – say, “/polls/34/”, Django will load the mysite.urls Python module because it’s pointed to by the [ROOT_URLCONF
](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-ROOT_URLCONF) setting. It finds the variable named urlpatterns
 and traverses the regular expressions in order. After finding the match at '^polls/', it strips off the matching text ("polls/") and sends the remaining text – "34/" – to the ‘polls.urls’ URLconf for further processing. There it matches r'^(?P<question_id>[0-9]+)/$', resulting in a call to the detail() view like so:

当有人请求你的网站的一个页面时 ——　比如“/polls/34/”，根据[ROOT_URLCONF](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-ROOT_URLCONF) 的设置，Django将加载mysite.urls Python 模块。它查找到名为urlpatterns的变量并按顺序匹配其中的正则表达式。当匹配到'^polls/'，它截断了匹配到的字符串('polls/')，然后将剩下的字符串– "34/" – 传递到‘polls.urls’这个URLconf进一步处理。在那它匹配到了 r'^(?P<question_id>[0-9]+)/$'，然后再以以下方式调用detail()函数：
```
detail(request=<HttpRequest object>, question_id='34')
```

The question_id='34' part comes from (?P<question_id>[0-9]+). Using parentheses around a pattern “captures” the text matched by that pattern and sends it as an argument to the view function; ?P<question_id> defines the name that will be used to identify the matched pattern; and [0-9]+ is a regular expression to match a sequence of digits (i.e., a number).

question_id='34'部分来自(?P<question_id>[0-9]+)。使用模式周围的括号“捕获”该模式匹配的文本，并将其作为参数发送到视图函数；?P<question_id> 定义一个名字，它将用于标识匹配的模式；[0-9]+是匹配一串数字的正则表达式。

Because the URL patterns are regular expressions, there really is no limit on what you can do with them. And there’s no need to add URL cruft such as .html – unless you want to, in which case you can do something like this:

因为URL模式是正则表达式，你如何使用它们没有什么限制。 不需要添加像.html这样繁琐的URL —— 除非你执意这么做，比如说你可以写成这样：

```
url(r'^polls/latest\.html$', views.index),
```

But, don’t do that. It’s silly.

但是，别这做，这很没必要。

#### **Write views that actually do something**[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial03/#write-views-that-actually-do-something)

#### **写点有实际作用的视图
Each view is responsible for doing one of two things: returning an [HttpResponse](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponse) object containing the content for the requested page, or raising an exception such as [Http404](https://docs.djangoproject.com/en/1.10/topics/http/views/#django.http.Http404). The rest is up to you.

每个视图负责一件事：要么返回一个包含所请求页面内容的[HttpResponse](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponse)对象，要么抛出一个诸如404的异常。这取决于你。

Your view can read records from a database, or not. It can use a template system such as Django’s – or a third-party Python template system – or not. It can generate a PDF file, output XML, create a ZIP file on the fly, anything you want, using whatever Python libraries you want.

你的视图可以从数据库中读取记录，或者不读取数据库。你可以用Django自带的模板系统或第三方的模板系统——甚至不用模板。 你还可以动态地生成一个PDF文件、输出XML文件、创建一个ZIP文件，使用你想用的Python 库生成任何你想要的。

All Django wants is that [HttpResponse](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponse). Or an exception.

Django只要求返回一个[HttpResponse](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponse)，或者抛出异常。

Because it’s convenient, let’s use Django’s own database API, which we covered in [Tutorial 2](https://docs.djangoproject.com/en/1.10/intro/tutorial02/). Here’s one stab at a new index() view, which displays the latest 5 poll questions in the system, separated by commas, according to publication date:

让我们来用用[教程第二部分](https://run-noob.gitbooks.io/django1-10/content/%E6%96%B0%E6%89%8B%E5%85%A5%E9%97%A8/%E7%AC%AC%E4%BA%8C%E9%83%A8%E5%88%86%EF%BC%9Amodels%E5%92%8Cadmin.html)学到的数据库API，它是非常方便的。下面是一个新的index()视图，它列出了最近的5个投票Question记录，并用逗号隔开，以发布日期排序：

polls/views.py

```
from django.http import HttpResponse

from .models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    output = ', '.join([q.question_text for q in latest_question_list])
    return HttpResponse(output)

# Leave the rest of the views (detail, results, vote) unchanged
```

There’s a problem here, though: the page’s design is hard-coded in the view. If you want to change the way the page looks, you’ll have to edit this Python code. So let’s use Django’s template system to separate the design from Python by creating a template that the view can use.

这里有一个问题：页面的设计被硬编码在视图中。 如果你想更改页面的外观，就得编辑这段Python代码。 因此，让我们使用Django的模板系统，通过创建一个视图能够调用的模板，将页面的html代码从Python中分离出来。

First, create a directory called templates in your polls
 directory. Django will look for templates in there.

首先，在你的polls目录下创建一个叫做 templates的目录。Django将在这里查找模板。

Your project’s [TEMPLATES](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-TEMPLATES) setting describes how Django will load and render templates. The default settings file configures a DjangoTemplates
 backend whose [APP_DIRS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-TEMPLATES-APP_DIRS) option is set to True. By convention DjangoTemplates looks for a “templates” subdirectory in each of the [INSTALLED_APPS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-INSTALLED_APPS).

你项目的[TEMPLATES](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-TEMPLATES) 设置决定了Django如何加载和渲染模板。默认配置下，Django模板引擎在[APP_DIRS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-TEMPLATES-APP_DIRS)设置为True。Django模板引擎会根据[INSTALLED_APPS]目录下查找名为templates的子目录。

Within the templates directory you have just created, create another directory called polls, and within that create a file called index.html. In other words, your template should be at polls/templates/polls/index.html. Because of how the app_directories template loader works as described above, you can refer to this template within Django simply as polls/index.html.

在你刚刚创建的templates目录中，创建另外一个目录polls，并在其中创建一个文件index.html。也就是说，你的模板应该位于 polls/templates/polls/index.html。由于app_directories 模板加载器按照上面描述的方式工作，在Django中你可以简单地用polls/index.html引用这个模板。

> **Template namespacing**

> Now we might be able to get away with putting our templates directly in polls/templates (rather than creating another polls subdirectory), but it would actually be a bad idea. Django will choose the first template it finds whose name matches, and if you had a template with the same name in a different application, Django would be unable to distinguish between them. We need to be able to point Django at the right one, and the easiest way to ensure this is by namespacing them. That is, by putting those templates inside another directory named for the application itself.

> **模板命名空间**

> 现在，我们可以直接将我们的模板放在polls/templates中（而不用创建另外一个polls子目录），但实际上这是个坏主意。Django将选择它找到的名字匹配的第一个模板文件，如果你在不同的应用有相同名字的模板文件，Django将不能区分它们。我们需要将Django指向正确的模板，最简单的方式是使用命名空间。具体实现方式是，将这些模板文件放在以应用的名字来命名的另一个目录下。

Put the following code in that template:

将以下代码写入刚创建的模板：
```
polls/templates/polls/index.html
{% if latest_question_list %}
    <ul>
    {% for question in latest_question_list %}
        <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
    {% endfor %}
    </ul>
{% else %}
    <p>No polls are available.</p>
{% endif %}
```

Now let’s update our index view in polls/views.py to use the template:

现在我们用刚刚的模板来更新polls/views.py的视图函数：
polls/views.py
```
from django.http import HttpResponse
from django.template import loader

from .models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    template = loader.get_template('polls/index.html')
    context = {
        'latest_question_list': latest_question_list,
    }
    return HttpResponse(template.render(context, request))
```

That code loads the template called polls/index.html
 and passes it a context. The context is a dictionary mapping template variable names to Python objects.

那段代码加载了 polls/index.html模板，并传递了一个context对象，context是一个字典， 将模板的变量和python对象一一对应。

Load the page by pointing your browser at “/polls/”, and you should see a bulleted-list containing the “What’s up” question from [Tutorial 2](https://docs.djangoproject.com/en/1.10/intro/tutorial02/). The link points to the question’s detail page.

浏览器访问“/polls”，你讲看到一个列表，包含了我们在[教程二](https://run-noob.gitbooks.io/django1-10/content/%E6%96%B0%E6%89%8B%E5%85%A5%E9%97%A8/%E7%AC%AC%E4%BA%8C%E9%83%A8%E5%88%86%EF%BC%9Amodels%E5%92%8Cadmin.html)创建的 “What’s up” question，这个链接指向了Question的详细页

#### **A shortcut: [render()](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/#django.shortcuts.render)[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial03/#a-shortcut-render)**

#### **小技巧: [render()](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/#django.shortcuts.render)**
It’s a very common idiom to load a template, fill a context and return an [HttpResponse](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponse) object with the result of the rendered template. Django provides a shortcut. Here’s the full index() view, rewritten:

加载模板、填充一个context 然后返回一个含有模板渲染结果的[HttpResponse](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponse)对象是非常频繁的。
Django为此提供一个快捷方式。下面是重写后的index()视图：

polls/views.py
```
from django.shortcuts import render

from .models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    context = {'latest_question_list': latest_question_list}
    return render(request, 'polls/index.html', context)
```

Note that once we’ve done this in all these views, we no longer need to import [loader](https://docs.djangoproject.com/en/1.10/topics/templates/#module-django.template.loader) and [HttpResponse](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponse) (you’ll want to keep HttpResponse if you still have the stub methods for detail, results, and vote).

注意，一旦我们在所有的视图上都应用这个快捷函数，我们将不再需要导入[loader](https://docs.djangoproject.com/en/1.10/topics/templates/#module-django.template.loader) and [HttpResponse](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponse)（如果你没有改变先前的detail、results和 vote方法，你将需要在导入中保留HttpResponse
）。

The [render()](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/#django.shortcuts.render) function takes the request object as its first argument, a template name as its second argument and a dictionary as its optional third argument. It returns an [HttpResponse](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponse) object of the given template rendered with the given context.

 [render()](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/#django.shortcuts.render)函数将请求对象作为它的第一个参数，模板的名字作为它的第二个参数，一个字典作为它可选的第三个参数。 
它返回一个 [HttpResponse](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponse)对象，含有用给定的context 渲染后的模板。

#### **Raising a 404 error**[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial03/#raising-a-404-error)
Now, let’s tackle the question detail view – the page that displays the question text for a given poll. Here’s the view:

现在让我们来处理question detail视图——显示某question内容的页面，下面是该视图：

polls/views.py
```
from django.http import Http404
from django.shortcuts import render

from .models import Question
# ...
def detail(request, question_id):
    try:
        question = Question.objects.get(pk=question_id)
    except Question.DoesNotExist:
        raise Http404("Question does not exist")
    return render(request, 'polls/detail.html', {'question': question})
```

The new concept here: The view raises the [Http404](https://docs.djangoproject.com/en/1.10/topics/http/views/#django.http.Http404) exception if a question with the requested ID doesn’t exist.

新概念：这个视图抛出了[Http404](https://docs.djangoproject.com/en/1.10/topics/http/views/#django.http.Http404)异常，如果请求的question ID不存在的情况下。  

We’ll discuss what you could put in that polls/detail.html
 template a bit later, but if you’d like to quickly get the above example working, a file containing just:

稍后我们将讨论polls/detail.html模板可以写点什么。但是如果你想快速让上面的例子工作，如下就可以：

polls/templates/polls/detail.html
```
{{ question }}
```

#### **A shortcut: [get_object_or_404()](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/#django.shortcuts.get_object_or_404)**[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial03/#a-shortcut-get-object-or-404)

#### **快捷方式：[get_object_or_404()](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/#django.shortcuts.get_object_or_404)**[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial03/#a-shortcut-get-object-or-404)

It’s a very common idiom to use [get()](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#django.db.models.query.QuerySet.get) and raise [Http404](https://docs.djangoproject.com/en/1.10/topics/http/views/#django.http.Http404) if the object doesn’t exist. Django provides a shortcut. Here’s the detail() view, rewritten:

当用 [get()](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#django.db.models.query.QuerySet.get)时，如果对象不存在抛出 [Http404](https://docs.djangoproject.com/en/1.10/topics/http/views/#django.http.Http404)异常是很常用的。Django提供了一个快捷方式，重写后的detail()视图：

polls/views.py
```
from django.shortcuts import get_object_or_404, render

from .models import Question
# ...
def detail(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/detail.html', {'question': question})
```

The [get_object_or_404()](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/#django.shortcuts.get_object_or_404) function takes a Django model as its first argument and an arbitrary number of keyword arguments, which it passes to the [get()](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#django.db.models.query.QuerySet.get) function of the model’s manager. It raises [Http404](https://docs.djangoproject.com/en/1.10/topics/http/views/#django.http.Http404) if the object doesn’t exist.

[get_object_or_404()](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/#django.shortcuts.get_object_or_404) 函数将Django模型作为它的第一个参数，任意数量关键词参数，它将传递给作为模型管理器的[get()](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#django.db.models.query.QuerySet.get)函数，如果对象不存在，它就引发一个 [Http404](https://docs.djangoproject.com/en/1.10/topics/http/views/#django.http.Http404)异常。

> **Philosophy**

> Why do we use a helper function [get_object_or_404()](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/#django.shortcuts.get_object_or_404) instead of automatically catching the[ObjectDoesNotExist](https://docs.djangoproject.com/en/1.10/ref/exceptions/#django.core.exceptions.ObjectDoesNotExist) exceptions at a higher level, or having the model API raise [Http404](https://docs.djangoproject.com/en/1.10/topics/http/views/#django.http.Http404) instead of [ObjectDoesNotExist](https://docs.djangoproject.com/en/1.10/ref/exceptions/#django.core.exceptions.ObjectDoesNotExist)?

> 为什么我们要用一个辅助函数 [get_object_or_404()](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/#django.shortcuts.get_object_or_404)而不是在上层捕获[ObjectDoesNotExist](https://docs.djangoproject.com/en/1.10/ref/exceptions/#django.core.exceptions.ObjectDoesNotExist) 异常，或者让模型的API引发 [Http404](https://docs.djangoproject.com/en/1.10/topics/http/views/#django.http.Http404)而不是[ObjectDoesNotExist](https://docs.djangoproject.com/en/1.10/ref/exceptions/#django.core.exceptions.ObjectDoesNotExist)

> Because that would couple the model layer to the view layer. One of the foremost design goals of Django is to maintain loose coupling. Some controlled coupling is introduced in the [django.shortcuts](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/#module-django.shortcuts) module.

> 因为那样会将模型层与视图层耦合。Django 最重要的设计目标就是保持松耦合。一些可控的耦合在 [django.shortcuts](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/#module-django.shortcuts) 有介绍。

There’s also a [get_list_or_404()](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/#django.shortcuts.get_list_or_404) function, which works just as [get_object_or_404()](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/#django.shortcuts.get_object_or_404) – except using[filter()](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#django.db.models.query.QuerySet.filter) instead of [get()](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#django.db.models.query.QuerySet.get). It raises [Http404](https://docs.djangoproject.com/en/1.10/topics/http/views/#django.http.Http404) if the list is empty.

还有一个 [get_list_or_404()](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/#django.shortcuts.get_list_or_404) 函数, 原理和[get_object_or_404()](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/#django.shortcuts.get_object_or_404) 一样——差别在与它用[filter()](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#django.db.models.query.QuerySet.filter)而不是 [get()](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#django.db.models.query.QuerySet.get). 当列表为空时，它抛出[Http404](https://docs.djangoproject.com/en/1.10/topics/http/views/#django.http.Http404) 异常。

#### **Use the template system**[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial03/#use-the-template-system)

#### **使用模板系统**
Back to the detail() view for our poll application. Given the context variable question, here’s what the polls/detail.html template might look like:

回到我们投票应用的detail()视图。 根据context 变量question，下面是polls/detail.html模板可能的样子：
polls/templates/polls/detail.html
```
<h1>{{ question.question_text }}</h1>
<ul>
{% for choice in question.choice_set.all %}
    <li>{{ choice.choice_text }}</li>
{% endfor %}
</ul>
```

The template system uses dot-lookup syntax to access variable attributes. In the example of {% raw %}{{question.question_text }}{% endraw %}, first Django does a dictionary lookup on the object question. Failing that, it tries an attribute lookup – which works, in this case. If attribute lookup had failed, it would’ve tried a list-index lookup.

模板系统使用点号查找语法来访问变量的属性。 在 {% raw %}{{question.question_text }}{% endraw %}这个例子中，Django首先将在question对象当做字典查询。如果失败，Django会接着尝试按属性查询 —— 在这个例子中，属性查询会成功。如果属性查询也失败，Django将尝试列表索引查询。

Method-calling happens in the {% raw %} [{% for %}] {% endraw %}(https://docs.djangoproject.com/en/1.10/ref/templates/builtins/#std:templatetag-for) loop: question.choice_set.all
 is interpreted as the Python codequestion.choice_set.all()
, which returns an iterable of Choice
 objects and is suitable for use in the {% raw %} [{%for %}
](https://docs.djangoproject.com/en/1.10/ref/templates/builtins/#std:templatetag-for) {% endraw %} tag.

方法调用发生在```{% for %}```循环中：question.choice_set.all被解释为Python的代码question.choice_set.all()，它返回一个由Choice对象组成的可迭代对象，并将其用于{% raw %} [{%for %}](https://docs.djangoproject.com/en/1.10/ref/templates/builtins/#std:templatetag-for) {% endraw %}  标签。

See the [template guide](https://docs.djangoproject.com/en/1.10/topics/templates/) for more about templates.

查看[template guide](https://docs.djangoproject.com/en/1.10/topics/templates/) 了解更多模板信息

#### **Removing hardcoded URLs in templates**[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial03/#removing-hardcoded-urls-in-templates)

### **模板中去除硬编码**

Remember, when we wrote the link to a question in the polls/index.html template, the link was partially hardcoded like this:

请记住，当我们在polls/index.html模板中编写一个指向Question的链接时，链接中一部分是硬编码的：

```
<li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
```

The problem with this hardcoded, tightly-coupled approach is that it becomes challenging to change URLs on projects with a lot of templates. However, since you defined the name argument in the [url()](https://docs.djangoproject.com/en/1.10/ref/urls/#django.conf.urls.url) functions in the polls.urls module, you can remove a reliance on specific URL paths defined in your url configurations by using the{% raw %} {% url %}{% endraw %} template tag:

这种紧耦合的硬编码有一个问题，就是如果我们想在模板众多的项目中修改URLs，将会变得非常困难。 但是，如果你在polls.urls模块的 [url()](https://docs.djangoproject.com/en/1.10/ref/urls/#django.conf.urls.url) 函数中定义了name 参数，你可以通过使用{% raw %} {% url %}{% endraw %}模板标签来移除对你的URL配置中定义的特定的URL的依赖：
```
<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>
```

The way this works is by looking up the URL definition as specified in the polls.urls module. You can see exactly where the URL name of ‘detail’ is defined below:

它的工作原理是在polls.urls模块里查找指定的URL。你可以看到名为‘detail’的URL的准确定义：
```
...
# the 'name' value as called by the {% url %} template tag
url(r'^(?P<question_id>[0-9]+)/$', views.detail, name='detail'),
...
```

If you want to change the URL of the polls detail view to something else, perhaps to something like polls/specifics/12/ instead of doing it in the template (or templates) you would change it in polls/urls.py:

如果你想把polls应用中detail视图的URL改成其它样子比如 polls/specifics/12/，就可以不必在该模板（或者多个模板）中修改它，只需要修改 polls/urls.py：

```
...
# added the word 'specifics'
url(r'^specifics/(?P<question_id>[0-9]+)/$', views.detail, name='detail'),
...
```

#### **Namespacing URL names**[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial03/#namespacing-url-names)

#### **URL命名空间**

The tutorial project has just one app, polls. In real Django projects, there might be five, ten, twenty apps or more. How does Django differentiate the URL names between them? For example, the polls app has a detail view, and so might an app on the same project that is for a blog. How does one make it so that Django knows which app view to create for a url when using the {% raw %} {% url %}{% endraw %} template tag?

教程中的这个项目只有一个应用polls。在真实的Django项目中，可能会有五个、十个、二十个或者更多的应用。 Django如何区分它们URL的名字呢？ 例如，polls 应用具有一个detail 视图，相同项目中的博客应用可能也有这样一个视图。当使用模板标签{% raw %} {% url %}{% endraw %} 时，人们该如何做才能使得Django知道为一个URL创建哪个应用的视图？

The answer is to add namespaces to your URLconf. In the polls/urls.py file, go ahead and add an app_name
 to set the application namespace:

答案是在你的主URLconf下添加命名空间。 在mysite/urls.py文件中，增加一个app_name的变量作为命名空间：

polls/urls.py
```
from django.conf.urls import url

from . import views

app_name = 'polls'
urlpatterns = [
    url(r'^$', views.index, name='index'),
    url(r'^(?P<question_id>[0-9]+)/$', views.detail, name='detail'),
    url(r'^(?P<question_id>[0-9]+)/results/$', views.results, name='results'),
    url(r'^(?P<question_id>[0-9]+)/vote/$', views.vote, name='vote'),
]
```

Now change your polls/index.html template from:

现在修改polls/index.html 模板：

polls/templates/polls/index.html
```
<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>
```

to point at the namespaced detail view:

并指向具有命名空间的detail的视图函数

polls/templates/polls/index.html
```
<li><a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a></li>
```
When you’re comfortable with writing views, read [part 4 of this tutorial](https://docs.djangoproject.com/en/1.10/intro/tutorial04/) to learn about simple form processing and generic views.

当你对你写的视图感到满意后，请阅读[*教程4*](https://run-noob.gitbooks.io/django1-10/content/%E6%96%B0%E6%89%8B%E5%85%A5%E9%97%A8/%E7%AC%AC%E5%9B%9B%E9%83%A8%E5%88%86%EF%BC%9AForms%E5%92%8Cgeneric%20views.html)来了解简单的表单处理和通用视图。
