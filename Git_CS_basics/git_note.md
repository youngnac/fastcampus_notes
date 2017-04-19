2017/1/16

#  VCS
####  Version Control System
: 파일의 변화를 시간에 따라 기록 후 다시 꺼내 올 수 있는 시스템. 거의 모든 파일의 버전 관리 가능.
= 파일의 변경이력을 기록하여 관리를 용이하게 함.

#####  이점
* 변경 이록 기록과 내용 공유 가능
* 타인과의 작업 병합 가능
* 과거 상태로 복구 가능
* 브랜치를 통해 병렬 관리

#####  Git의 활용
* 복잡한 브랜치 관리
* 로컬과 리모트 저장소 분리
* 빠른 속도
* 다양한 서비스 업체(예: github)
* 다양한 보조툴(예: 소스트리)

####  Stage Area (add)
* 변경이력 관리
* 추가 및 수정 가능
* 준비구역
* why need? 안정성, (목적에따라) 작업을 분리 가능

####  Local Repo (commit)
* 변경이력 저장
* 컴퓨터 내 존재
* 오프라인->빠른속도
* 잦은 처리 가능
* 빠르게 복구 가능
####  Remote Repo (push)
* 전통적인 저장소의 개념
* 외부 서버
* 인터넷 필
* 다른 사용자와의 접전


##  Git의 구조
Working D ->(add) Stage A ->(commit) Local R. -> (push) Remote R.


* Fetch: Remote R -> Local R
* Merge: Local R. -> Working D.
* Pull: Remote R -> Wording D. 




2017/1/17
#  Using Terminal
#####  ETC git commands
* git config --list
* git --help
* ls -a :show all files even hidden files)
* rm -rf folder : delete a folder
* open . :open a curren foler)
* cd .. : move to previous folder

##  git시작
1. git --version : git version check
2. git init : initializing git local repo creation
3. ls -a (shows git file)

##  git file add
1. git status : status check
2. create file
3. git add file_name.ext : add action
4. git add -A: add all files

###  Git Unstage
* git reset HEAD file.ext

##  git commit and msg
1. git commit -m "xxx" 또는 git commit (new window) : commit and commit msg 
2. git log : history check
3. git commit -a -m "XX": skip add action and commit right away 
4. git commit file.ext -m "XX": commit certain file

##  git Clone
* git clone url-copy&paste


###  git RESET and Recovery
* git reset --hard HEAD^ : reset to previous
* git reset --hard HEAD~#  : reset to # stage before (# =2: 두개 삭제)
* git reset --hard ORIG_HEAD

### git Branch
* git checkout -b name
* git branch name & git checkout name (위와 같은 결과)
* git branch : check list of branches.
* git checkout: move to another
* git log --all --graph: see graphs of commit/branches
* git branch -d name : delete a branch
* git push --all. : push all branches

###  ``git remove and add new remote repo
* git remote set-url origin git://new.url.here
* git remote rm origin and git add remote url

##  .gitignore (레포생성후 바로 생성)
1. git init
2. 바로 vim .gitignore
3. gitignore.io
4. ctrl c, ctrl v
5. :wq
6. initial commit


