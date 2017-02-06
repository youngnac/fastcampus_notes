#django tutorial

#01:
##Install Django
1. install git
2. `git clone git://github.com/django/django.git`
3. set virtual environment (pyenv virtualenv...)
4. Install django: `pip install -e django/`
5. check version: `python -m django --version`

##start my project:
1. 코드를 저장할 디렉토리로 이동 한 후
2. `django-admin startproject mysite`
3. `mysite` directory 가 생성됨

```
mysite/			# 단순히 프로젝트를 담는 공간
    manage.py		#상호작용 하는 커맨드라인의 유틸리티
    mysite/		#project 를 위한 실제 Python 패키지들이 저장
        __init__.py		# 패키지 처럼 다루라고 알려주는 용도
        settings.py		# project 의 환경/구성을 저장
        urls.py			#URL 선언을 저장
        wsgi.py			#project 를 서비스 하기 위한 WSGI 호환 웹 서버의 진입점
``` 
###서버 테스트
**`python manage.py runserver`**
>`python manage.py runserver 8080` : 사용 포트 명시 가능
>`python manage.py runserver 0.0.0.0:8000` : 다른 IP이용

##create "polls" app
**`python manage.py startapp polls`**
```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```
###check your tree
>`tree`
#####*트리에서 pycache 숨기기: tree -I '__pycache__'*
>alias 설정함(tree-pycache)

###create first view
>polls/views.py 에 view 작성
>
```python
from django.http import HttpResponse
def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```
>polls/url.py 에
>작성된 view를 url과 연결하기
>
```python 
from django.conf.urls import url
from . import views
urlpatterns = [
    url(r'^$', views.index, name='index'),
    # url에 처음부터 끝 아무것도 없을 때 즉 아무것도 입력되지 않았을 떄 views.index를 실행한다
]
```
>mystie/url.py 에
>point the root(최상단) URLconf at the polls.urls module
>
```python
from django.conf.urls import include, url
from django.contrib import admin
urlpatterns = [
	#polls로 시작하고 그 뒤에 뭐든 오든 다음의 요즘은..polls.urls에서 시행한다
    url(r'^polls/', include('polls.urls')),
    url(r'^admin/', admin.site.urls),
]
```

#02:
###DB설정
>mysite/settings.py 에서 `DATABASE =...` engine, name설정

###INSTALLED_APPS
>`INSTALLED_APPS` : Django 인스턴스에서 활성화된 모든 Django 어플리케이션들의 이름
>
>* django.contrib.admin – 관리용 사이트
>* django.contrib.auth – 인증 시스템
>* django.contrib.contenttypes – 컨텐츠 타입을 위한 프레임워크
>* django.contrib.sessions – 세션 프레임워크.
>* django.contrib.messages – 메세징 프레임워크.
>* django.contrib.staticfiles – 정적 파일을 관리하는 프레임워크.

##Migrate
**`python manage.py migrate`**
> migrate 명령은 INSTALLED_APPS 의 설정을 탐색하여, mysite/settings.py 의 데이터베이스 설정과 app 과 함께 제공되는 데이터베이스 migrations에 따라 필요한 데이터베이스 테이블을 생성. 

##Model
>부가적인 메타데이터를 가진 데이터베이스의 구조(layout)
>in polls app: choice and question
>
>polls/models.py
>
```python
from django.db import models
class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')
class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```
>Field: needs required arguments
>>- CharField 의 경우 max_length
>>- default=0 : votes 의 기본값을 0

##activate models
>models 은 이러한 역할을 한다:

>- 이 app 에 대하여 데이터베이스 스키마 생성 (CREATE TABLE statements)
>- Question 과 Choice 객체에 접근하기 위한 Python 데이터베이스 접근 API 를 생성

####INSTALLED_APPS: (모델로 인해)
>`INSTALLED_APPS = ['polls.apps.PollsConfig']` 추가됨.

###Make migrations
**`python manage.py makemigrations polls`**
>모델을 변경시킨 사실과(이 경우에는 새로운 모델을 만들었습니다) 이 변경사항을 migration 으로 저장시키고 싶다는 것을 Django 에게 알림

#####SQL상의 변동사항 보기
> `python manage.py sqlmigrate polls 0001`

