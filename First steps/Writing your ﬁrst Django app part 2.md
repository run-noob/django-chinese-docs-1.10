This tutorial begins where [Tutorial 1](https://docs.djangoproject.com/en/1.10/intro/tutorial01/) left off. We’ll setup the database, create your first model, and get a quick introduction to Django’s automatically-generated admin site.

紧接着教程[教程1](https://run-noob.gitbooks.io/django-chinese-docs-1-10/content/First%20steps/Writing%20your%20%EF%AC%81rst%20Django%20app%20part%201.html)，我们开始配置数据库，创建你的第一个model，并且快速了解下Django的后台管理系统

**Database setup**

Now, open up mysite/settings.py. It’s a normal Python module with module-level variables representing Django settings.

现在，打开 mysite/settings.py. 这是一个普通的python模块包，包含了Django配置的模块级变量。

By default, the configuration uses SQLite. If you’re new to databases, or you’re just interested in trying Django, this is the easiest choice. SQLite is included in Python, so you won’t need to install anything else to support your database. When starting your first real project, however, you may want to use a more scalable database like PostgreSQL, to avoid database-switching headaches down the road.

默认情况下，Django使用的是SQLite数据库。如果你是数据库菜鸟或者你仅仅想试用一下Django，这是最简单的选择了。python自带了SQLite模块，所以你不需要安装任何东西。然后当你真正想写一个项目时，你可能需要用到功能更丰富的数据库，比如说PostgreSQL，避免后面面临数据库切换的头疼问题。

If you wish to use another database, install the appropriate [database bindings](https://docs.djangoproject.com/en/1.10/topics/install/#database-installation) and change the following keys in the[DATABASES](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-DATABASES) 'default' item to match your database connection settings:

如果你想用其他的数据库，请先安装好相应的[数据库软件](https://docs.djangoproject.com/en/1.10/topics/install/#database-installation) ，并修改settings文件中[DATABASES](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-DATABASES) 'default' 选项，用于连接你的数据库：

+ [ENGINE](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-DATABASE-ENGINE) – Either 'django.db.backends.sqlite3','django.db.backends.postgresql', 'django.db.backends.mysql', or 'django.db.backends.oracle'.  Other backends are [also available](https://docs.djangoproject.com/en/1.10/ref/databases/#third-party-notes).

+ 可以是’django.db.backends.sqlite3’或者’django.db.backends.postgresql’,’django.db.backends.mysql’, or ’django.db.backends.oracle’，当然 [其它可用](https://docs.djangoproject.com/en/1.10/ref/databases/#third-party-notes)的也行。

+ [NAME](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-NAME) – The name of your database. If you’re using SQLite, the database will be a file on your computer; in that case, [NAME](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-NAME) should be the full absolute path, including filename, of that file. The default value, os.path.join(BASE_DIR, 'db.sqlite3'), will store the file in your project directory.

+ 数据库的名字。如果你使用的是默认的SQLite，那么数据库将作为一个文件将存放在你的本地机器内，NAME应该是这个文件的完整绝对路径，包括文件名。设置中的默认值os.path.join(BASE_DIR, ’db.sqlite3’)，将把该文件储存在你的项目目录下。

If you are not using SQLite as your database, additional settings such as [USER](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-USER), [PASSWORD](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-PASSWORD), and [HOST](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-HOST) must be added. For more details, see the reference documentation for [DATABASES](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-DATABASES).

 如果你不是用SQLite，那你需要配置其他的设置，如[USER](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-USER), [PASSWORD](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-PASSWORD), and [HOST](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-HOST) 都是必需的. 详细请看[DATABASES](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-DATABASES)相关.

> **For databases other than SQLite**
> If you’re using a database besides SQLite, make sure you’ve created a database by this point. Do that with “CREATE DATABASE database_name;” within your database’s interactive prompt.
Also make sure that the database user provided in mysite/settings.py
 has “create database” privileges. This allows automatic creation of a [test database](https://docs.djangoproject.com/en/1.10/topics/testing/overview/#the-test-database) which will be needed in a later tutorial.
If you’re using SQLite, you don’t need to create anything beforehand - the database file will be created automatically when it is needed.

> **SQLite外的其他数据库
>在使用非SQLite的数据库时，请务必首先在数据库提示符交互模式下创建数据库，你可以使用命令：“CREATE DATABASE database_name;”
> 确保你在settings文件中提供的数据库用户具有创建数据库表的权限，因为在接下来的教程中，我们需要自动创建一个test数据库。
>如果你使用的是SQLite，那么你无需做任何预先配置，数据库文件会自动创建

While you’re editing mysite/settings.py, set TIME_ZONE to your time zone.

Also, note the INSTALLED_APPS setting at the top of the file. That holds the names of all Django applications that are activated in this Django instance. Apps can be used in multiple projects, and you can package and distribute them for use by others in their projects.

By default, INSTALLED_APPS contains the following apps, all of which come with Django:

+ django.contrib.admin – The admin site. You’ll use it shortly.
+ django.contrib.auth – An authentication system.
+ django.contrib.contenttypes – A framework for content types.
+ django.contrib.sessions – A session framework.
+ django.contrib.messages – A messaging framework.
+ django.contrib.staticfiles – A framework for managing static files.

These applications are included by default as a convenience for the common case.

Some of these applications make use of at least one database table, though, so we need to create the tables in the database before we can use them. To do that, run the following command:

在修改settings文件时，请顺便将TIME_ZONE设置为你所在的时区。

同时，请注意settings文件中顶部的INSTALLED_APPS设置项。它保存了所有的在当前项目中被激活的Django应用。你必须将你自定义的app注册在这里。每个应用可以被多个项目使用，而且你可以打包和分发给其他人在他们的项目中使用。

默认情况，INSTALLED_APPS中包含以下应用，它们都是Django自带的：

+ django.contrib.admin：admin站点
+ django.contrib.auth：身份认证系统
+ django.contrib.contenttypes：内容类型框架
+ django.contrib.sessions：会话框架
+ django.contrib.messages：消息框架
+ django.contrib.staticfiles：静态文件管理框架

以上应用默认会包含，对于大多数项目都是需要的

上面的每个应用都至少需要使用一个数据库表，所以在使用它们之前我们需要在数据库中创建这些表。使用这个命令：

```
python manage.py migrate
```

The migrate command looks at the INSTALLED_APPS setting and creates any necessary database tables according to the database settings in your mysite/settings.py file and the database migrations shipped with the app (we’ll cover those later). You’ll see a message for each migration it applies. If you’re interested, run the command-line client for your database and type \dt (PostgreSQL), SHOW TABLES; (MySQL), .schema (SQLite), or SELECT TABLE_NAME FROM USER_TABLES; (Oracle) to display the tables Django created.

migrate命令将根据INSTALLED_APPS、mysite/settings.py的数据库设置以及app的数据库迁移（稍后会讲到）信息创建相关的表 ，你讲看到每一个数据库消息的信息。如果你感兴趣，可以在你的数据库命令行下输入：\dt (PostgreSQL), SHOW TABLES; (MySQL), 或 .schema (SQLite)， 或SELECT TABLE_NAME FROM USER_TABLES; (Oracle) 来列出 Django 所创建的表。



> **For the minimalists**

> Like we said above, the default applications are included for the common case, but not everybody needs them. If you don’t need any or all of them, feel free to comment-out or delete the appropriate line(s) from INSTALLED_APPS before running migrate. The migrate command will only run migrations for apps in INSTALLED_APPS.

> 就像我们上面说的，默认情况下会自带那些公共应用。如果你不需要其中一个或所有的，你可以在migrate前，将INSTALLED_APPS内注释掉或者删除对应的行。 migrate命令只会针对INSTALL_APPS进行操作


### Creating models

Now we’ll define your models – essentially, your database layout, with additional metadata.

> Philosophy

> A model is the single, definitive source of truth about your data. It contains the essential fields and behaviors of the data you’re storing. Django follows the [DRY Principle](https://docs.djangoproject.com/en/1.10/misc/design-philosophies/#dry). The goal is to define your data model in one place and automatically derive things from it.

> This includes the migrations - unlike in Ruby On Rails, for example, migrations are entirely derived from your models file, and are essentially just a history that Django can roll through to update your database schema to match your current models.

### 创建models

现在我们开始定义models - 本质上，就是你的数据库结构，和其他的一些元数据信息

> Philosophy

> model就是你唯一可信的真实数据源。它包含了你所要存储数据的必须字段和行为信息。Django遵循[DRY Principle](https://docs.djangoproject.com/en/1.10/misc/design-philosophies/#dry)，目的是让你可以在一个地方定义你的数据模型，并以它来驱动整个项目

> 关于migrations - 它不想ruby的Rails，比如说，migrations纯靠你的models来驱动。本质上是根据当前models来更新数据库表的一个历史记录。


In our simple poll app, we’ll create two models: Question and Choice. A Question has a question and a publication date. A Choice has two fields: the text of the choice and a vote tally. Each Choice is associated with a Question.

These concepts are represented by simple Python classes. Edit the polls/models.py file so it looks like this:

在我们的投票应用中，我们将创建两个模型：Question and Choice。Question表包含问题和发布日期两个字段。Choice表有两个字段：选择文本和投票计数。每一条choice都关联到一条Question

这些概念都由python的类来定义。编辑polls/models.py，如下

polls/models.py

```
from django.db import models


class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

The code is straightforward. Each model is represented by a class that subclasses [django.db.models.Model](https://docs.djangoproject.com/en/1.10/ref/models/instances/#django.db.models.Model). Each model has a number of class variables, each of which represents a database field in the model.

以上代码简单明了。每一个model都由一个类来定义，该类是[django.db.models.Model](https://docs.djangoproject.com/en/1.10/ref/models/instances/#django.db.models.Model)的子类。每个类都由数个变量组成，分别对应了数据库里的字段

Each field is represented by an instance of a  [Field](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.Field) class – e.g., [CharField](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.CharField)for character fields and [DateTimeField](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.DateTimeField) for datetimes. This tells Django what type of data each field holds.

每一个字段都是[Field](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.Field) 类的实例 -- 如 [CharField](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.CharField)是字符串字段，[DateTimeField](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.DateTimeField)是时间字段。这告诉Django每个字段用来存储什么类型的数据

The name of each Field instance (e.g. question_text or pub_date) is the field’s name, in machine-friendly format. You’ll use this value in your Python code, and your database will use it as the column name.

每个Field的实例名（如question_text 或 pub_date）就是字段的名字名，一个机器友好型的名字。在代码里你会用到它，而数据库里用它来作字段名。

You can use an optional first positional argument to a Field to designate a human-readable name. That’s used in a couple of introspective parts of Django, and it doubles as documentation. If this field isn’t provided, Django will use the machine-readable name. In this example, we’ve only defined a human-readable name for Question.pub_date. For all other fields in this model, the field’s machine-readable name will suffice as its human-readable name.

每个Filed里的第一个可选参数，用来指定一个可读性好的名字。这在Django的一些内部机制中有所用处，也可以兼做文档。如果没有指定这个字段，Django会默认使用机器可读的名字。在这个例子中，我们只为pub_data定义了友好的名字。model的其他字段已经通俗易懂。


Some Field classes have required arguments. CharField, for example, requires that you give it a max_length. That’s used not only in the database schema, but in validation, as we’ll soon see.

有一些字段有必选参数。例如，CharField要求你给它一个max_length。这个参数不仅用于数据库模式，而且数据验证中也会用到，我们稍后会看到。

A Field can also have various optional arguments; in this case, we’ve set the  [default](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.Field.default) value of votes to 0.

Field 还具有各种可选参数。在这个例子中，我们设置votes字段的 [默认](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.Field.default)值 为0。

Finally, note a relationship is defined, using [ForeignKey](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.ForeignKey). That tells Django each Choice is related to a single Question. Django supports all the common database relationships: many-to-one, many-to-many, and one-to-one.

最后，注意我们使用[ForeignKey](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.ForeignKey)定义了一个关联。它告诉Django每个Choice都只关联一个Question。Django支持所有常见的数据库关联：多对一、多对多和一对一。

### **Activating models**

That small bit of model code gives Django a lot of information. With it, Django is able to:

+ Create a database schema (CREATE TABLE statements) for this app.

+ Create a Python database-access API for accessing Question
 and Choice objects.

But first we need to tell our project that the polls app is installed.

> **Philosophy**

> Django apps are “pluggable”: You can use an app in multiple projects, and you can distribute apps, because they don’t have to be tied to a given Django installation.

这简短的model代码给了Django许多信息。有了它，Django就可以：

+ 为该应用创建数据库表（CREATE TABLE 语句）。
+ 为Question对象和Choice对象创建一个访问数据库的python API。

但是，我们首先得告诉项目：polls应用已经安装。 

> 理念

> Django 应用是可以“插件化”的，即可以在多个项目中使用同一个应用，也可以分发这些应用， 因为它们不需要与某个特定的Django安装绑定。

To include the app in our project, we need to add a reference to its configuration class in the [INSTALLED_APPS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-INSTALLED_APPS)setting. The PollsConfig class is in the polls/apps.py file, so its dotted path is 'polls.apps.PollsConfig'
. Edit the mysite/settings.py file and add that dotted path to the [INSTALLED_APPS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-INSTALLED_APPS) setting. It’ll look like this:

为了安装应用，我们需要在配置中的[INSTALLED_APPS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-INSTALLED_APPS)中增加相关设置。PollsConfig 类就是polls/apps.py文件，以点分割的路径就是'polls.apps.PollsConfig'，编辑mysite/settings.py文件，[INSTALLED_APPS](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-INSTALLED_APPS)配置中增加点分隔符路径，文件看起来应该像这样：

mysite/settings.py
 
```
INSTALLED_APPS = [
    'polls.apps.PollsConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

Now Django knows to include the polls app. Let’s run another command:

现在Django就知道要安装polls应用了。我们输入以下命令：

```
python manage.py makemigrations polls
```

You should see something similar to the following:

你可以看到类似以下的输出：

```
Migrations for 'polls':
  polls/migrations/0001_initial.py:
    - Create model Choice
    - Create model Question
    - Add field question to choice
```

By running makemigrations, you’re telling Django that you’ve made some changes to your models (in this case, you’ve made new ones) and that you’d like the changes to be stored as a *migration*.

运行makemigrations，告诉Django你已经对你的models做了一些修改（在本例中，你创建了新的model类），这些修改会作为migration记录来存储

Migrations are how Django stores changes to your models (and thus your database schema) - they’re just files on disk. You can read the migration for your new model if you like; it’s the filepolls/migrations/0001_initial.py. Don’t worry, you’re not expected to read them every time Django makes one, but they’re designed to be human-editable in case you want to manually tweak how Django changes things.

Migrations就是Django存储的models的修改记录（也就是你的数据库表格）--在磁盘上他们仅仅是文件而已。如果你想，你可以阅读那些为新模型建立的迁移文件，该文件是filepolls/migrations/0001_initial.py。别担心，在Django生成迁移文件后，你不必每次都去阅读，这些文件是可编辑的，以便你手动修改一下Django的操作行为。

There’s a command that will run the migrations for you and manage your database schema automatically - that’s called [migrate
](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-migrate), and we’ll come to it in a moment - but first, let’s see what SQL that migration would run. The[sqlmigrate](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-sqlmigrate) command takes migration names and returns their SQL:

有一个命令可以运行这些migrations文件并自动管理你的数据库表 -- 它是migrate，我们待会儿会用到它 -- 但首先我们看一下，那些迁移会执行哪些sql语句。[sqlmigrate](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-sqlmigrate) 命令接收迁移文件信息，并返回sql语句：
```
python manage.py sqlmigrate polls 0001
```

You should see something similar to the following (we’ve reformatted it for readability):

你应该看到如下内容（为了好看我们重新排了下格式）：

```
BEGIN;
--
-- Create model Choice
--
CREATE TABLE "polls_choice" (
    "id" serial NOT NULL PRIMARY KEY,
    "choice_text" varchar(200) NOT NULL,
    "votes" integer NOT NULL
);
--
-- Create model Question
--
CREATE TABLE "polls_question" (
    "id" serial NOT NULL PRIMARY KEY,
    "question_text" varchar(200) NOT NULL,
    "pub_date" timestamp with time zone NOT NULL
);
--
-- Add field question to choice
--
ALTER TABLE "polls_choice" ADD COLUMN "question_id" integer NOT NULL;
ALTER TABLE "polls_choice" ALTER COLUMN "question_id" DROP DEFAULT;
CREATE INDEX "polls_choice_7aa0f6ee" ON "polls_choice" ("question_id");
ALTER TABLE "polls_choice"
  ADD CONSTRAINT "polls_choice_question_id_246c99a640fbbd72_fk_polls_question_id"
    FOREIGN KEY ("question_id")
    REFERENCES "polls_question" ("id")
    DEFERRABLE INITIALLY DEFERRED;

COMMIT;
```

Note the following:

+ The exact output will vary depending on the database you are using. The example above is generated for PostgreSQL.

+ Table names are automatically generated by combining the name of the app (polls) and the lowercase name of the model – question and choice. (You can override this behavior.)

+ Primary keys (IDs) are added automatically. (You can override this, too.)

+ By convention, Django appends "_id" to the foreign key field name. (Yes, you can override this, as well.)

+ The foreign key relationship is made explicit by a FOREIGN KEY constraint. Don’t worry about the DEFERRABLE parts; that’s just telling PostgreSQL to not enforce the foreign key until the end of the transaction.

+ It’s tailored to the database you’re using, so database-specific field types such as auto_increment (MySQL), serial (PostgreSQL), or integer primary key autoincrement (SQLite) are handled for you automatically. Same goes for the quoting of field names – e.g., using double quotes or single quotes.

+ The [sqlmigrate](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-sqlmigrate) command doesn’t actually run the migration on your database - it just prints it to the screen so that you can see what SQL Django thinks is required. It’s useful for checking what Django is going to do or if you have database administrators who require SQL scripts for changes.

请注意以下几点：

输出的具体内容会依据你使用的数据库而不同。 以上例子使用的数据库是PostgreSQL。

表名是自动生成的，由app的名字（polls）和模型名字的小写字母组合而成 —— question和choice。（你可以重写这个行为。)

主键（IDs）是自动添加的。 （你也可以重写这个行为。)

按照惯例，Django会在外键的字段名后面添加 "_id"。（是的，你依然可以重写这个行为。）

外键关系由FOREIGN KEY约束显式声明。不用在意DEFERRABLE
部分；它只是告诉PostgreSQL直到事务的最后再执行外键关联。

这些SQL语句是针对你所使用的数据库定制的，所以会为你自动处理某些数据库所特有的字段例如auto_increment(MySQL)、 serial (PostgreSQL)或integer primary key autoincrement (SQLite) 。在处理字段名的引号时也是如此 —— 例如，使用双引号还是单引号。

[sqlmigrate](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-sqlmigrate)命令并不会在你的数据库上真正运行迁移文件 —— 它只是把Django 认为需要的SQL打印在屏幕上以让你能够看到。 
这对于检查Django将要进行的数据库操作或者你的数据库管理员需要这些SQL脚本是非常有用的。

If you’re interested, you can also run [python manage.py check
](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-check); this checks for any problems in your project without making migrations or touching the database.

Now, run [migrate](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-migrate) again to create those model tables in your database:

如果有兴趣，你还可以运行 [python manage.py check
](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-check)；它会检查你的项目中的模型是否存在问题，而不用执行迁移或者操作数据库。

现在，再次运行 [migrate](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-migrate)以在你的数据库中创建模型所对应的表：

```
python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, polls, sessions
Running migrations:
  Rendering model states... DONE
  Applying polls.0001_initial... OK
```

The [migrate](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-migrate) command takes all the migrations that haven’t been applied (Django tracks which ones are applied using a special table in your database called django_migrations) and runs them against your database - essentially, synchronizing the changes you made to your models with the schema in the database.

[migrate](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-migrate) 命令将找到所有尚未被应用的迁移文件（django_migrations的特殊表来追踪哪些迁移文件已经被应用过），并且在你的数据库上运行它们 —— 本质上来讲，就是使你的数据库模式和你改动后的模型进行同步。）

Migrations are very powerful and let you change your models over time, as you develop your project, without the need to delete your database or tables and make new ones - it specializes in upgrading your database live, without losing data. We’ll cover them in more depth in a later part of the tutorial, but for now, remember the three-step guide to making model changes:

迁移功能非常强大，在你开发项目的过程中，使你在更改models后，无需删除数据库或表，然后再重建新的。它专注于热升级你的数据库且不丢数据。我们将在本教程的后续章节中详细介绍，但是现在，请记住模型变更的三个基本步骤：

+ Change your models (in models.py).

+ Run [python manage.py makemigrations](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-makemigrations) to create migrations for those changes

+ Run [python manage.py migrate](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-migrate) to apply those changes to the database.

+ 修改你的模型（在models.py文件中）。

+ 运行 [python manage.py makemigrations](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-makemigrations)  ，为这些修改创建迁移文件

+ 运行[python manage.py migrate](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-migrate)，将这些改变更新到数据库中。

The reason that there are separate commands to make and apply migrations is because you’ll commit migrations to your version control system and ship them with your app; they not only make your development easier, they’re also useable by other developers and in production.

将生成和应用迁移文件分开，是因为你可能需要将迁移文件提交到你的版本控制器并跟随你的应用。这样做不仅使开发变得更加简单，而且对其他开发者以及线上环境非常有用。

Read the [django-admin documentation](https://docs.djangoproject.com/en/1.10/ref/django-admin/) for full information on what the manage.py utility can do.

阅读[django-admin documentation](https://docs.djangoproject.com/en/1.10/ref/django-admin/) 来详细了解manage.py工具所能做的事情。

### Playing with the API

Now, let’s hop into the interactive Python shell and play around with the free API Django gives you. To invoke the Python shell, use this command:

现在，我们切换到python shell的交互模式，感受一下Django给的API。用如下命令来调出python shell

```
python manage.py shell
```
We’re using this instead of simply typing “python”, because manage.py
 sets the DJANGO_SETTINGS_MODULE environment variable, which gives Django the Python import path to your mysite/settings.py file.

我们替代使用简单的python命令进入python shell，是因为manage.py设置了DJANGO_SETTINGS_MODULE环境变量，它告诉Django导入了mysite/settings.py模块

> ** Bypassing manage.py**

> If you’d rather not use manage.py, no problem. Just set the [DJANGO_SETTINGS_MODULE](https://docs.djangoproject.com/en/1.10/topics/settings/#envvar-DJANGO_SETTINGS_MODULE) environment variable to mysite.settings, start a plain Python shell, and set up Django:

> ```
>>> import django
>>> django.setup()
```

> If this raises an [AttributeError
](https://docs.python.org/3/library/exceptions.html#AttributeError), you’re probably using a version of Django that doesn’t match this tutorial version. You’ll want to either switch to the older tutorial or the newer Django version.

> You must run python from the same directory manage.py
 is in, or ensure that directory is on the Python path, so that import mysite works.

> For more information on all of this, see the [django-admin documentation](https://docs.djangoproject.com/en/1.10/ref/django-admin/).

> ** 跳过manage.py**

>  如果你不想使用manage.py，没问题，只需设置 [DJANGO_SETTINGS_MODULE](https://docs.djangoproject.com/en/1.10/topics/settings/#envvar-DJANGO_SETTINGS_MODULE)  变量为 mysite.settings,启动一个普通的 Python shell, 然后设置Django:

> ```
>>> import os
>>>#设置DJANGO_SETTINGS_MODULE环境变量
>>> os.environ['DJANGO_SETTINGS_MODULE']='mysite.settings'
>>> import django
>>> django.setup()
```

> 如果你引发了[AttributeError](https://docs.python.org/3/library/exceptions.html#AttributeError)，那你使用的Django版本可能与本教程的版本不符，你可以切换到旧教程，或安装更新的版本。

> 你应该在 manage.py同级目录敲python命令，或者确保该目录在python path内，这样你就可以import mysite了

> 更多详细信息，请看 [django-admin documentation](https://docs.djangoproject.com/en/1.10/ref/django-admin/).

Once you’re in the shell, explore the [database API](https://docs.djangoproject.com/en/1.10/topics/db/queries/):

一旦你进入了这个python shell，那就探索 [database API](https://docs.djangoproject.com/en/1.10/topics/db/queries/)吧:

```
>>> from polls.models import Question, Choice   # Import the model classes we just wrote.

# No questions are in the system yet.
>>> Question.objects.all()
<QuerySet []>

# Create a new Question.
# Support for time zones is enabled in the default settings file, so
# Django expects a datetime with tzinfo for pub_date. Use timezone.now()
# instead of datetime.datetime.now() and it will do the right thing.
>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now())

# Save the object into the database. You have to call save() explicitly.
>>> q.save()

# Now it has an ID. Note that this might say "1L" instead of "1", depending
# on which database you're using. That's no biggie; it just means your
# database backend prefers to return integers as Python long integer
# objects.
>>> q.id
1

# Access model field values via Python attributes.
>>> q.question_text
"What's new?"
>>> q.pub_date
datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=<UTC>)

# Change values by changing the attributes, then calling save().
>>> q.question_text = "What's up?"
>>> q.save()

# objects.all() displays all the questions in the database.
>>> Question.objects.all()
<QuerySet [<Question: Question object>]>
```

Wait a minute. <Question: Question object> is, utterly, an unhelpful representation of this object. Let’s fix that by editing the Question
 model (in the polls/models.py file) and adding a [__str__()](https://docs.djangoproject.com/en/1.10/ref/models/instances/#django.db.models.Model.__str__) method to both Question
 and Choice:

等会，<Question: Question object> 是什么鬼，这是很不友好的对象表示方法。我们可以修复这个问题，编辑Question model (in the polls/models.py file)，在 Question and Choice类增加 [__str__()](https://docs.djangoproject.com/en/1.10/ref/models/instances/#django.db.models.Model.__str__) 方法：

polls/models.py

```
from django.db import models
from django.utils.encoding import python_2_unicode_compatible

@python_2_unicode_compatible  # 仅当你需要兼容python2版本的时候
class Question(models.Model):
    # ...
    def __str__(self):
        return self.question_text

@python_2_unicode_compatible  # only if you need to support Python 2
class Choice(models.Model):
    # ...
    def __str__(self):
        return self.choice_text
```

It’s important to add [__str__()](https://docs.djangoproject.com/en/1.10/ref/models/instances/#django.db.models.Model.__str__) methods to your models, not only for your own convenience when dealing with the interactive prompt, but also because objects’ representations are used throughout Django’s automatically-generated admin.

给你的模型类添加[__str__()](https://docs.djangoproject.com/en/1.10/ref/models/instances/#django.db.models.Model.__str__) 方法很重要，不仅会使你自己在使用交互式命令行时看得更加方便，而且会在Django自动生成的管理界面中使用对象的这种表示。

Note these are normal Python methods. Let’s add a custom method, just for demonstration:

这是非常普通的Python方法。让我们来演示一下如何添加自定义方法：

polls/models.py

```
import datetime

from django.db import models
from django.utils import timezone


class Question(models.Model):
    # ...
    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)
```

Note the addition of import datetime and from django.utils import timezone, to reference Python’s standard [datetime](https://docs.python.org/3/library/datetime.html#module-datetime) module and Django’s time-zone-related utilities in [django.utils.timezone](https://docs.djangoproject.com/en/1.10/ref/utils/#module-django.utils.timezone), respectively. If you aren’t familiar with time zone handling in Python, you can learn more in the [time zone support docs](https://docs.djangoproject.com/en/1.10/topics/i18n/timezones/).

Save these changes and start a new Python interactive shell by running python manage.py shell again:

注意import datetime 和from django.utils import timezone分别引用Python 的标准[datetime](https://docs.python.org/3/library/datetime.html#module-datetime) 模块和Django[django.utils.timezone
](https://docs.djangoproject.com/en/1.10/ref/utils/#module-django.utils.timezone)中时区相关的工具。如果你不了解Python中时区的处理方法，你可以在[*时区相关文档*](https://docs.djangoproject.com/en/1.10/topics/i18n/timezones/)中了解更多的知识。

保存这些改动，然后通过python manage.py shell再次打开一个新的Python 交互式shell：

```
>>> from polls.models import Question, Choice

# Make sure our __str__() addition worked.
>>> Question.objects.all()
<QuerySet [<Question: What's up?>]>

# Django provides a rich database lookup API that's entirely driven by
# keyword arguments.
>>> Question.objects.filter(id=1)
<QuerySet [<Question: What's up?>]>
>>> Question.objects.filter(question_text__startswith='What')
<QuerySet [<Question: What's up?>]>

# Get the question that was published this year.
>>> from django.utils import timezone
>>> current_year = timezone.now().year
>>> Question.objects.get(pub_date__year=current_year)
<Question: What's up?>

# Request an ID that doesn't exist, this will raise an exception.
>>> Question.objects.get(id=2)
Traceback (most recent call last):
    ...
DoesNotExist: Question matching query does not exist.

# Lookup by a primary key is the most common case, so Django provides a
# shortcut for primary-key exact lookups.
# The following is identical to Question.objects.get(id=1).
>>> Question.objects.get(pk=1)
<Question: What's up?>

# Make sure our custom method worked.
>>> q = Question.objects.get(pk=1)
>>> q.was_published_recently()
True

# Give the Question a couple of Choices. The create call constructs a new
# Choice object, does the INSERT statement, adds the choice to the set
# of available choices and returns the new Choice object. Django creates
# a set to hold the "other side" of a ForeignKey relation
# (e.g. a question's choice) which can be accessed via the API.
>>> q = Question.objects.get(pk=1)

# Display any choices from the related object set -- none so far.
>>> q.choice_set.all()
<QuerySet []>

# Create three choices.
>>> q.choice_set.create(choice_text='Not much', votes=0)
<Choice: Not much>
>>> q.choice_set.create(choice_text='The sky', votes=0)
<Choice: The sky>
>>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)

# Choice objects have API access to their related Question objects.
>>> c.question
<Question: What's up?>

# And vice versa: Question objects get access to Choice objects.
>>> q.choice_set.all()
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
>>> q.choice_set.count()
3

# The API automatically follows relationships as far as you need.
# Use double underscores to separate relationships.
# This works as many levels deep as you want; there's no limit.
# Find all Choices for any question whose pub_date is in this year
# (reusing the 'current_year' variable we created above).
>>> Choice.objects.filter(question__pub_date__year=current_year)
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>

# Let's delete one of the choices. Use delete() for that.
>>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
>>> c.delete()
```

For more information on model relations, see [Accessing related objects](https://docs.djangoproject.com/en/1.10/ref/models/relations/). For more on how to use double underscores to perform field lookups via the API, see [Field lookups](https://docs.djangoproject.com/en/1.10/topics/db/queries/#field-lookups-intro). For full details on the database API, see our [Database API reference](https://docs.djangoproject.com/en/1.10/topics/db/queries/).

更多关于模型相关的信息，请查看[Accessing related objects](https://docs.djangoproject.com/en/1.10/ref/models/relations/)。更多关于如何在这些API中使用双下划线来执行字段查询的信息，请查看 [*字段查询*](https://docs.djangoproject.com/en/1.10/topics/db/queries/#field-lookups-intro)。想了解所有数据库API的信息，请查看我们的 [*数据库API参考*](https://docs.djangoproject.com/en/1.10/topics/db/queries/).

### Introducing the Django Admin

> **Philosophy**

> Generating admin sites for your staff or clients to add, change, and delete content is tedious work that doesn’t require much creativity. For that reason, Django entirely automates creation of admin interfaces for models.

> 为你的团队或客户编写增删改查的admin管理系统是非常乏味的，也没有多少创造性价值。基于这考虑，Django自动的为你的models创建了管理借口。

> Django was written in a newsroom environment, with a very clear separation between “content publishers” and the “public” site. Site managers use the system to add news stories, events, sports scores, etc., and that content is displayed on the public site. Django solves the problem of creating a unified interface for site administrators to edit content.

> Django最初是为新闻站点开发的，新闻发布者和公共站点界限明显。管理员通过这个系统添加新闻、时间、体育赛事等，这些信息展示在公共网站上。Django提供了一个统一接口给站点管理员来编辑内容。

> The admin isn’t intended to be used by site visitors. It’s for site managers.

> admin系统并不是为了访客设计的，只对站点管理员开放。

#### **Creating an admin user**

First we’ll need to create a user who can login to the admin site. Run the following command:
```
python manage.py createsuperuser
```

Enter your desired username and press enter.
```
Username: admin
```

You will then be prompted for your desired email address:
```
Email address: admin@example.com
```

The final step is to enter your password. You will be asked to enter your password twice, the second time as a confirmation of the first.
```
Password: **********
Password (again): *********
Superuser created successfully.
```

#### **创建管理员**

首先我们需要创建一个能登录admin站点的用户，运行以下命令：
```
python manage.py createsuperuser
```

输入你喜欢的名字，按下回车.
```
Username: admin
```

然后会提示输入你的邮箱：
```
Email address: admin@example.com
```

最后一步是输入密码。你需要输入两次，第二次作为第一次的进一步确认：
```
Password: **********
Password (again): *********
Superuser created successfully.
```

#### **Start the development server**
The Django admin site is activated by default. Let’s start the development server and explore it.

If the server is not running start it like so:

```
python manage.py runserver
```

Now, open a Web browser and go to “/admin/” on your local domain – e.g., [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/). You should see the admin’s login screen:

#### **启动开发服务器**
Django的admin站点默认是激活的。让我们开启开发服务器来探索一下：

如果服务器没开启，用下面的方法来启动：
```
python manage.py runserver
```

现在，打开web浏览器，本地域名访问“/admin/” -- 比如说[http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/)。你可以看到admin的登录界面：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2784070-793fac84d42c3809.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Since [translation](https://docs.djangoproject.com/en/1.10/topics/i18n/translation/) is turned on by default, the login screen may be displayed in your own language, depending on your browser’s settings and if Django has a translation for this language.

[translation](https://docs.djangoproject.com/en/1.10/topics/i18n/translation/) 默认是开启的，登录界面显示的应该是你的母语，这取决于你的浏览器设置，如果Django有这种翻译版本的话。

#### **Enter the admin site**

Now, try logging in with the superuser account you created in the previous step. You should see the Django admin index page:

现在，你可以用你刚刚创建的超级用户来登录了。登录后你应该可以看到如下index页面：

![Django admin index page](https://docs.djangoproject.com/en/1.10/_images/admin02.png)

You should see a few types of editable content: groups and users. They are provided by [django.contrib.auth](https://docs.djangoproject.com/en/1.10/topics/auth/#module-django.contrib.auth), the authentication framework shipped by Django.

当前已经有两个可编辑的内容：groups和users。它们是 [django.contrib.auth](https://docs.djangoproject.com/en/1.10/topics/auth/#module-django.contrib.auth)模块提供的身份认证框架。

#### **Make the poll app modifiable in the admin**

#### **在admin中注册投票应用**

But where’s our poll app? It’s not displayed on the admin index page.
Just one thing to do: we need to tell the admin that Question
 objects have an admin interface. To do this, open the polls/admin.py
 file, and edit it to look like this:

但是我们创建的投票应用哪去了呢？它并没有在admin的首页里。我们还有一件事要做：告诉admin站点Question要有admin系统。编辑polls/admin.py文件，代码如下：

polls/admin.py
```
from django.contrib import admin

from .models import Question

admin.site.register(Question)
```

#### **Explore the free admin functionality**

Now that we’ve registered Question, Django knows that it should be displayed on the admin index page:

Question注册之后，Django就知道要将它展示在admin的首页了。
![Django admin index page, now with polls displayed](https://docs.djangoproject.com/en/1.10/_images/admin03t.png)

Click “Questions”. Now you’re at the “change list” page for questions. This page displays all the questions in the database and lets you choose one to change it. There’s the “What’s up?” question we created earlier:

点击“Questions”。 现在，你会进入Question的“变更列表”。 这个界面显示了数据库中的所有question，你可以选择一个来更改它。 我们在前面创建的“What’s up?” 问题也在这里:

![Polls change list page](https://docs.djangoproject.com/en/1.10/_images/admin04t.png)Click the “What’s up?” question to edit it:
![Editing form for question object](https://docs.djangoproject.com/en/1.10/_images/admin05t.png)

Things to note here:

+ The form is automatically generated from the Question model.

+ The different model field types (DateTimeField, CharField) correspond to the appropriate HTML input widget. Each type of field knows how to display itself in the Django admin.

+ Each DateTimeField gets free JavaScript shortcuts. Dates get a “Today” shortcut and calendar popup, and times get a “Now” shortcut and a convenient popup that lists commonly entered times.

The bottom part of the page gives you a couple of options:

+ Save – Saves changes and returns to the change-list page for this type of object.

+ Save and continue editing – Saves changes and reloads the admin page for this object.

+ Save and add another – Saves changes and loads a new, blank form for this type of object.

+ Delete – Displays a delete confirmation page.

注意事项：

+ 这个表单是根据Question模型文件自动生成的。 
+ 模型中不同类型的字段（DateTimeField、CharField）会对应相应的HTML输入控件。每一种类型的字段，Django管理站点都知道如何显示它们。

+ 每个DateTimeField字段都会有个方便的JavaScript快捷键。Date有个“Today”的快捷键和一个弹出式日历，time栏有个“Now”的快捷键和一个列出常用时间选项的弹出式窗口。

界面的底部提供了几个按钮：

+ save  —— 保存更改，并返回当前类型对象的变更列表界面。

+ save and continue editing —— 保存更改并且重新载入当前对象的管理界面。

+ save and add another  —— 保存更改并且载入一个当前类型对象的新的、空白的表单。

+ selete  —— 显示一个删除确认界面。


If the value of “Date published” doesn’t match the time when you created the question in [Tutorial 1](https://docs.djangoproject.com/en/1.10/intro/tutorial01/), it probably means you forgot to set the correct value for the [TIME_ZONE
](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-TIME_ZONE) setting. Change it, reload the page and check that the correct value appears.

如果“Date published”的值和你在教程1中创建这个Question对象时的时间不相符，可能是因为你忘记将TIME_ZONE设置为你当地的时区。修改它，然后刷新界面，再次检查一下是否正确。

Change the “Date published” by clicking the “Today” and “Now” shortcuts. Then click “Save and continue editing.” Then click “History” in the upper right. You’ll see a page listing all changes made to this object via the Django admin, with the timestamp and username of the person who made the change:

通过“Today”和“Now”这两个快捷键更改“Date published”字段。，然后点击 “Save and continue editing”。然后点击右上角的“History”按钮。 你将看到一个页面，列出了通过Django管理界面对此对象所做的全部更改的清单，包含有时间戳和修改人的姓名等信息：

![History page for question object](https://docs.djangoproject.com/en/1.10/_images/admin06t.png)When you’re comfortable with the models API and have familiarized yourself with the admin site, read [part 3 of this tutorial](https://docs.djangoproject.com/en/1.10/intro/tutorial03/) to learn about how to add more views to our polls app.

当你适应了models的API，熟悉了admin站点后，请移步
[教程3](https://run-noob.gitbooks.io/django-chinese-docs-1-10/content/First%20steps/Writing%20your%20%EF%AC%81rst%20Django%20app%20part%203.html) 来学习如何给投票应用编写更多的views函数.

