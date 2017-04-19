

#  스태틱(s3)이랑 db(RDS)나눠 관리하기

##  S3로 static 과 media 관리 

#####  Amazon S3 : [django-storages](https://django-storages.readthedocs.io/en/latest/backends/amazon-S3.html) 로 사용되는 것 중 하나

기존에는 [DEFAULT_FILE_STORAGE](https://docs.djangoproject.com/en/1.10/ref/settings/# default-file-storage)를 사용하고 있었음. 

###   Amazon S3로 관리하게 되면? 

사용자가 upload하는 이미지 > S3 저장됨

deploy 했을 때 static 파일들이 static root로 옮겨졌지만, 이제는 **collectstatic > S3로 올라가게 될 것.** 

####  Boto 3  - backends for S3

[Boto3](https://boto3.readthedocs.io/en/latest/) 참고

- local shell에서 `pip install boto3`
  - python 실행- import boto3 > create_bucket 
  - bucket name 은 고유해야함 (이 bucket이 media와 static files 저장소 )

```
client.create_bucket(Bucket='fc-ync-bucket',CreateBucketConfiguration={'LocationConstraint':'ap-northeast-2'})
```

###  django에서 storage를 S3로 설정하기 

- settings.py - storage관련 설정

```Python
STORAGE_S3 = os.environ.get('STORAGE') == 'S3' or DEBUG is False
#  서버 실행시 static 루트를 사용할 것인가
if STORAGE_S3:
    DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
    STATICFILES_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
else:
    STATIC_ROOT = os.path.join(ROOT_DIR, 'static_root')
    STATIC_URL = '/static/'
    
INSTALLED_APPS= ...'storages' # 추가!
```

- settings_common.json

```json
{
  "django": 
    "secret_key": "~~~"
  },
  "aws":{
    "access_key_id":"~~~",	
    "secret_access_key":"~~~", 
    "storage_bucket_name":"fc-ync-bucket" # s3 console에서 확인 가능
  }
}
```

- 그리고 settings.py에서 config로 불러오기

```python
AWS_ACCESS_KEY_ID = config['aws']['access_key_id']
AWS_SECRET_ACCESS_KEY = config['aws']['secret_access_key']
AWS_STORAGE_BUCKET_NAME = config['aws']['storage_bucket_name']
```

- Django storage 설치

  -  `pip install django-storages`

- local에서 서버환경 DEBUG 모드:

  - `MODE='DEBUG' STORAGE='S3' ./manage.py`로 돌려야함

  - 스크맆트로 작성 (vi ~/.scripts/manages3); 작성 후 권한 변경 chmod 755

    ```Shell
    # !/usr/local/bin/zsh
    MODE='DEBUG' STORAGE='S3' ./manage.py $*
    ```

  - 작성 후 권한 변경 `chmod 755 ~/.scripts/manageS3`

#####  이제 collectstatic 해보자!

```
MODE='DEBUG' STORAGE='S3' ./manage.py collectstatic
```

- S3 console가서 버켓 확인하면 static files이 올라가 있는 것을 확인 가능

![](/Users/yn_c/notes_wps/image_note/static_loaded_bucket.png)

####  STATIC_URL 설정하기

- settings.py에 S3 storage 사용시 static url을 지정

  ```python
  if STORAGE_S3:
      DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
      STATICFILES_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
      #  s3 bucket에서 사진들의 url을 확인해보자
      STATIC_URL = 's3.{region}.amazonaws.com/{bucket_name}/'.format(
          region = config['aws']['s3_region'],
          bucket_name = config['aws']['s3_storage_bucket_name']
      )
  ```

- 서버로 보내주고(scp) > requirement 설치 > 사이트에서 정적파일들을 확인 

  > error가 뜰때는: 
  >
  > - 서버에서 MODE='DEBUG' ./manage.py runserver 0:8080 
  >
  >
  > - ec2 console 의 security groups에 8080 포트 추가
  > - public DNS:8080 으로 접속

  > uwsgi 500 error를  /tmp/uwsgi.log 에기록하려면
  >
  > - `sudo vi /etc/uwsgi/sites/app.ini`해서 아래 내용 작성
  >
  > ```shell
  > logger = file:/tmp/uwsgi.log
  > logger = internalservererror file:/tmp/uwsgi500.log
  > ```

- 서울지역에서 S3를 사용하려면 

  - settings_common.json에 `"s3_signature_version" :"s3v4"` 추가 
    - `"s3_region":"ap-northeast-2"`도 추가해주자
  - settings.py 에 `AWS_S3_SIGNATURE_VERSION = config['aws']['s3_signature_version']`



####  Staticfiles 을 S3의 Static dir에 올리기

장고의 스토리지: external-lib / site-packages/ storages/backends/s3boto3.py와 [boto3로 S3사용하기](https://boto3.readthedocs.io/en/latest/guide/s3.html) 를 맞춰주어서 사용할 수 있게 해줌. 따라서 변수명을 맞춰주어야 한다.

- settings.py와 같은 경로에 **storages.py 추가 후 아래 내용 작성**

  ```python
  from django.conf import settings
  from storages.backends.s3boto3 import S3Boto3Storage
  ```


  class StaticStorage(S3Boto3Storage):
      location = 'static'
      #  overwrite 가능하게
      file_overwrite = True 
- S3 bucket 삭제 후 다시 `manages3 collectstatic` > static dir 생성되고 파일들이 그 곳으로 올라간 것을 확인

#####  하지만 사이트를 보면 아직 불러오지는 못 하고 있다. s3에 static 폴더에 static files들이 올라갔지만, site에서 볼 때 불러오지 못하고 있음. 불러오는 경로 STATIC_URL을 재 설정하자 

- settings.py 수정

  - 서버에서 정적파일 주소는 
    `https://fc-kizmo-bucket.s3.amazonaws.com/images/img1.gif`
    즉 `AWS_S3_CUSTOM_DOAMIN` 뒤에 붙어 나온다. `AWS_S3_CUSTOM_DOAMIN` 은 필수! 

```python

AWS_S3_CUSTOM_DOMAIN = '{}.s3.amazonaws.com'.format(AWS_STORAGE_BUCKET_NAME)

  ...

  if STORAGE_S3:
      DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
      STATICFILES_STORAGE = 'deploy_ec2.storages.StaticStorage'
      STATICFILES_LOCATION = 'static'
      STATIC_URL = 'https://{custom_domain}/{staticfiles_location}/'.format(
          custom_domain=AWS_S3_CUSTOM_DOMAIN,
          staticfiles_location=STATICFILES_LOCATION
      )
```

- 그리고 storages.py `location = 'static'` > `location = settings.STATICFILES_LOCATION` 수정
- scp & reset 후 사이트 접속해 이미지가 잘 불러와지는 것을 볼 수 있음

####  media files는 S3 > media dir 

user가 직접 upload하게되는 media files 도 S3의 media dir로 따로 관리/저장할 수 있게 설정하자 

- storages.py

  ```python
  class MediaStorage(S3Boto3Storage):
      location = settings.MEDIAFILES_LOCATION
  	#  같은 이름의 파일이 들어오면 overwrite못하게
      file_overwrite = False
  ```

  - #####  storage overwrite 옵션 주기 - 장고

    [amazon S3](https://django-storages.readthedocs.io/en/latest/backends/amazon-S3.html)문서 참고해서 overwrite옵션 바꿔준다. 소스 내부를 보면 file_overwrite로 직접 옵션 줄 수 있는 것을 알 수 있음!

    > AWS_S3_FILE_OVERWRITE (optional: default is True)

    > By default files with the same name will overwrite each other. Set this to False to have extra characters appended.
    >
    > 즉, overwrite = False 일 때, overwrite되지 않고 같은 파일명으로 새롭게 저장을 시도할 때 파일명 뒤에 extra 문자들이 붙어 저장된다. 

  ​

- 새로 settings.py에 지정, MEDIA_URL 도 수정

  ```Python
  if STORAGE_S3:
      STATICFILES_STORAGE = 'deploy_ec2.storages.StaticStorage'
      STATICFILES_LOCATION = 'static'
      STATIC_URL = 'https://{custom_domain}/{staticfiles_location}/'.format(
          custom_domain=AWS_S3_CUSTOM_DOMAIN,
          staticfiles_location=STATICFILES_LOCATION
      )
      DEFAULT_FILE_STORAGE = 'deploy_ec2.storages.MediaStorage'
      MEDIAFILES_LOCATION = 'media'
      MEDIA_URL = 'https://{custom_domain}/{mediafiles_location}/'.format(
          custom_domain=AWS_S3_CUSTOM_DOMAIN,
          mediafiles_location=MEDIAFILES_LOCATION
      )
  ```

> staticfiles_storage : collectstatic 할때 올려주는 스토리지
>
> default_file_storage : 유저가 업로드(ex: ImageField의 upload_to) 하거나 그밖의 모든 경우에 쓰인다. )
>
> 이 파일들을 불러올 때는 S3에서 확인 할 수 있는 URL을 사용한다!

##  AWS RDS의 사용

[Amazon RDS(Relational Database Service)](http://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/Welcome.html)

#####  DB서버를 하나 만들어주는 것과 같다.

####  RDS 생성

아마존 RDS는 세군데 나눠서 저장. DB서버를 하나 만들어주는 것과 같다.

- Aws console 에서 rds 선택 > free tier 옵션 선택 > postgresSQL db 만든다.
  - 즉, RDS 와 postgresSQL 연결
- rds instance 의 설정페이지에서 security group 선택 
  - security group inboud 에 **type postgresSQL port 5432 myip** 로 추가

####  pgadmin 연결

- pgadmin4 열고 >  create server
- instance 생성시 만든 db(deploy_ec2)/user/pw사용
- connection탭 > hostname / address 는 RDS console 에서 **end point를 복사**해서 `:5432` 지우고 넣기
- 나머지는 알아서 입력( port 5432, username은 RDS 생성시의 user

####  django setting

- settings_local.json 수정

  ```json
  "db-rds": {
      "engine": "django.db.backends.postgresql_psycopg2",
      "name": "deploy_ec2", #  db name
      "user": "RDS_USER",
      "password": "RDS_PW",
      "host": "fc-kizmo-db.czdqhzlgxd1t.ap-northeast-2.rds.amazonaws.com", #  RDS console 의 endpoint
      "port": "5432"
    }
  ```

- settings.py 수정 

  - db를 RDS를 사용하도록 수정

```python
DB_RDS = os.environ.get('RDS') == 'RDS'

...

if DEBUG and DB_RDS:
    #  디버그 모드이며 DB_RDS옵션일경우 로컬 postgreSQL이 아닌 RDS로 접속해 테스트 한다
    config_db = config['db_rds']
else:
    #  그 외의 경우에는 해당 db 설정을 따름
    config_db = config['db']

DATABASES = {
    'default': {
        'ENGINE': config_db['engine'],
        'NAME': config_db['name'],
        'USER': config_db['user'],
        'PASSWORD': config_db['password'],
        'HOST': config_db['host'],
        'PORT': config_db['port'],

    }
}
```

- `MODE='DEBUG' DB='RDS' STORAGE='S3' ./manage.py` DEBUG 모드로
- 하지만 `django.db.utils.ProgrammingError: relation "django_session" does not exist` 발생 > **migrate 하자** 
- 사이트확인 하면 db가 잘 적용되어 페이지가 잘뜬다 
- *settings_local.json 에서 host 도 AWS_RDS 값으로 넣어준다.* 

####  서버에서 postgresql 삭제

**이제 서버에서 postgresql을 사용하지 않는다.**

- 서버 접속하여 postgresql 삭제
  - `sodo apt-get remove postgresql`

#####  settings_deploy.json 도 db 의 host 를 RDS 의 endpoint 값으로 바꿔준다.

####  마지막으로...

이제 로컬과 배포 환경에서 다른 db를 사용하도록 분리되었다. 

- 이후 서버로 **deploy** 하고

###  중요!!

AWS RDS쪽 security group의 inbound가 현재 my ip만 허용 

- 따라서 AWS RDS쪽 security group의 inbound에 ec2의 **security group인 Rule을 추가**

  ![](/Users/yn_c/notes_wps/image_note/rds_security_add.jpg)

######  그렇지 않으면 504에러

- 서버 reset > `createsuperuser` ( 로컬에서든 서버에서든 아무데서나 만들어도 상관없음) 다만 `MODE='DEBUG' STORAGE='S3' DB='RDS'`꼭 켜고 `createsuperuser` 해줘야한다. 

#####  admin page에 잘 로그인

#####   EC2를 사용한 배포 끝