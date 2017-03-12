
翻译Django官方文档完全出于自己兴趣。一方面是想提升下英语水平，另一方面也能进一步的熟悉Django

官方文档大概1500页，仅能上班业余时间进行翻译，工作量略大，有想练练手的，欢迎入坑！

# Django文档简介


### **Django documentation**

### **Django 文档**

Everything you need to know about Django.

关于Django，你需要知道的知识点，这里都有

**How the documentation is organized**

Django has a lot of documentation. A high-level overview of how it’s organized will help you know where to look for certain things:

Django
[Tutorials](https://docs.djangoproject.com/en/1.10/intro/) take you by the hand through a series of steps to create a Web application. Start here if you’re new to Django or Web application development. Also look at the “[First steps](https://docs.djangoproject.com/en/1.10/#index-first-steps)” below.
[Topic guides](https://docs.djangoproject.com/en/1.10/topics/) discuss key topics and concepts at a fairly high level and provide useful background information and explanation.
[Reference guides](https://docs.djangoproject.com/en/1.10/ref/) contain technical reference for APIs and other aspects of Django’s machinery. They describe how it works and how to use it but assume that you have a basic understanding of key concepts.
[How-to guides](https://docs.djangoproject.com/en/1.10/howto/) are recipes. They guide you through the steps involved in addressing key problems and use-cases. They are more advanced than tutorials and assume some knowledge of how Django works.

First steps[¶](https://docs.djangoproject.com/en/1.10/#first-steps)
Are you new to Django or to programming? This is the place to start!
**From scratch:** [Overview](https://docs.djangoproject.com/en/1.10/intro/overview/) | [Installation](https://docs.djangoproject.com/en/1.10/intro/install/)
**Tutorial:** [Part 1: Requests and responses](https://docs.djangoproject.com/en/1.10/intro/tutorial01/) | [Part 2: Models and the admin site](https://docs.djangoproject.com/en/1.10/intro/tutorial02/) | [Part 3: Views and templates](https://docs.djangoproject.com/en/1.10/intro/tutorial03/) | [Part 4: Forms and generic views](https://docs.djangoproject.com/en/1.10/intro/tutorial04/) | [Part 5: Testing](https://docs.djangoproject.com/en/1.10/intro/tutorial05/) | [Part 6: Static files](https://docs.djangoproject.com/en/1.10/intro/tutorial06/) | [Part 7: Customizing the admin site](https://docs.djangoproject.com/en/1.10/intro/tutorial07/)
**Advanced Tutorials:** [How to write reusable apps](https://docs.djangoproject.com/en/1.10/intro/reusable-apps/) | [Writing your first patch for Django](https://docs.djangoproject.com/en/1.10/intro/contributing/)

The model layer[¶](https://docs.djangoproject.com/en/1.10/#the-model-layer)
Django provides an abstraction layer (the “models”) for structuring and manipulating the data of your Web application. Learn more about it below:
**Models:** [Introduction to models](https://docs.djangoproject.com/en/1.10/topics/db/models/) | [Field types](https://docs.djangoproject.com/en/1.10/ref/models/fields/) | [Meta options](https://docs.djangoproject.com/en/1.10/ref/models/options/) | [Model class](https://docs.djangoproject.com/en/1.10/ref/models/class/)
**QuerySets:** [Executing queries](https://docs.djangoproject.com/en/1.10/topics/db/queries/) | [QuerySet method reference](https://docs.djangoproject.com/en/1.10/ref/models/querysets/) | [Lookup expressions](https://docs.djangoproject.com/en/1.10/ref/models/lookups/)
**Model instances:** [Instance methods](https://docs.djangoproject.com/en/1.10/ref/models/instances/) | [Accessing related objects](https://docs.djangoproject.com/en/1.10/ref/models/relations/)
**Migrations:** [Introduction to Migrations](https://docs.djangoproject.com/en/1.10/topics/migrations/) | [Operations reference](https://docs.djangoproject.com/en/1.10/ref/migration-operations/) | [SchemaEditor](https://docs.djangoproject.com/en/1.10/ref/schema-editor/) | [Writing migrations](https://docs.djangoproject.com/en/1.10/howto/writing-migrations/)
**Advanced:** [Managers](https://docs.djangoproject.com/en/1.10/topics/db/managers/) | [Raw SQL](https://docs.djangoproject.com/en/1.10/topics/db/sql/) | [Transactions](https://docs.djangoproject.com/en/1.10/topics/db/transactions/) | [Aggregation](https://docs.djangoproject.com/en/1.10/topics/db/aggregation/) | [Search](https://docs.djangoproject.com/en/1.10/topics/db/search/) | [Custom fields](https://docs.djangoproject.com/en/1.10/howto/custom-model-fields/) | [Multiple databases](https://docs.djangoproject.com/en/1.10/topics/db/multi-db/) |[Custom lookups](https://docs.djangoproject.com/en/1.10/howto/custom-lookups/) | [Query Expressions](https://docs.djangoproject.com/en/1.10/ref/models/expressions/) | [Conditional Expressions](https://docs.djangoproject.com/en/1.10/ref/models/conditional-expressions/) | [Database Functions](https://docs.djangoproject.com/en/1.10/ref/models/database-functions/)
**Other:** [Supported databases](https://docs.djangoproject.com/en/1.10/ref/databases/) | [Legacy databases](https://docs.djangoproject.com/en/1.10/howto/legacy-databases/) | [Providing initial data](https://docs.djangoproject.com/en/1.10/howto/initial-data/) | [Optimize database access](https://docs.djangoproject.com/en/1.10/topics/db/optimization/) | [PostgreSQL specific features](https://docs.djangoproject.com/en/1.10/ref/contrib/postgres/)

The view layer[¶](https://docs.djangoproject.com/en/1.10/#the-view-layer)
Django has the concept of “views” to encapsulate the logic responsible for processing a user’s request and for returning the response. Find all you need to know about views via the links below:
**The basics:** [URLconfs](https://docs.djangoproject.com/en/1.10/topics/http/urls/) | [View functions](https://docs.djangoproject.com/en/1.10/topics/http/views/) | [Shortcuts](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/) | [Decorators](https://docs.djangoproject.com/en/1.10/topics/http/decorators/)
**Reference:** [Built-in Views](https://docs.djangoproject.com/en/1.10/ref/views/) | [Request/response objects](https://docs.djangoproject.com/en/1.10/ref/request-response/) | [TemplateResponse objects](https://docs.djangoproject.com/en/1.10/ref/template-response/)
**File uploads:** [Overview](https://docs.djangoproject.com/en/1.10/topics/http/file-uploads/) | [File objects](https://docs.djangoproject.com/en/1.10/ref/files/file/) | [Storage API](https://docs.djangoproject.com/en/1.10/ref/files/storage/) | [Managing files](https://docs.djangoproject.com/en/1.10/topics/files/) | [Custom storage](https://docs.djangoproject.com/en/1.10/howto/custom-file-storage/)
**Class-based views:** [Overview](https://docs.djangoproject.com/en/1.10/topics/class-based-views/) | [Built-in display views](https://docs.djangoproject.com/en/1.10/topics/class-based-views/generic-display/) | [Built-in editing views](https://docs.djangoproject.com/en/1.10/topics/class-based-views/generic-editing/) | [Using mixins](https://docs.djangoproject.com/en/1.10/topics/class-based-views/mixins/) | [API reference](https://docs.djangoproject.com/en/1.10/ref/class-based-views/) |[Flattened index](https://docs.djangoproject.com/en/1.10/ref/class-based-views/flattened-index/)
**Advanced:** [Generating CSV](https://docs.djangoproject.com/en/1.10/howto/outputting-csv/) | [Generating PDF](https://docs.djangoproject.com/en/1.10/howto/outputting-pdf/)
**Middleware:** [Overview](https://docs.djangoproject.com/en/1.10/topics/http/middleware/) | [Built-in middleware classes](https://docs.djangoproject.com/en/1.10/ref/middleware/)

The template layer[¶](https://docs.djangoproject.com/en/1.10/#the-template-layer)
The template layer provides a designer-friendly syntax for rendering the information to be presented to the user. Learn how this syntax can be used by designers and how it can be extended by programmers:
**The basics:** [Overview](https://docs.djangoproject.com/en/1.10/topics/templates/)
**For designers:** [Language overview](https://docs.djangoproject.com/en/1.10/ref/templates/language/) | [Built-in tags and filters](https://docs.djangoproject.com/en/1.10/ref/templates/builtins/) | [Humanization](https://docs.djangoproject.com/en/1.10/ref/contrib/humanize/)
**For programmers:** [Template API](https://docs.djangoproject.com/en/1.10/ref/templates/api/) | [Custom tags and filters](https://docs.djangoproject.com/en/1.10/howto/custom-template-tags/)

Forms[¶](https://docs.djangoproject.com/en/1.10/#forms)
Django provides a rich framework to facilitate the creation of forms and the manipulation of form data.
**The basics:** [Overview](https://docs.djangoproject.com/en/1.10/topics/forms/) | [Form API](https://docs.djangoproject.com/en/1.10/ref/forms/api/) | [Built-in fields](https://docs.djangoproject.com/en/1.10/ref/forms/fields/) | [Built-in widgets](https://docs.djangoproject.com/en/1.10/ref/forms/widgets/)
**Advanced:** [Forms for models](https://docs.djangoproject.com/en/1.10/topics/forms/modelforms/) | [Integrating media](https://docs.djangoproject.com/en/1.10/topics/forms/media/) | [Formsets](https://docs.djangoproject.com/en/1.10/topics/forms/formsets/) | [Customizing validation](https://docs.djangoproject.com/en/1.10/ref/forms/validation/)

The development process[¶](https://docs.djangoproject.com/en/1.10/#the-development-process)
Learn about the various components and tools to help you in the development and testing of Django applications:
**Settings:** [Overview](https://docs.djangoproject.com/en/1.10/topics/settings/) | [Full list of settings](https://docs.djangoproject.com/en/1.10/ref/settings/)
**Applications:** [Overview](https://docs.djangoproject.com/en/1.10/ref/applications/)
**Exceptions:** [Overview](https://docs.djangoproject.com/en/1.10/ref/exceptions/)
**django-admin and manage.py:** [Overview](https://docs.djangoproject.com/en/1.10/ref/django-admin/) | [Adding custom commands](https://docs.djangoproject.com/en/1.10/howto/custom-management-commands/)
**Testing:** [Introduction](https://docs.djangoproject.com/en/1.10/topics/testing/) | [Writing and running tests](https://docs.djangoproject.com/en/1.10/topics/testing/overview/) | [Included testing tools](https://docs.djangoproject.com/en/1.10/topics/testing/tools/) | [Advanced topics](https://docs.djangoproject.com/en/1.10/topics/testing/advanced/)
**Deployment:** [Overview](https://docs.djangoproject.com/en/1.10/howto/deployment/) | [WSGI servers](https://docs.djangoproject.com/en/1.10/howto/deployment/wsgi/) | [Deploying static files](https://docs.djangoproject.com/en/1.10/howto/static-files/deployment/) | [Tracking code errors by email](https://docs.djangoproject.com/en/1.10/howto/error-reporting/)

The admin[¶](https://docs.djangoproject.com/en/1.10/#the-admin)
Find all you need to know about the automated admin interface, one of Django’s most popular features:
[Admin site](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/)
[Admin actions](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/actions/)
[Admin documentation generator](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/admindocs/)

Security[¶](https://docs.djangoproject.com/en/1.10/#security)
Security is a topic of paramount importance in the development of Web applications and Django provides multiple protection tools and mechanisms:
[Security overview](https://docs.djangoproject.com/en/1.10/topics/security/)
[Disclosed security issues in Django](https://docs.djangoproject.com/en/1.10/releases/security/)
[Clickjacking protection](https://docs.djangoproject.com/en/1.10/ref/clickjacking/)
[Cross Site Request Forgery protection](https://docs.djangoproject.com/en/1.10/ref/csrf/)
[Cryptographic signing](https://docs.djangoproject.com/en/1.10/topics/signing/)
[Security Middleware](https://docs.djangoproject.com/en/1.10/ref/middleware/#security-middleware)

Internationalization and localization[¶](https://docs.djangoproject.com/en/1.10/#internationalization-and-localization)
Django offers a robust internationalization and localization framework to assist you in the development of applications for multiple languages and world regions:
[Overview](https://docs.djangoproject.com/en/1.10/topics/i18n/) | [Internationalization](https://docs.djangoproject.com/en/1.10/topics/i18n/translation/) | [Localization](https://docs.djangoproject.com/en/1.10/topics/i18n/translation/#how-to-create-language-files) | [Localized Web UI formatting and form input](https://docs.djangoproject.com/en/1.10/topics/i18n/formatting/)
[Time zones](https://docs.djangoproject.com/en/1.10/topics/i18n/timezones/)

Performance and optimization[¶](https://docs.djangoproject.com/en/1.10/#performance-and-optimization)
There are a variety of techniques and tools that can help get your code running more efficiently - faster, and using fewer system resources.
[Performance and optimization overview](https://docs.djangoproject.com/en/1.10/topics/performance/)

Python compatibility[¶](https://docs.djangoproject.com/en/1.10/#python-compatibility)
Django aims to be compatible with multiple different flavors and versions of Python:
[Jython support](https://docs.djangoproject.com/en/1.10/howto/jython/)
[Python 3 compatibility](https://docs.djangoproject.com/en/1.10/topics/python3/)

Geographic framework[¶](https://docs.djangoproject.com/en/1.10/#geographic-framework)
[GeoDjango](https://docs.djangoproject.com/en/1.10/ref/contrib/gis/) intends to be a world-class geographic Web framework. Its goal is to make it as easy as possible to build GIS Web applications and harness the power of spatially enabled data.

Common Web application tools[¶](https://docs.djangoproject.com/en/1.10/#common-web-application-tools)
Django offers multiple tools commonly needed in the development of Web applications:
**Authentication:** [Overview](https://docs.djangoproject.com/en/1.10/topics/auth/) | [Using the authentication system](https://docs.djangoproject.com/en/1.10/topics/auth/default/) | [Password management](https://docs.djangoproject.com/en/1.10/topics/auth/passwords/) | [Customizing authentication](https://docs.djangoproject.com/en/1.10/topics/auth/customizing/) | [API Reference](https://docs.djangoproject.com/en/1.10/ref/contrib/auth/)
[Caching](https://docs.djangoproject.com/en/1.10/topics/cache/)
[Logging](https://docs.djangoproject.com/en/1.10/topics/logging/)
[Sending emails](https://docs.djangoproject.com/en/1.10/topics/email/)
[Syndication feeds (RSS/Atom)](https://docs.djangoproject.com/en/1.10/ref/contrib/syndication/)
[Pagination](https://docs.djangoproject.com/en/1.10/topics/pagination/)
[Messages framework](https://docs.djangoproject.com/en/1.10/ref/contrib/messages/)
[Serialization](https://docs.djangoproject.com/en/1.10/topics/serialization/)
[Sessions](https://docs.djangoproject.com/en/1.10/topics/http/sessions/)
[Sitemaps](https://docs.djangoproject.com/en/1.10/ref/contrib/sitemaps/)
[Static files management](https://docs.djangoproject.com/en/1.10/ref/contrib/staticfiles/)
[Data validation](https://docs.djangoproject.com/en/1.10/ref/validators/)

Other core functionalities[¶](https://docs.djangoproject.com/en/1.10/#other-core-functionalities)
Learn about some other core functionalities of the Django framework:
[Conditional content processing](https://docs.djangoproject.com/en/1.10/topics/conditional-view-processing/)
[Content types and generic relations](https://docs.djangoproject.com/en/1.10/ref/contrib/contenttypes/)
[Flatpages](https://docs.djangoproject.com/en/1.10/ref/contrib/flatpages/)
[Redirects](https://docs.djangoproject.com/en/1.10/ref/contrib/redirects/)
[Signals](https://docs.djangoproject.com/en/1.10/topics/signals/)
[System check framework](https://docs.djangoproject.com/en/1.10/topics/checks/)
[The sites framework](https://docs.djangoproject.com/en/1.10/ref/contrib/sites/)
[Unicode in Django](https://docs.djangoproject.com/en/1.10/ref/unicode/)

The Django open-source project[¶](https://docs.djangoproject.com/en/1.10/#the-django-open-source-project)
Learn about the development process for the Django project itself and about how you can contribute:
**Community:** [How to get involved](https://docs.djangoproject.com/en/1.10/internals/contributing/) | [The release process](https://docs.djangoproject.com/en/1.10/internals/release-process/) | [Team organization](https://docs.djangoproject.com/en/1.10/internals/organization/) | [Meet the team](https://docs.djangoproject.com/en/1.10/internals/team/) | [Current roles](https://docs.djangoproject.com/en/1.10/internals/roles/) | [The Django source code repository](https://docs.djangoproject.com/en/1.10/internals/git/) | [Security policies](https://docs.djangoproject.com/en/1.10/internals/security/) | [Mailing lists](https://docs.djangoproject.com/en/1.10/internals/mailing-lists/)
**Design philosophies:** [Overview](https://docs.djangoproject.com/en/1.10/misc/design-philosophies/)
**Documentation:** [About this documentation](https://docs.djangoproject.com/en/1.10/internals/contributing/writing-documentation/)
**Third-party distributions:** [Overview](https://docs.djangoproject.com/en/1.10/misc/distributions/)
**Django over time:** [API stability](https://docs.djangoproject.com/en/1.10/misc/api-stability/) | [Release notes and upgrading instructions](https://docs.djangoproject.com/en/1.10/releases/) | [Deprecation Timeline](https://docs.djangoproject.com/en/1.10/internals/deprecation/)



