## basic settings
1. create directory for the project : `mkdir djangogirls_tutorial`
2. `cd djangogirls_tutorial`
3. create virtual enviorment : `pyenv virtualenv 3.4.3 djangogirl`
4. set pyenv : `pyenv local djangogirl`
5. install django package :`pip install django`
6. create ignore file : `vi .gitignore`
	- pycharm (.idea 상단, 무시)
	- django
	- pythonc
	- macos
7. create requirements.txt that tells required packages for the project: `pip freeze > requirements.txt`
8. initialize git : `git init`
9. create repository
10. `git remote add origin https://github.com/youngnac/django_girls.git`
11. Do the first commit
12. `git push -u origin master`

## start project
1. django-admin startproejct _name_
2. python manage.py _appname_
3. models.py에 model construct
4. add `'appname.apps.AppNameConfig'` in INSTALLED_APPS in settings.py
5. makemigrations
6. migrate
7. in admin.py: `admin.site.register(Class_name)`
8. createsuperuser

### optional
- ipython
- pip isntall ipython
- pip install django_extensions
