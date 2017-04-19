[RESTfulAPI](http://blog.remotty.com/blog/2014/01/28/lets-study-rest/)

과거에는 정적인 파일이 전부여서 URL(자원의 위치, locator)만 썼다. URL **상위개념으로 URI**(자원의 주소, identifier)가 있는데,  현재는 URI를 많이 씀?

###   CRUD

개념 : create, read, update, delete

HTTP method : post, put, fetch, delete



###   [REST API(Representational State Transfer API)](http://hyunalee.tistory.com/1)

크게 **리소스, 메서드, 메세지**로 이루어져 있다. 예로 "이름이 Terry인 사용자를 생성한다."라는 호출이 있을 때,

- 리소스 : "사용자", http://myweb/users/ 라는 형태의 URI
- 메서드 : "생성한다"라는 행위, HTTP POST 메서드
- 메세지 : "이름이 Terry인 사용자", 생성하고자 하는 사용자의 디테일한 내용을 JSON문서로 표현

```json
HTTP POST, http://myweb/users/
{
  "users": {
    "name": "Terry"
  }
}
```



###   [REST 아키텍처에 적용되는 6가지 제한 조건(RESTful)](https://ko.wikipedia.org/wiki/REST)

다음 제한 조건을 준수하는 한 개별 컴포넌트는 자유롭게 구현할 수 있다.

- [클라이언트/서버 구조](https://en.wikipedia.org/wiki/Client%E2%80%93server): 일관적인 인터페이스로 분리되어야 한다
- [무상태(Stateless)](http://en.wikipedia.org/wiki/Stateless_server): 각 요청 간 클라이언트의 콘텍스트가 서버에 저장되어서는 안 된다
- [캐시 처리 가능(Cacheable)](https://ko.wikipedia.org/wiki/%EC%9B%B9_%EC%BA%90%EC%8B%9C): WWW에서와 같이 클라이언트는 응답을 캐싱할 수 있어야 한다.잘 관리되는 캐싱은 클라이언트-서버 간 상호작용을 부분적으로 또는 완전하게 제거하여 scalability와 성능을 향상시킨다.
- [계층화(Layered System)](http://en.wikipedia.org/wiki/Layered_system): 클라이언트는 보통 대상 서버에 직접 연결되었는지, 또는 중간 서버를 통해 연결되었는지를 알 수 없다. 중간 서버는 [로드 밸런싱](https://ko.wikipedia.org/wiki/%EB%A1%9C%EB%93%9C_%EB%B0%B8%EB%9F%B0%EC%8B%B1) 기능이나 [공유 캐시](https://ko.wikipedia.org/w/index.php?title=%EA%B3%B5%EC%9C%A0_%EC%BA%90%EC%8B%9C&action=edit&redlink=1) 기능을 제공함으로써 시스템 규모 확장성을 향상시키는 데 유용하다.
- [Code on demand (optional)](http://en.wikipedia.org/wiki/Client-side_scripting) - 자바 애플릿이나 자바스크립트의 제공을 통해 서버가 클라이언트가 실행시킬 수 있는 로직을 전송하여 기능을 확장시킬 수 있다.
- 인터페이스 일관성: 아키텍처를 단순화시키고 작은 단위로 분리(decouple)함으로써 클라이언트-서버의 각 파트가 독립적으로 개선될 수 있도록 해준다..

프로젝트 세팅

```shell
(insta_api_env) ➜  instagram_api git:(master) ✗ tree . -a
.
├── .conf
│   └── settings_deploy.json
├── .conf-secret
│   ├── settings_common.json
│   └── settings_local.json
├── .git
│   ├── HEAD
│   ├── config
│   ├── description
│   ├── hooks
│   │   ├── applypatch-msg.sample
│   │   ├── commit-msg.sample
│   │   ├── post-update.sample
│   │   ├── pre-applypatch.sample
│   │   ├── pre-commit.sample
│   │   ├── pre-push.sample
│   │   ├── pre-rebase.sample
│   │   ├── pre-receive.sample
│   │   ├── prepare-commit-msg.sample
│   │   └── update.sample
│   ├── info
│   │   └── exclude
│   ├── objects
│   │   ├── info
│   │   └── pack
│   └── refs
│       ├── heads
│       └── tags
├── .gitignore
├── .idea
│   ├── instagram_api.iml
│   ├── misc.xml
│   ├── modules.xml
│   └── workspace.xml
├── .python-version
├── django_app
│   ├── config
│   │   ├── __init__.py
│   │   ├── __pycache__
│   │   │   ├── __init__.cpython-35.pyc
│   │   │   ├── settings.cpython-35.pyc
│   │   │   ├── urls.cpython-35.pyc
│   │   │   └── wsgi.cpython-35.pyc
│   │   ├── settings.py
│   │   ├── urls.py
│   │   └── wsgi.py
│   ├── db.sqlite3
│   └── manage.py
└── requirements.txt
```

###   Model:

- member

  ```Python
  class MyUser(AbstractUser):
  	pass
  ```

- Post

  ```
  class Post(models.Model):
  	author = models.Foreignken(settings.AUTH_USER_MODEL)
      created_data = models.DateTimeField(auto_now_add=True)
      
  class PostPhoto(models.Model):
      post = models.Foreignkey(Post)
      photo - models.ImageField(up_load_to='post')
      
      class Meta:
          order_with_respect_to='post'
  ```

  ​



> PostPhoto 모델에 사진을 정렬하기 위해 [order_with_respect_to](https://docs.djangoproject.com/en/1.10/ref/models/options/#  order-with-respect-to) 사용 ( [model meta option](https://docs.djangoproject.com/en/1.10/ref/models/options/))

- settings_local.json & settings.py

  - DB : postgre 사용하도록 설정하자 (engine, name,user, pw, host, port)

  ​

####   스크립트 다시 짜기

```shell
#  !/bin/bash

while getopts "d" #   opt d 란 옵션 하나만 받는다 
do
        case $opt in
            d) mode='DEBUG' ;; #   옵션에 모드=디버그 값을 할당
        esac
done
					할당한 값을 사용 
manageCommand="MODE='$mode' ./manage.py ${@:OPTIND}" #   OPTIND는 옵션 인덱스. 
echo $manageCommand 저 변수 출력 
eval $manageCommand 커맨드라인에서 실행 
```

- `chmod 755 ~/.scripts/manage` 로 권한 변경

- 실행 : `manage -d runserver`

  ```shell
  (insta_api_env) ➜  django_app git:(master) manage -d runserver
  MODE='DEBUG' ./manage.py runserver
  Performing system checks...

  System check identified no issues (0 silenced).
  March 13, 2017 - 15:36:41
  Django version 1.10.6, using settings 'config.settings'
  Starting development server at http://127.0.0.1:8000/
  Quit the server with CONTROL-C.
  ```

#####   admin.site.register() 로 post,postphoto 등록하고 admin 접속 (superuser생성해서 로그인 해보자)

###   View - post-create

- Post_create view 작성( url과도 연결 /post/post- create/ )

  ```Python
  User = get_user_model()
  def post_create(request):
  	if request.method == 'POST':
  		try:
  			author_id = request.POST['author_id']
  			author = User.objects.get(id=author_id)
  		except KeyError:
              return HttpResponse('author id is required')
          except User.DoesNotExist:
              return HttpResponse(
                  'author id {} does not exist'.format(request.POST['author_id']))
          post = Post.objects.create(author=author)
          return HttpResponse('{}'.format(post.pk))
      
  ```

####   postman

- method: **post** >  url : 127.0.0.1:8000**/post/post-create/** > 요청을 보내보자 **SEND**

#####   Error! 403 Forbidden 

csrf가 없기 때문에 발생 > 해결 : `@csrf_exempt`를 view에 적용

####   @csrf_exempt

API형태로 사용할때는 CSRF토큰 보내기 어렵다. 따라서   `@csrf_exempt`로 해제한다. 

(뷰에서) API는 애초에 앱 안쪽에서 돌아간다고 간주하고  csrf 는 신경쓰지 않고 진행

```Python
@csrf_exempt
User = get_user_model()
def post_create(request):
	if request.method == 'POST':
```

####   postman

- method: **post** **>**  url : 127.0.0.1:8000**/post/post-create/** 
-  body : {author_id : 1}  **>** 요청을 보내보자 **SEND**

###   View - post_photo_add

- Post_create view 작성( url과도 연결 /post/photo/add/ )

  ```Python
  @csrf_exempt
  def post_photo_add(request):
  	if request.method == 'POST':
  		try:
  			post_id = request.POST['post_id']
  			photo = request.FILEs['photo']
              post = Post.objects.get(id=post_id  )
  		except KeyError:
              return HttpResponse('post_id and photo are required fields')
          except Post.DoesNotExist:
              return HttpResponse(
                  'post id {} does not exist'.format(request.POST['post_id']))
          PostPhoto.objects.create(post=post, photo=photo) 
          return HttpResponse('post: {}, photo list: {}'.format(
              post_id, [photo.id for photo in post.postphoto_set.all()])
      
  ```

####   postman

- method: **post** **>**  url : 127.0.0.1:8000**/post/photo/add/** 
-  body : {author_id : 1}  **>** 요청을 보내보자 **SEND**



> 간략하게 view 를 작성해, postman으로 api의 동작을 확인 하였다. 

#####   media_root (join with BASE_DIR), url ('/media/') 설정

###   View - post_list return json

- Post_list 를  [JsonResponse](https://docs.djangoproject.com/en/1.10/ref/request-response/#  jsonresponse-objects) 로 return 하는 view 작성

  ```Python
  def post_list(request):
      context = {
        'post_list':Post.objects.all()
      }
      return JsonReponse(data=context)
  ```

####   postman

- method: **get** **>**  url : 127.0.0.1:8000**/post/**  **>** 요청을 보내보자 **SEND**

####   error: \<QuerySet ….> is not Json Serializable 

queryset은 자동으로 json 형태로 바뀌지 않는다. 

#####   View - post_list return json 수정하자

- Post_list 를  [JsonResponse](https://docs.djangoproject.com/en/1.10/ref/request-response/#  jsonresponse-objects) 로 return 하는 view 작성

  ```Python
  def post_list(request):
      post_dict_list = []
      for post in Post.objects.select_related('author'):
          cur_post_dict = {
              'pk': post.pk,
              'created_date':post.created_date,
              'photo_list': [],
              'author': {
                  'pk': post.author.pk,
                  'username': post.author.username,

              }
          }

          cur_post_photo_list = post.postphoto_set.all()
          cur_post_photo_dict_list = []
          for post_photo in cur_post_photo_list:
              cur_post_photo_dict = {
                  'pk': post_photo.pk,
                  'photo': post_photo.photo.url
              }
              cur_post_dict['photo_list'].append(cur_post_photo_dict)
              post_dict_list.append(cur_post_dict)
      context = {
          'post-list': post_dict_list,
      }
      context = {
        'post_list':Post.objects.all()
      }
      return JsonReponse(data=context)
  ```

####   

하지만 이렇게 dict로 만들어 주는 것은 post_create_list 를 생성하고 또 다른 list 반환에 있어 매우 비효율적이다

따라서, **model에 dict로 전환해주는 함수를 작성하자**

###   Model:

- member

  ```Python
  class MyUser(AbstractUser):
     pass

      def to_dict(self):
          ret = {
              'pk': self.pk,
              'name': self.username,
              'img_profile': self.img_profile.url,
          }
          return ret

  ```

- Post

  ```Python
  class Post(models.Model):
  	...
      def to_dict(self):
          ret = {
              'pk': self.pk,
              'created_date': self.created_date,
              'author': self.author.to_dict(),
              'photo_list': [photo.to_dict() for photo in self.postphoto_set.all()]
          }
          return ret


  class PostPhoto(models.Model):
      ...
      def to_dict(self):
          ret = {
              'pk': self.pk,
              'photo': self.photo.url,

          }
          return ret

      class Meta:
          order_with_respect_to = 'post'
  ```

###   View:

```python
def post_list(request):
    context = {
        'post_list': [post.to_dict() for post in Post.objects.select_related()]
    }
    return JsonResponse(context)
```



직렬화, 역직렬화, 시리얼라이즈 

