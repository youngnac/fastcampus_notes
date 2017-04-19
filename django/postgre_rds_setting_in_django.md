## POSTGRE 설정: 
```
yn_c git/master
❯ brew services start postgresql
==> Tapping homebrew/services
Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-services'...
remote: Counting objects: 10, done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 10 (delta 0), reused 5 (delta 0), pack-reused 0
Unpacking objects: 100% (10/10), done.
Tapped 0 formulae (37 files, 50.8K)
==> Successfully started `postgresql` (label: homebrew.mxcl.postgresql)

yn_c git/master
❯ createuser -s ync

yn_c git/master
❯ psql postgres
psql (9.6.2)
Type "help" for help.

postgres=#  \q

yn_c git/master  13s
❯ dropuser ync

yn_c git/master
❯ createuser -s -P ync
Enter password for new role:
Enter it again:

yn_c git/master
❯ createdb deploy_ec2 --owner=ync
```
## settings.py
```
DEBUG = os.environ.get('MODE') == 'DEBUG'
print(DEBUG)
## 	그냥 런서버: DEBUG = False
##  MODE='DEBUG' ./manage.py runserver : DEBUG = True 
DATABASES = {
    'default': {
        'ENGINE': config['db']['engine'],
        'NAME': config['db']['name'],
        'USER': config['db']['user'],
        'PASSWORD': config['db']['password'],
        'HOST': config['db']['host'],
        'PORT': config['db']['port'],
        'PORT': config['db']['port'],
    }
}
```
## settings_local.json

```
{
  "django": {
    "allowed_hosts": [
      "*"
    ]
  },
  "db": {
    "engine": "django.db.backends.postgresql_psycopg2",
    "name": "deploy_ec2",
    "user": "ync",
    "password": "dudsk123",
    "host": "localhost",
    "port": "5432"
  }
}
```
## settings_deploy.json
```
{
  "django": {
    "allowed_hosts": [
      "AWS EC2 IP ADDRESS HERE",
      ".ap-northeast-2.compute.amazonaws.com",
      "URL DOMAIN HERE"
    ]
  },
  "db": {
    "engine": "django.db.backends.postgresql_psycopg2",
    "name": "deploy_ec2",
    "user": "USERNAME",
    "password": "PASSWORD",
    "host": "localhost",
    "port": "5432"
  }
}
```
### SCRIPT로 debug 모드 runserver 후:
```(deploy_ec2) ❯  ~/.scripts/manage runserver  ```