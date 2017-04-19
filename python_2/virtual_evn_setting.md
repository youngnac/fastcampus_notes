2017-2-2


## 가상환경 지우기
`pyenv uninstall`

## 가상환경 만들기
- make directory: `mkdir pip_test`
- enter into directory: `cd pip_test`
- create virtual enviroment: `pyenv virtualenv 3.4.3 pip_test_env`
- change to create pyenv: `pyenv local pip_test_env`
- make it as git: `git init`
- create .gigtignore: `vi .gitignore`

##### gitignore.io에서 pycharm, python, git 다 긁어와 삽입 후 저장

#### pycharm interpreter 설정하자
-  directory 에서 `py .`
-  설정에서 add local: `/usr/local/var/pyenv/versions/3.4.3/envs/pip_test_env/bin/python`


#### pip로 설치한것들 txt로 저장
> 누군가 내 레포를 클론 받았을 때 사용하기 편하게.

- `pip freeze`: pacakge version들 다 알려줌
- `pip freeze > requirements.txt`

##### 레포 클론해서 패키지 설치할때
- `pip install -r requirements.txt`


### git 설정
>터미널에서:

- `git add .gitignore`
- `git add .idea/`
- `git commit`
- `git add something.py`
- `git commit`
-  git repo생성
-  git clone 해오기
-  git push



pip_test git/master
(pip_test_env) ❯ pyenv versions
  system
  3.4.3
  3.4.3/envs/pip_test_env
* pip_test_env (set by /Users/yn_c/projects/pip_test/.python-version)

pip_test git/master
(pip_test_env) ❯ pip list
You are using pip version 6.0.8, however version 9.0.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
pip (6.0.8)
setuptools (12.0.5)

pip_test git/master
(pip_test_env) ❯ pip install --upgrade pip
You are using pip version 6.0.8, however version 9.0.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
Collecting pip from https://pypi.python.org/packages/b6/ac/7015eb97dc749283ffdec1c3a88ddb8ae03b8fad0f0e611408f196358da3/pip-9.0.1-py2.py3-none-any.whl# md5=297dbd16ef53bcef0447d245815f5144
  Using cached pip-9.0.1-py2.py3-none-any.whl
Installing collected packages: pip
  Found existing installation: pip 6.0.8
    Uninstalling pip-6.0.8:
      Successfully uninstalled pip-6.0.8

Successfully installed pip-9.0.1

pip_test git/master
(pip_test_env) ❯ pip install requests
Collecting requests
  Using cached requests-2.13.0-py2.py3-none-any.whl
Installing collected packages: requests
Successfully installed requests-2.13.0
p%
pip_test git/master
(pip_test_env) ❯ pip install beautifulsoup4
Collecting beautifulsoup4
  Using cached beautifulsoup4-4.5.3-py3-none-any.whl
Installing collected packages: beautifulsoup4
Successfully installed beautifulsoup4-4.5.3

pip_test git/master
(pip_test_env) ❯ pip list
DEPRECATION: The default format will switch to columns in the future. You can use --format=(legacy|columns) (or define a format=(legacy|columns) in your pip.conf under the [list] section) to disable this warning.
beautifulsoup4 (4.5.3)
pip (9.0.1)
requests (2.13.0)
setuptools (12.0.5)