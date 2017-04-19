[reference_blog](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)
# docker 
## what is it?
- "an open platform for developers and sysadmins to build, ship, and run distributed applications, whether on laptops, data center VMs, or the cloud"
- 다양한 프로그램, 실행환경을 컨테이너로 추상화하고 동일한 인터페이스를 제공하여 프로그램의 배포 및 관리를 단순하게 해주는 open platform

### docker container  컨테이너
: 컨테이너는 격리된 공간에서 프로세스가 동작하는 가상화 기술 중 하나

#### 가상화의 발전
- 기존의 가상화 방식은 주로 OS를 가상화 > 호스트OS위에 게스트 OS 전체를 가상화하기 때문에 매우 무겁고 느림. 
- 반가상화 방식: CPU의 가상화 기술(HVM)을 이용한 KVMKernel-based Virtual Machine과 반가상화 Paravirtualization방식의 Xen이 등장
	- ex:  OpenStack이나 AWS, Rackspace같은 클라우드 서비스
- **가상화든 반가상화든 추가적으로 os설치로 성능 저하!**

##### 프로세스를 격리
- 가상화보다 가볍고 빠르게 동작
- CPU나 메모리는 딱 프로세스가 필요한 만큼만 추가로 사용
- 하나의 서버에 여러개의 컨테이너를 실행하면 서로 영향을 미치지 않고 독립적으로 실행됨

> 도커의 기본 네트워크 모드는 Bridge모드: 약간의 성능 손실 발생
> 
> 네트워크 성능이 중요한 프로그램의 경우 `--net=host` 사용 고려

### image 이미지
: 컨테이너 실행에 필요한 파일과 설정값등을 포함하고 있는 것

- immutable
- container = image 실행한 상태
	- 추가되거나 변하는 값은 컨테이너에 저장
	- ex: ubuntu이미지는 ubuntu를 실행하기 위한 모든 파일을 가지고 있고 MySQL이미지는 debian을 기반으로 MySQL을 실행하는데 필요한 파일과 실행 명령어, 포트 정보등을 가지고 있음.
- 도커 이미지는 [Docker hub](https://hub.docker.com/)에 등록하거나 [Docker Registry](https://docs.docker.com/registry/) 저장소를 직접 만들어 관리할 수 있습니다
- 이미지는 url 방식으로 관리


###  layer 레이어
- image (이미지)는 여러개의 읽기 전용read only 레이어로 구성되고 파일이 추가되거나 수정되면 새로운 layer (레이어)가 생성됨
- ex: ubuntu 이미지가 A + B + C; ubuntu 이미지를 베이스로 만든 nginx 이미지는 A + B + C + nginx
- 컨테이너를 생성할 때도 레이어 방식을 사용; 기존의 이미지 레이어 위에 읽기/쓰기read-write 레이어를 추가하는 것

### create image
- 도커는 이미지를 만들기 위해 Dockerfile 이라는 파일에 자체 DSLDomain-specific language 언어를 이용하여 이미지 생성 과정을 기록
- 서버에 프로그램을 설치와 의존성 패키지를 설치, 그리고 설정파일 또한 Dockerfile 로 관리

#### [docker commands](https://docs.docker.com/engine/reference/commandline/docker/# description)

## docker installation
- [docker for mac](https://docs.docker.com/docker-for-mac/)
- docker for mac은 [xhyve](https://github.com/mist64/xhyve)라는 macOS에서 제공하는 가상환경을 이용
- 설치 후 확인 > `docker version`

## ubuntu container
- `docker run ubuntu:16.04` :실행중인 프로세스 없음으로 바로 종료됨
- `docker run --rm -it ubuntu:16.04 /bin/bash`: bash로 docker에 들어가 -it (키보드 입력), --rm (프로세스 종료 후 삭제)
- `exit` : container 종료
## redis container
:모리기반의 다양한 기능을 가진 스토리지 ([참고](https://redis.io/))

- `docker run -d -p 1234:6379 redis`: -d (detached mode 백그라운드 모드), -p (컨테이너의 포트를 호스트의 포트로 연결)

	> -d : id만 보여주고 종료
	
	> -p : 호스트의 1234포트를 컨테이너의 6379포트로 연결
	
	> 호스트의 포트만 다르게 하면 하나의 서버에 여러개의 redis 서버 띄우기 가능
	
	```
	Trying ::1...
Connected to localhost.
Escape character is '^]'.
set mykey hello	#  새로운 키 저장
+OK
get mykey	#  키 불러오기
$5
hello
quit
+OK
Connection closed by foreign host.
	```
	

## MySQL 5.7 container
- `docker run -d -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name ync_mysql mysql:5.7`

> 패스워드 없이 root계정을 만들기 위해 MYSQL_ALLOW_EMPTY_PASSWORD 환경변수를 설정
> 
> 컨테이너 이름: ync_mysql
> 

## stop > rm
- 컨테이너를 삭제한다는 건 컨테이너에서 생성된 파일이 사라진다는 뜻
- 컨테이너 삭제시 유지해야하는 데이터는 반드시 컨테이너 내부가 아닌 외부 스토리지에 저장해야 합니다
	- 가장 좋은 방법은 AWS S3같은 클라우드 서비스를 이용하는 것이고 그렇지 않으면 데이터 볼륨Data volumes을 컨테이너에 추가해서 사용

```
deploy_ec2 git/master*
(deploy_ec2) ❯ docker run -d -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name mysql -v /my/own/datadir:/var/lib/ync_mysql mysql:5.7
1517c30578fe4f673226206a73ff202602df44aa6b5f784b53c37839cbee5660
docker: Error response from daemon: Mounts denied:
The path /my/own/datadir
is not shared from OS X and is not known to Docker.
You can configure shared paths from Docker -> Preferences... -> File Sharing.
See https://docs.docker.com/docker-for-mac/osxfs/# namespaces for more info.
..
```










