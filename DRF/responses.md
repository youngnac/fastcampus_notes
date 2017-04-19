##  Responses

DRF는`Response` class를 제공함으로써  HTTP content negotiation을  지원한다. 

`Response` class는 client의 request에 따라 여러 종류의 content로render될 수 있는 content 반환을 가능하게 한다. 

Response를 반환하는 views에서는 항상 `APIView` class 또는 `@api_view`를 사용하도록 한다. 이는 view가 response에 대한 content negotiation과 적절한 renderer를 선택하도록 하기 때문이다. 



###  Creating responses

####  Response()

- `Response(data, status=None, template_name=None, headers=None, content_type=None)`

렌더된 내용으로 response object를 객체화하지 않는다. Data를 직렬화하여 Response object를 생성한다. 

#####  Arguments:

- `data`: response의 직렬화된 data
- `status`: response의 status code; Default = 200. ([status codes](http://www.django-rest-framework.org/api-guide/status-codes/).)
- `template_name`: `HTMLRenderer` 선택시 사용할 template 이름
- `headers`: response에 사용될 HTTP headers 의 딕션너리
- `content_type`: response의 content type  

###  Attributes

####  .data

Request object에 렌더되지 않은 content

####  .status_code

HTTP response의  status code

####  .content

Response의 render된 content

- `.render()`가 먼저 호출되어야 사용가능 하다. 

####  .template_name

`HTMLRenderer` 일 시 사용

####  .accepted_renderer

response를 생성할 때(render) 사용되는 renderer instance

####  .accepted_media_type

Content negotiation stage에서 선택되는 미디어 타입

####  .renderer_context

renderer의`.render()`에 전달되는 추가적인 정보 dictionary



