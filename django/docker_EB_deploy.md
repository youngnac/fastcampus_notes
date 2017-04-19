#  docker

##  [Docker commands](https://docs.docker.com/v1.11/engine/reference/commandline/cli/)

####  컨테이너 실행 : run

```
docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```

`run`명령어를 사용하면 사용할 이미지가 저장되어 있는지 확인하고 없다면 다운로드(`pull`)를 한 후 컨테이너를 생성(`create`)하고 시작(`start`) 합니다.

- `-p`, `-publish=[]`: 호스트에 연결된 컨테이너의 특정 포트를 외부에 노출. 주로 웹 서버의 포트를 노출할 때 사용. `<ip주소>:<호스트 포트>:<컨테이너 포트>`이며 포트하나만 쓰면 컨테이너 포트가 지정되고 호스트 포트 번호는 무작위로 설정된다.

####  컨테이너 목록 확인 : ps

```
docker ps [OPTIONS]
```

ps 명령어는 실행중인 컨테이너 목록을 보여준다. `-a`,`--all`옵션을 사용하면 됨.



####  컨테이너 실행 중지 : stop

```
docker stop [OPTIONS] CONTAINER [CONTAINER...]
```

실행중인 하나 이상의 컨테이너를 실행 중지한다. 컨테이너 이름 혹은 ID를 띄어쓰기로 구분해서 넣어주면 된다. 

> 도커 ID의 전체 길이는 64자리 입니다. 하지만 명령어의 인자로 전달할 때는 전부 입력하지 않아도 됩니다. 예를 들어 ID가 `abcdefgh...`라면 `abcd`만 입력해도 됩니다. 앞부분이 겹치지 않는다면 1-2자만 입력해도 됩니다.



####  컨테이너 제거하기 : rm

```
docker rm [OPTIONS] CONTAINER [CONTAINER...]
```

'**종료된**' 컨테이너를 제거한다. 

> 중지된 컨테이너를 일일이 삭제 하는 건 귀찮은 일입니다. `docker rm -v $(docker ps -a -q -f status=exited)` 명령어를 입력하면 중지된 컨테이너 ID를 가져와서 한번에 삭제합니다.



####  이미지 목록 확인 : images

```
docker images [OPTIONS] [REPOSITORY[:TAG]]
```

도커가 다운로드 한 이미지 목록을 확인한다. 

- `-q`, `--quiet`: 숫자 ID 값만 보여준다.
- `-f`, `—filter=[]`: 필터
- `-a`, `--all`: 전부 보여준다. 

####  이미지 다운로드 : pull

```
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
```

이미지를 최신 버전으로 다시 다운받을 때 사용한다. 



####  이미지 삭제 : rmi

```
docker rmi [OPTIONS] IMAGE [IMAGE...]
```

**컨테이너가 실행 종료 된** 이미지를 삭제할 때 사용한다. 



####  컨테이너 로그 확인 : logs

```
docker logs [OPTIONS] CONTAINER
```

docker의 로그를 출력해준다. `-f`,`--tail` 옵션을 사용. 



####  컨테이너 명령어 실행 : exec

```
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
```

실행중인 컨테이너에서 커맨드를 실행하게 한다. `-it`옵션.



####  이미지 빌드 : build

```
docker build [OPTIONS] PATH | URL | -
```

도커 이미지를 빌드해준다. PATH에는 도커파일이 있는 경로를 입력한다.



####  OPTIONS

| 옵션       | 설명                                       |
| -------- | :--------------------------------------- |
| -d       | detached mode 흔히 말하는 백그라운드 모드            |
| -p       | 호스트와 컨테이너의 포트를 연결 (포워딩) , '호스트 포트 : 컨테이너 포트' 로 입력 |
| -v       | 호스트와 컨테이너의 디렉토리를 연결 (마운트)                |
| -e       | 컨테이너 내에서 사용할 환경변수 설정                     |
| –-name   | 컨테이너 이름 설정, 지정하지 않으면 도커가 자동으로 생성해 준다.    |
| –-rm     | 프로세스 종료시 컨테이너 자동 제거                      |
| -it      | -i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션        |
| -–link   | 컨테이너 연결 [컨테이너명:별칭]                       |
| -a, —all | 실행했다 종료된(Exited) 컨테이너까지 포함해 목록을 전부 보여준다. |
| -t, —tag | 이름 태그를 지정 / logs: show timestamps        |
| -f       | logs: 실시간 로그를 출력 / rm,rmi : —force, 강제로 삭제 |
| —tail    | 마지막 10개의 로그만 출력                          |



