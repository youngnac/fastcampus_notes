2017-2-1 w4.d2
####crawling practice
>where: projects/Python/crawling/crawl.py

- `f12 + alt` open terminal on pycharm
- check `pyenv version` on terminal
- `pip install BeautifulSoup4`
- `pip install requests`
- 해석기 `pip install lxml`
- [request_python](http://docs.python-requests.org/en/master/)
- [beautiful_soup](https://cryptosan.github.io/pythondocuments/documents/beautifulsoup4/)
- [practice page](http://comic.naver.com/webtoon/list.nhn?titleId=626907&weekday=wed)

>shortcut keys
>
>- shift + cmd + (-): 함수만 모아보기



	~/zshrc 설정 
	- alias py="open -a /Applications/PyCharm\ CE.app/Contents/MacOS/pycharm"
	- alias cd-wpsnotes="cd /Users/yn_c/notes_wps/fastcampus_notes"
	- alias open-wpsnotes="open /Users/yn_c/notes_wps/fastcampus_notes"
##How to Crawl: 웹툰 에피소드 제목, 생성일, 링크 
- 우선 url을 찾아보자:
> 
http://comic.naver.com/webtoon/list.nhn?titleId=626907
http://comic.naver.com/webtoon/list.nhn?titleId=626907&page=2
> 
> **parameters: titleId=626907, page=2**

- create packages/files

>	1. **crawl.py** : 메인 동작 코드 파일
>	2. directory: **parser**
>		- **communication.py** : 서버에서 긁어오기
>		- **page.py** : 그 외 모든 동작을 하는 함수들

###commuincation.py

> request를 사용하여 get, find text를 해야하기 때문에 `import request`
> 
> BeautifulSoup을 사용하여 url의 html text를 긁어온다

```python
import requests
from bs4 import BeautifulSoup
```

>url, param을 받아 soup을 가져오는 함수를 만들어주자.
>>- url은 공통적인 부분: http://comic.naver.com/webtoon/list.nhn
>>- param: id와 page 값이. 들어갈 곳이다. 


```python
def get_soup_from_url(url, params=None):
    # requests.get요청을 보낸 결과값(response객체)을 r변수에 할당
    r = requests.get(url, params=params)
    # response객체에서 text메서드를 사용해 내용을 html_doc변수에 할당
    html_doc = r.text
    # BeautifulSoup객체를 생성, 인자는 html text
    soup = BeautifulSoup(html_doc, 'lxml')
    return soup
```
###page.py
>대부분의 동작을 실행하는 함수들을 작성한다.
>
>함수들을 실행하기에 앞서, url에 request하여 생성된 soup을 import해야 한다. 
>>communication파일에서 soup import 하기

```python
from .communication import get_soup_from_url
```
이제 원하는 동작을 실행할 함수들을 작성한다. 
궁극적으로 하고 하는 동작은 해당 웹툰의 모든 에피소드 리스트(title,link, created)를 긁어오는 것이다 .

1. 특정 페이지의 soup을 불러온다
2. 불러온 soup의 최신 에피소드 숫자를 불러온다 (몇 페이지 있는지 확인하기 위해)
3. 페이지 마다 리스트를 뽑는다
4. 모든 리스트를 뽑는다

####1. 특정 페이지 soup get
>웹툰의 id와 페이지 넘버를 받아 communication.py로 전달할 params를 생성
>
>여기서 url 또한 설정한다.
 
```python
def get_soup_from_naver_webtoon_by_page(webtoon_id, page=1):
    # 네이버웹툰 사이트 주소
    url = 'http://comic.naver.com/webtoon/list.nhn'
    # GET parameters로 전달할 값들의 dict
    params = {'titleId': webtoon_id, 'page': page}
    return get_soup_from_url(url, params)
``` 
####2.첫페이지의 최신화가 몇번인지 찾기
>우선 이전 function에서의 결과, 즉 default page인 첫 페이지의 soup을 받고
>
```python
def get_webtoon_recent_episode_number(webtoon_id):
	soup = get_soup_from_naver_webtoon_by_page(webtoon_id)
	recent_episode_number = 0
```
>
>그 soup의 table 속에서: 
>>iterate rows 
>>>`for tr in tr_list:`
>>
>>to find `<a>` tag and get its `href` 

>>>```python
td = tr.find('td', class_='title')
	if td:
		a = td.find('a')
		href = a.get('href')
```
>>
>>to get # of episode
>>>```python
import re
	# 정규표현식, 아무문자열이나 반복되다가 ?no=또는 &no=이후의 숫자와 매칭된다
	p = re.compile(r'.*[?&]no=(\d+).*')
	m = re.match(p, href)
```

>>stop when you get the first(recent) episode number
>>>```python
if m:
	recent_episode_number = m.group(1)
	break
```

>마지막으로: 정수형으로 형변환 후 리턴
>
>`return int(recent_episode_number)`

####3. 해당 페이지 리스트 뽑기 
>마찬가지로 soup을 불러오고  list를 담을 episode_list 생성해 둔다.
> 
>```python
def get_episode_list_from_page(webtoon_id, page=1):
    soup = get_soup_from_naver_webtoon_by_page(webtoon_id, page)
    # 리턴할 리스트
    episode_list = []
```
>soup 속에서...
>>iterate rows and its cells 
>>>
```python
tr_list = soup.find_all('tr')
    for index, tr in enumerate(tr_list):
        td_list = tr.find_all('td')
        # 각 row가 자식 td요소를 4개 미만으로 가지면 loop건너뜀
        if len(td_list) < 4:
            continue
```
>>find what you want: title, created date, rating etc...
>>>
```python
        td_thumbnail = td_list[0]
        td_title = td_list[1]
        td_rating = td_list[2]
        td_created = td_list[3]
        title = td_title.get_text(strip=True)
        created = td_created.get_text(strip=True)
        link = td_title.find('a').get('href')
```
>>cur_episode로 dictionary 작성하여 담고 리스트로 추가해서 return
>>>
```python        
        cur_episode = {
            'title': title,
            'link': link,
            'created': created,
        }
        episode_list.append(cur_episode)
    return episode_list
```

####4. 모든 페이지 수를 구하여 페이를 다 돌며 리스트 다 뽑아온다
>함수 생성 후 
>
>우선 2번 함수`get_webtoon_recent_episode_number`에서 구한 최신 에피소드 # (즉 총 episode의 #)를 사용해 몇개의 페이지가 있는지 구한다
>>페이지 마다 10개씩 에피소드가 있다는 것(`page_count =10`)을 고려한다
>
```python
def get_webtoon_episode_list(webtoon_id):
    page_count = 10
    total_episode_list = []
    total_episode_number = get_webtoon_recent_episode_number(webtoon_id)
    import math
    total_page_number = math.ceil(total_episode_number / page_count)
```
>> iterate pages and extend to list
>>> use function 3 `get_episode_list_from_page` to get the list of episodes from a single page
>>>
```python
    for i in range(total_page_number):
        cur_page_num = i + 1
        cur_episode_list = get_episode_list_from_page(webtoon_id, cur_page_num)
        total_episode_list.extend(cur_episode_list)
    return total_episode_list
```

#Module Package
>where: projects/Python/package/files.py

## Module