###MIGRATE:
>`python manage.py migrate`
>
>테이블에 손대지 않고도 모델의 반복적인 변경 가능

###manage.py as shell
>`python manage.py shell`

##in Shell:
>모델을 사용해보자

```python
>>> from polls.models import Question, Choice  >>>> Question.objects.all()
<QuerySet []>
>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now())
>>> q.save()
>>> q.id
1
>>> q.question_text
"What's new?"
>>> q.pub_date
datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=<UTC>)
>>> q.question_text = "What's up?"
>>> q.save()
>>> Question.objects.all()
<QuerySet [<Question: Question object>]>
```
##\_\_str__형식으로 model 수정
>Django 가 자동으로 생성하는 관리 사이트 에서도 객체의 표현을 사용하기 때문에...이렇게 수정하는 것이 용이

```python
class Question(models.Model):
...
	def __str__(self):
		return self.question_.text
class Choice(models.Model):
...
    def __str__(self):
        return self.choice_text
```

###in Shell: (` python manage.py shell_plus`
```python
>>> from polls.models import Question, Choice
>>> Question.objects.all()
<QuerySet [<Question: What's up?>]>
>>> Question.objects.filter(id=1)
<QuerySet [<Question: What's up?>]>
>>> Question.objects.filter(question_text__startswith='What')
<QuerySet [<Question: What's up?>]>
>>> from django.utils import timezone
>>> current_year = timezone.now().year
>>> Question.objects.get(pub_date__year=current_year)
<Question: What's up?>
>>> Question.objects.get(pk=1)
<Question: What's up?>
>>> q = Question.objects.get(pk=1)
>>> q.was_published_recently()
True
>>> q = Question.objects.get(pk=1)
>>> q.choice_set.all()
<QuerySet []>
>>> q.choice_set.create(choice_text='Not much', votes=0)
<Choice: Not much>
>>> q.choice_set.create(choice_text='The sky', votes=0)
<Choice: The sky>
>>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)
>>> c.question
<Question: What's up?>
>>> q.choice_set.all()
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
>>> q.choice_set.count()
3
>>>Choice.objects.filter(question__pub_date__year=current_year)
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
>>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
>>> c.delete()
```

##Admin module
###login as admin
1. python manage.py createsuperuser
2. Username: admin
3. Email address: admin@example.com
4. PW

###개발 서버 실행
>**`python manage.py runserver`**

###Make the poll app modifiable in the admin(관리자로 app 수정하도록 설정):
>tell admin that Question objects have an admin interface

>polls/admin.py
>
```python
from django.contrib import admin
from .models import Question
admin.site.register(Question)
```
>python manage.py runserver

>go check website

>you can see Question now
>
> now try: change the published time of the question 
and save it.
then check if it really changed...

```python 
>>>from polls.models import Question, Choice
>>> q.pub_date
>>> q = Question.objects.get(pk=1)
>>> q.pub_date
datetime.datetime(2017, 2, 5, 10, 14, 49, tzinfo=<UTC>)
```

#03:
#View
> type of webpage in your application that often serves a specific function
 
1. Question “index” – displays the latest few questions.
2. Question “detail” – displays a question text, with no results but with a form to vote.
3. Question “results” – displays results for a particular question.
4. Vote action – handles voting for a particular choice in a particular question.

###How to get from a URL to a view?
> Django uses what are known as ‘**URLconfs**’.


##Writing Views
###views.py

```python
def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)

def results(request, question_id):
    response = "You're looking at the results of question %s."
    return HttpResponse(response % question_id)

def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)
```

###url.py

```python
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

####Look at browser: http://127.0.0.1:8000/polls/34/
>It is a 'detail' view of Question 34.

>It shows: You are looking at questoin 34.

####how??
>views.py 
>>
```python
def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)
```

>url.py
>>
```python
from django.conf.urls import url
from . import views
urlpatterns = [
	url(r'^$', views.index, name='index'),
	]
```

#04:
##form
> where: **`polls/templates/polls/detail.html`**
> 
> vote할 수 있도록 라디오 버튼 생성하고, submit 버튼 생성

```python
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
####Generic Views:
>URL에서 전달 된 매개 변수에 따라 데이터베이스에서 데이터를 가져 오는 것과 템플릿을 로드하고 렌더링 된 템플릿을 리턴하는 기본 웹 개발의 일반적인 경우
>generic view로 전환