####  Docker commands

| 명령어        | 구문                                       | 설명                                       |
| ---------- | ---------------------------------------- | ---------------------------------------- |
| FROM       | `FROM <image>:<tag>`                     | 베이스 이미지를 지정                              |
| MAINTAINER |                                          | Dockerfile을 관리하는 사람의 정보.                 |
| COPY       | `COPY <src>... <dest>`                   | 파일이나 디렉토리를 이미지 안으로 복사, target 디렉토리가 없으면 자동으로 생성 |
| ADD        | `ADD <src>... <dest>`                    | COPY와 유사하지만! `<src>`에 URL과 압축파일을 입력 가능. 압축파일을 입력하면 자동으로 압축 해제와 복사 |
| RUN        | `RUN <command>`                          | 명령어를 그대로 실행, 내부적으로 `/bin/sh -c`뒤에 명령어를 실행한다 |
| CMD        | `CMD ["executable","param1","param2"]`   | 컨테이너가 실행되었을 때 실행할 명령어를 정의. 이미지를 빌드할 때는 실행되지 않으며 여러개의 CMD가 존재할 경우 가장 마지막 CMD만 실행. 한번에 여러개를 실행하고 싶을 경우 `run.sh`파일을 작성하여 데몬으로 실행하거나 `supervisord`같은 프로그램을 사용한다. |
| WORKDIR    | `WORKDIR /path/to/workdir`               | RUN, CMD, ADD, COPY등이 이루어질 기본 디렉토리를 설정합니다. 각 명령어의 현재 디렉토리는 한 줄 한 줄마다 초기화되기 때문에 `RUN cd /path`를 하더라도 다음 명령어에선 다시 위치가 초기화 됩니다. 같은 디렉토리에서 계속 작업하기 위해서 `WORKDIR`을 사용합니다. |
| EXPOSE     | `EXPOSE <port> [<port>…]`                | 도커 컨테이너가 실행되었을 때 요청을 기다리고 있는(Listen) 포트를 지정합니다. 여러개의 포트를 지정할 수 있습니다. |
| VOLUME     | `VOLUME ["/data"]`                       | 컨테이너 외부에 파일시스템을 마운트 할 때 사용합니다. 반드시 지정하지 않아도 마운트 할 수 있지만, 기본적으로 지정하는 것이 좋습니다. |
| ENV        | `ENV <key> <value>`, `ENV <key>=<value>` | 컨테이너에서 사용할 환경변수를 지정합니다. 컨테이너를 실행할 때 `-e`옵션을 사용하면 기존 값을 오버라이딩 |

###  프로젝트 세팅

- settings_common: allowed hosts 
- settings_local: django - secret key

###  project name change 

설정폴더 이름이 항상 프로젝트 이름과 같았는데 config로 바꿔준다. 그냥 리네임 해주면됨. 대신에 이렇게 하고 세군데를 바꿔줘야 한다.

> settings.py

```python
ROOT_URLCONF = 'config.urls'
WSGI_APPLICATION = 'config.wsgi.application'
```

> manage.py

```python
	os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings")
```

> wsgi.py

```
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings")
```

- 그리고 해당 프로젝트가 어떤 프로젝트인지 모를 수도 있으니 settings.py 맨위 주석 내용에 다 적어준다 .(주석은 웬만하면 지우지 않는 방향으로 … 버전 정보 등 나중에 참고해야할 정보들이 있다)

###  docker를 실행해보자 

- 우분투로 컨테이너 열어서 setting 해보자

```shell
docker run --rm -it ubuntu:16.04 /bin/bash # 우분투이미지로 컨테이너 여릴림
apt-get update
apt-get install python3
apt-get install python3-pip
apt-get isntall nginx
cd /srv.
mkdir app
cd app
pip3 install django
pip3 install uwsgi
django-admin start project eb
python2 manage.py runserver
#  런서버해도 이상태에서는 확인 불가  (포트를 열어주고...해야함)
exit
```

