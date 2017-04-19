### requirements:
-
1. pip install django
v1. brew install chromedriver

### open a web: functional_text.py
```python
from selenium import webdriver

browser = webdriver.Chrome()	# open a Chrome web
browser.get('http://localhost:8000')	# go to web served by local PC

assert 'Django' in browser.title #  check in a word 'Django' in the title
```

> OFCOURSE, it will return `AssertionError`

```python
(TDD-1) ❯ python functional_tests.py
Traceback (most recent call last):
  File "functional_tests.py", line 6, in <module>
    assert 'Django' in browser.title
AssertionError
```
## Start project
#### `django-admin startproject superlists`

```
.
├── functional_tests.py
├── requirements.txt
└── superlists
    ├── manage.py
    └── superlists
        ├── __init__.py
        ├── settings.py
        ├── urls.py
        └── wsgi.py
```
### `./manage.py runserver`
#### in original dir: `python functional_tests.py`
Now: you will not have an `AssertionError` and you will see the browser popped up with Django welcome message


