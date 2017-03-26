
#### **URL dispatcher[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#url-dispatcher)**
A clean, elegant URL scheme is an important detail in a high-quality Web application. Django lets you design URLs however you want, with no framework limitations.

简洁、优雅的URL设计对于高质量的web应用来说是非常重要的，Django允许你随心所欲的设计URLs，而不受框架的约束。

There’s no .php or .cgi required, and certainly none of that 0,2097,1-1-1928,00 nonsense.

不再要求.php 或 .cgi ，更不会要求0,2097,1-1-1928,00这样无意义 的东西

See [Cool URIs don’t change](http://www.w3.org/Provider/Style/URI), by World Wide Web creator Tim Berners-Lee, for excellent arguments on why URLs should be clean and usable.

参见World Wide Web的发明者写的[Cool URIs don’t change](http://www.w3.org/Provider/Style/URI)，里面详细说了为什么URLs应该简洁易用。

#### **Overview[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#overview)**

#### **概览[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#overview)**


To design URLs for an app, you create a Python module informally called a **URLconf** (URL configuration). This module is pure Python code and is a simple mapping between URL patterns (simple regular expressions) to Python functions (your views).

要给应用设计URLs，需首先创建一个叫URLconf（URL 配置）的Python模块。这个模块是纯粹的Python代码，包含了URL模式（简单正则表达式）到Python函数（你的视图）的简单映射。

This mapping can be as short or as long as needed. It can reference other mappings. And, because it’s pure Python code, it can be constructed dynamically.

这个映射关系可长可短。也可以引用其他的映射。而且，由于它是纯粹的Python代码，它可以动态的调整。

Django also provides a way to translate URLs according to the active language. See the [internationalization documentation](https://docs.djangoproject.com/en/1.11/topics/i18n/translation/#url-internationalization) for more information.

Django还提供当前语言翻译URL的一种方法，详细信息参考[internationalization documentation](https://docs.djangoproject.com/en/1.11/topics/i18n/translation/#url-internationalization)

How Django processes a request[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#how-django-processes-a-request)



#### **How Django processes a request[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#how-django-processes-a-request)**


When a user requests a page from your Django-powered site, this is the algorithm the system follows to determine which Python code to execute:

当一个用户请求Django 站点的一个页面，下面是Django 系统决定执行哪个Python 代码的算法：

1. Django determines the root URLconf module to use. Ordinarily, this is the value of the [ROOT_URLCONF](https://docs.djangoproject.com/en/1.11/ref/settings/#std:setting-ROOT_URLCONF)setting, but if the incoming HttpRequest
 object has a [urlconf](https://docs.djangoproject.com/en/1.11/ref/request-response/#django.http.HttpRequest.urlconf) attribute (set by middleware), its value will be used in place of the [ROOT_URLCONF](https://docs.djangoproject.com/en/1.11/ref/settings/#std:setting-ROOT_URLCONF) setting.

  Django首先会使用root URLconf模块。通常，这是 [ROOT_URLCONF](https://docs.djangoproject.com/en/1.11/ref/settings/#std:setting-ROOT_URLCONF)配置，但如果这个请求带有[urlconf](https://docs.djangoproject.com/en/1.11/ref/request-response/#django.http.HttpRequest.urlconf) 属性（通过中间件设置），那它的值会覆盖[ROOT_URLCONF](https://docs.djangoproject.com/en/1.11/ref/settings/#std:setting-ROOT_URLCONF)的配置

2. Django loads that Python module and looks for the variable urlpatterns. This should be a Python list of [django.conf.urls.url()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.url) instances.

  Django加载该Python模块并匹配url模式。它是[django.conf.urls.url()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.url) 实例的Python列表。

3.  Django runs through each URL pattern, in order, and stops at the first one that matches the requested URL.

  Django 依次匹配每个URL模式，直到匹配到第一个为止。

4. Once one of the regexes matches, Django imports and calls the given view, which is a simple Python function (or a [class-based view](https://docs.djangoproject.com/en/1.11/topics/class-based-views/)). The view gets passed the following arguments:

  一旦其中的一个正则表达式匹配上，Django 将导入并调用给出的视图，它是一个简单的Python 函数（ 或者是[class-based view](https://docs.djangoproject.com/en/1.11/topics/class-based-views/)）。视图函数将获得如下参数：

  * An instance of [HttpRequest](https://docs.djangoproject.com/en/1.11/ref/request-response/#django.http.HttpRequest).

  * 一个[HttpRequest](https://docs.djangoproject.com/en/1.11/ref/request-response/#django.http.HttpRequest)实例

  * If the matched regular expression returned no named groups, then the matches from the regular expression are provided as positional arguments.

  * 如果匹配的正则表达式返回的是未命名的组，那么正则表达式匹配的内容将作为位置参数提供给视图

  * The keyword arguments are made up of any named groups matched by the regular expression, overridden by any arguments specified in the optional kwargs argument to [django.conf.urls.url()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.url).

  * 关键字参数由正则表达式匹配的命名组组成，但是可被[django.conf.urls.url()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.url)的可选参数kwargs覆盖。

  * If no regex matches, or if an exception is raised during any point in this process, Django invokes an appropriate error-handling view. See [Error handling](https://docs.djangoproject.com/en/1.11/topics/http/urls/#error-handling) below.

* 如果没有匹配到正则表达式，或者如果过程中抛出一个异常，Django 将调用一个错误处理视图。请参见下面的[Error handling](https://docs.djangoproject.com/en/1.11/topics/http/urls/#error-handling) 。

#### **Example[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#example)**

#### **示例[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#example)**

Here’s a sample URLconf:

```
from django.conf.urls import url

from . import views

urlpatterns = [
    url(r'^articles/2003/$', views.special_case_2003),
    url(r'^articles/([0-9]{4})/$', views.year_archive),
    url(r'^articles/([0-9]{4})/([0-9]{2})/$', views.month_archive),
    url(r'^articles/([0-9]{4})/([0-9]{2})/([0-9]+)/$', views.article_detail),
]
```

Notes:

* To capture a value from the URL, just put parenthesis around it.

* 若要从URL捕获一个值，只需要加一个圆括号括起来即可

* There’s no need to add a leading slash, because every URL has that. For example, it’s ^articles, not ^/articles.

* 开头不需要再加斜杠，因为每个URL都有。比如说，应写^articles, 而不是 ^/articles.

* The 'r' in front of each regular expression string is optional but recommended. It tells Python that a string is “raw” – that nothing in the string should be escaped. See [Dive Into Python’s explanation](http://www.diveintopython.net/regular_expressions/street_addresses.html#re.matching.2.3).

* 每个正则表达式前面的‘r’不强求，但是推荐加上。它告诉python这个字符串是“原始的”——字符串中任何字符都不用转义。参见[Dive Into Python’s explanation](http://www.diveintopython.net/regular_expressions/street_addresses.html#re.matching.2.3)

Example requests:

请求的例子：

* A request to /articles/2005/03/ would match the third entry in the list. Django would call the function views.month_archive(request, '2005', '03').

* /articles/2005/03/ 请求会匹配到url列表中的第三个。Django会调用views.month_archive(request, '2005', '03').

* /articles/2005/3/ would not match any URL patterns, because the third entry in the list requires two digits for the month.

* /articles/2005/3/ 不匹配任何URL 模式，因为列表中的第三个模式要求月份应该是两个数字。

* /articles/2003/ would match the first pattern in the list, not the second one, because the patterns are tested in order, and the first one is the first test to pass. Feel free to exploit the ordering to insert special cases like this. Here, Django would call the function views.special_case_2003(request)

* /articles/2003/ 将匹配列表中的第一个模式不是第二个，因为模式按顺序匹配，第一个会首先被匹配通过。为了应对这种情况，你可以在之前插入一些url特例，比如对这里而言，Django会调用views.special_case_2003(request)。

* /articles/2003 would not match any of these patterns, because each pattern requires that the URL end with a slash.

* /articles/2003不会匹配任何模式，因为每个URL模式都要求以斜杠结尾

* /articles/2003/03/03/ would match the final pattern. Django would call the function views.article_detail(request, '2003', '03', '03').

* /articles/2003/03/03/将匹配最后一个，Django会调用views.article_detail(request, '2003', '03', '03')

#### **Named groups[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#named-groups)**

#### **命名组[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#named-groups)**

The above example used simple, *non-named* regular-expression groups (via parenthesis) to capture bits of the URL and pass them as *positional* arguments to a view. In more advanced usage, it’s possible to use *named* regular-expression groups to capture URL bits and pass them as *keyword* arguments to a view.

上面的示例使用简单的、没有命名的正则表达式组（通过圆括号）来捕获URL 中的值并以位置参数传递给视图。在更高级的用法中，可以使用命名的正则表达式组来捕获URL 中的值并以关键字参数传递给视图。

In Python regular expressions, the syntax for named regular-expression groups is (?P<name>pattern), where name
 is the name of the group and pattern is some pattern to match.

在Python 正则表达式中，命名正则表达式组的语法是(?P<name>pattern)，其中name 是组的名称，pattern 是要匹配的模式。

Here’s the above example URLconf, rewritten to use named groups:

在上面示例中的URLconf中，用命名组重写后：

```
from django.conf.urls import url

from . import views

urlpatterns = [
    url(r'^articles/2003/$', views.special_case_2003),
    url(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),
    url(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/$', views.month_archive),
    url(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/(?P<day>[0-9]{2})/$', views.article_detail),
]
```

This accomplishes exactly the same thing as the previous example, with one subtle difference: The captured values are passed to view functions as keyword arguments rather than positional arguments. For example:

这基本与上次的例子是一样的，只有一个细微差别：捕获的值作为关键字参数而不是位置参数传递给视图函数。例如：

* A request to /articles/2005/03/ would call the function views.month_archive(request, year='2005', month='03'), instead of views.month_archive(request, '2005', '03').

* /articles/2005/03/请求将调用views.month_archive(request, year='2005', month='03')，而不是views.month_archive(request, '2005', '03')

* A request to /articles/2003/03/03/ would call the function views.article_detail(request, year='2003', month='03', day='03').

* /articles/2003/03/03/ 请求将调用views.article_detail(request, year='2003', month='03', day='03').

In practice, this means your URLconfs are slightly more explicit and less prone to argument-order bugs – and you can reorder the arguments in your views’ function definitions. Of course, these benefits come at the cost of brevity; some developers find the named-group syntax ugly and too verbose.

在实际应用中，这意味你的URLconf 会更加明晰且不容易产生参数顺序问题的错误 —— 你可以在你的视图函数定义中重新安排参数的顺序。当然，这带来好处的同时牺牲了简洁性；有些开发人员认为命名组语法丑陋且繁琐。

#### **The matching/grouping algorithm[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#the-matching-grouping-algorithm)**

#### **匹配/分组的算法[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#the-matching-grouping-algorithm)**

Here’s the algorithm the URLconf parser follows, with respect to named groups vs. non-named groups in a regular expression:

下面是URLconf 解析器使用的算法，针对正则表达式中的命名组和非命名组：

1. If there are any named arguments, it will use those, ignoring non-named arguments.

  如果有命名参数，则使用这些命名参数，忽略非命名参数。

2. Otherwise, it will pass all non-named arguments as positional arguments.
  否则，它将以位置参数传递所有的非命名参数。

In both cases, any extra keyword arguments that have been given as per [Passing extra options to view functions](https://docs.djangoproject.com/en/1.11/topics/http/urls/#passing-extra-options-to-view-functions)(below) will also be passed to the view.

以上两种情况， [Passing extra options to view functions](https://docs.djangoproject.com/en/1.11/topics/http/urls/#passing-extra-options-to-view-functions)里的参数都会传递给view函数

#### **What the URLconf searches against[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#what-the-urlconf-searches-against)** 

#### **What the URLconf searches against[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#what-the-urlconf-searches-against)** 


#### **URLconf如何搜索[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#what-the-urlconf-searches-against)** 

The URLconf searches against the requested URL, as a normal Python string. This does not include GET or POST parameters, or the domain name.

URLconf将请求的URL当做Python的字符串来处理。所以这不包括GET或POST的参数，以及域名。

For example, in a request to https://www.example.com/myapp/, the URLconf will look for myapp/.

比如说，https://www.example.com/myapp/请求中，URLconf会查找myapp/

In a request to https://www.example.com/myapp/?page=3, the URLconf will look for myapp/.

在http://www.example.com/myapp/?page=3 请求中，URLconf 仍将查找myapp/。

The URLconf doesn’t look at the request method. In other words, all request methods – POST, GET, HEAD, etc. – will be routed to the same function for the same URL.

URLconf 不检查使用了哪种请求方法。换句话讲，所有的请求方法 —— 即，对同一个URL的无论是POST请求、GET请求、或HEAD请求方法等等 —— 都将路由到相同的函数。

#### **Captured arguments are always strings[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#captured-arguments-are-always-strings)**

#### **捕获的参数值均为字符串[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#captured-arguments-are-always-strings)**

Each captured argument is sent to the view as a plain Python string, regardless of what sort of match the regular expression makes. For example, in this URLconf line:

每个捕获的参数都作为一个普通的Python 字符串传递给视图，无论正则表达式使用的是什么匹配方式。例如，下面这行URLconf 中：
```
url(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),
```

...the year
 argument passed to views.year_archive()
 will be a string,not an integer, even though the [0-9]{4} will only match integer strings.

... views.year_archive() 的year 参数将是一个字符串，即使[0-9]{4} 值匹配整数字符串。

#### **Specifying defaults for view arguments[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#specifying-defaults-for-view-arguments)**

#### **指定视图参数的默认值[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#specifying-defaults-for-view-arguments)**

A convenient trick is to specify default parameters for your views’ arguments. Here’s an example URLconf and view:

有一个方便的小技巧是指定视图参数的默认值。 下面是一个URLconf 和视图的示例：

```
# URLconf
from django.conf.urls import url

from . import views

urlpatterns = [
    url(r'^blog/$', views.page),
    url(r'^blog/page(?P<num>[0-9]+)/$', views.page),
]

# View (in blog/views.py)
def page(request, num="1"):
    # Output the appropriate page of blog entries, according to num.
    ...
```

In the above example, both URL patterns point to the same view – views.page – but the first pattern doesn’t capture anything from the URL. If the first pattern matches, the page() function will use its default argument for num, "1". If the second pattern matches, page() will use whatever num value was captured by the regex.

在上面的例子中，两个URL模式指向同一个视图views.page —— 但是第一个模式不会从URL 中捕获任何值。如果第一个模式匹配，page() 函数将使用num参数的默认值"1"。如果第二个模式匹配，page() 将使用正则表达式捕获的num 值。

#### **Performance[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#performance)**

#### **性能[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#performance)**

Each regular expression in a urlpatterns is compiled the first time it’s accessed. This makes the system blazingly fast.

urlpatterns 中的每个正则表达式在第一次访问它们时被编译。这使得系统相当快。

#### **Syntax of the urlpatterns
 variable[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#syntax-of-the-urlpatterns-variable)**

#### **urlpatterns变量的语法[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#syntax-of-the-urlpatterns-variable)**

urlpatterns should be a Python list of [url()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.url) instances.

urlpatterns 应该是一个[url()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.url)  实例类型的Python 列表。

#### **Error handling[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#error-handling)**

#### **错误处理[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#error-handling)**

When Django can’t find a regex matching the requested URL, or when an exception is raised, Django will invoke an error-handling view.

当Django 找不到一个匹配请求的URL 的正则表达式，或者当抛出一个异常时，Django 将调用一个错误处理视图。

The views to use for these cases are specified by four variables. Their default values should suffice for most projects, but further customization is possible by overriding their default values.

这些情况发生时使用的视图通过4个变量指定。它们的默认值应该满足大部分项目，但是通过赋值给它们以进一步的自定义也是可以的。

See the documentation on [customizing error views](https://docs.djangoproject.com/en/1.11/topics/http/views/#customizing-error-views) for the full details.

详细文档参见 [customizing error views](https://docs.djangoproject.com/en/1.11/topics/http/views/#customizing-error-views) 

Such values can be set in your root URLconf. Setting these variables in any other URLconf will have no effect.

这些值可以在你的根URLconf 中设置。在其它URLconf 中设置这些变量将不起作用。

Values must be callables, or strings representing the full Python import path to the view that should be called to handle the error condition at hand.

它们的值必须是可调用的或者是表示视图的Python 完整导入路径的字符串，可以方便地调用它们来处理错误情况。这些值是：

* handler400 – See [django.conf.urls.handler400](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.handler400).

* handler403 – See [django.conf.urls.handler403](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.handler403).

* handler404 – See [django.conf.urls.handler404](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.handler404).

* handler500 – See [django.conf.urls.handler500](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.handler500).


#### **Including other URLconfs[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#including-other-urlconfs)**

#### **引入其他 URLconfs[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#including-other-urlconfs)**

At any point, your urlpatterns can “include” other URLconf modules. This essentially “roots” a set of URLs below other ones.

在任何时候，你的urlpatterns 都可以包含其它URLconf 模块。本质上说“roots”是其他URLs的一个集合。

For example, here’s an excerpt of the URLconf for the [Django website](https://www.djangoproject.com/) itself. It includes a number of other URLconfs:

例如，下面是 [Django 网站](https://www.djangoproject.com/)自己的URLconf 中一个片段。它包含许多其它URLconf：

```
from django.conf.urls import include, url

urlpatterns = [
    # ... snip ...
    url(r'^community/', include('django_website.aggregator.urls')),
    url(r'^contact/', include('django_website.contact.urls')),
    # ... snip ...
]
```

Note that the regular expressions in this example don’t have a $
 (end-of-string match character) but do include a trailing slash. Whenever Django encounters include()
 ([django.conf.urls.include()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.include)), it chops off whatever part of the URL matched up to that point and sends the remaining string to the included URLconf for further processing.

注意，这个例子中的正则表达式没有包含$
（字符串结束匹配符），但是包含一个末尾的斜杠。
每当Django 遇到include()（[django.conf.urls.include()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.include)）时，它会去掉URL 中匹配的部分并将剩下的字符串发送给包含的URLconf 做进一步处理。

Another possibility is to include additional URL patterns by using a list of [url()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.url) instances. For example, consider this URLconf:

另外一种包含其它URL 模式的方式是使用一个[url()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.url) 实例的列表。
例如，请看下面的URLconf：

```
from django.conf.urls import include, url

from apps.main import views as main_views
from credit import views as credit_views

extra_patterns = [
    url(r'^reports/$', credit_views.report),
    url(r'^reports/(?P<id>[0-9]+)/$', credit_views.report),
    url(r'^charge/$', credit_views.charge),
]

urlpatterns = [
    url(r'^$', main_views.homepage),
    url(r'^help/', include('apps.help.urls')),
    url(r'^credit/', include(extra_patterns)),
]
```

In this example, the /credit/reports/ URL will be handled by the credit_views.report() Django view.

在这个例子中，/credit/reports/ 将调用credit_views.report()视图

This can be used to remove redundancy from URLconfs where a single pattern prefix is used repeatedly. For example, consider this URLconf:

这个可以用来去除冗余的URL前缀。比如下面这个URLconf：

```
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^(?P<page_slug>[\w-]+)-(?P<page_id>\w+)/history/$', views.history),
    url(r'^(?P<page_slug>[\w-]+)-(?P<page_id>\w+)/edit/$', views.edit),
    url(r'^(?P<page_slug>[\w-]+)-(?P<page_id>\w+)/discuss/$', views.discuss),
    url(r'^(?P<page_slug>[\w-]+)-(?P<page_id>\w+)/permissions/$', views.permissions),
]
```

We can improve this by stating the common path prefix only once and grouping the suffixes that differ:

我们可以改进这种情况，使路径的前缀只用一次，然后将后面不同的部分分组：

```
from django.conf.urls import include, url
from . import views

urlpatterns = [
    url(r'^(?P<page_slug>[\w-]+)-(?P<page_id>\w+)/', include([
        url(r'^history/$', views.history),
        url(r'^edit/$', views.edit),
        url(r'^discuss/$', views.discuss),
        url(r'^permissions/$', views.permissions),
    ])),
]
```

#### **Captured parameters[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#captured-parameters)**

#### **捕获的参数[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#captured-parameters)**

An included URLconf receives any captured parameters from parent URLconfs, so the following example is valid:

被包含的URLconf 会收到来自父URLconf 捕获的任何参数，所以下面的例子是合法的：

```
# In settings/urls/main.py
from django.conf.urls import include, url

urlpatterns = [
    url(r'^(?P<username>\w+)/blog/', include('foo.urls.blog')),
]

# In foo/urls/blog.py
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^$', views.blog.index),
    url(r'^archive/$', views.blog.archive),
]
```

In the above example, the captured "username" variable is passed to the included URLconf, as expected.

在上面的例子中，捕获的"username"变量将被传递给include()指向的URLconf。

#### **Nested arguments[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#nested-arguments)**

#### **参数嵌套[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#nested-arguments)**

Regular expressions allow nested arguments, and Django will resolve them and pass them to the view. When reversing, Django will try to fill in all outer captured arguments, ignoring any nested captured arguments. Consider the following URL patterns which optionally take a page argument:

正则表达式允许嵌套参数，Django 将解析它们并传递给视图。当反查时，Django 将尝试填满所有外围捕获的参数，并忽略嵌套捕获的参数。考虑下面的URL 模式，它带有一个可选的page 参数：

```
from django.conf.urls import url

urlpatterns = [
    url(r'blog/(page-(\d+)/)?$', blog_articles),                  # bad
    url(r'comments/(?:page-(?P<page_number>\d+)/)?$', comments),  # good
]
```

Both patterns use nested arguments and will resolve: for example, blog/page-2/ will result in a match to blog_articles with two positional arguments: page-2/ and 2. The second pattern for comments will match comments/page-2/ with keyword argument page_number set to 2. The outer argument in this case is a non-capturing argument (?:...).

两个模式都使用嵌套的参数，其解析方式是：例如blog/page-2/ 将匹配blog_articles并带有两个位置参数page-2/ 和2。第二个comments 的模式将匹配comments/page-2/ 并带有一个值为2 的关键字参数page_number。这个例子中外围参数是一个不捕获的参数(?:...)。

The blog_articles view needs the outermost captured argument to be reversed, page-2/ or no arguments in this case, while comments can be reversed with either no arguments or a value for page_number.

blog_articles 视图需要最外层捕获的参数来反查，在这个例子中是page-2/或者没有参数，而comments可以不带参数或者用一个page_number值来反查。

Nested captured arguments create a strong coupling between the view arguments and the URL as illustrated by blog_articles: the view receives part of the URL (page-2/) instead of only the value the view is interested in. This coupling is even more pronounced when reversing, since to reverse the view we need to pass the piece of URL instead of the page number.

嵌套捕获的参数使得视图参数和URL 之间存在强耦合，正如blog_articles 所示：视图接收URL（page-2/）的一部分，而不只是视图所要的值。这种耦合在反查时更加显著，因为反查视图时我们需要传递URL 的一个片段而不只是page 的值。

As a rule of thumb, only capture the values the view needs to work with and use non-capturing arguments when the regular expression needs an argument but the view ignores it.

作为一个经验的法则，当正则表达式需要一个参数但视图忽略它的时候，只捕获视图需要的值并使用非捕获参数。

#### **Passing extra options to view functions[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#passing-extra-options-to-view-functions)**

#### **传递其他参数给view函数[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#passing-extra-options-to-view-functions)**

URLconfs have a hook that lets you pass extra arguments to your view functions, as a Python dictionary.

URLconfs 有个钩子功能，让你可以传递一个Python 字典作为额外的参数传递给视图函数。

The [django.conf.urls.url()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.url) function can take an optional third argument which should be a dictionary of extra keyword arguments to pass to the view function.

[django.conf.urls.url()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.url) 函数可以接收一个可选的第三个参数，它是一个字典，表示想要传递给视图函数的额外关键字参数。

For example:

```
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^blog/(?P<year>[0-9]{4})/$', views.year_archive, {'foo': 'bar'}),
]
```
In this example, for a request to /blog/2005/, Django will all views.year_archive(request,year='2005', foo='bar').

在这个例子中，对于/blog/2005/
请求，Django 将调用views.year_archive(request, year='2005', foo='bar')。

This technique is used in the [syndication framework](https://docs.djangoproject.com/en/1.11/ref/contrib/syndication/) to pass metadata and options to views.

这个技术在 [syndication framework](https://docs.djangoproject.com/en/1.11/ref/contrib/syndication/)中使用，来传递元数据和选项给视图。

> **Dealing with conflicts**

> It’s possible to have a URL pattern which captures named keyword arguments, and also passes arguments with the same names in its dictionary of extra arguments. When this happens, the arguments in the dictionary will be used instead of the arguments captured in the URL.

> **处理冲突**

> URL 模式捕获的命名关键字参数和在字典中传递的额外参数有可能具有相同的名称。当这种情况发生时，将使用字典中的参数而不是URL 中捕获的参数。

#### **Passing extra options to include()
[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#passing-extra-options-to-include)**

Similarly, you can pass extra options to [include()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.include). When you pass extra options to include(), *each* line in the included URLconf will be passed the extra options.

类似地，你可以传递额外的选项给 [include()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.include)。当你传递额外的选项给include()时，被包含的URLconf 的每一行将被传递这些额外的选项。

For example, these two URLconf sets are functionally identical:

例如，下面两个URLconf 设置功能上完全相同：

Set one:

```
# main.py
from django.conf.urls import include, url

urlpatterns = [
    url(r'^blog/', include('inner'), {'blogid': 3}),
]

# inner.py
from django.conf.urls import url
from mysite import views

urlpatterns = [
    url(r'^archive/$', views.archive),
    url(r'^about/$', views.about),
]
```

Set two:
```
# main.py
from django.conf.urls import include, url
from mysite import views

urlpatterns = [
    url(r'^blog/', include('inner')),
]

# inner.py
from django.conf.urls import url

urlpatterns = [
    url(r'^archive/$', views.archive, {'blogid': 3}),
    url(r'^about/$', views.about, {'blogid': 3}),
]
```

Note that extra options will always be passed to every line in the included URLconf, regardless of whether the line’s view actually accepts those options as valid. For this reason, this technique is only useful if you’re certain that every view in the included URLconf accepts the extra options you’re passing.

注意，额外的选项将永远传递给被包含的URLconf 中的每一行，无论该行的视图实际上是否认为这些选项是合法的。由于这个原因，该技术只有当你确定被包含的URLconf 中的每个视图都接收你传递给它们的额外的选项时才有价值。

#### **Reverse resolution of URLs[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#reverse-resolution-of-urls)**

#### **URLs反向解析[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#reverse-resolution-of-urls)**

A common need when working on a Django project is the possibility to obtain URLs in their final forms either for embedding in generated content (views and assets URLs, URLs shown to the user, etc.) or for handling of the navigation flow on the server side (redirections, etc.)

在开发Django 项目时，一个常见的需求是获得URL 的最终形式，以用于嵌入到生成的内容中（视图中和显示给用户的URL等）或者用于处理服务器端的导航（重定向等）。

It is strongly desirable to avoid hard-coding these URLs (a laborious, non-scalable and error-prone strategy). Equally dangerous is devising ad-hoc mechanisms to generate URLs that are parallel to the design described by the URLconf, which can result in the production of URLs that become stale over time.

人们强烈希望不要硬编码这些URL（费力、不可扩展且容易产生错误）或者设计一种与URLconf 毫不相关的专门的URL 生成机制，因为这样容易导致一定程度上产生过期的URL。

In other words, what’s needed is a DRY mechanism. Among other advantages it would allow evolution of the URL design without having to go over all the project source code to search and replace outdated URLs.

换句话讲，需要的是一个DRY 机制。除了其它优点，它还允许设计的URL 可以自动更新而不用遍历项目的源代码来搜索并替换过期的URL。

The primary piece of information we have available to get a URL is an identification (e.g. the name) of the view in charge of handling it. Other pieces of information that necessarily must participate in the lookup of the right URL are the types (positional, keyword) and values of the view arguments.

我们能从URL里获取到的最主要部分信息是找到处理它的视图函数。以及还有其他一些必要信息，如视图的参数类型（位置参数，关键字参数）和值。

Django provides a solution such that the URL mapper is the only repository of the URL design. You feed it with your URLconf and then it can be used in both directions:

Django 提供的解决方案是使得URL 映射是URL 设计唯一的储存库。你用你的URLconf填充它，然后可以双向使用它：

* Starting with a URL requested by the user/browser, it calls the right Django view providing any arguments it might need with their values as extracted from the URL.

* 用户/浏览器发起URL 请求，它调用正确的Django 视图，并从URL 中提取它的参数需要的值。

* Starting with the identification of the corresponding Django view plus the values of arguments that would be passed to it, obtain the associated URL.

* 根据Django 视图的标识和将要传递给它的参数的值，获取与之关联的URL。

The first one is the usage we’ve been discussing in the previous sections. The second one is what is known as reverse resolution of URLs, reverse URL matching, reverse URL lookup, or simply URL reversing.

第一种方式是我们在前面的章节中一直讨论的用法。第二种方式叫做反向解析URL、反向URL匹配、反向URL查询或者简单的URL反查。

Django provides tools for performing URL reversing that match the different layers where URLs are needed:

在需要URL 的地方，对于不同层级，Django 提供不同的工具用于URL 反查：

* In templates: Using the [url](https://docs.djangoproject.com/en/1.11/ref/templates/builtins/#std:templatetag-url) template tag.

* 在模板中：使用[url](https://docs.djangoproject.com/en/1.11/ref/templates/builtins/#std:templatetag-url) 标签

* In Python code: Using the [reverse()](https://docs.djangoproject.com/en/1.11/ref/urlresolvers/#django.urls.reverse) function.

* Python代码里，用 [reverse()](https://docs.djangoproject.com/en/1.11/ref/urlresolvers/#django.urls.reverse) 函数

* In higher level code related to handling of URLs of Django model instances: The [get_absolute_url()](https://docs.djangoproject.com/en/1.11/ref/models/instances/#django.db.models.Model.get_absolute_url)method.

* 在更高层的与处理Django 模型实例相关的代码中：使用[get_absolute_url()
](http://python.usyiyi.cn/documents/django_182/ref/models/instances.html#django.db.models.Model.get_absolute_url) 方法。

#### **Examples[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#examples)**

Consider again this URLconf entry:

```
from django.conf.urls import url

from . import views

urlpatterns = [
    #...
    url(r'^articles/([0-9]{4})/$', views.year_archive, name='news-year-archive'),
    #...
]
```

According to this design, the URL for the archive corresponding to year nnnn is /articles/nnnn/.

You can obtain these in template code by using:
```
<a href="{% url 'news-year-archive' 2012 %}">2012 Archive</a>
{# Or with the year in a template context variable: #}
<ul>
{% for yearvar in year_list %}
<li><a href="{% url 'news-year-archive' yearvar %}">{{ yearvar }} Archive</a></li>
{% endfor %}
</ul>
```

Or in Python code:

```
from django.urls import reverse
from django.http import HttpResponseRedirect

def redirect_to_year(request):
    # ...
    year = 2006
    # ...
    return HttpResponseRedirect(reverse('news-year-archive', args=(year,)))
```

If, for some reason, it was decided that the URLs where content for yearly article archives are published at should be changed then you would only need to change the entry in the URLconf.

如果出于某种原因决定按年归档文章发布的URL应该调整一下，那么你将只需要修改URLconf 中的内容。

In some scenarios where views are of a generic nature, a many-to-one relationship might exist between URLs and views. For these cases the view name isn’t a good enough identifier for it when comes the time of reversing URLs. Read the next section to know about the solution Django provides for this.

在某些场景中，一个视图是通用的，所以在URL 和视图之间存在多对一的关系。对于这些情况，当反查URL 时，只有视图的名字还不够。请阅读下一节来了解Django 为这个问题提供的解决办法。

#### **Naming URL patterns[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#naming-url-patterns)**

#### **命名URL patterns[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#naming-url-patterns)**

In order to perform URL reversing, you’ll need to use **named URL patterns** as done in the examples above. The string used for the URL name can contain any characters you like. You are not restricted to valid Python names.

为了完成上面例子中的URL 反查，你将需要使用命名的URL 模式。URL 的名称可以包含任何你喜欢的字符。并不仅限于合法的Python 名称。

When naming URL patterns, choose names that are unlikely to clash with other applications’ choice of names. If you call your URL pattern comment and another application does the same thing, the URL that [reverse()](https://docs.djangoproject.com/en/1.11/ref/urlresolvers/#django.urls.reverse) finds depends on whichever pattern is last in your project’s urlpatterns list.

当命名你的URL 模式时，请确保使用的名称不会与其它应用中名称冲突。如果你的URL 模式叫做comment，而另外一个应用中也有一个同样的名称， [reverse()](https://docs.djangoproject.com/en/1.11/ref/urlresolvers/#django.urls.reverse) 返回的是项目urlpatterns 列表里最后一个同名的URL。

Putting a prefix on your URL names, perhaps derived from the application name (such as myapp-comment instead of comment), decreases the chance of collision.

在URL 名称中加上一个前缀，比如应用的名称，将减少冲突的可能。我们建议使用myapp-comment 而不是comment。

You can deliberately choose the *same URL name* as another application if you want to override a view. For example, a common use case is to override the [LoginView](https://docs.djangoproject.com/en/1.11/topics/auth/default/#django.contrib.auth.views.LoginView). Parts of Django and most third-party apps assume that this view has a URL pattern with the name login. If you have a custom login view and give its URL the name login,[reverse()](https://docs.djangoproject.com/en/1.11/ref/urlresolvers/#django.urls.reverse) will find your custom view as long as it’s in urlpatterns after django.contrib.auth.urls is included (if that’s included at all).

你可以故意选择与其他应用同名的URL名字来重写一个视图函数。比如说，常用的是重写登录视图[LoginView](https://docs.djangoproject.com/en/1.11/topics/auth/default/#django.contrib.auth.views.LoginView)。部分Django或大多数的第三方应用都认为这个登录视图的URL名称为login。如果你有一个登录视图并命名为login，在将它的urlpatterns用django.contrib.auth.urls引入后，[reverse()](https://docs.djangoproject.com/en/1.11/ref/urlresolvers/#django.urls.reverse) 函数返回的将是你自定义的view。

You may also use the same name for multiple URL patterns if they differ in their arguments. In addition to the URL name, [reverse()](https://docs.djangoproject.com/en/1.11/ref/urlresolvers/#django.urls.reverse) matches the number of arguments and the names of the keyword arguments.

你同样可以为多个URL模式起相同的名字，如果他们的参数不同的话。除了URL名字外， [reverse()](https://docs.djangoproject.com/en/1.11/ref/urlresolvers/#django.urls.reverse)还会匹配参数的个数以及关键字参数。

#### **URL namespaces[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#url-namespaces)**

**Introduction[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#introduction)**

URL namespaces allow you to uniquely reverse [named URL patterns](https://docs.djangoproject.com/en/1.11/topics/http/urls/#naming-url-patterns) even if different applications use the same URL names. It’s a good practice for third-party apps to always use namespaced URLs (as we did in the tutorial). Similarly, it also allows you to reverse URLs if multiple instances of an application are deployed. In other words, since multiple instances of a single application will share named URLs, namespaces provide a way to tell these named URLs apart.

URL 命名空间允许你反查到唯一的 [named URL patterns](https://docs.djangoproject.com/en/1.11/topics/http/urls/#naming-url-patterns) ，即使不同的应用使用相同的URL 名称。第三方应用始终使用带命名空间的URL 是一个很好的实践（我们在教程中也是这么做的）。类似地，它还允许你在一个应用有多个实例部署的情况下反查URL。换句话讲，因为一个应用的多个实例共享相同的命名URL，命名空间提供了一种区分这些命名URL 的方法。

Django applications that make proper use of URL namespacing can be deployed more than once for a particular site. For example [django.contrib.admin](https://docs.djangoproject.com/en/1.11/ref/contrib/admin/#module-django.contrib.admin) has an [AdminSite](https://docs.djangoproject.com/en/1.11/ref/contrib/admin/#django.contrib.admin.AdminSite) class which allows you to easily [deploy more than one instance of the admin](https://docs.djangoproject.com/en/1.11/ref/contrib/admin/#multiple-admin-sites). In a later example, we’ll discuss the idea of deploying the polls application from the tutorial in two different locations so we can serve the same functionality to two different audiences (authors and publishers).

在一个站点上，正确使用URL命名空间的Django 应用可以部署多次。
例如， [django.contrib.admin](https://docs.djangoproject.com/en/1.11/ref/contrib/admin/#module-django.contrib.admin)  具有一个[AdminSite](https://docs.djangoproject.com/en/1.11/ref/contrib/admin/#django.contrib.admin.AdminSite) 类，它允许你很容易地 [deploy more than one instance of the admin](https://docs.djangoproject.com/en/1.11/ref/contrib/admin/#multiple-admin-sites)。
在下面的例子中，我们将讨论在两个不同的地方部署教程中的polls应用，这样我们可以为两种不同的用户（作者和发布者）提供相同的功能。

A URL namespace comes in two parts, both of which are strings:

一个URL命名空间有两个部分，它们都是字符串：

**application namespace**
This describes the name of the application that is being deployed. Every instance of a single application will have the same application namespace. For example, Django’s admin application has the somewhat predictable application namespace of 'admin'.

它表示正在部署的应用的名称。一个应用的每个实例具有相同的应用命名空间。例如，容易知道Django 的管理站点应用命名空间是'admin'。

**instance namespace**
This identifies a specific instance of an application. Instance namespaces should be unique across your entire project. However, an instance namespace can be the same as the application namespace. This is used to specify a default instance of an application. For example, the default Django admin instance has an instance namespace of 'admin'.

它表示应用的一个特定的实例。实例的命名空间在你的全部项目中应该是唯一的。但是，一个实例的命名空间可以和应用的命名空间相同。它可用于表示一个应用的默认实例。例如，Django 管理站点实例的默认的实例命名空间是'admin'。

Namespaced URLs are specified using the ':' operator. For example, the main index page of the admin application is referenced using 'admin:index'. This indicates a namespace of 'admin', and a named URL of 'index'.

URL 的命名空间使用':' 分隔符分开。例如，管理站点应用的主页使用'admin:index'。它表示'admin' 的一个命名空间和'index' 的一个命名URL。

Namespaces can also be nested. The named URL 'sports:polls:index' would look for a pattern named 'index' in the namespace 'polls' that is itself defined within the top-level namespace 'sports'.

命名空间也可以嵌套。对于这个URL名称'sports:polls:index' ，首先在顶层命名空间'sports' 中找到polls命名空间，再找到index实例命名空间。

#### **Reversing namespaced URLs[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#reversing-namespaced-urls)**


#### **反向解析带命名空间的URLs[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#reversing-namespaced-urls)**

When given a namespaced URL (e.g. 'polls:index') to resolve, Django splits the fully qualified name into parts and then tries the following lookup:

当解析一个带命名空间的URL（例如'polls:index'）时，Django 将切分名称为多个部分，然后按下面的步骤查找：

1. First, Django looks for a matching [application namespace](https://docs.djangoproject.com/en/1.11/topics/http/urls/#term-application-namespace) (in this example, 'polls'). This will yield a list of instances of that application.

  首先，Django 查找匹配的 [application namespace](https://docs.djangoproject.com/en/1.11/topics/http/urls/#term-application-namespace) 在这个例子中为'polls'。这将得到该应用实例的一个列表。

2. If there is a current application defined, Django finds and returns the URL resolver for that instance. The current application can be specified with the current_app
 argument to the [reverse()](https://docs.djangoproject.com/en/1.11/ref/urlresolvers/#django.urls.reverse) function.
  
  如果当前应用属性被定义，Django将查找并返回那个实例的URL解析器。当前应用属性可在[reverse()](https://docs.djangoproject.com/en/1.11/ref/urlresolvers/#django.urls.reverse) 函数中通过current_app来指定

  The [url](https://docs.djangoproject.com/en/1.11/ref/templates/builtins/#std:templatetag-url) template tag uses the namespace of the currently resolved view as the current application in a[RequestContext](https://docs.djangoproject.com/en/1.11/ref/templates/api/#django.template.RequestContext). You can override this default by setting the current application on the [request.current_app
](https://docs.djangoproject.com/en/1.11/ref/request-response/#django.http.HttpRequest.current_app) attribute.

  [url](https://docs.djangoproject.com/en/1.11/ref/templates/builtins/#std:templatetag-url) 模板标签在[RequestContext](https://docs.djangoproject.com/en/1.11/ref/templates/api/#django.template.RequestContext)将当前解析的视图当作当前应用。你可以通过设置[request.current_app
](https://docs.djangoproject.com/en/1.11/ref/request-response/#django.http.HttpRequest.current_app) 属性重写这个默认行为

3.  If there is no current application. Django looks for a default application instance. The default application instance is the instance that has an [instance namespace](https://docs.djangoproject.com/en/1.11/topics/http/urls/#term-instance-namespace) matching the [application namespace](https://docs.djangoproject.com/en/1.11/topics/http/urls/#term-application-namespace) (in this example, an instance of polls called 'polls').

  如果没有当前应用。Django 将查找一个默认的应用实例。
默认的应用实例是[instance namespace](https://docs.djangoproject.com/en/1.11/topics/http/urls/#term-instance-namespace) 与[application namespace](https://docs.djangoproject.com/en/1.11/topics/http/urls/#term-application-namespace) 一致的那个实例（在这个例子中，polls
 的一个叫做'polls' 的实例）。

4. If there is no default application instance, Django will pick the last deployed instance of the application, whatever its instance name may be.

  如果没有默认的应用实例，Django 将挑选该应用最后部署的实例，不管实例的名称是什么。

5. If the provided namespace doesn’t match an [application namespace](https://docs.djangoproject.com/en/1.11/topics/http/urls/#term-application-namespace) in step 1, Django will attempt a direct lookup of the namespace as an [instance namespace](https://docs.djangoproject.com/en/1.11/topics/http/urls/#term-instance-namespace).

  如果提供的命名空间与第1步中的[application namespace](https://docs.djangoproject.com/en/1.11/topics/http/urls/#term-application-namespace) 不匹配，Django 将尝试直接将此命名空间作为一个 [instance namespace](https://docs.djangoproject.com/en/1.11/topics/http/urls/#term-instance-namespace)查找。

If there are nested namespaces, these steps are repeated for each part of the namespace until only the view name is unresolved. The view name will then be resolved into a URL in the namespace that has been found.

如果有嵌套的命名空间，将为命名空间的每个部分重复调用这些步骤直至剩下视图的名称还未解析。然后该视图的名称将被解析到找到的这个命名空间中的一个URL。

**Example[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#id4)**

To show this resolution strategy in action, consider an example of two instances of the polls application from the tutorial: one called 'author-polls' and one called 'publisher-polls'. Assume we have enhanced that application so that it takes the instance namespace into consideration when creating and displaying polls.

为了演示解析的策略，如教程中两个polls应用的实例：'author-polls' 和'publisher-polls'。假设我们已经改进了这个应用，在创建和显示投票时考虑了实例命名空间。
urls.py

```
from django.conf.urls import include, url

urlpatterns = [
    url(r'^author-polls/', include('polls.urls', namespace='author-polls')),
    url(r'^publisher-polls/', include('polls.urls', namespace='publisher-polls')),
]
```
polls/urls.py

```
from django.conf.urls import url

from . import views

app_name = 'polls'
urlpatterns = [
    url(r'^$', views.IndexView.as_view(), name='index'),
    url(r'^(?P<pk>\d+)/$', views.DetailView.as_view(), name='detail'),
    ...
]
```

Using this setup, the following lookups are possible:

根据以上设置，可按如下查询：

* If one of the instances is current - say, if we were rendering the detail page in the instance 'author-polls' - 'polls:index' will resolve to the index page of the 'author-polls' instance; i.e. both of the following will result in "/author-polls/".

  In the method of a class-based view:

*  如果其中一个实例是当前实例 —— 如果我们正在渲染'author-polls' 实例的detail 页面 —— 'polls:index' 将解析成'author-polls' 实例的主页面；例如下面两个都将解析成"/author-polls/"。

  在基于类的视图的方法中：

```
reverse('polls:index', current_app=self.request.resolver_match.namespace)
```

  and in the template:

```
{% url 'polls:index' %}
```

注意，在模板中的反查需要添加request 的current_app 属性，像这样：

* If there is no current instance - say, if we were rendering a page somewhere else on the site - 'polls:index' will resolve to the last registered instance of polls. Since there is no default instance (instance namespace of 'polls'), the last instance of polls that is registered will be used. This would be 'publisher-polls' since it’s declared last in the urlpatterns.

* 如果没有当前实例——假如说我们在站点的其他地方渲染页面——'polls:index'将解析到最后注册到polls的实例。因为没有默认的实例（实例命名空间为polls），将用注册的polls的最后一个实例。它将是'publisher-polls'，因为它是urlpatterns中最后一个声明的

* 'author-polls:index' will always resolve to the index page of the instance 'author-polls' (and likewise for 'publisher-polls') .

* 'author-polls:index' 将永远解析到 'author-polls' 实例的主页（'publisher-polls' 类似）。

If there were also a default instance - i.e., an instance named 'polls' - the only change from above would be in the case where there is no current instance (the second item in the list above). In this case 'polls:index' would resolve to the index page of the default instance instead of the instance declared last in urlpatterns.

如果还有一个默认的实例 —— 例如，一个名为'polls' 的实例 —— 上面例子中唯一的变化是当没有当前实例的情况（上述第二种情况）。在这种情况下 'polls:index' 将解析到默认实例而不是urlpatterns 中最后声明的实例的主页。

#### **URL namespaces and included URLconfs[¶](https://docs.djangoproject.com/en/1.11/topics/http/urls/#url-namespaces-and-included-urlconfs)**

Application namespaces of included URLconfs can be specified in two ways.

 included URLconfs的应用命名空间可通过两种方式指定。

Firstly, you can set an app_name
 attribute in the included URLconf module, at the same level as the urlpatterns attribute. You have to pass the actual module, or a string reference to the module, to [include()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.include), not the list of urlpatterns itself.

第一种方式，你可以在被include的模块中设置app_name属性，与urlpatterns 变量同级。你需传递一个实际的模块，或者一个关联到那模块的字符串，而不是urlpatterns列表自己

polls/urls.py

```
from django.conf.urls import url

from . import views

app_name = 'polls'
urlpatterns = [
    url(r'^$', views.IndexView.as_view(), name='index'),
    url(r'^(?P<pk>\d+)/$', views.DetailView.as_view(), name='detail'),
    ...
]
```
urls.py

```
from django.conf.urls import include, url

urlpatterns = [
    url(r'^polls/', include('polls.urls')),
]
```

The URLs defined in polls.urls will have an application namespace polls.

polls.urls中定义URLs将有一个应用命名空间polls

Secondly, you can include an object that contains embedded namespace data. If you include() a list of [url()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.url)instances, the URLs contained in that object will be added to the global namespace. However, you can also include()
 a 2-tuple containing:

第二种方式，你可以引入一个对象，对象要包含一个内嵌的命名空间数据。如果你 include()一个[url()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.url)实例的列表，那么包含那个对象URLs将被添加到全局命名空间。然而，你也可以include()一个2个元素的元组：


```
(<list of url() instances>, <application namespace>)
```

For example:
```
from django.conf.urls import include, url

from . import views

polls_patterns = ([
    url(r'^$', views.IndexView.as_view(), name='index'),
    url(r'^(?P<pk>\d+)/$', views.DetailView.as_view(), name='detail'),
], 'polls')

urlpatterns = [
    url(r'^polls/', include(polls_patterns)),
]
```

This will include the nominated URL patterns into the given application namespace.

这样会包含命名的URL模式进入到给定的应用和实例命名空间中。

The instance namespace can be specified using the namespace
 argument to [include()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.include). If the instance namespace is not specified, it will default to the included URLconf’s application namespace. This means it will also be the default instance for that namespace.

 对于[include()](https://docs.djangoproject.com/en/1.11/ref/urls/#django.conf.urls.include)函数，实例命名空间可以用namespace参数指定。如果没有指定instance namespace，它将默认include 与URLconf的应用命名空间。这意味着，它也是默认的实例命名空间。