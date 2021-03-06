# Code Repository for CIDM6325

## Fall 2020

We will work through Antonio Melé's [Django By Example - Third Edition](https://www.packtpub.com/web-development/django-3-by-example-third-edition) Applications Together:

* Building a Blog
* Building a Social Website
* Building an Online Shop (eCommerce)
* Building a Chat Server using an API

I encourage you to hand-code from the book and resist the temptation to just copy and paste from the source files.

This README file is constructed using [MarkDown](https://www.markdownguide.org/basic-syntax)

## Python Tips and Guidance

I will keep some general Python notes here:
* For [Visual Studio Code](https://code.visualstudio.com/), please use the [Python Extension](https://marketplace.visualstudio.com/items?itemName=ms-python.python).
* The Python Extension should help you to adhere to the [Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/) with its [linting](https://en.wikipedia.org/wiki/Lint_(software)) capabilities.

## Build a Blog Notes

Here are observations and notes from the chapters

### Chapter 1 - Build a Blog

---

* Make sure you install the latest version of any software the author refers to
    * For Python, you can go to [Python.org](https://www.python.org/)
    * For any packages, such as [Django](https://www.djangoproject.com/), you can consult the [Python Package Index](https://pypi.org/)
    * There are altneratives for **Virtual Environment** implementation, but making one is necessary regardless:
        * The book mentions `venv` which is a part of the standard Python 3.x install.  Note that the command is run like this: `python -m venv`
        * You can also install [miniconda](https://docs.conda.io/en/latest/miniconda.html) which is popular among the data science community.  I have started to use `miniconda` more lately.
    * **Virtual Environments** make a localized copy of Python and any libraries you use for your application so that they are separated from other applications.
        * virtual environments must be enabled and disabled
        * These are all command-line activities and I will be demonstrating this
    * **CLI options**:
        * _Windows_: I recommend that you install [Git For Windows](https://gitforwindows.org/) and use the **GitBash** command-line tool that comes with it
        * _Mac_: The built-in `terminal` App will work
        * **VS Code**: In all cases, try to do your terminal work from within VSCode's built-in terminal.

#### Models

With `Django` you are persisting business data using [Object/Relational Mapping](https://en.wikipedia.org/wiki/Object-relational_mapping), which is a fairly standard approach to web application development.

This is there to structure your database.  Django Models are classes.  Review the basic structures and provisions of the [Python language](https://www.w3schools.com/python/default.asp).

#### Admin

A great feature of Django is the built-in admin interface - you can perform CRUD operations here.

#### Queries

There is a built-in query expression structure that saves time and avoids the need to write SQL directly in most cases.

#### Views

We will try to use Class-Based-Views as much as possible.  However, Function-Based-Views are also possible.

#### Secret Key

Django uses a SECRET_KEY for many security purposes in your project.  Since I will be encouraging the use of Github, publishing your secret key is a problem to avoid.  In fact, if you look at this repository, the initial commit contains a secret key (since changed).

Relevant part of the documentation: [The secret key must be a large random value and it must be kept secret.](https://docs.djangoproject.com/en/3.1/howto/deployment/checklist/#secret-key)

Also, since we'll use Git and Github, placing an effective GITIGNORE file into our repository is important: [Gitignore for a Django project](https://djangowaves.com/tips-tricks/gitignore-for-a-django-project/)

#### Templates

Templates are HTML files with escapes to contain minimal display and presentation logic.  The _beauty_ of your pages will completely rely on one of a few options:
* Your skill with CSS
* Use of a robust component library
* Some template you find/purchase

### Chapter 2 - Enhancing Your Blog with Advanced Features

---

Antonio really adds in some useful features here and provides a fairly advanced use of Django within two chapters of the book:
* Email
* Comments
* Tags and metadata
* Recommendations

#### Forms and Email

Django provides a few classes for working with Forms:
* [Form](https://docs.djangoproject.com/en/3.0/ref/forms/api/)
* [ModelForm](https://docs.djangoproject.com/en/3.0/ref/forms/models/)

There are a number of [FormFields](https://docs.djangoproject.com/en/3.0/ref/forms/fields/) that you include in your Form models to create a user interface.

**Passwords**
Immediately, Antonio starts to talk about using an email account.  Putting authentication information into your source files is a non-starter...

Let's use [DOTENV](https://pypi.org/project/python-dotenv/).  You can see how I handle this in the code.

**django shell**

Notice how Antonio opens the [django shell](https://docs.djangoproject.com/en/3.0/ref/django-admin/#shell) to quickly test email sending. The shell is very handy for this purpose and others.

`python manage.py shell`

**gmail and smtp**

If you use the gmail example from the book, it will likely fail due to the security you have with your account (2-factor and whatnot).  The solution is to use [App Passwords](https://support.google.com/accounts/answer/185833?p=InvalidSecondFactor&visit_id=637344913016438884-914897482&rd=1).

Or, try one of the approaches Antonio mentions.  

We will also explore using the free tier of services like [SendGrid](https://sendgrid.com/) or [MailChimp](https://mailchimp.com/)

#### Comments and Tags

We see another application of foreign key, which reminds us that the Django ORM is there to help shape the tables in an RDBMS.

Don't forget to run `python manage.py makemigrations`

Also, if we want to use the Django Admin, we always need to register our new models with the admin:

``` python
from .models import Post, Comment
@admin.register(Comment)
class CommentAdmin(admin.ModelAdmin):
    list_display = ('name', 'email', 'post', 'created', 'active')
    list_filter = ('active', 'created', 'updated')
    search_fields = ('name', 'email', 'body')

```

**Models and Forms**

We can build forms straight from models, which is handy.

**Taggit**

By using the [Django Taggit](https://github.com/jazzband/django-taggit) app/library, you are getting your first taste of extending Django by using 3rd-party code, we will do more of this as time goes on.

Although Antonio specifies a version, if you install `django-taggit` without a version specified, it will install the latest: 

`pip install django-taggit`

***GOTCHA***: Make sure your virtual environment is active when you run the above.

You will notice that a significant amout of power comes from the [taggit app](https://django-taggit.readthedocs.io/en/latest/getting_started.html)

### Chapter 3 - Extending the Blog Application

---

Although very powerful as-is, in Chapter 3, Antonio rounds out the Blog app with the following features:
* Custom template tags and filters
* Sitemap and syndication (both useful for SEO)
* Full-text search in Postgres (will be difficult to do on desktop)

**Template Tags and Filters**

The templating system provides for dynamic content presentation on both the backend (with views and models) as well as in the front end with template tags and filters.

The Django Template Language has a number of [Built-in template tags and filters](https://docs.djangoproject.com/en/3.0/ref/templates/builtins/)

However, a remarkable feature is the ability to extend these built-in tags and filters with your own custom versions.  In order to do so, we must create our own custom app to hold our customizations.

***GOTCHA***: Because our tag customizations are created in a django app, make sure restart the server after code changes to the tag customizations:
* `CTRL + C` to quit
* `python manage.py runserver` to restart

Custom template filters allow for further text processing as a Django Template is rendered.

* Recall that filters operate in the following manner: `variable|filter_name`
* You can also pass arguments to filters: `variable|filter_name:"arg"`

Django has a number of [built-in filters](https://docs.djangoproject.com/en/3.0/ref/templates/builtins/#built-in-filter-reference).


**Markdown Filter**

Earlier, I mentioned that this README utilizes [Markdown](https://daringfireball.net/projects/markdown/basics).

As a blog application, text input would be greatly enhanced by the use of markdown.  So, we turn to PIP to acquire [Markdown Capabilities](https://python-markdown.github.io/) for our Django Project.

To install: `pip install Markdown`

We can then create custom filters in our `blog_tags` app.

**Sitemaps**

SEO is significantly enhanced when you help Google's crawlers and spiders with a well-crafted sitemap.

Django facilitates this with `django.contrib.sites` and `django.contrib.sitemaps`

To generate sitemaps programmatically - which is common - allows for a responsive tailoring of how you reveal your site to Google et al.

Read more about [the sitemap framework](https://docs.djangoproject.com/en/3.0/ref/contrib/sitemaps/).

**Feeds**

Although social mediea (Facebook, Instragram, Twitter, etc.) have somewhat *regrettably* relegated syndication strategies like RSS and Atom, these are still useful features.

There are [built-in syndication features](https://docs.djangoproject.com/en/3.0/ref/contrib/syndication/) in Django: `django.contrib.syndication`

**Full-Text Search and PostGres**

[PostgreSQL](https://www.postgresql.org/) is a [Relational Database Management System](https://en.wikipedia.org/wiki/Relational_database#RDBMS) that is very full-featured, free, and open source.  It is also the preferred RDBMS in the Django community.

There are installers for Windows, Mac, and Linux.  Postgres will happily run on your local machine and, eventually, on your VPS.

To complete chapter 3, Antonio has us using Postgres for its full-text search capabilities.

***Windows Note***
The commands that Antonio shows at the end of chapter 3 will work on your VPS, but won't necessarily work as shown on windows. For windows, we will use pgAdmin, which is a browser-based management tool for PostgreSQL.  I will demo this in the video that goes along with this code and these notes.

**Settings**

The settings file needs to be adjusted to use Postgres.

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'blog',
        'USER': 'blog',
        'PASSWORD': '*****',
    }
}
```

DO NOT HARD-CODE THIS INFORMATION, use the dotenv approach.

**Fulltext Search**

The postgres contrib package makes this very easy.
