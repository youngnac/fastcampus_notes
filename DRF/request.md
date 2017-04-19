#  Requests

REST framework 의 `Request` class 는 기본` HttpReqeust `의 연장선으로, 좀 더 유연한 request parsing 과 authentication을 지원한다. 



###  Request Parsing

REST framework의 Request objects는 JSON 이나 다른 미디어 종류의 request도 form data 처럼 처리할 수 있도록 한다. 

####  .data

`request.data`는 request body의 파싱된 내용을 반환한다.

아래의 항목들을 제외하고는 `request.POST` 또는 `request.FILEES`와 유사하다. 

1. File 과 non-file inputs 을 포함한 모든 파싱된 내용을 포함한다. 
2. `POST` 이외의 다른 HTTP methods ( `put`, `patch` 과 같은)의 내용들의 파싱을 지원한다. 

####  .query_params

`request.query_params` 는 `request.GET`과 같다

- 더 명확하게 하기 위해서는 `request.query_params`를 권장한다. 

####  .parsers

`APIView` class 또는 `@api_view` 데코레이터는 `Parser`객체로 자동인식하도록 한다. 

###  Content negotiation

- content의 type과 같은 것을 결정하는 과정

request는 content negotiation stage의 결과는 결정하도록 하는 속성들을 expose 한다

다른 serialization scheme을 선택하도록 할 수 있다. 

####  .accepted_renderer

Content negotiation stage에서 선택된 renderer instance

####  .accepted_media_type

Content negotiation stage에서 하용된 media type을 나타내는 string

###  Authentication

DRF는 하나의 request 에 대한 인증을 제공한다. 이는 :

- 다른 API에 다양한 auth policies 를 사용할 수 있게 한다
- 여러개의 auth policies 사용을 지원한다
- Incoming request에 해당하는 user와 토큰 정보 모두를 제공한다

####  .user

`request.user`는 대게 `django.contrib.auth.models.User`의 객체를 반환한다. 

- auth되지 않았을 때: `django.contrib.auth.models.AnonymousUser`를 반환한다

####  .auth

`request.auth`는 추가적인 auth context를 반환하며, 이는 auth policies에 따라 정확한 반환값은 결정된다. auth되지 않거나 추가적인 auth context가 없을시에는 `none`을 반환한다

####  .authenticators

`authenication_classes`에 따라서`APIView`class 또는 `@api_view` 데코레이터는 `Authentication`객체들에 자동으로 속성이 적용되도록 한다.



###  Browser enhancements

DRF는 put, patch, delete와 같은 browser enhancements 을 지원한다. 

####  .methods

`.request.method`HTTP method를 대문자 문자열로 반환한다

####  .content_type

`.request.content_type`은 HTTP request의 미디어 타잎을 문자로 반환한다. 

####  .stream

`.request.stream` request body의 내용을 stream으로 반환한다





 