- exit하는 순간 다 날아간다. 
- image 는 불변하기 때문에 남지만, container는 종료시 다 삭제.
- 따라서 **dockerfile** (project 상위 경로에) 을 생성하여 설정 내용을 작성/저장




##  docker file 작성 및 이미지 생성 
- 프로젝트 폴더의 가장 상위에 작성: `deploy_eb_docker/Dockerfile`

```Dockerfile
FROM        ubuntu:16.04 #  우분투환경에서 부터 시작하자
MAINTAINER  me@gmail.com

RUN         apt-get update
RUN         apt-get install python3
RUN         apt-get install python3-pip
RUN         apt-get install nginx

WORKDIR     /srv	#  working directory 바꾸기
RUN         mkdir app
WORKDIR     /srv/app
```

>꿀팁!! 설치할때 예/아니오 묻는거 대비해서 미리 -y 옵션 넣어준다.
>
>```docker
>RUN         apt-get -y update
>RUN         apt-get -y install python3
>RUN         apt-get -y install python3-pip
>RUN         apt-get -y install nginx
>```
>

- docker **image 를 "eb"** 라는 이름으로 dockerfile 내용으로 **생성**

```shell
docker build -t eb .
```

>  이제 `docker images` 해보면 eb라는 이름으로 도커 이미지가 생겨있다.

```
(docker_env) ➜  deploy_eb_docker git:(master) ✗ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
eb                  latest              68779008ec73        3 minutes ago       510 MB
```

- "eb" 이미지를 사용하여 컨테이너 실행

```
docker run --rm -it eb /bin/bash
```
해보면 이제 설치하는 과정 필요없이 도커파일에 적어둔 것이 다 설치되어 있다. 

####  Docker command lines

######  도커 image/container 삭제방법

- 먼저 컨테이너 한꺼번에 지우기 `docker rm $(docker ps -a -q)`
- 이미지 한꺼번에 지우기 `docker rmi $(docker images -q)`

######  이미지가 안 지워질때:  먼저 container stop > rm >rim

```Shell
docker stop 컨테이너 id
docker rm 컨테이너 id
docker rmi 이미지 id # 이미지 삭제
```

#####  none 이미지 지우기

같은 이름:태그로  이미지를 빌드할 때마다 생성된 필요없는 (none)이미지는 용량만 차지하므로 삭제한다. (그전에 생성됐던 이미지의 이름과 태그가 사라지고 <none> 으로 처리/변환된다.)공

> This will display untagged images, that are the leaves of the images tree (not intermediary layers). These images occur when a new build of an image takes the `repo:tag` away from the image ID, leaving it as `<none>:<none>` or untagged. 

다음 커맨드를 입력해서 **none이미지들을 한꺼번에 삭제**한다.

```
docker rmi $(docker images -f "dangling=true" -q)
```

######  이미지를 빌드할때 Dockerfile 커맨드를 한줄씩 실행하는데, 이때마다 이미지 레이어를 생성하고 저장한다. 

> 결론적으로 도커 빌드는 `임시 컨테이너 생성` > `명령어 수행` > `이미지로 저장` > `임시 컨테이너 삭제` > `새로 만든 이미지 기반 임시 컨테이너 생성` > `명령어 수행` > `이미지로 저장` > `임시 컨테이너 삭제` > … 의 과정을 계속해서 반복. 



####  Docker layer docker base file 작성

도커 이미지 만들때 변경사항 있을때마다(디렉토리를 더 만든다던가) 모든걸 다시 설치하는건 비효율적이다

따라서, 초기 설정에 필요한 것들 설치하는 부분만을 도커파일 베이스 작성

- _Dockerfile_base: 초기 설정에 필요한 것들 설치하는 부분 작성
  - `.gitignore`에도 `_Dockerfile*`을 추가해준다.

```dockerfile
FROM        ubuntu:16.04
MAINTAINER  usergmail.com

RUN         apt-get -y update
RUN         apt-get -y install python3
RUN         apt-get -y install python3-pip
RUN         apt-get -y install nginx


```

> 기존 Dockerfile에서는 pip들 설치하는 부 분 주석처리 

- 이 파일을 사용하여 eb-base 이미지 생성

