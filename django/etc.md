python manage.py migrate blog zero
python manage.py mirgate blog. 0001_initial (요기로 되돌린다)
forms.py에 html - form만들때 사용

- name space and url

> template/polls/list나오는 html

```html
<a href="{% url 'polls:detail' question_id=question.id %}">
```
>polls/urls.py
```python
from django.conf.urls import url

from . import views

app_name = 'polls'
urlpatterns = [
```