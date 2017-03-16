## **Writing your first Django app, part 5[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#writing-your-first-django-app-part-5)**

This tutorial begins where [Tutorial 4](https://docs.djangoproject.com/en/1.10/intro/tutorial04/) left off. We’ve built a Web-poll application, and we’ll now create some automated tests for it.

紧接着[教程4](https://run-noob.gitbooks.io/django-chinese-docs-1-10/content/First%20steps/Writing%20your%20%EF%AC%81rst%20Django%20app%20part%204.html)。我们已经创建了一个投票的web应用，现在我们为它创建一些自动化测试用例。

### **Introducing automated testing[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#introducing-automated-testing)**

### **自动化测试简介**

#### **What are automated tests?[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#what-are-automated-tests)**

#### **什么是自动化测试**

Tests are simple routines that check the operation of your code.

测试是检查你的代码是否能正常运行。

Testing operates at different levels. Some tests might apply to a tiny detail (*does a particular model method return values as expected?*) while others examine the overall operation of the software (*does a sequence of user inputs on the site produce the desired result?*). That’s no different from the kind of testing you did earlier in [Tutorial 2](https://docs.djangoproject.com/en/1.10/intro/tutorial02/), using the[shell](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-shell) to examine the behavior of a method, or running the application and entering data to check how it behaves.

测试可以划分为不同的级别。 一些测试可能专注于小细节（*某一个模型的方法是否会返回预期的值？*）， 其他的测试可能会检查软件的整体运行是否正常（*用户在对网站进行了一系列的操作后，是否返回了正确的结果？*）。这些其实和你早前在[教程2](https://run-noob.gitbooks.io/django1-10/content/%E6%96%B0%E6%89%8B%E5%85%A5%E9%97%A8/%E7%AC%AC%E4%BA%8C%E9%83%A8%E5%88%86%EF%BC%9Amodels%E5%92%8Cadmin.html)做的差不多， 使用[shell](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-shell) 来检测一个方法的行为，或者运行程序并输入数据来检查它的行为方式。

What’s different in *automated* tests is that the testing work is done for you by the system. You create a set of tests once, and then as you make changes to your app, you can check that your code still works as you originally intended, without having to perform time consuming manual testing.

自动化测试的不同之处就在于这些测试会由系统来帮你完成。你创建了一组测试程序，当你修改了你的应用，你就可以用这组测试程序来检查你的代码是否仍然同预期的那样运行，而无需执行耗时的手动测试。

#### **Why you need to create tests[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#why-you-need-to-create-tests)**
#### **你为什么需要测试**

So why create tests, and why now?

那么为什么要测试，而且是现在？

You may feel that you have quite enough on your plate just learning Python/Django, and having yet another thing to learn and do may seem overwhelming and perhaps unnecessary. After all, our polls application is working quite happily now; going through the trouble of creating automated tests is not going to make it work any better. If creating the polls application is the last bit of Django programming you will ever do, then true, you don’t need to know how to create automated tests. But, if that’s not the case, now is an excellent time to learn.

你可能感觉学习Python/Django已经足够，再去学习其他的东西也许需要付出巨大的努力而且没有必要。 毕竟，我们的投票应用已经运行良好了； 将时间运用在自动化测试上，也并不能改进我们的应用。 如果你学习Django就是为了创建一个投票应用，那么创建自动化测试显然没有必要。 但如果不是这样，现在是一个很好的学习机会。

#### **Tests will save you time[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#tests-will-save-you-time)**


#### **测试将节省你的时间**

Up to a certain point, ‘checking that it seems to work’ will be a satisfactory test. In a more sophisticated application, you might have dozens of complex interactions between components.

在某种程度上， ‘确认它似乎在正常运行’，对测试来说是一种令人满意的事情。 在更复杂的应用中，你可能有几十种组件之间的复杂的交互行为。

A change in any of those components could have unexpected consequences on the application’s behavior. Checking that it still ‘seems to work’ could mean running through your code’s functionality with twenty different variations of your test data just to make sure you haven’t broken something - not a good use of your time.

这些组件的任何一个小的变化，都可能对应用造成意想不到的影响。 确定它‘似乎正常工作’可能意味着你需要运用二十种不同的测试数据测试你的代码功能，来确保你没有搞砸某些事情 —— 这会浪费很多时间。

That’s especially true when automated tests could do this for you in seconds. If something’s gone wrong, tests will also assist in identifying the code that’s causing the unexpected behavior.

自动化测试只需要数秒就可以完成以上的任务。 如果出现了错误，测试程序还能够帮助找出引发这个异常行为的代码。

Sometimes it may seem a chore to tear yourself away from your productive, creative programming work to face the unglamorous and unexciting business of writing tests, particularly when you know your code is working properly.

有时候你可能会觉得这单调乏味、无趣的编写测试程序简直像家务活，让你脱离了有价值的、创造性的编程工作，尤其是当你知道你的代码能工作正常。

However, the task of writing tests is a lot more fulfilling than spending hours testing your application manually or trying to identify the cause of a newly-introduced problem.

而，比起用几个小时的时间来手动测试你的程序，或者试图找出代码中一个新引入的问题的原因，编写测试程序还是令人惬意的。

#### **Tests don’t just identify problems, they prevent them[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#tests-don-t-just-identify-problems-they-prevent-them)**

It’s a mistake to think of tests merely as a negative aspect of development.

将测试工作仅仅当成开发过程中的负担，是错误的

Without tests, the purpose or intended behavior of an application might be rather opaque. Even when it’s your own code, you will sometimes find yourself poking around in it trying to find out what exactly it’s doing.

没有测试，应用程序的目的和行为将会变得相当模糊。 甚至即使是你自己的代码，有时候你也需要费很大力气去搞懂它到底干了些什么。

Tests change that; they light up your code from the inside, and when something goes wrong, they focus light on the part that has gone wrong - *even if you hadn’t even realized it had gone wrong*.

测试改变了这一切； 它们使你的代码内部变得明晰，当错误出现后，它们会明确地指出哪部分代码出了问题 —— 甚至你自己都不会料到问题会出现在那里。

#### **Tests make your code more attractive[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#tests-make-your-code-more-attractive)**

#### **让你的代码更有魅力**

You might have created a brilliant piece of software, but you will find that many other developers will simply refuse to look at it because it lacks tests; without tests, they won’t trust it. Jacob Kaplan-Moss, one of Django’s original developers, says “Code without tests is broken by design.”

你可能开发过堪称辉煌的软件，但是你会发现许多其他的开发者会由于它缺少测试程序而拒绝查看它一眼；没有测试程序，他们不会信任它。 Jacob Kaplan-Moss，Django最初的几个开发者之一，说过“不具有测试程序的代码是设计上的错误。”


That other developers want to see tests in your software before they take it seriously is yet another reason for you to start writing tests.

你需要开始编写测试的另一个原因就是其他的开发者在他们深入使用你的程序前可能想要查看一下它有没有测试。

#### **Tests help teams work together[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#tests-help-teams-work-together)**

#### **测试有助于团队合作**

The previous points are written from the point of view of a single developer maintaining an application. Complex applications will be maintained by teams. Tests guarantee that colleagues don’t inadvertently break your code (and that you don’t break theirs without knowing). If you want to make a living as a Django programmer, you must be good at writing tests!

之前的观点是从单个开发人员来维护一个程序这个方向来阐述的。 复杂的应用将会被一个团队来维护。 测试能够减少同事在无意间破坏你的代码的机会（和你在不知情的情况下破坏别人的代码的机会）。 如果你想在团队中做一个好的Django开发者，你必须擅长测试！

#### **Basic testing strategies[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#basic-testing-strategies)**

There are many ways to approach writing tests.

编写测试有很多种方法。

Some programmers follow a discipline called “[test-driven development](https://en.wikipedia.org/wiki/Test-driven_development)”; they actually write their tests before they write their code. This might seem counter-intuitive, but in fact it’s similar to what most people will often do anyway: they describe a problem, then create some code to solve it. Test-driven development simply formalizes the problem in a Python test case.


一些开发者遵循一种叫做“[test-driven development](https://en.wikipedia.org/wiki/Test-driven_development)”的规则；
他们在编写代码前会先编好测试。 
这似乎与直觉不符，尽管这种方法与大多数人经常的做法很相似：人们先描述一个问题，然后创建一些代码来解决这个问题。 
测试驱动开发可以用Python测试用例将这个问题简单地形式化。

More often, a newcomer to testing will create some code and later decide that it should have some tests. Perhaps it would have been better to write some tests earlier, but it’s never too late to get started.

更常见的情况是，刚接触测试的人会先编写一些代码，然后才决定为这些代码创建一些测试。也许在之前就编写一些测试会好一点，但什么时候开始都不算晚。

Sometimes it’s difficult to figure out where to get started with writing tests. If you have written several thousand lines of Python, choosing something to test might not be easy. In such a case, it’s fruitful to write your first test the next time you make a change, either when you add a new feature or fix a bug.

有时候很难解决从什么地方开始编写测试。 
如果你已经编写了数千行Python代码，挑出一些功能去测试是很困难的。
这种情况下，在下次你对代码进行变更，或者添加一个新功能或者修复一个bug时，编写你的第一个测试，效果会非常好。

So let’s do that right away.

现在，让我们马上来编写一个测试。

#### **Writing our first test[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#writing-our-first-test)**

#### **编写我们的第一个测试**

##### **We identify a bug[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#we-identify-a-bug)**

##### **我们找出一个错误**

Fortunately, there’s a little bug in the polls application for us to fix right away: the Question.was_published_recently() method returns True if the Question was published within the last day (which is correct) but also if the Question’s pub_date
 field is in the future (which certainly isn’t).

幸运的是，polls应用中有一个小错误让我们可以马上来修复它：如果Question在最近一天发布，Question.was_published_recently() 方法返回True（这是对的），但是如果Question的pub_date 字段是在未来，它还返回True（这肯定是不对的）。

To check if the bug really exists, using the Admin create a question whose date lies in the future and check the method using the [shell
](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-shell):

你可以在管理站点中看到这一点； 创建一个发布时间在未来的一个Question； 你可以看到Question 的变更列表声称它是最近发布的。你还可以使用[shell](http://python.usyiyi.cn/documents/django_182/ref/django-admin.html#django-admin-shell)看到这点：

```
>>> import datetime
>>> from django.utils import timezone
>>> from polls.models import Question
>>> # create a Question instance with pub_date 30 days in the future
>>> future_question = Question(pub_date=timezone.now() + datetime.timedelta(days=30))
>>> # was it published recently?
>>> future_question.was_published_recently()
True
```

Since things in the future are not ‘recent’, this is clearly wrong.

将来的事情并不能称之为‘最近’，这确实是一个错误。

#### **Create a test to expose the bug[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#create-a-test-to-expose-the-bug)**

#### **创建一个测试来暴露这个bug**

What we’ve just done in the [shell](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-shell) to test for the problem is exactly what we can do in an automated test, so let’s turn that into an automated test.

我们需要在自动化测试里做的和刚才在 [shell](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-shell) 里做的差不多，让我们来将它转换成一个自动化测试。

A conventional place for an application’s tests is in the application’s tests.py file; the testing system will automatically find tests in any file whose name begins with test.

应用的测试用例安装惯例一般放在该应用的tests.py文件中；测试系统将自动在任何以test开头的文件中查找测试用例。 

Put the following in the tests.py file in the polls application:

将下面的代码放入polls应用下的tests.py文件中：

polls/tests.py
```
import datetime

from django.utils import timezone
from django.test import TestCase

from .models import Question


class QuestionMethodTests(TestCase):

    def test_was_published_recently_with_future_question(self):
        """
        was_published_recently() should return False for questions whose
        pub_date is in the future.
        """
        time = timezone.now() + datetime.timedelta(days=30)
        future_question = Question(pub_date=time)
        self.assertEqual(future_question.was_published_recently(), False)
```

What we have done here is created a [django.test.TestCase](https://docs.djangoproject.com/en/1.10/topics/testing/tools/#django.test.TestCase) subclass with a method that creates a Question
instance with a pub_date in the future. We then check the output of was_published_recently() - which *ought* to be False.

我们在这里做的是创建一个[django.test.TestCase](https://docs.djangoproject.com/en/1.10/topics/testing/tools/#django.test.TestCase) 子类，它具有一个方法可以创建一个pub_date在未来的Question实例。然后我们检查was_published_recently()的输出 —— 它*应该*是 False.

#### **Running tests[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#running-tests)**

#### **运行测试[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#running-tests)**

In the terminal, we can run our test:

在终端上，运行我们的测试：

```
python manage.py test polls
```

and you’ll see something like:

```
Creating test database for alias 'default'...
F
======================================================================
FAIL: test_was_published_recently_with_future_question (polls.tests.QuestionMethodTests)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/path/to/mysite/polls/tests.py", line 16, in test_was_published_recently_with_future_question
    self.assertIs(future_question.was_published_recently(), False)
AssertionError: True is not False
----------------------------------------------------------------------
Ran 1 test in 0.001s

FAILED (failures=1)
Destroying test database for alias 'default'...
```

What happened is this:

+ python manage.py test polls looked for tests in he polls application

+ it found a subclass of the [django.test.TestCase](https://docs.djangoproject.com/en/1.10/topics/testing/+ tools/#django.test.TestCase) class

+ it created a special database for the purpose of testing

+ it looked for test methods - ones whose names begin with test
 

+ in test_was_published_recently_with_future_question

+  it created a Question instance whose pub_date field is 30 days in the future

+ ... and using the assertIs() method, it discovered that its was_published_recently() returns True, though we wanted it to return False

The test informs us which test failed and even the line on which the failure occurred.

发生了如下事情:

+ python manage.py test polls 查找polls下的测试用例

+ 它找到 [django.test.TestCase](https://docs.djangoproject.com/en/1.10/topics/testing/+ tools/#django.test.TestCase) 子类

+ 它为测试创建了一个特定的数据库

+ 它查找用于测试的方法 —— 名字以test开始
 
+ 它运行test_was_published_recently_with_future_question创建一个pub_date为未来30天的 Question实例

+ ... 然后利用assertEqual()方法，它发现was_published_recently() 返回True，尽管我们希望它返回False

这个测试通知我们哪个测试失败，甚至是错误出现在哪一行。

#### **Fixing the bug[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#fixing-the-bug)**

#### **修复这个bug[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#fixing-the-bug)**

We already know what the problem is: Question.was_published_recently() should return False
 if its pub_date is in the future. Amend the method in models.py, so that it will only return True if the date is also in the past:

我们已经知道问题是什么：Question.was_published_recently() 应该返回 False，如果它的pub_date是在未来。在models.py中修复这个方法，让它只有当日期是在过去时才返回True ：

polls/models.py
```
def was_published_recently(self):
    now = timezone.now()
    return now - datetime.timedelta(days=1) <= self.pub_date <= now
```

and run the test again:
```
Creating test database for alias 'default'...
.
----------------------------------------------------------------------
Ran 1 test in 0.001s

OK
Destroying test database for alias 'default'...
```

After identifying a bug, we wrote a test that exposes it and corrected the bug in the code so our test passes.

在找出一个错误之后，我们编写一个测试来暴露这个错误，然后在代码中更正这个错误让我们的测试通过。

Many other things might go wrong with our application in the future, but we can be sure that we won’t inadvertently reintroduce this bug, because simply running the test will warn us immediately. We can consider this little portion of the application pinned down safely forever.

未来，我们的应用可能会出许多其它的错误，但是我们可以保证我们不会无意中再次引入这个错误，因为简单地运行一下这个测试就会立即提醒我们。我们可以认为这个应用的这一小部分会永远安全了。

#### **More comprehensive tests[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#more-comprehensive-tests)**
While we’re here, we can further pin down the was_published_recently() method; in fact, it would be positively embarrassing if in fixing one bug we had introduced another.

在这里，我们可以使was_published_recently() 方法更加稳定；事实上，在修复一个错误的时候引入一个新的错误将是一件很令人尴尬的事。

Add two more test methods to the same class, to test the behavior of the method more comprehensively:

在同一个类中添加两个其它的测试方法，来更加综合地测试这个方法：

polls/tests.py
```
def test_was_published_recently_with_old_question(self):
    """
    was_published_recently() should return False for questions whose
    pub_date is older than 1 day.
    """
    time = timezone.now() - datetime.timedelta(days=30)
    old_question = Question(pub_date=time)
    self.assertEqual(old_question.was_published_recently(), False)

def test_was_published_recently_with_recent_question(self):
    """
    was_published_recently() should return True for questions whose
    pub_date is within the last day.
    """
    time = timezone.now() - datetime.timedelta(hours=1)
    recent_question = Question(pub_date=time)
    self.assertEqual(recent_question.was_published_recently(), True)
```

And now we have three tests that confirm that Question.was_published_recently() returns sensible values for past, recent, and future questions.

现在我们有三个测试来保证无论发布时间是在过去、现在还是未来 Question.was_published_recently()都将返回合理的数据。

Again, polls is a simple application, but however complex it grows in the future and whatever other code it interacts with, we now have some guarantee that the method we have written tests for will behave in expected ways.

再说一次，polls 应用虽然简单，但是无论它今后会变得多么复杂以及会和多少其它的应用产生相互作用，我们都能保证我们刚刚为它编写过测试的那个方法会按照预期的那样工作。

#### **Test a view[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#test-a-view)**

The polls application is fairly undiscriminating: it will publish any question, including ones whose pub_date field lies in the future. We should improve this. Setting a pub_date
 in the future should mean that the Question is published at that moment, but invisible until then.

这个投票应用没有区分能力：它将会发布任何一个Question，包括 pub_date字段位于未来。我们应该改进这一点。 设定pub_date在未来应该表示Question在未来发布，但是直到那个时间点才会变得可见。

##### **A test for a view[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#a-test-for-a-view)**

#### **测试视图[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#a-test-for-a-view)**

When we fixed the bug above, we wrote the test first and then the code to fix it. In fact that was a simple example of test-driven development, but it doesn’t really matter in which order we do the work.

当我们修复上面的错误时，我们先写测试，然后修改代码来修复它。 事实上，这是测试驱动开发的一个简单的例子，但做的顺序并不真的重要。

In our first test, we focused closely on the internal behavior of the code. For this test, we want to check its behavior as it would be experienced by a user through a web browser.


在我们的第一个测试中，我们专注于代码内部的行为。 在这个测试中，我们想要通过浏览器从用户的角度来检查它的行为。

Before we try to fix anything, let’s have a look at the tools at our disposal.

在我们试着修复任何事情之前，让我们先查看一下我们能用到的工具。
#### **The Django test client[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#the-django-test-client)**

Django provides a test [Client](https://docs.djangoproject.com/en/1.10/topics/testing/tools/#django.test.Client) to simulate a user interacting with the code at the view level. We can use it in tests.py or even in the [shell](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-shell).

Django提供了一个测试客户端[Client](https://docs.djangoproject.com/en/1.10/topics/testing/tools/#django.test.Client)来模拟用户和视图函数代码的交互，我们可以在tests.py中使用，甚至在[shell](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-shell)中使用。

We will start again with the [shell](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-shell), where we need to do a couple of things that won’t be necessary in tests.py. The first is to set up the test environment in the [shell](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-shell):

我们再次启动[shell](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-shell)，但是在shell中我们需要做很多在tests.py中不必做的事。首先是在[shell](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-shell)中设置测试环境：

```
>>> from django.test.utils import setup_test_environment
>>> setup_test_environment()
```

[setup_test_environment()](https://docs.djangoproject.com/en/1.10/topics/testing/advanced/#django.test.utils.setup_test_environment) installs a template renderer which will allow us to examine some additional attributes on responses such as response.context that otherwise wouldn’t be available. Note that this method *does not* setup a test database, so the following will be run against the existing database and the output may differ slightly depending on what questions you already created. You might get unexpected results if your TIME_ZONE in settings.py isn’t correct. If you don’t remember setting it earlier, check it before continuing.

[setup_test_environment()](https://docs.djangoproject.com/en/1.10/topics/testing/advanced/#django.test.utils.setup_test_environment)安装一个模板渲染器，可以使我们来检查响应的一些额外属性比如response.context，否则是访问不到的。请注意，这种方法不会建立一个测试数据库，所以以下命令将运行在现有的数据库上，输出的内容也会根据你已经创建的Question不同而稍有不同。如果你的TIME_ZONE 设置不正确，得出的结果可能会跟预期不一致。如果之前你忘记设置了，测试开始前请先确认一下。


Next we need to import the test client class (later in tests.py
 we will use the [django.test.TestCase](https://docs.djangoproject.com/en/1.10/topics/testing/tools/#django.test.TestCase) class, which comes with its own client, so this won’t be required):

下一步我们需要导入测试客户端类（在之后的tests.py
 中，我们将使用 [django.test.TestCase](https://docs.djangoproject.com/en/1.10/topics/testing/tools/#django.test.TestCase) 类，它具有自己的客户端，将不需要导入这个类）：

```
>>> from django.test import Client
>>> # create an instance of the client for our use
>>> client = Client()
```

With that ready, we can ask the client to do some work for us:
```
>>> # get a response from '/'
>>> response = client.get('/')
>>> # we should expect a 404 from that address
>>> response.status_code
404
>>> # on the other hand we should expect to find something at '/polls/'
>>> # we'll use 'reverse()' rather than a hardcoded URL
>>> from django.urls import reverse
>>> response = client.get(reverse('polls:index'))
>>> response.status_code
200
>>> response.content
b'\n    <ul>\n    \n        <li><a href="/polls/1/">What&#39;s up?</a></li>\n    \n    </ul>\n\n'
>>> # If the following doesn't work, you probably omitted the call to
>>> # setup_test_environment() described above
>>> response.context['latest_question_list']
<QuerySet [<Question: What's up?>]>
```

#### **Improving our view[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#improving-our-view)**

The list of polls shows polls that aren’t published yet (i.e. those that have a pub_date in the future). Let’s fix that.In [Tutorial 4](https://docs.djangoproject.com/en/1.10/intro/tutorial04/) we introduced a class-based view, based on [ListView](https://docs.djangoproject.com/en/1.10/ref/class-based-views/generic-display/#django.views.generic.list.ListView):

投票的列表中包含了那些还没有发布的投票（即pub_date在未来的投票）。让我们来修复它。在[教程4](https://run-noob.gitbooks.io/django1-10/content/%E6%96%B0%E6%89%8B%E5%85%A5%E9%97%A8/%E7%AC%AC%E5%9B%9B%E9%83%A8%E5%88%86%EF%BC%9AForms%E5%92%8Cgeneric%20views.html)中我们引入了基于[ListView](https://docs.djangoproject.com/en/1.10/ref/class-based-views/generic-display/#django.views.generic.list.ListView)的通用类视图：

polls/views.py 
```
class IndexView(generic.ListView):
    template_name = 'polls/index.html'
    context_object_name = 'latest_question_list'

    def get_queryset(self):
        """Return the last five published questions."""
        return Question.objects.order_by('-pub_date')[:5]
```

We need to amend the get_queryset() method and change it so that it also checks the date by comparing it with timezone.now(). First we need to add an import:

我们需要修改get_queryset方法并让它将日期与timezone.now()进行比较。首先我们需要添加一行导入：

polls/views.py
```
from django.utils import timezone
and then we must amend the get_queryset method like so:
```

polls/views.py
```
def get_queryset(self):
    """
    Return the last five published questions (not including those set to be
    published in the future).
    """
    return Question.objects.filter(
        pub_date__lte=timezone.now()
    ).order_by('-pub_date')[:5]
```

Question.objects.filter(pub_date__lte=timezone.now()) returns a queryset containing Questions whose pub_date is less than or equal to - that is, earlier than or equal to - timezone.now.

Question.objects.filter(pub_date__lte=timezone.now()) 返回一个查询集，包含pub_date小于等于timezone.now的Question。

#### **Testing our new view[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#testing-our-new-view)**

#### **测试我们的视图[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#testing-our-new-view)**

Now you can satisfy yourself that this behaves as expected by firing up the runserver, loading the site in your browser, creating Questions with dates in the past and future, and checking that only those that have been published are listed. You don’t want to have to do that *every single time you make any change that might affect this* - so let’s also create a test, based on our [shell](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-shell) session above.

启动服务器、在浏览器中载入站点、创建一些发布时间在过去和将来的Questions，然后检验只有已经发布的Question会展示出来，至此，你应该感到满意了。你不想*每次修改可能与这相关的代码时都重复这样做* —— 所以让我们基于以上[shell](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-shell)会话中的内容，再编写一个测试。

Add the following to polls/tests.py:

将下面的代码添加到polls/tests.py：

polls/tests.py
```
from django.urls import reverse 
```

and we’ll create a shortcut function to create questions as well as a new test class:

polls/tests.py
```
def create_question(question_text, days):
    """
    Creates a question with the given `question_text` and published the
    given number of `days` offset to now (negative for questions published
    in the past, positive for questions that have yet to be published).
    """
    time = timezone.now() + datetime.timedelta(days=days)
    return Question.objects.create(question_text=question_text, pub_date=time)


class QuestionViewTests(TestCase):
    def test_index_view_with_no_questions(self):
        """
        If no questions exist, an appropriate message should be displayed.
        """
        response = self.client.get(reverse('polls:index'))
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, "No polls are available.")
        self.assertQuerysetEqual(response.context['latest_question_list'], [])

    def test_index_view_with_a_past_question(self):
        """
        Questions with a pub_date in the past should be displayed on the
        index page.
        """
        create_question(question_text="Past question.", days=-30)
        response = self.client.get(reverse('polls:index'))
        self.assertQuerysetEqual(
            response.context['latest_question_list'],
            ['<Question: Past question.>']
        )

    def test_index_view_with_a_future_question(self):
        """
        Questions with a pub_date in the future should not be displayed on
        the index page.
        """
        create_question(question_text="Future question.", days=30)
        response = self.client.get(reverse('polls:index'))
        self.assertContains(response, "No polls are available.")
        self.assertQuerysetEqual(response.context['latest_question_list'], [])

    def test_index_view_with_future_question_and_past_question(self):
        """
        Even if both past and future questions exist, only past questions
        should be displayed.
        """
        create_question(question_text="Past question.", days=-30)
        create_question(question_text="Future question.", days=30)
        response = self.client.get(reverse('polls:index'))
        self.assertQuerysetEqual(
            response.context['latest_question_list'],
            ['<Question: Past question.>']
        )

    def test_index_view_with_two_past_questions(self):
        """
        The questions index page may display multiple questions.
        """
        create_question(question_text="Past question 1.", days=-30)
        create_question(question_text="Past question 2.", days=-5)
        response = self.client.get(reverse('polls:index'))
        self.assertQuerysetEqual(
            response.context['latest_question_list'],
            ['<Question: Past question 2.>', '<Question: Past question 1.>']
        )
```

Let’s look at some of these more closely.

我们详细解释一下

First is a question shortcut function, create_question, to take some repetition out of the process of creating questions.

首先是question的快捷函数create_question，封装了创建Question的重复步骤

test_index_view_with_no_questions doesn’t create any questions, but checks the message: “No polls are available.” and verifies the latest_question_list is empty. Note that the [django.test.TestCase](https://docs.djangoproject.com/en/1.10/topics/testing/tools/#django.test.TestCase) class provides some additional assertion methods. In these examples, we use [assertContains()](https://docs.djangoproject.com/en/1.10/topics/testing/tools/#django.test.SimpleTestCase.assertContains) and[assertQuerysetEqual()](https://docs.djangoproject.com/en/1.10/topics/testing/tools/#django.test.TransactionTestCase.assertQuerysetEqual).

test_index_view_with_no_questions 并不会创建人和的Questions，但是会检查“No polls are available.” 信息并且验证latest_question_list 是空。注意 [django.test.TestCase](https://docs.djangoproject.com/en/1.10/topics/testing/tools/#django.test.TestCase)类还封装了其他的assertion方法，在这里，我们用了 [assertContains()](https://docs.djangoproject.com/en/1.10/topics/testing/tools/#django.test.SimpleTestCase.assertContains) and[assertQuerysetEqual()](https://docs.djangoproject.com/en/1.10/topics/testing/tools/#django.test.TransactionTestCase.assertQuerysetEqual).

In test_index_view_with_a_past_question, we create a question and verify that it appears in the list.

test_index_view_with_a_past_question函数中，我们创建了一个Question然后验证它是否在列表中

In test_index_view_with_a_future_question, we create a question with a pub_date in the future. The database is reset for each test method, so the first question is no longer there, and so again the index shouldn’t have any questions in it.

在test_index_view_with_a_future_question中，我们创建一个pub_date 在未来的Question。数据库会为每一个测试方法进行重置，所以第一个Question已经不在那里，因此首页面里不应该有任何Question。

And so on. In effect, we are using the tests to tell a story of admin input and user experience on the site, and checking that at every state and for every new change in the state of the system, the expected results are published.

等等。 事实上，我们是在用测试模拟站点上的管理员输入和用户体验，检查针对系统每一个状态和状态的新变化，发布的是预期的结果。

#### **Testing the DetailView[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#testing-the-detailview)**

#### **测试DetailView[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#testing-the-detailview)**

What we have works well; however, even though future questions don’t appear in the *index*, users can still reach them if they know or guess the right URL. So we need to add a similar constraint to DetailView:

现在一切都还顺利；但是即使未来发布的Question不会出现在index中，如果用户知道或者猜出正确的URL依然可以访问它们。所以我们需要给DetailView添加一个这样的约束：

polls/views.py
```
class DetailView(generic.DetailView):
    ...
    def get_queryset(self):
        """
        Excludes any questions that aren't published yet.
        """
        return Question.objects.filter(pub_date__lte=timezone.now())
```

And of course, we will add some tests, to check that a Question whose pub_date is in the past can be displayed, and that one with a pub_date in the future is not:

当然，我们将增加一些测试来检验pub_date 在过去的Question 可以显示出来，而pub_date在未来的不可以：

polls/tests.py
```
class QuestionIndexDetailTests(TestCase):
    def test_detail_view_with_a_future_question(self):
        """
        The detail view of a question with a pub_date in the future should
        return a 404 not found.
        """
        future_question = create_question(question_text='Future question.', days=5)
        url = reverse('polls:detail', args=(future_question.id,))
        response = self.client.get(url)
        self.assertEqual(response.status_code, 404)

    def test_detail_view_with_a_past_question(self):
        """
        The detail view of a question with a pub_date in the past should
        display the question's text.
        """
        past_question = create_question(question_text='Past Question.', days=-5)
        url = reverse('polls:detail', args=(past_question.id,))
        response = self.client.get(url)
        self.assertContains(response, past_question.question_text)
```

#### **Ideas for more tests[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#ideas-for-more-tests)**

We ought to add a similar get_queryset method to ResultsView and create a new test class for that view. It’ll be very similar to what we have just created; in fact there will be a lot of repetition.

我们应该添加一个类似get_queryset的方法到ResultsView并为该视图创建一个新的类。这将与我们刚刚创建的非常类似；实际上将会有许多重复。

We could also improve our application in other ways, adding tests along the way. For example, it’s silly that Questions can be published on the site that have no Choices. So, our views could check for this, and exclude such Questions. Our tests would reate a Question without Choices and then test that it’s not published, as well as create a similar Question *with* Choices, and test that it *is* published.

我们还可以在其它方面改进我们的应用，并随之不断增加测试。例如，发布一个没有Choices的Questions就显得很愚蠢。所以，我们的视图应该检查这点并排除这些 Questions。我们的测试应该创建一个不带Choices 的 Question然后测试它不会发布出来， 同时创建一个类似的带有 Choices的Question 并验证它会 发布出来。

Perhaps logged-in admin users should be allowed to see unpublished Questions, but not ordinary visitors. Again: whatever needs to be added to the software to accomplish this should be accompanied by a test, whether you write the test first and then make the code pass the test, or work out the logic in your code first and then write a test to prove it.

也许登陆的用户应该被允许查看还没发布的 Questions，但普通游客不行。 再说一次：无论添加什么代码来完成这个要求，需要提供相应的测试代码，无论你是否是先编写测试然后让这些代码通过测试，还是先用代码解决其中的逻辑然后编写测试来证明它。

At a certain point you are bound to look at your tests and wonder whether your code is suffering from test bloat, which brings us to:

从某种程度上来说，你一定会查看你的测试，然后想知道是否你的测试程序过于臃肿，这将我们带向下面的内容：

#### **When testing, more is better[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#when-testing-more-is-better)**

It might seem that our tests are growing out of control. At this rate there will soon be more code in our tests than in our application, and the repetition is unaesthetic, compared to the elegant conciseness of the rest of our code.

看起来我们的测试代码的增长正在失去控制。 以这样的速度，测试的代码量将很快超过我们的应用，对比我们其它优美简洁的代码，重复毫无美感。

**It doesn’t matter**. Let them grow. For the most part, you can write a test once and then forget about it. It will continue performing its useful function as you continue to develop your program.

没关系。让它们继续增长。最重要的是，你可以写一个测试一次，然后忘了它。 当你继续开发你的程序时，它将继续执行有用的功能。

Sometimes tests will need to be updated. Suppose that we amend our views so that only Questions with Choices are published. In that case, many of our existing tests will fail - *telling us exactly which tests need to be amended to bring them up to date*, so to that extent tests help look after themselves.

有时，测试需要更新。 假设我们修改我们的视图使得只有具有Choices的 Questions 才会发布。在这种情况下，我们许多已经存在的测试都将失败 —— 这会告诉我们哪些测试需要被修改来使得它们保持最新，所以从某种程度上讲，测试可以自己照顾自己。

At worst, as you continue developing, you might find that you have some tests that are now redundant. Even that’s not a problem; in testing redundancy is a *good* thing.

在最坏的情况下，在你的开发过程中，你会发现许多测试现在变得冗余。 即使这样，也不是问题；对测试来说，冗余是一件好 事。

As long as your tests are sensibly arranged, they won’t become unmanageable. Good rules-of-thumb include having:

只要你的测试被合理地组织，它们就不会变得难以管理。 从经验上来说，好的做法是：

+ a separate TestClass for each model or view

+ 每个模型或试图具有一个单独的TestClass

+ a separate test method for each set of conditions you want to test

+ 为你想测试的各种情况创建一个单独的测试方法


+ test method names that describe their function

+ 测试方法的名字可以描述它的功能

#### **Further testing[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#further-testing)**

This tutorial only introduces some of the basics of testing. There’s a great deal more you can do, and a number of very useful tools at your disposal to achieve some very clever things.

本教程只介绍了一些基本的测试。 还有很多你可以做，有许多非常有用的工具可以随便使用来你实现一些非常有用的事情。

For example, while our tests here have covered some of the internal logic of a model and the way our views publish information, you can use an “in-browser” framework such as [Selenium](http://seleniumhq.org/) to test the way your HTML actually renders in a browser. These tools allow you to check not just the behavior of your Django code, but also, for example, of your JavaScript. It’s quite something to see the tests launch a browser, and start interacting with your site, as if a human being were driving it! Django includes [LiveServerTestCase](https://docs.djangoproject.com/en/1.10/topics/testing/tools/#django.test.LiveServerTestCase) to facilitate integration with tools like Selenium.

例如，虽然我们的测试覆盖了模型的内部逻辑和视图发布信息的方式，但你还可以使用一个“浏览器”框架例如[Selenium](http://seleniumhq.org/)来测试你的HTML文件在浏览器中真实渲染的样子。 这些工具不仅可以让你检查你的Django代码的行为，还能够检查你的JavaScript的行为。它会启动一个浏览器，并开始与你的网站进行交互，就像有一个人在操纵一样，非常值得一看！ Django 包含一个 [LiveServerTestCase](https://docs.djangoproject.com/en/1.10/topics/testing/tools/#django.test.LiveServerTestCase)来帮助与Selenium 这样的工具集成。

If you have a complex application, you may want to run tests automatically with every commit for the purposes of [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration), so that quality control is itself - at least partially - automated. A good way to spot untested parts of your application is to check code coverage. This also helps identify fragile or even dead code. If you can’t test a piece of code, it usually means that code should be refactored or removed. Coverage will help to identify dead code. See [Integration with coverage.py](https://docs.djangoproject.com/en/1.10/topics/testing/advanced/#topics-testing-code-coverage) for details.

发现你应用中未经测试的代码的一个好方法是检查测试代码的覆盖率。 这也有助于识别脆弱的甚至死代码。如果你不能测试一段代码，这通常意味着这些代码需要被重构或者移除。Coverage将帮助我们识别死代码。 查看[*与coverage.py 集成*](http://python.usyiyi.cn/documents/django_182/topics/testing/advanced.html#topics-testing-code-coverage)来了解更多细节。

[Testing in Django](https://docs.djangoproject.com/en/1.10/topics/testing/) has comprehensive information about testing.

[Testing in Django](https://docs.djangoproject.com/en/1.10/topics/testing/)有更多关于测试的信息

#### **What’s next?[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#what-s-next)**

#### **接下来怎么办?[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#what-s-next)**

For full details on testing, see [Testing in Django](https://docs.djangoproject.com/en/1.10/topics/testing/).

所有关于测试的细节，请看[Testing in Django](https://docs.djangoproject.com/en/1.10/topics/testing/).

When you’re comfortable with testing Django views, read [part 6 of this tutorial](https://docs.djangoproject.com/en/1.10/intro/tutorial06/) to learn about static files management.

如果你你对测试Django视图的已经熟悉，请看[教程6](https://run-noob.gitbooks.io/django-chinese-docs-1-10/content/First%20steps/Writing%20your%20%EF%AC%81rst%20Django%20app%20part%206.html)来学习静态文件的处理
