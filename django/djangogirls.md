#django girls
(in cmd: `./manage.py`
##basic setup
###directory, pyenv, install django, git
1. create directory for the project : `mkdir djangogirls_tutorial`
2. `cd djangogirls_tutorial`
3. create virtual enviorment : `pyenv virtualenv 3.4.3 djangogirl`
4. set pyenv : `pyenv local djangogirl`
5. install django package :`pip install django`
6. create ignore file : `vi .gitignore`
	- pycharm (.idea 상단, 무시)
	- django
	- python
	- macos
7. create requirements.txt that tells required packages for the project: `pip freeze > requirements.txt`
8. initialize git : `git init`
9. create repository
10. `git remote add origin https://github.com/youngnac/django_girls.git`
11. Do the first commit
12. `git push -u origin master`

##Start your project
**`django-admin startproject mysite`**

###Lets look @ tree
```
├── djangogirls			# changed from mysite
│   ├── manage.py
│   └── mysite
│       ├── __init__.py
│       ├── settings.py
│       ├── urls.py
│       └── wsgi.py
└── requirements.txt
```

##Change settings.py
mysite/settings.py
###timezone
```python
TIME_ZONE = 'Asia/Seoul'
```
###set STATIC_root
```python
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```
##Database
>DATABASE is installed in mysite/setting.py 

###to create DB:
- **`python manage.py migrate`**

##Lets run server:
- **`python manage.py runserver`**
- check with browser : [link](http://127.0.0.1:8000/)

#blog를 만들어 보자:
##Object Oriented...
Post (object): 

- (properties)
- title
- text
- author
- created_date
- published_date

* method: publish

##start your app - blog
1. `python manage.py startapp blog`
2. add `blog.apps.BlogConfig` in `INSTALLED_APPS =[...` of mysite/settings.py 
	- INSTALLED_APPS 순서는: 장고, 외부 library, 그리고 우리가 만드는 apps

##models.py and create tables for models
> for obejct 'Post', properties with fields and methods 

```python
from django.db import models
from django.utils import timezone

class Post(models.Model):
    #properties and fields
    author = models.ForeignKey('auth.User')
    title = models.CharField(max_length=200)
    #long text without limit
    text = models.TextField()
    created_date = models.DateTimeField(default=timezone.now)
    published_date = models.DateTimeField(blank=True, null=True)
   
    #method
    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
```

###notify these changes in models.py to django
- **`python manage.py makemigrations blog`**
- **`python manage.py migrate blog`**
> now Post model is in DB

##Admin:

###blog/admin.py
- make our model visible on admin page

```python
from django.contrib import admin
from .models import Post

#to make our model visible on admin page
admin.site.register(Post)
```
###create superuser to get admin control
- **`python manage.py createsuperuser`**
- input userid, email, pw
- try: runserver and login 

####make some posts as admin

##url
>django uses URLconf to find matching urls with views 

###mysite/urls.py

```python
from django.conf.urls import include, url
from django.contrib import admin

    urlpatterns = [
        #url for admin 
        url(r'^admin/', include(admin.site.urls)),
        #import blog.urls
        url(r'', include('blog.urls'))
    ]
```

###blog/urls.py
> import Django's function url and all of our views from the blog application.

```python
from django.conf.urls import url
from . import views

urlpatterns = [
	#directs from 'http://127.0.0.1:8000/' to views.post_list
    url(r'^$', views.post_list, name = 'post_list'),
	]
```

##views
>- request information from the model and pass it to a template
>- put the logic of app
###blog/views.py
> takes a request and return a function render that renders our tmeplate

```python 
from django.shortcuts import render
def post_list(request):
    return render(request, 'blog/post_list.html', {})
```

##templates 
###templates/blog/post_list.html
>before putting codes in post_list.html
>change settings.py to set templates' path
>add 

`TEMPLATES_DIR = os.path.join(BASE_DIR, 'templates')` and 
```python
TEMPLATES= [{'DIRS': [
            TEMPLATES_DIR,
        ],
```

>post_list.html (stub method)

```html
<html>
    <head>
        <title>Django Girls blog</title>
    </head>
    <body>
        <div>
            <h1><a href="">Django Girls Blog</a></h1>
        </div>

        <div>
            <p>published: 14.06.2014, 12:14</p>
            <h2><a href="">My first post</a></h2>
            <p>Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa justo sit amet risus.</p>
        </div>

        <div>
            <p>published: 14.06.2014, 12:14</p>
            <h2><a href="">My second post</a></h2>
            <p>Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut f.</p>
        </div>
    </body>
</html>
```

##using shell

**`./manage.py shell`**
>>`publised_date__lte`: less than equal (gte: greater than equal)

```python
>>> from blog.models import Post
>>> from django.contrib.auth.models import User

>>> Post.objects.all()
<QuerySet [<Post: post 1>, <Post: post 2 yeah>, <Post: post 3 oh yeahhhh>]>
>>> me = User.objects.get(username='youngna')
>>> Post.objects.create(author=me, title='post 6', text='Test')
>>> Post.objects.all()
>>> Post.objects.filter(author=me)
>>> from django.utils import timezone
>>> post = Post.objects.get(title="post 6")
>>> post.publish()
*AttributeError: 'str' object has no attribute 'save'*
>>> Post.objects.filter(published_date__lte=timezone.now())
>>> Post.objects.order_by('created_date')
>>> Post.objects.order_by('-created_date')
>>> Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
<QuerySet [<Post: post 1>, <Post: post 2 yeah>, <Post: post 3 oh yeahhhh>, <Post: post 6>]>
>>> Post.objects.filter(published_date__lte=timezone.now()).order_by('-published_date')
<QuerySet [<Post: post 6>, <Post: post 3 oh yeahhhh>, <Post: post 2 yeah>, <Post: post 1>]>
```

##views, html edited to show real posts

###views.py
```python
from django.shortcuts import render
from django.utils import timezone
from .models import Post


def post_list(request):
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
    return render(request, 'blog/post_list.html', {'posts': posts})
```
###templates/blog/post_list.html
```html
<div>
    <h1><a href="/">Django Girls Blog</a></h1>
</div>

{% for post in posts %}
<div>
    <p>published: {{ post.published_date }}</p>
    <h1><a href="">{{ post.title }}</a></h1>
    <p>{{ post.text|linebreaksbr }}</p>
</div>
{% endfor %}
```

 
 
 
 
 
 
 
 
 
 