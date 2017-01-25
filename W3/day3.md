2017-01-25
#python 설정 (mac)

### ~/.zshrc 파일에 root설정

```
export PYENV_ROOT=/usr/local/var/pyenv
if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi
if which pyenv-virtualenv-init > /dev/null; then eval "$(pyenv virtualenv-init -)"; fi
```

>Install utilities to solve issues regarding arrow keys in cell
`brew install readline xz`

### Install python 3.4.3 using pyenv

`pyenv install 3.4.3`

#### Create virtual environment
>**pyenv virtualenv version#.# env_name**

`pyenv virtualenv 3.4.3 fc-python` 

#### Select the virtualenv as loca

`pyenv global 3.4.3`

*중간에 pyenv versions 치면 모든 버전 나옴*
#### Check which python version 
`python --version`

>후에 사용 폴더를 만든 후 그 폴더로 이동

#### Switch to virtualenv in local
`pyenv local fc-python`
 
> `$ l` 로 확인후 
> `python` 실행
> `exit()` 종료

####Interactive python shell iPython (install)
>`pip list` : 패키지 리스트 (`--format=colums` : 컬럼형태로 나옴)

`pip install ipython`

>pip란? 가상환경 안에서 파이썬 패키지 관리자
>


#PYTHON grammar

##extend append
extend 리스트안으로 추가
append 리스트안에 집합체로 리스트 추가

since list is not iteral if you 
