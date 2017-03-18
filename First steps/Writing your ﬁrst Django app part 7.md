#### **Writing your first Django app, part 7[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial07/#writing-your-first-django-app-part-7)**
This tutorial begins where [Tutorial 6](https://docs.djangoproject.com/en/1.10/intro/tutorial06/) left off. We’re continuing the Web-poll application and will focus on customizing Django’s automatically-generated admin site that we first explored in [Tutorial 2](https://docs.djangoproject.com/en/1.10/intro/tutorial02/).

紧接着[Tutorial 6](https://run-noob.gitbooks.io/django-chinese-docs-1-10/content/First%20steps/Writing%20your%20%EF%AC%81rst%20Django%20app%20part%206.html)。我们继续开发投票应用并主要关注Django自动生成的admin站点，这部分我们在[教程2](https://run-noob.gitbooks.io/django-chinese-docs-1-10/content/First%20steps/Writing%20your%20%EF%AC%81rst%20Django%20app%20part%202.html)中已经了解过。

#### **Customize the admin form[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial07/#customize-the-admin-form)**
By registering the Question model with admin.site.register(Question), Django was able to construct a default form representation. Often, you’ll want to customize how the admin form looks and works. You’ll do this by telling Django the options you want when you register the object.

通过admin.site.register(Question)语句将Question模型注册到admin，Django会自动生成该模型的默认表单。一般情况下，你需要定制表单的样式和功能。这样在你注册模型的时候就需要告诉Django你想要的选项。

Let’s see how this works by reordering the fields on the edit form. Replace the admin.site.register(Question)
 line with:

我们通过演示对表单字段的重新排序，来了解其工作原理。用以下代码替换admin.site.register(Question)语句：

polls/admin.py
```
from django.contrib import admin

from .models import Question


class QuestionAdmin(admin.ModelAdmin):
    fields = ['pub_date', 'question_text']

admin.site.register(Question, QuestionAdmin)
```

You’ll follow this pattern – create a model admin class, then pass it as the second argument to admin.site.register() – any time you need to change the admin options for a model.

一般遵循以下步骤——创建一个model admin类，然后作为admin.site.register()的第二个参数——你可以随时修改模型的admin选项。

This particular change above makes the “Publication date” come before the “Question” field:

这个操作将使“Publication date” 放在“Question” 字段之前：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2784070-99582da07d4734a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

This isn’t impressive with only two fields, but for admin forms with dozens of fields, choosing an intuitive order is an important usability detail.

只有两个字段的情况下，感觉没什么作用，但是对于有几十个字段的表单来说，采用更直观的排序方式将是一个值得引起注意的细节问题。

And speaking of forms with dozens of fields, you might want to split the form up into fieldsets:

而且对于有几十个字段的表单来说，你也许想要通过fieldsets来分隔开：

polls/admin.py

```
from django.contrib import admin

from .models import Question


class QuestionAdmin(admin.ModelAdmin):
    fieldsets = [
        (None,               {'fields': ['question_text']}),
        ('Date information', {'fields': ['pub_date']}),
    ]

admin.site.register(Question, QuestionAdmin)
```

The first element of each tuple in [fieldsets](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.fieldsets) is the title of the fieldset. Here’s what our form looks like now:

[fieldsets](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.fieldsets) 内元组的第一个字段是fieldsets的标题。我们的表单看起来应该像以下这样了：


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2784070-c73c1e0376acdc1e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### **Adding related objects[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial07/#adding-related-objects)**
OK, we have our Question admin page, but a Question
 has multiple Choices, and the admin page doesn’t display choices.

好的，我们已经有Question的admin页面了，但是一个Question有很多个Choices，admin页面也没有展示这些choices。

Yet. There are two ways to solve this problem. The first is to register Choice with the admin just as we did with Question. That’s easy:

但是，我们有两种方法来解决这个问题。第一种方法是像Question一样将Choice注册到admin站点，这非常简单：

polls/admin.py
```
from django.contrib import admin

from .models import Choice, Question
# ...
admin.site.register(Choice)
```

Now “Choices” is an available option in the Django admin. The “Add choice” form looks like this:

现在“Choices”已经展示在Django admin站点了，“增加choice”表单看起来如下图：


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2784070-0d3103e71f8b061d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

In that form, the “Question” field is a select box containing every question in the database. Django knows that a [ForeignKey](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.ForeignKey) should be represented in the admin as a <select> box. In our case, only one question exists at this point.

在这个表单中，“Question”字段是一个包含数据库中所有Question的的单选框。Django知道这是[ForeignKey](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.ForeignKey) ，应该在admin中作为单选框来展示。在我们的例子中，只有一个Question存在。

Also note the “Add Another” link next to “Question.” Every object with a ForeignKey relationship to another gets this for free. When you click “Add Another”, you’ll get a popup window with the “Add question” form. If you add a question in that window and click “Save”, Django will save the question to the database and dynamically add it as the selected choice on the “Add choice” form you’re looking at.

同时注意到Question附近的“增加另一个”的链接按钮。每个
ForeignKey 关系的对象都会自动有这个按钮。当你点击“Add Another”，会弹出一个“Add Question”表单。如果你通过那弹窗点击save增加一个Question，Django会保存这个Question到数据库并在 “Add choice”表单中增加一个可选项。

But, really, this is an inefficient way of adding Choice
 objects to the system. It’d be better if you could add a bunch of Choices directly when you create the Question
 object. Let’s make that happen.

但是，这真的不是增加Choice高效途径。当你创建一个Question的时候，如果你能增加一打choices该多好。让我们试试看。

Remove the register() call for the Choice model. Then, edit the Question registration code to read:

去掉register()调用Choice模型的代码，然后编辑Question 的注册代码，如下：

polls/admin.py
```
from django.contrib import admin

from .models import Choice, Question


class ChoiceInline(admin.StackedInline):
    model = Choice
    extra = 3


class QuestionAdmin(admin.ModelAdmin):
    fieldsets = [
        (None,               {'fields': ['question_text']}),
        ('Date information', {'fields': ['pub_date'], 'classes': ['collapse']}),
    ]
    inlines = [ChoiceInline]

admin.site.register(Question, QuestionAdmin)
```

This tells Django: “Choice objects are edited on the Question admin page. By default, provide enough fields for 3 choices.”

这告诉Django：“Choice对象可在Question的admin页面编辑。默认情况下，有3个choices字段”

Load the “Add question” page to see how that looks:

访问“Add question” 页，看看它长啥样：


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2784070-a3e8b322950c59ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

It works like this: There are three slots for related Choices – as specified by extra – and each time you come back to the “Change” page for an already-created object, you get another three extra slots.

它的工作原理如下：有3个表单组件关联Choice对象，而且当你每次回到一个已存在对象的change页面时，你又会看到三个新的表单组件

At the end of the three current slots you will find an “Add another Choice” link. If you click on it, a new slot will be added. If you want to remove the added slot, you can click on the X to the top right of the added slot. Note that you can’t remove the original three slots. This image shows an added slot:

在三个表单组件的最后，你可以看到“Add another Choice”链接，点击它后，会创建一个新的表单选项。。如果你想移除已增加的槽，点击右上角X即可。注意你不能去掉那三个原有的槽。下图展示增加一个槽：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2784070-cb35fa79051823ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

One small problem, though. It takes a lot of screen space to display all the fields for entering related Choice objects. For that reason, Django offers a tabular way of displaying inline related objects; you just need to change the ChoiceInline declaration to read:

然后，还有个小问题。展示所有关联的Choice对象会占据非常多的屏幕空间。为此，Django提供了一个扁平化的显示方式。你只需要将ChoiceInline 类替换即可，如下：

polls/admin.py
```
class ChoiceInline(admin.TabularInline):
    #...
```


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2784070-16ab26397a498b75.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Note that there is an extra “Delete?” column that allows removing rows added using the “Add Another Choice” button and rows that have already been saved.

注意有一个 “Delete?”列，允许你删除通过“Add Another Choice” 按钮增加并保存的行。

#### **Customize the admin change list[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial07/#customize-the-admin-change-list)**

Now that the Question admin page is looking good, let’s make some tweaks to the “change list” page – the one that displays all the questions in the system.

现在Question的admin页面已经非常漂亮了，我们继续对“change list”做一下微调——就是那个展示所有Question列表的页面

Here’s what it looks like at this point:

这就是它现在的样子：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2784070-19b9cf7f20e34233.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

By default, Django displays the str()
 of each object. But sometimes it’d be more helpful if we could display individual fields. To do that, use the [list_display](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_display) admin option, which is a tuple of field names to display, as columns, on the change list page for the object:

默认情况下，Django调用str()展示每个对象。但是有时候，如果我们可以展示其他的字段的话，将会更好。为此，你可以用[list_display](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_display) 选项，这是想要展示的字段名的集合，在change list页面上将作为列来展示：

polls/admin.py

```
class QuestionAdmin(admin.ModelAdmin):
    # ...
    list_display = ('question_text', 'pub_date')
```

Just for good measure, let’s also include the was_published_recently()
 method from [Tutorial 2](https://docs.djangoproject.com/en/1.10/intro/tutorial02/):

更好的是，我们也可以展示was_published_recently()方法：

polls/admin.py
```
class QuestionAdmin(admin.ModelAdmin):
    # ...
    list_display = ('question_text', 'pub_date', 'was_published_recently')
```

Now the question change list page looks like this:

现在Question的change list页面看起来如下图：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2784070-2758d6abdb216303.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

You can click on the column headers to sort by those values – except in the case of the was_published_recently header, because sorting by the output of an arbitrary method is not supported. Also note that the column header for was_published_recently is, by default, the name of the method (with underscores replaced with spaces), and that each line contains the string representation of the output.

你可以点击列标题来排序——除了was_published_recently外，因为arbitrary 方法的输出并不支持排序。同时注意was_published_recently 列标题，默认下是函数名（下划线替换为了空格），内容则是输出的字符串表示法。

You can improve that by giving that method (in polls/models.py) a few attributes, as follows:

你可以在polls/models.py中增加如下属性来修复这个问题：

polls/models.py
```
class Question(models.Model):
    # ...
    def was_published_recently(self):
        now = timezone.now()
        return now - datetime.timedelta(days=1) <= self.pub_date <= now
    was_published_recently.admin_order_field = 'pub_date'
    was_published_recently.boolean = True
    was_published_recently.short_description = 'Published recently?'
```

For more information on these method properties, see [list_display](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_display).

想了解跟多信息，请看 [list_display](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_display).

Edit your polls/admin.py file again and add an improvement to the Question change list page: filters using the[list_filter](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_filter). Add the following line to QuestionAdmin:

再次编辑polls/admin.py，并进一步改善Question change list页面：用[list_filter](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_filter)来作过滤器。在QuestionAdmin类里加如下行：

```
list_filter = ['pub_date']
```
That adds a “Filter” sidebar that lets people filter the change list by the pub_date field:

这会增加一个过滤器面板，让用户可以通过pub_date 来过滤change list页面：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2784070-b1d3184212c95178.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

The type of filter displayed depends on the type of field you’re filtering on. Because pub_date is a [DateTimeField](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.DateTimeField), Django knows to give appropriate filter options: “Any date”, “Today”, “Past 7 days”, “This month”, “This year”.

根据你选择的过滤条件的不同，Django会在面板中添加不容的过滤选项。由于pub_date是一个DateTimeField，因此，Django自动添加了这些选项：“Any date”, “Today”, “Past 7 days”, “This month”, “This year”。

This is shaping up well. Let’s add some search capability:

为了做的更好。我们再增加搜索框：

```
search_fields = ['question_text']
```

That adds a search box at the top of the change list. When somebody enters search terms, Django will search the question_text field. You can use as many fields as you’d like – although because it uses a LIKE query behind the scenes, limiting the number of search fields to a reasonable number will make it easier for your database to do the search.

这会在页面的顶部增加一个搜索框。当输入搜索关键字后，Django会在question_text字段内进行搜索。只要你愿意，你可以使用任意多个搜索字段，Django在后台使用的都是SQL查询语句的LIKE语法，但是，有限制的搜索字段有助于后台的数据库查询效率。

Now’s also a good time to note that change lists give you free pagination. The default is to display 100 items per page. [Change list pagination](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_per_page), [search boxes](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.search_fields), [filters](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_filter), [date-hierarchies](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.date_hierarchy), and [column-header-ordering](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_display) all work together like you think they should.

现在你应该注意到 了，change list有分页功能，默认下显示100个每页。现在，[Change list pagination](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_per_page), [search boxes](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.search_fields), [filters](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_filter), [date-hierarchies](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.date_hierarchy), and [column-header-ordering](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_display)等，都以你想的方式工作了。

#### **Customize the admin look and feel[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial07/#customize-the-admin-look-and-feel)**

#### **定制admin外观[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial07/#customize-the-admin-look-and-feel)**

Clearly, having “Django administration” at the top of each admin page is ridiculous. It’s just placeholder text.

很明显，在每一个admin页面顶端都显示“Django administration”是很可笑的，它仅仅是个占位文本。

That’s easy to change, though, using Django’s template system. The Django admin is powered by Django itself, and its interfaces use Django’s own template system.

利用Django的模板系统，很容易修改它。Django的admin站点是由Django自己驱动的，用的也是Django自己的模板接口。

#### **Customizing your *project’s* templates[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial07/#customizing-your-project-s-templates)**

#### **定制项目模板[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial07/#customizing-your-project-s-templates)**

Create a templates directory in your project directory (the one that contains manage.py). Templates can live anywhere on your filesystem that Django can access. (Django runs as whatever user your server runs.) However, keeping your templates within the project is a good convention to follow.

在你的项目目录（包含manage.py文件的目录）下创建templates目录。Templates可以放在任何Django可以访问到的地方。（你的服务以什么用户跑，DJango就以什么用户运行）庵后，将你的templates目录放在project下是非常好的习惯。

Open your settings file (mysite/settings.py
, remember) and add a [DIRS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-TEMPLATES-DIRS) option in the [TEMPLATES](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-TEMPLATES) setting:

打开settings配置文件(mysite/settings.py, 记住 )，[TEMPLATES](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-TEMPLATES) 设置中增加 [DIRS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-TEMPLATES-DIRS)属性
mysite/settings.py
```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

[DIRS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-TEMPLATES-DIRS) is a list of filesystem directories to check when loading Django templates; it’s a search path.

[DIRS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-TEMPLATES-DIRS)是一个文件系统目录的列表，当加载Django模板时，会在DIRS中进行查找。它是搜索路径。



> **Organizing templates**

> Just like the static files, we *could* have all our templates together, in one big templates directory, and it would work perfectly well. However, templates that belong to a particular application should be placed in that application’s template directory (e.g. polls/templates) rather than the project’s (templates). We’ll discuss in more detail in the [reusable apps tutorial](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/) *why* we do this.

> **组织templates**

> 就像静态文件一样，我们可以把所有的模板都放在一起，形成一个大大的模板文件夹，并且工作正常。但是我们不建议这样！我们建议每一个模板都应该存放在它所属应用的模板目录内（例如polls/templates）而不是整个项目的模板目录（templates），因为这样每个应用才可以被方便和正确的重用。请参考2.10节《如何重用apps》。

Now create a directory called admin inside templates, and copy the template admin/base_site.html from within the default Django admin template directory in the source code of Django itself (django/contrib/admin/templates) into that directory.

接下来，在刚才创建的templates中创建一个admin目录，将admin/base_site.html模板文件拷贝到该目录内。这个html文件来自Django源码，它位于django/contrib/admin/templates目录内。

##### **Where are the Django source files?**

If you have difficulty finding where the Django source files are located on your system, run the following command:

如果你不知道Django的源代码文件在哪，运行以下命令：
```
$ python -c "import django; print(django.__path__)"```

Then, just edit the file and replace{% raw %} {{ site_header|default:_('Django administration') }}{% endraw %} (including the curly braces) with your own site’s name as you see fit. You should end up with a section of code like:

编辑该文件，用你喜欢的站点名字替换掉{% raw %} {{ site_header|default:_('Django administration') }}{% endraw %}（包括两个大括号一起），看起来像下面这样：

```
{% block branding %}
<h1 id="site-name"><a href="{% url 'admin:index' %}">Polls Administration</a></h1>
{% endblock %}
```

We use this approach to teach you how to override templates. In an actual project, you would probably use the [django.contrib.admin.AdminSite.site_header](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.AdminSite.site_header) attribute to more easily make this particular customization.

在这里，我们使用这个方法教会你如何重写模板。但是在实际的项目中，你可以使用 [django.contrib.admin.AdminSite.site_header](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.AdminSite.site_header)，方便的对这个页面title进行自定义。

This template file contains lots of text like {% block branding %} and {{ title }}. The {%
 and {{ tags are part of Django’s template language. When Django renders admin/base_site.html, this template language will be evaluated to produce the final HTML page, just like we saw in [Tutorial 3](https://docs.djangoproject.com/en/1.10/intro/tutorial03/).

这个模板文件包含了大量的如{% raw %}{% block branding %} and {{ title }}{% endraw %},{% raw %} {% and {{  {% endraw %}是Django的模板语言。当Django渲染 admin/base_site.html文件时，这些模板语言会生成最终的HTML页面，就像[Tutorial 3](https://docs.djangoproject.com/en/1.10/intro/tutorial03/)所说

Note that any of Django’s default admin templates can be overridden. To override a template, just do the same thing you did with base_site.html – copy it from the default directory into your custom directory, and make changes.

请注意，所有Django默认的admin模板都可以被重写。类似刚才重写base_site.html模板的方法一样，从源代码目录将html文件拷贝至你自定义的目录内，然后修改文件。

#### **Customizing your *application’s* templates[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial07/#customizing-your-application-s-templates)**

#### **定制 *application’s* 模板[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial07/#customizing-your-application-s-templates)**

Astute readers will ask: But if [DIRS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-TEMPLATES-DIRS) was empty by default, how was Django finding the default admin templates? The answer is that, since [APP_DIRS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-TEMPLATES-APP_DIRS) is set to True, Django automatically looks for a templates/ subdirectory within each application package, for use as a fallback (don’t forget that django.contrib.admin is an application).

聪明的读者可能会问：但是DIRS默认是空的，Django是如何找到默认的admin模板呢？回答是，由于APP_DIRS被设置为True，Django将自动查找每一个应用包内的templates/子目录（不要忘了django.contrib.admin也是一个应用）。

Our poll application is not very complex and doesn’t need custom admin templates. But if it grew more sophisticated and required modification of Django’s standard admin templates for some of its functionality, it would be more sensible to modify the *application’s* templates, rather than those in the *project*. That way, you could include the polls application in any new project and be assured that it would find the custom templates it needed.

我们的投票应用不太复杂，因此不需要自定义admin模板。但是如果它变得越来越复杂，因为某些功能而需要修改Django的标准admin模板，那么修改app的模板就比修改项目的模板更加明智。这样的话，你可以将投票应用加入到任何新的项目中，并且保证能够找到它所需要的自定义模板。

See the [template loading documentation](https://docs.djangoproject.com/en/1.10/topics/templates/#template-loading) for more information about how Django finds its templates.

查看[template loading documentation](https://docs.djangoproject.com/en/1.10/topics/templates/#template-loading)获取更多关于Django如何查找模板的信息。

#### **Customize the admin index page[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial07/#customize-the-admin-index-page)**

#### **定制admin 首页[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial07/#customize-the-admin-index-page)**
On a similar note, you might want to customize the look and feel of the Django admin index page.

很多人都会想定制化Django admin的首页的外观和感觉。

By default, it displays all the apps in [INSTALLED_APPS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-INSTALLED_APPS) that have been registered with the admin application, in alphabetical order. You may want to make significant changes to the layout. After all, the index is probably the most important page of the admin, and it should be easy to use.

默认情况下，首页展示所有[INSTALLED_APPS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-INSTALLED_APPS) 中被注册到admin的应用，你或许想改变一下布局。毕竟，首页是admin站点最重要的页面，它应该要简单易用。

The template to customize is admin/index.html. (Do the same as with admin/base_site.html in the previous section – copy it from the default directory to your custom template directory). Edit the file, and you’ll see it uses a template variable called app_list. That variable contains every installed Django app. Instead of using that, you can hard-code links to object-specific admin pages in whatever way you think is best.

要定制的模板是admin/index.html，（像上节中更改admin/base_site.html模板一样，从它默认的目录拷贝到你的定制化模板目录）。编辑文件，你会看到模板有一个app_list的变量。你可以硬编码链接到指定对象的admin页面，使用任何你认为好的方法，用于替代这个app_list。

#### **What’s next?[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial07/#what-s-next)**

#### **接下来学习什么?[¶](https://docs.djangoproject.com/en/1.10/intro/tutorial07/#what-s-next)**

The beginner tutorial ends here. In the meantime, you might want to check out some pointers on [where to go from here](https://docs.djangoproject.com/en/1.10/intro/whatsnext/).

入门教程到这里就结束了。此时，你应该转到 [where to go from here](https://docs.djangoproject.com/en/1.10/intro/whatsnext/)看看接下该学习什么

If you are familiar with Python packaging and interested in learning how to turn polls into a “reusable app”, check out [Advanced tutorial: How to write reusable apps](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/).

或者你对Python包机制很熟悉，对如何将投票应用转换成一个可重用的app感兴趣，请看[Advanced tutorial: How to write reusable apps](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/).