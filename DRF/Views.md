#  Views

##  Class-based Views

##  APIView 

[참고](http://www.django-rest-framework.org/api-guide/views/)

`View` class의 하위 class 인 `APIView`는 일반 `View` classes 와 조금 다르다

- 처리방법(handler method)는 장고의 HttpResponse 대신  REST framework의 `Response`를 반환한다
- Handler method에 처리 요청이 되기 전 Incoming request는 적절한 permission check와 인증을 거치게 된다. 
- APIException예외들이 적절하게 중재된다

```Python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import authentication, permissions

class ListUsers(APIView):
    #  적절하게 permission check와 인증
    authentication_classes = (authentication.TokenAuthentication,)
    permission_classes = (permissions.IsAdminUser,)
	#  Handler method 
    def get(self, request, format=None):
        """
        Return a list of all users.
        """
        usernames = [user.username for user in User.objects.all()]
        return Response(usernames)
```

####  Dispatch method

view의 `.dispatch()` method가 직접적으로 호출하는 methods...

#####  .initial

permissions, throttling, content negotiation 실행

#####  .handle_exception

Handler method에서 일어나는 예외들을 처리

##  Function Based Views

- 일반 function based views지만 간단한 decorator를 사용하여`Request` instance를 받도록 보장한다

####  @api_view()

- default로 `GET`만 허용된다. 따라서 다른 method들을 지정하여 사용한다

  ```Python
  @api_view(['GET', 'POST'])
  def hello_world(request):
      if request.method == 'POST':
          return Response({"message": "Got some data!", "data": request.data})
      return Response({"message": "Hello, world!"})
  ```

####  API policy decorators

- `@throttle_classes` : 특정 user가 하루에 한번 호출할 수 있도록 한다.

  ```Python
  class OncePerDayUserThrottle(UserRateThrottle):
          rate = '1/day'

  @api_view(['GET'])
  @throttle_classes([OncePerDayUserThrottle])
  def view(request):
      return Response({"message": "Hello for today! See you tomorrow!"})
  ```

- `@renderer_classes(...)`

- `@parser_classes(...)`

- `@authentication_classes(...)`

- `@throttle_classes(...)`

- `@permission_classes(...)`