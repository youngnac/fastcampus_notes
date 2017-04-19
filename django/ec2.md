[참고](http://technerd.tistory.com/55)
# EC2(Elastic Compute Cloud)로 배포하기

![diagram](/Users/yn_c/notes_wps/image_note/ec2_diagram.png)

#### django-admin startproject deploy_ec2
- root url(views.py - index) 접속: hello world보여주기

### .conf-secret > settings\_common.json과 settings_loca.json 분리
- common:

```json
{"django": {
	"secret_key": "~~~~~"
	}
}
```

- local:

```json
{"django": {
	"allowed_hosts": [
	"*"
	]
	}
}
```

- .conf, .conf_common으로 settings.py에서 json.loads하자

```python
CONFIG_FILE_COMMON = os.path.join(os.path.join(os.path.dirname(BASE_DIR), '.conf-secret'), 'settings_common.json')
config_common = json.loads(open(CONFIG_FILE_COMMON).read())
CONFIG_FILE = os.path.join(os.path.join(os.path.dirname(BASE_DIR), '.conf-secret'), 'settings_local.json')
config = json.loads(open(CONFIG_FILE).read())

ALLOWED_HOSTS = config['django']['allowed_hosts']
SECRET_KEY = config_common['django']['secret_key']
```
- 그리고 config에서 config_common을 합친다 (이 방법말고 update를 사용할 수도..?)

```python
for key, key_dict in config_common:
	if not config.get(key):
		config[key] = {}
		for innerkey, innerkey_dict in key_dict.items():
			config[key][innerkey] = innerkey_dict

```
### .conf-secret never upload to git!!!

## Instance 생성
[aws link](http://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/Instances.html)

- amazon aws services > ec2검색 **(check region: Seoul)**
- "0 Running Instances"
 
### key pair 생성 (aws 접속시 사용) 
- Network & Security > Key Pairs
- Create Key Pair 
- 자동으로 다운로드 된다. 
	- ync.pem 이 다운로드 됨
- 매우 중요; 노출 X!!
- 숨긴폴더 ~/.ssh 로 (없으면 mkdir) 복사 (root X, homefolder ~)
	- `cp ~Downloads/ync.pen ~/.ssh`

### Launch Instance
- launch instance
- select: ubuntu server 16.04 LTS > select: free tier 
- Review and Launch! 

#### server is **too open!**
- configure Security Group:
	- security group name과 description 새로 설정
	- SSH, TCP, 22, **My IP** 로 우선 설정해두자
> 왜 security GROUP?: 다른 instance에도 적용가능 

- 생성해둔 key pair(ync.pem)로 LAUNCH 한다
![instance_view](/Users/yn_c/notes_wps/image_note/aws_instance_view.png)

- launch 된 해당 서버로 접근 방법: public DNS or IP 사용
	- ex: Public DNS: ec2-13-124-56-218.ap-northeast-2.compute.amazonaws.com

### key pair 소유권 정의 (소유자만 접근하도록)
- `cd ~/.ssh` > `chmod 400 ync.pem`  

[참고](http://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)
### ssh 명령을 사용하여 인스턴스에 연결
- 프라이빗 키, user_name@public_dns_name을 지정
-  Amazon Linux의 경우 사용자 이름은 ec2-user입니다. RHEL의 경우 사용자 이름은 ec2-user 또는 root입니다. **Ubuntu의 경우 사용자 이름은 ubuntu** 또는 root입니다. 

>`ssh -i /path/my-key-pair.pem ec2-user@ec2-198-51-100-1.compute-1.amazonaws.com`

`ssh -i ~/.ssh/ync.pem ubuntu@ec2-13-124-56-218.ap-northeast-2.compute.amazonaws.com`

#### 서버에 연결됨 
- **새로운 컴퓨터** 하나가 생겼다고 가정하자

>warning: sudo apt-get install language-pack-ko OR sudo locale-gen ko_KR.UTF-8 (let's just install)

- sudo apt-get update

- pip도 깔자 > `sudo apt-get install python-pip`
- 그외:

	```
	sudo apt-get install zsh
	sudo curl -L http://install.ohmyz.sh | sh
	sudo chsh ubuntu -s /usr/bin/zsh 
	# shell zsh로 변경, exit 하고 적용 확인
	```

- pyenv 설치 [참고](https://github.com/pyenv/pyenv/wiki/Common-build-problems)
`sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils`
- 설치 후 보여지는 pyenv path 설정을 copy/paste > vi ~/.zshrc (맨 아래)

	```
	export PATH="/home/ubuntu/.pyenv/bin:$PATH"
	eval "$(pyenv init -)"
	eval "$(pyenv virtualenv-init -)"
	```
