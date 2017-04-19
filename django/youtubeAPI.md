#### 1. Create a project: [google API manager](https://console.developers.google.com/apis/dashboard?project=useful-flame-159403&duration=PT1H)
#### 2. pip install --upgrade google-api-python-client
#### 3. Get a API_KEY 

#### 4. mkdir .conf/settings\_local.json > API_KEY 	보관
.conf git에 올라가지 않도록 관리 필!! **중요**
![](/Users/yn_c/Desktop/sc/c.png)

#### 5. [request 사용](http://docs.python-requests.org/en/master/) : pip install request  

#### 6. https://www.googleapis.com/youtube/v3/search) 이 주소로 postman 사용하여 리턴해주는 값을 확인 
parameters: q=키워드, part=snippet,...등; 

* [참고](https://developers.google.com/youtube/v3/docs/search/list)

#### 7. 읽어 오는 code

```python
def search_youtube(keyword, num):
    current_pwd = os.path.abspath(__file__)
    conf = os.path.join(os.path.dirname(os.path.dirname(os.path.dirname(current_pwd))), '.conf')
    key_ = json.loads(open(os.path.join(conf, 'settings_local.json')).read())
    part = "snippet"
    keyword = keyword
    r_key = key_['youtube']['API_KEY']
    num = num
    my_param = {
        'part': part,
        'q': keyword,
        'key': r_key,
        'maxResults': num,
        'type': 'video',
    }
    youtube_url = "https://www.googleapis.com/youtube/v3/search"
    r = requests.get(youtube_url, params=my_param)
    r_text = json.loads(r.text)
```

   