```shell
docker build -t eb-base . -f Dockerfile_base
```

>  도커 이미지 필요없는 것 지움 (none)

- dockerfile 

```dockerfile
FROM        eb-base
MAINTAINER  user@gmail.com

#  RUN         apt-get -y update
#  RUN         apt-get -y install python3
#  RUN         apt-get -y install python3-pip
#  RUN         apt-get -y install nginx

COPY        . /srv/app
WORKDIR     /srv/app

RUN         pip3 install -r requirements.txt
RUN         pip3 install uwsgi
WORKDIR     /srv/app/django_app
CMD         ["python3", "manage.py", "runserver", "0:8080"]
```

도커파일 바꾸고 나면 다시 빌드해준다. 현재는 eb-base 를 기반으로 만들어둔 도커파일. 

>도커 이미지에 문제 있어서 도커파일 베이스로 이미지를 만들어보려고 할 때 
>`docker build . -t eb_base -f _Dockerfile_base`



- `docker build . -t eb` : 다시 eb image 생성



#####  도커 실행 : `docker run --rm -it -p 4567:8080 eb`

코드에서 도커 컨테이너의 8080 포트와 로컬의 4567 포트를 연결해주었으므로 
localhost:4567로 브라우저에서 서버가 잘 작동되나 테스트해본다.

##  docker내에서 uwsgi setting

####  uwsgi-app.ini 설정파일 작성

기존에 ec2에서 uwsgi setting (cat /etc/uwsgi/sites/app.ini) 와 같이 세팅을 eb에서도 해준다 . 

- deploy_eb_docker/.conf-secret/uwsgi-app.ini

```ini
[uwsgi]
chdir = /srv/app/django_app #  앱을 로드하기 전에 디렉토리를 특정한다. 
module = config.wsgi:application #  WSGI 모듈을 로드한다. 여기서는 우리가 만든 장고 앱
; home 은 삭제해도 무방, 서버의 root에서 파이썬 실행해도 됨

;uid = www-data
;gid = www-data

socket = /tmp/app.sock
chmod-socket = 666
;chown-socket = www-data:www-data #  우분투 컨테이너에서는 root권한 으로 실행되기 때문에 주석처리 했다.

enable-threads = true
master = true
vacuum = true
pidfile = /tmp/app.pid

logger = file:/tmp/uwsgi.log
logger = internalservererror file:/tmp/uwsgi500.log
```

#####  현재 uwsgi 설정 내용을 dockerfile을 수정하여 이미지 생성시 적용한다 

- dockerfile 수정

  ```dockerfile
  FROM        eb-base
  MAINTAINER userㅕ@gmail.com

  #  RUN         apt-get -y update
  #  RUN         apt-get -y install python3
  #  RUN         apt-get -y install python3-pip
  #  RUN         apt-get -y install nginx

  COPY        . /srv/app
  WORKDIR     /srv/app

  RUN         pip3 install -r requirements.txt
  #  install django 
  RUN         pip3 install uwsgi

  COPY        .conf-secret/uwsgi-app.ini /etc/uwsgi/sites/app.ini
  ```

###  실행해보기

- 새로만든 이미지로 **local 4040포트와 컨테이너의 8080포트 를 연결**시켜 실행해본다.

  `docker run --rm -it -p 4040:8080 eb`

  ​

- 그리고 컨테이너에서 **uwsgi 로 서버에 접속**해본다 

  ` uwsgi --http :8080 --chdir /srv/app/django_app/ -w config.wsgi`

해보면 브라우저에서 localhost:4040으로 접속된다.

- 우선 종료하고 다시 도커 컨테이너 실행 후 uWSGI 실행한다.
  - 이번엔 dockefile로 copy 한 app.ini로 실행한다

```shell
uwsgi --http :8080 -i /etc/uwsgi/sites/app.ini 
```

- 이 상태에서 실행되고 있는 현재 컨테이서(서버)를 접속

  - 실행되고 있는 container#  확인은 `docker ps` 로 확인하자

  ```
  docker exec -it container_number/bin/bash
  ```

- 그리고  `cat /tmp/uwsgi.log`하면 log 확인 할 수 있다 (새로고침하면 계속 바뀜)

  ​