1. URLconf를 변환하십시오.
2. 불필요한 오래된보기 중 일부를 삭제하십시오.
3. Django의 제너릭 뷰를 기반으로 새로운 뷰를 도입하십시오.
```python
from django.conf.urls import url
from . import views
app_name = 'polls'		#namespace
urlpatterns = [
    url(r'^$', views.IndexView.as_view(), name='index'),
    url(r'^(?P<pk>[0-9]+)/$', views.DetailView.as_view(), name='detail'),
    url(r'^(?P<pk>[0-9]+)/results/$', views.ResultsView.as_view(), name='results'),
    url(r'^(?P<question_id>[0-9]+)/vote/$', views.vote, name='vote'),
]
```
>polls/views.py

```python
from django.shortcuts import get_object_or_404, render
from django.http import HttpResponseRedirect
from django.urls import reverse
from django.views import generic
from .models import Choice, Question
class IndexView(generic.ListView):
    template_name = 'polls/index.html'
    context_object_name = 'latest_question_list'
    def get_queryset(self):
        return Question.objects.order_by('-pub_date')[:5]
class DetailView(generic.DetailView):
    model = Question
    template_name = 'polls/detail.html'
class ResultsView(generic.DetailView):
    model = Question
    template_name = 'polls/results.html'
def vote(request, question_id):
#...원래와 같이
```


#05:
##TEST
>shell을 사용해 테스트하는 것처럼 automated test(나대신 system이 해주는)를 만들자

##Test가 왜 필요할까??
###시간 절약
> 내가 했던 코딩을 손대지 않고 잘 돌아가는지, 문제가 있는지 바로바로 테스트할 수 있다. 

###더나은 코드 설계와 신뢰성

###팀워크
>팀원들의 코드를 망치지 않고 테스트를 통해 검사

##Basic Testing Strategy
>test-driven development: 테스트를 만들어 놓고 이에 맞춰 개발
>아니면... 새로운 기능을 추가하거나 코드 변경을 할 떄 첫 테스트를 만들자

##첫 테스트 만들기
###ID bugs
>Question.was_published_recently()에서 pub_date가 미래일 경우에도 True 반환.
>>shell에서 확인하자
>
```python
>>> import datetime
>>> from django.utils import timezone
>>> from polls.models import Question
>>> future_question = Question(pub_date=timezone.now() + datetime.timedelta(days=30))
>>> future_question.was_published_recently()
True
```
###Create a test to expose bugs
> `polls/tests.py` 에서 미래의 시간을 pub_date로 설정해 반환 값을 False인 것을 확인하는 test.
> 이를 django.test.TestCase subclass를 만들어서 작성한다. 
>
```python
import datetime
from django.utils import timezone
#TestCase불러온다
from django.test import TestCase
from .models import Question
#TestCase의 subclass로 작성
class QuestionMethodTests(TestCase):
    def test_was_published_recently_with_future_question(self):
        time = timezone.now() + datetime.timedelta(days=30)
        future_question = Question(pub_date=time)
        self.assertIs(future_question.was_published_recently(), False)
```

###Run test: python manage.py test polls
>test를 실행하면, false가 아닌 true를 반환하여 AssertionError가 발생

### Fix bugs
>fix models.py
>날짜가 과거일때만 True
>
```python
def was_published_recently(self):
    now = timezone.now()
    return now - datetime.timedelta(days=1) <= self.pub_date <= now
```
##Test a View
>pub_date의 미래값이 보지 않도록 설정되어 있는 test
###Django Test Client
> simulate a user interacting with the code at the view leve (test.py나 shell에서 사용가능)
>- shell setting:
>
```python
>>>> from django.test.utils import setup_test_environment
>>>> setup_test_environment()
```

#06:
##앱 sytle.css
>`polls 디렉토리에 static 디렉토리 생성`
>
>`polls/static/polls/style.css`

in this file:
>
```css
li a {
    color: green;		#색 넣기
}
body {
    background: white url("images/background.gif") no-repeat right bottom;		#배경이미지 삽입
}
```

polls/templates/polls/index.html

```python
{% load static %}
<link rel="stylesheet" type="text/css" href="{% static 'polls/style.css' %}" />

```