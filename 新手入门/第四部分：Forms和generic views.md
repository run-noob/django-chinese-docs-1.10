#### **Writing your first Django app, part 4**[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial04/#writing-your-first-django-app-part-4)

This tutorial begins where [Tutorial 3](https://docs.djangoproject.com/en/1.10/intro/tutorial03/) left off. We’re continuing the Web-poll application and will focus on simple form processing and cutting down our code.

紧接着[Tutorial 3](https://docs.djangoproject.com/en/1.10/intro/tutorial03/)，我们继续开发这个投票的web应用，本章将关注简单的表单处理和代码优化

#### **Write a simple form**[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial04/#write-a-simple-form)

#### **编写一个简单的表单**

Let’s update our poll detail template (“polls/detail.html”) from the last tutorial, so that the template contains an HTML <form>
 element:

让我们更新一下在上一个教程中编写的投票详细页面的模板（“polls/detail.html”），让它包含一个HTML<form> 元素： 

polls/templates/polls/detail.html
```
<h1>{{ question.question_text }}</h1>

{% if error_message %}<p><strong>{{ error_message }}</strong></p>{% endif %}

<form action="{% url 'polls:vote' question.id %}" method="post">
{% csrf_token %}
{% for choice in question.choice_set.all %}
    <input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{ choice.id }}" />
    <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label><br />
{% endfor %}
<input type="submit" value="Vote" />
</form>
```

A quick rundown:

简要说明：

+ The above template displays a radio button for each question choice. The value of each radio button is the associated question choice’s ID. The name of each radio button is "choice". That means, when somebody selects one of the radio buttons and submits the form, it’ll send the POST data choice=# where # is the ID of the selected choice. This is the basic concept of HTML forms.

+ 在detail网页模板中，我们为Question对应的每个Choice都添加了一个单选按钮用于选择。每个单选按钮的value属性是对应的各个Choice的ID。每个单选按钮的name是"choice"。这意味着，当有人选择一个单选按钮并提交表单时，它将发送一个POST数据choice=#，其中# 为选择的Choice的ID。这是HTML 表单的基本概念。

+ We set the form’s action to {% raw %}{% url 'polls:vote' question.id %}{% raw %}, and we set method="post". Using method="post" (as opposed to method="get") is very important, because the act of submitting this form will alter data server-side. Whenever you create a form that alters data server-side, use method="post". This tip isn’t specific to Django; it’s just good Web development practice.

+ 我们设置表单的action为{% raw %}{% url 'polls:vote' question.id %}{% raw %}，并设置 method="post"。使用method="post"（与其相对的是method="get"）是非常重要的，因为这个提交表单的行为会改变服务器端的数据。 无论何时，当你需要创建一个改变服务器端数据的表单时，请使用 method="post"。这不是Django的特定技巧；这是优秀的网站开发实践。

+ forloop.counter indicates how many times the {% raw %}[for](https://docs.djangoproject.com/en/1.10/ref/templates/builtins/#std:templatetag-for){% endraw %} tag has gone through its loop

+ forloop.counter 表示{% raw %}[for](https://docs.djangoproject.com/en/1.10/ref/templates/builtins/#std:templatetag-for){% endraw %} 标签已经循环的次数

+ Since we’re creating a POST form (which can have the effect of modifying data), we need to worry about Cross Site Request Forgeries. Thankfully, you don’t have to worry too hard, because Django comes with a very easy-to-use system for protecting against it. In short, all POST forms that are targeted at internal URLs should use the {% raw %}[{%csrf_token %}](https://docs.djangoproject.com/en/1.10/ref/templates/builtins/#std:templatetag-csrf_token) {% endraw %}template tag.

+ 由于我们创建的是一个POST表单(会改变服务器端的数据)，那我们就会担心跨站伪造请求的攻击。但幸运的是，我们不需要太担心，因为Django自带了一个简单易用的系统来防御。简而言之，所有提交到目标URLs的POST表单，都应该设置{% raw %}[{%csrf_token %}](https://docs.djangoproject.com/en/1.10/ref/templates/builtins/#std:templatetag-csrf_token) {% endraw %}模板标签

Now, let’s create a Django view that handles the submitted data and does something with it. Remember, in [Tutorial 3](https://docs.djangoproject.com/en/1.10/intro/tutorial03/), we created a URLconf for the polls application that includes this line:

现在，让我们来创建一个Django视图来处理提交的数据。 
记住，在[*教程 3*](https://run-noob.gitbooks.io/django1-10/content/%E6%96%B0%E6%89%8B%E5%85%A5%E9%97%A8/%E7%AC%AC%E4%B8%89%E9%83%A8%E5%88%86%EF%BC%9Aviews%E5%92%8Ctemplate.html)中，我们为投票应用创建了一个URLconf ，包含这一行：

polls/urls.py
```
url(r'^(?P<question_id>[0-9]+)/vote/$', views.vote, name='vote'),
```

We also created a dummy implementation of the vote() function. Let’s create a real version. Add the following to polls/views.py:

我们也创建了一个没什么实际功能的vote()函数。现在让我们来真的。将下面代码添加到polls/views.py：

polls/views.py
```
from django.shortcuts import get_object_or_404, render
from django.http import HttpResponseRedirect, HttpResponse
from django.urls import reverse

from .models import Choice, Question
# ...
def vote(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    try:
        selected_choice = question.choice_set.get(pk=request.POST['choice'])
    except (KeyError, Choice.DoesNotExist):
        # Redisplay the question voting form.
        return render(request, 'polls/detail.html', {
            'question': question,
            'error_message': "You didn't select a choice.",
        })
    else:
        selected_choice.votes += 1
        selected_choice.save()
        # Always return an HttpResponseRedirect after successfully dealing
        # with POST data. This prevents data from being posted twice if a
        # user hits the Back button.
        return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))
```

This code includes a few things we haven’t covered yet in this tutorial:

这段代码有一些我们未在教程提及的知识：

+ [request.POST](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpRequest.POST) is a dictionary-like object that lets you access submitted data by key name. In this case,request.POST['choice'] returns the ID of the selected choice, as a string. [request.POST](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpRequest.POST) values are always strings.

[request.POST](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpRequest.POST) 是一个类似字典的对象，让你可以通过关键字的名字获取提交的数据。这个例子中，request.POST['choice'] 以字符串形式返回选择的Choice的ID。[request.POST](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpRequest.POST) 的值永远是字符串

Note that Django also provides [request.GET](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpRequest.GET) for accessing GET data in the same way – but we’re explicitly using [request.POST
](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpRequest.POST) in our code, to ensure that data is only altered via a POST call.

同样的，Django也提供了[request.GET](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpRequest.GET)方法来获取GET数据，但我们在代码中明确地使用[request.POST
](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpRequest.POST)，以保证数据只能通过POST调用改动。

request.POST['choice'] will raise [KeyError](https://docs.python.org/3/library/exceptions.html#KeyError) if choice wasn’t provided in POST data. The above code checks for [KeyError](https://docs.python.org/3/library/exceptions.html#KeyError) and redisplays the question form with an error message if choice
 isn’t given.

如果POST数据没有choice，request.POST['choice'] 会抛出 [KeyError](https://docs.python.org/3/library/exceptions.html#KeyError)异常。上面的代码会检查 [KeyError](https://docs.python.org/3/library/exceptions.html#KeyError)，如果没有对应的choice，将会重新显示question表单和一个错误信息。

After incrementing the choice count, the code returns an [HttpResponseRedirect](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponseRedirect) rather than a normal [HttpResponse](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponse). [HttpResponseRedirect
](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponseRedirect) takes a single argument: the URL to which the user will be redirected (see the following point for how we construct the URL in this case).

在增加choice的票数之后，代码返回一个[HttpResponseRedirect](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponseRedirect)而不是[HttpResponse](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponse)，[HttpResponseRedirect
](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponseRedirect) 只接收一个参数：用户将被重定向的URL（下面请看我们如何构造这个URL）

As the Python comment above points out, you should always return an [HttpResponseRedirect](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponseRedirect) after successfully dealing with POST data. This tip isn’t specific to Django; it’s just good Web development practice.

正如上面的Python注释指出的，你应该在成功处理POST数据后总是返回一个[HttpResponseRedirect](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponseRedirect)。 这不是Django的特定技巧； 这是那些优秀网站在开发实践中形成的共识。

We are using the [reverse()](https://docs.djangoproject.com/en/1.10/ref/urlresolvers/#django.urls.reverse) function in the [HttpResponseRedirect](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponseRedirect) constructor in this example. This function helps avoid having to hardcode a URL in the view function. It is given the name of the view that we want to pass control to and the variable portion of the URL pattern that points to that view. In this case, using the URLconf we set up in [Tutorial 3](https://docs.djangoproject.com/en/1.10/intro/tutorial03/), this [reverse()](https://docs.djangoproject.com/en/1.10/ref/urlresolvers/#django.urls.reverse) call will return a string like:

```
'/polls/3/results/'
```

在这个例子中，我们用 [reverse()](https://docs.djangoproject.com/en/1.10/ref/urlresolvers/#django.urls.reverse) 函数作为 [HttpResponseRedirect](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpResponseRedirect)的参数，这个函数避免了我们在视图函数中硬编码URL。它需要我们给出我们想要跳转的视图的名字和该视图所对应的URL模式中需要给该视图提供的参数。 在本例中，使用在[教程3](https://run-noob.gitbooks.io/django1-10/content/%E6%96%B0%E6%89%8B%E5%85%A5%E9%97%A8/%E7%AC%AC%E4%B8%89%E9%83%A8%E5%88%86%EF%BC%9Aviews%E5%92%8Ctemplate.html)中设定的URLconf，调用 [reverse()](http://python.usyiyi.cn/documents/django_182/ref/urlresolvers.html#django.core.urlresolvers.reverse) 将返回一个这样的字符串：

```
'/polls/3/results/'
```

where the 3 is the value of question.id. This redirected URL will then call the 'results' view to display the final page.
3就是 question.id的值。重定向的URL将调用'results' 视图来显示最终的页面。

As mentioned in [Tutorial 3](https://docs.djangoproject.com/en/1.10/intro/tutorial03/), request
 is an [HttpRequest](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpRequest) object. For more on [HttpRequest](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpRequest) objects, see the [request and response documentation](https://docs.djangoproject.com/en/1.10/ref/request-response/).

正如[教程3](https://run-noob.gitbooks.io/django1-10/content/%E6%96%B0%E6%89%8B%E5%85%A5%E9%97%A8/%E7%AC%AC%E4%B8%89%E9%83%A8%E5%88%86%EF%BC%9Aviews%E5%92%8Ctemplate.html)提到的，request是一个[HttpRequest](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpRequest)对象，更多关于[HttpRequest](https://docs.djangoproject.com/en/1.10/ref/request-response/#django.http.HttpRequest)对象的内容，请看[request and response documentation](https://docs.djangoproject.com/en/1.10/ref/request-response/).

After somebody votes in a question, the vote()
 view redirects to the results page for the question. Let’s write that view:

当有人对Question进行投票后，vote()视图将请求重定向到Question的结果界面。让我们来编写这个视图：

polls/views.py
```
from django.shortcuts import get_object_or_404, render


def results(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/results.html', {'question': question})
```

This is almost exactly the same as the detail()
 view from [Tutorial 3](https://docs.djangoproject.com/en/1.10/intro/tutorial03/). The only difference is the template name. We’ll fix this redundancy later.
Now, create a polls/results.html template:

这几乎和[教程3](https://run-noob.gitbooks.io/django1-10/content/%E6%96%B0%E6%89%8B%E5%85%A5%E9%97%A8/%E7%AC%AC%E4%B8%89%E9%83%A8%E5%88%86%EF%BC%9Aviews%E5%92%8Ctemplate.html)中的detail()视图一模一样，我们将在稍后解决这个冗余问题。现在，我们创建 polls/results.html 模板：

polls/templates/polls/results.html
```
<h1>{{ question.question_text }}</h1>

<ul>
{% for choice in question.choice_set.all %}
    <li>{{ choice.choice_text }} -- {{ choice.votes }} vote{{ choice.votes|pluralize }}</li>
{% endfor %}
</ul>

<a href="{% url 'polls:detail' question.id %}">Vote again?</a>
```

Now, go to /polls/1/ in your browser and vote in the question. You should see a results page that gets updated each time you vote. If you submit the form without having chosen a choice, you should see the error message.

> **Note**

> The code for our vote() view does have a small problem. It first gets the selected_choice object from the database, then computes the new value of votes, and then saves it back to the database. If two users of your website try to vote at *exactly the same time*, this might go wrong: The same value, let’s say 42, will be retrieved for votes. Then, for both users the new value of 43 is computed and saved, but 44 would be the expected value.

> This is called a *race condition*. If you are interested, you can read [Avoiding race conditions using F()](https://docs.djangoproject.com/en/1.10/ref/models/expressions/#avoiding-race-conditions-using-f)to learn how you can solve this issue.


> **注意**

> vote()视图函数的代码可能会有问题。首先它从数据库取出所选的choice对象，然后计算votes的最新值，然后保存到数据库。如果你的网站有两个用户在同一时间投票，这就会出错了：同样的值，比如说42，将被votes同时获取到，然后，对两个用户来说最终计算的值都是43，并保存了，但是实际应该是44.

> 这叫做竞态条件。如果你有兴趣，你可以阅读[Avoiding race conditions using F()](https://docs.djangoproject.com/en/1.10/ref/models/expressions/#avoiding-race-conditions-using-f)学习如何解决这个问题。

#### **Use generic views: Less code is better[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial04/#use-generic-views-less-code-is-better)**

#### **使用普通view：代码还是少点好**

The detail() (from [Tutorial 3](https://docs.djangoproject.com/en/1.10/intro/tutorial03/)) and results() views are very simple – and, as mentioned above, redundant. The index() view, which displays a list of polls, is similar.

[教程3](https://run-noob.gitbooks.io/django1-10/content/%E6%96%B0%E6%89%8B%E5%85%A5%E9%97%A8/%E7%AC%AC%E4%B8%89%E9%83%A8%E5%88%86%EF%BC%9Aviews%E5%92%8Ctemplate.html)detail()视图和results()都非常简单——并且，如上面所说，代码比较荣誉。显示投票列表的index()视图也和他们类似。

These views represent a common case of basic Web development: getting data from the database according to a parameter passed in the URL, loading a template and returning the rendered template. Because this is so common, Django provides a shortcut, called the “generic views” system.

这些视图反映基本的Web开发中的一个常见情况：根据URL中的参数从数据库中获取数据、载入模板文件然后返回渲染后的模板。 由于这种情况非常普遍，Django提供了一种叫做“generic views”的系统可以方便地进行处理。

Generic views abstract common patterns to the point where you don’t even need to write Python code to write an app.

Generic views会将常见的模式抽象化，可以使你在编写app时甚至不需要编写Python代码。

Let’s convert our poll app to use the generic views system, so we can delete a bunch of our own code. We’ll just have to take a few steps to make the conversion. We will:

1. Convert the URLconf.

2. Delete some of the old, unneeded views.

3.Introduce new views based on Django’s generic views.

Read on for details.

让我们将我们的投票应用转换成使用通用视图系统，这样我们可以删除许多我们的代码。我们仅仅需要做以下几步来完成转换： 我们将：

1. 转换URLconf

2. 删除一些旧的、不再需要的代码。

3. 引进基于Django通用视图的新视图。

请继续阅读来了解详细信息。

> **Why the code-shuffle?**

> Generally, when writing a Django app, you’ll evaluate whether generic views are a good fit for your problem, and you’ll use them from the beginning, rather than refactoring your code halfway through. But this tutorial intentionally has focused on writing the views “the hard way” until now, to focus on core concepts.

> You should know basic math before you start using a calculator.

> **为什么重构代码**

> 一般来说，当编写一个Django应用时，你应该先评估一下通用视图是否可以解决你的问题，你应该在一开始使用它，而不是进行到一半时重构代码。本教程目前为止是有意将重点放在以“艰难的方式”编写视图，这是为将重点放在核心概念上。

> 就像在使用计算器之前你需要知道基本的数学一样。

#### **Amend URLconf[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial04/#amend-urlconf)**

#### **改良的URLconf[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial04/#amend-urlconf)**

First, open the polls/urls.py URLconf and change it like so:

首先，编辑 polls/urls.py URLconf，修改成如下：

polls/urls.py

```
from django.conf.urls import url

from . import views

app_name = 'polls'
urlpatterns = [
    url(r'^$', views.IndexView.as_view(), name='index'),
    url(r'^(?P<pk>[0-9]+)/$', views.DetailView.as_view(), name='detail'),
    url(r'^(?P<pk>[0-9]+)/results/$', views.ResultsView.as_view(), name='results'),
    url(r'^(?P<question_id>[0-9]+)/vote/$', views.vote, name='vote'),
]
```

Note that the name of the matched pattern in the regexes of the second and third patterns has changed from <question_id> to <pk>.

注意在第二个和第三个模式的正则表达式中，匹配的模式的名字由<question_id> 变成 <pk>。

#### **Amend views[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial04/#amend-views)**

#### **改良的view[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial04/#amend-views)**

Next, we’re going to remove our old index, detail, and results views and use Django’s generic views instead. To do so, open the polls/views.py file and change it like so:

接下来我们将去除老的index, detail, and results 视图，并替换为Django的通用视图。要完成这些，请打开 polls/views.py，按如下修改：
polls/views.py

```
from django.shortcuts import get_object_or_404, render
from django.http import HttpResponseRedirect
from django.urls import reverse
from django.views import generic

from .models import Choice, Question


class IndexView(generic.ListView):
    template_name = 'polls/index.html'
    context_object_name = 'latest_question_list'

    def get_queryset(self):
        """Return the last five published questions."""
        return Question.objects.order_by('-pub_date')[:5]


class DetailView(generic.DetailView):
    model = Question
    template_name = 'polls/detail.html'


class ResultsView(generic.DetailView):
    model = Question
    template_name = 'polls/results.html'


def vote(request, question_id):
    ... # same as above, no changes needed.
```

We’re using two generic views here: [ListView](https://docs.djangoproject.com/en/1.10/ref/class-based-views/generic-display/#django.views.generic.list.ListView) and [DetailView](https://docs.djangoproject.com/en/1.10/ref/class-based-views/generic-display/#django.views.generic.detail.DetailView). Respectively, those two views abstract the concepts of “display a list of objects” and “display a detail page for a particular type of object.”

这里我们用了两个通用视图： [ListView](https://docs.djangoproject.com/en/1.10/ref/class-based-views/generic-display/#django.views.generic.list.ListView) 和[DetailView](https://docs.djangoproject.com/en/1.10/ref/class-based-views/generic-display/#django.views.generic.detail.DetailView)。这两个视图分别抽象“显示一个对象列表”和“显示一个特定类型对象的详细信息页面”这两种概念。

+ Each generic view needs to know what model it will be acting upon. This is provided using the model attribute.

+ 每个通用视图需要知道它将作用于哪个模型。 这由model 属性提供。

+ The [DetailView](https://docs.djangoproject.com/en/1.10/ref/class-based-views/generic-display/#django.views.generic.detail.DetailView) generic view expects the primary key value captured from the URL to be called "pk", so we’ve changed question_id to pk for the generic views.

[DetailView
](http://python.usyiyi.cn/documents/django_182/ref/class-based-views/generic-display.html#django.views.generic.detail.DetailView)期望从URL中捕获名为"pk"
的主键值，因此我们把polls/urls.py中question_id改成了pk以使通用视图可以找到主键值 。

By default, the [DetailView](https://docs.djangoproject.com/en/1.10/ref/class-based-views/generic-display/#django.views.generic.detail.DetailView) generic view uses a template called <app name>/<model name>_detail.html. In our case, it would use the template "polls/question_detail.html". The template_name attribute is used to tell Django to use a specific template name instead of the autogenerated default template name. We also specify the template_name for the results
 list view – this ensures that the results view and the detail view have a different appearance when rendered, even though they’re both a [DetailView](https://docs.djangoproject.com/en/1.10/ref/class-based-views/generic-display/#django.views.generic.detail.DetailView) behind the scenes.

默认情况下，通用视图 [DetailView](https://docs.djangoproject.com/en/1.10/ref/class-based-views/generic-display/#django.views.generic.detail.DetailView)  使用一个叫做<app name>/<model name>_detail.html的模板。
在我们的例子中，它是 "polls/question_detail.html"模板。template_name属性是用来告诉Django使用一个指定的模板名字，而不是自动生成的默认名字。 我们也为results列表视图指定了template_name —— 这确保results视图和detail视图在渲染时具有不同的外观，即使它们在后台都是同一个 [DetailView](https://docs.djangoproject.com/en/1.10/ref/class-based-views/generic-display/#django.views.generic.detail.DetailView) 。

Similarly, the [ListView](https://docs.djangoproject.com/en/1.10/ref/class-based-views/generic-display/#django.views.generic.list.ListView) generic view uses a default template called <app name>/<model name>_list.html; we use template_name to tell [ListView](https://docs.djangoproject.com/en/1.10/ref/class-based-views/generic-display/#django.views.generic.list.ListView) to use our existing "polls/index.html" template.

类似地，[ListView](https://docs.djangoproject.com/en/1.10/ref/class-based-views/generic-display/#django.views.generic.list.ListView) 使用一个叫做<app name>/<model name>_list.html
的默认模板；我们使用template_name 来告诉[ListView](https://docs.djangoproject.com/en/1.10/ref/class-based-views/generic-display/#django.views.generic.list.ListView)  使用我们自己的"polls/index.html"模板。

In previous parts of the tutorial, the templates have been provided with a context that contains the question and latest_question_list context variables. For DetailView the question variable is provided automatically – since we’re using a Django model (Question), Django is able to determine an appropriate name for the context variable. However, for ListView, the automatically generated context variable is question_list. To override this we provide the context_object_name attribute, specifying that we want to use latest_question_list instead. As an alternative approach, you could change your templates to match the new default context variables – but it’s a lot easier to just tell Django to use the variable you want.

在之前的教程中，提供模板文件时都带有一个包含question 和 latest_question_list 变量的context。对于DetailView ，question变量会自动提供—— 因为我们使用Django 的模型 (Question)， Django 能够为context 变量决定一个合适的名字。然而对于ListView， 自动生成的context 变量是question_list。为了覆盖这个行为，我们提供 context_object_name 属性，表示我们想使用latest_question_list。作为一种替换方案，你可以改变你的模板来匹配新的context变量 —— 但直接告诉Django使用你想要的变量会省事很多。

Run the server, and use your new polling app based on generic views.

启动服务器，使用一下基于通用视图的新投票应用。

For full details on generic views, see the [generic views documentation](https://docs.djangoproject.com/en/1.10/topics/class-based-views/).

更多关于通用视图的详细信息，请查看 [通用视图文档](https://docs.djangoproject.com/en/1.10/topics/class-based-views/).

When you’re comfortable with forms and generic views, read [part 5 of this tutorial](https://docs.djangoproject.com/en/1.10/intro/tutorial05/) to learn about testing our polls app.

当你熟悉了表单和通用视图，请看[教程5](https://run-noob.gitbooks.io/django1-10/content/%E6%96%B0%E6%89%8B%E5%85%A5%E9%97%A8/%E7%AC%AC%E4%BA%94%E9%83%A8%E5%88%86%EF%BC%9ATesting.html)学习如何测试我们的投票应用