##  Docker 내 nginx setting

###  설정파일 작성

> 아까만든 uwsgi-app.ini 는 다시 .conf에 옮겨준다. 딱히 숨길 필요가 없어서. 

- `/etc/nginx/nginx.conf`의 내용을 복사 > `deploy_eb_docker/.conf/nginx.conf` 에 paste
  -  유저는 바꿔주자

#####  nginx.conf

```
user root;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
	worker_connections 768;
	#  multi_accept on;
}

http {

	## 
	#  Basic Settings
	## 

	sendfile on;
	tcp_nopush on;
	tcp_nodelay on;
	keepalive_timeout 65;
	types_hash_max_size 2048;
	#  server_tokens off;

	server_names_hash_bucket_size 512;
	#  server_name_in_redirect off;

	include /etc/nginx/mime.types;
	default_type application/octet-stream;

	## 
	#  SSL Settings
	## 

	ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #  Dropping SSLv3, ref: POODLE
	ssl_prefer_server_ciphers on;

	## 
	#  Logging Settings
	## 

	access_log /var/log/nginx/access.log;
	error_log /var/log/nginx/error.log;

	## 
	#  Gzip Settings
	## 

	gzip on;
	gzip_disable "msie6";

	#  gzip_vary on;
	#  gzip_proxied any;
	#  gzip_comp_level 6;
	#  gzip_buffers 16 8k;
	#  gzip_http_version 1.1;
	#  gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

	## 
	#  Virtual Host Configs
	## 

	include /etc/nginx/conf.d/*.conf;
	include /etc/nginx/sites-enabled/*;
}


# mail {
# 	#  See sample authentication script at:
# 	#  http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
# 
# 	#  auth_http localhost/auth.php;
# 	#  pop3_capabilities "TOP" "USER";
# 	#  imap_capabilities "IMAP4rev1" "UIDPLUS";
# 
# 	server {
# 		listen     localhost:110;
# 		protocol   pop3;
# 		proxy      on;
# 	}
# 
# 	server {
# 		listen     localhost:143;
# 		protocol   imap;
# 		proxy      on;
# 	}
# }
```

- 이번엔 `/etc/nginx/sites-available/app `의 내용을 `deploy_eb_docker/.conf/nginx-app.conf `에 넣어준다. 

#####  

```
server {
    listen 4040;
    server_name localhost ec2-13-124-55-38.ap-northeast-2.compute.amazonaws.com ec2.kizmo04.com;
    charset utf-8;
    client_max_body_size 128M;


    location / {
        uwsgi_pass    unix:///tmp/app.sock;
        include       uwsgi_params;
    }
}
```

#####  위 파일 내용 적용위해 dockerfiles 수정

```
FROM        eb-base
MAINTAINER  user@gmail.com

# RUN         apt-get -y update
# RUN         apt-get -y install python3
# RUN         apt-get -y install python3-pip
# RUN         apt-get -y install nginx

COPY        . /srv/app
WORKDIR     /srv/app

# RUN         pip3 install -r requirements.txt
# RUN         pip3 install uwsgi

COPY        .conf/uwsgi-app.ini /etc/uwsgi/sites/app.ini
COPY        .conf/nginx.conf    /etc/nginx/nginx.conf
COPY        .conf/nginx-app.conf /etc/nginx/sites-available/app.conf
RUN         ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled/app.conf
```

- 다시 빌드 : `docker build . -t eb`
- 그리고 `docker run --rm -it -p 5050:4040 eb` > `cd /srv/app` > `nginx` > `uwsgi --ini /etc/uwsgi/sites/app.ini`
- 그리고 확인해보자 (local:5050)

###  Docker 컨테이너 안에서 서비스 무중단

<u>도커 안쪽</u>에서는 *systemctl* 즉, *서비스 형태로 프로그램을 실행하는것이 불가능* 해서 다른 방법으로 실행해야 한다. [링크](https://bestna.wordpress.com/2014/11/10/docker-container-run-%EC%9D%B4%EC%95%BC%EA%B8%B0/)

```
docker run --rm -it -p 5050:4040 eb
```

그리고 **컨테이너 안에서 nginx uwsgi** 함 근데 이렇게 하다가 *프로세스가 꺼지면 안되니까*

**supervisor라는 것을 사용한다** 

###  supervisor 추가

supervisor는 프로세스가 꺼지지 않게 해주는 역할을 한다. supervisord로 실행하면 데몬으로 백그라운드에서 돌면서 계속 실행시켜준다.  [링크:10분만에 익히는 supervisor 설치와 사용법](https://jwkcp.github.io/2016/11/07/how-to-use-supervisor-in-one-minute/)

#####  .conf/supervisor-app.conf 추가

```
[program:uwsgi]
command = uwsgi --ini /etc/uwsgi/sites/app.ini

[program:nginx]
command = nginx
```

#####  dockerfile 수정

```Dockerfile
FROM        eb-base
MAINTAINER  user@gmail.com

#  RUN         apt-get -y update
#  RUN         apt-get -y install python3
#  RUN         apt-get -y install python3-pip
#  RUN         apt-get -y install nginx

COPY        . /srv/app
WORKDIR     /srv/app

#  RUN         pip3 install -r requirements.txt
#  RUN         pip3 install uwsgi

COPY        .conf/uwsgi-app.ini /etc/uwsgi/sites/app.ini
COPY        .conf/nginx.conf    /etc/nginx/nginx.conf
COPY        .conf/nginx-app.conf /etc/nginx/sites-available/app.conf
COPY        .conf/supervisor-app.conf   /etc/supervisor/conf.d/
RUN         ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled/app.conf

EXPOSE      4040
CMD         supervisord -n
```

도커 안에서 4040 포트를 열어주고 수퍼 바이저를 실행하고 설정파일을 복사하게 해줬다. supervisord는 데몬으로 돌아가는 슈퍼바이저 

#####  docker_base 수정

```dockerfile
FROM        ubuntu:16.04
MAINTAINER  user04@gmail.com

RUN         apt-get -y update
RUN         apt-get -y install python3
RUN         apt-get -y install python3-pip
RUN         apt-get -y install nginx
RUN         apt-get -y install supervisor # 추가사항

RUN         pip3 install django
RUN         pip3 install uwsgi
```

**supervisor를 install 해준다.**

#####  nginx.conf 수정

```
user root;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;
daemon off; 
```

맨 위에 daemon off 추가해서 데몬으로 돌아가는 supervisor를 데몬 아닌것처럼 실행되게 해준다.

#####  실행 테스트

이제 다시 

`docker run --rm -it -p 5050:4040 eb ` 로 실행. 로컬의 5050 포트를 도커안의 4040 포트로 연결해줬으니 localhost:5050 으로 브라우저에서 테스트해본다.

> nginx로 접속했을때 500에러 뜨면 `/var/log/nginx/error.log`확인 

##  elasticbeanstalk 에 올리기

그동안 빠른 테스트를 위해 만들었던 dockerfile_base는 삭제한다. 대신 그 내용을 모두 dockerfile로 다시 옮겨준다. 

###  dockerfile 수정

```dockerfile
FROM        ubuntu:16.04
MAINTAINER  kizmo04@gmail.com

COPY        . /srv/app
WORKDIR     /srv/app

RUN         apt-get -y update
RUN         apt-get -y install python3
RUN         apt-get -y install python3-pip
RUN         apt-get -y install nginx
RUN         apt-get -y install supervisor

RUN         pip3 install -r requirements.txt
RUN         pip3 install uwsgi

COPY        .conf/uwsgi-app.ini /etc/uwsgi/sites/app.ini
COPY        .conf/nginx.conf    /etc/nginx/nginx.conf
COPY        .conf/nginx-app.conf /etc/nginx/sites-available/app.conf
COPY        .conf/supervisor-app.conf   /etc/supervisor/conf.d/
RUN         ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled/app.conf

EXPOSE      4040
CMD         supervisord -n
```

이제 더이상 base를 참조하지 않고 ubuntu:16.04 를 참고한다.

###  Elastic Beanstalk 시작하기

![](/Users/yn_c/notes_wps/image_note/eb_strcut.jpg)

[Elastic Beanstalk](https://aws.amazon.com/ko/elasticbeanstalk/?sc_channel=PS&sc_campaign=acquisition_KR&sc_publisher=google&sc_medium=Brand_Beanstalk_E&sc_content=Brand_Elastic_Beanstalk_E&sc_detail=aws%20elastic%20beanstalk&sc_category=beanstalk&sc_segment=150437681477&sc_matchtype=e&sc_country=KR&s_kwcid=AL!4422!3!150437681477!e!!g!!aws%20elastic%20beanstalk&ef_id=WIsrPwAABLM6ZuCp:20170310073542:s)설명

[도커 컨테이너에서 Elastic Beanstalk 앱 배포하기](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker.html)  

도커 컨테이너가 죽으면 엘라스틱 빈스톡이 알아서 다시 실행해줌

[Elastic Beanstalk 시작하기](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/GettingStarted.html)를 따라해본다.(나중에 다시 읽어보기)

우리는 우선 콘솔에서 시작해봄

###  설치하기

1. **eb cli 설치** : 로컬 프로젝트 폴더에서 `pip install awsebcli`로 설치한다.

2. **aws iam console에서 권한 부여** :

   IAM 콘솔 - Users - 유저선택 - add permision  - elasticbeanstalk full access

3. **eb cli configuration** : [참고](http://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/eb-cli3-configuration.html) 

   - 로컬의 프로젝트 폴더에서 `eb init` 하고 잘 선택한다 (region - Asia Seoul, docker 최신 버전, keypair 등). 
   - 그리고  `.elasticbeanstalk`디렉토리와 설정파일이 생겨있다. 

> 문제 발생시 `~/.aws/credentials` 와 `~/.aws/config` 를 확인해본다 

4. `eb create` ([참고](http://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/eb-cli3-getting-started.html))  (env_name 설정, classic 설정)

   - DNS CNAME은 전세계에서 고유한 값임.

5. 프로젝트 폴더에 **Dockerfile.aws.json 생성**한다.

   - Dockerfile.aws.json

   ```json
   {
     "AWSEBDockerrunVersion": 2,
     "volumes": [
       {
         "name": "fc-deploy-eb",
         "host": {
           "sourcePath": "/var/app/current/django_app"
         }
       }
     ],
     "containerDefinitions": [
       {
         "name": "fc-deploy-eb",
         "essential": true,
         "memory": 512,
         "portMappings": [
           {
             "hostPort": 80,
             "containerPort": 4040
           }
         ]
       }
     ]
   }
   ```



6. `eb deploy` (scp와 같은 역할) :eb는 git폴더가 있으면 commit된 내용들만 전송을 한다. 

####  eb deploy ! 

> `eb logs` 로그를 확인
>
> `eb open` 실행?



7. `eb open` : 디플로이 성공 후에 오픈해본다. 에러가 남. 

8. `eb ssh`  >  접속이 됨. /var/log 로 가면 로그가 쌓여있다. eb-activity.log 를 확인해본다.

   ​

*deploy할때마다 우분투 받는것부터 설치랑 전부 다 하기 때문에 오래걸림.* 

> 에러 로그 확인하기!
>
> `eb ssh`
>
> ```shell
> [ec2-user@ip-172-31-15-214 ~]$ sudo docker exec -it `sudo docker ps --no-trunc -q | head -n 1` /bin/bash
> root@49e378316da5:/srv/app#  cd /tmp
> root@49e378316da5:/tmp#  l
> app.pid  app.sock=  uwsgi.log
> root@49e378316da5:/tmp#  vi uwsgi.log 
> ```
>
> 

###  .ebignore 추가

.gitignore에 .conf-secret 이 추가되어서 커밋할때 .conf-secret 내용이 eb deploy할때 안들어가진다. 그래서 settings.py에 config 파일을 못 가져와서 에러가 남.  **.ebignore 에 .conf-secret 을 빼준다**

#####  .ebignore 파일 작성 (`vi deploy_eb_docker/.ebignore`)

```
!.conf-secret/
```

다시 eb deploy 하고 eb open 해보면 

```
Invalid HTTP_HOST header: 'fc-wps-eb.ap-northeast-2.elasticbeanstalk.com'. You may need to add 'fc-wps-eb.ap-northeast-2.elasticbeanstalk.com' to ALLOWED_HOSTS.
```

이런 에러가 뜬다. 아직 ALLOWED HOSTS 를 설정하지 않았기 때문. 