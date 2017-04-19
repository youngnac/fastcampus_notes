#   Serializers

querysets 또는 model instances 와 같은 복잡한 data를 Json, xml 등으로 쉽게 전환가능 하게 하는 native python 으로 변환하여 준다.

반대로 deserialization 도 제공한다. 

###   Declaring Serializers

- 우선 모델을 하나 만들자

```python
from datetime import datetime

class Comment(object):
    def __init__(self, email, content, created=None):
        self.email = email
        self.content = content
        self.created = created or datetime.now()


#   commnet 객체 하나 생성
comment = Comment(email='leila@example.com', content='foo bar')
```

- Serializer 선언

```python
from rest_framework import serializers

class CommentSerializer(serializers.Serializer):
    email = serializers.EmailField()
    content = serializers.CharField(max_length=200)
    created = serializers.DateTimeField()
```

#####   Serializing 해보자

- 생성해둔 comment instance 사용

1. Model instance > python native datatypes

```Python
serializer = CommentSerializer(comment)
serializer.data
#   {'email': 'leila@example.com', 'content': 'foo bar', 'created': '2016-01-27T15:17:10.375877'}
```

2. Python native datatypes > json 

```Python
from rest_framework.renderers import JSONRenderer

json = JSONRenderer().render(serializer.data)
json
#   b'{"email":"leila@example.com","content":"foo bar","created":"2016-01-27T15:17:10.375877"}'
```

#####   Deserializing 하자

1. Stream > python native datatypes

   ```Python
   from django.utils.six import BytesIO
   from rest_framework.parsers import JSONParser

   stream = BytesIO(json)
   data = JSONParser().parse(stream)
   ```

2. python native datatypes > dictionary of validated data

   ```Python
   serializer = CommentSerializer(data=data)
   serializer.is_valid()
   #   True
   serializer.validated_data
   #   {'content': 'foo bar', 'email': 'leila@example.com', 'created': datetime.datetime(2012, 08, 22, 16, 20, 09, 822243)}
   ```

####   Saving instances

Validated data로 instance를 반환하기 위해서는 `create()` 또는 `update()` 를 serlializer에 구현해야 한다 .

```Python
class CommentSerializer(serializers.Serializer):
    ...
    
    def create(self, validated_data):
        return Comment(**validated_data)

    def update(self, instance, validated_data):
        instance.email = validated_data.get('email', instance.email)
        instance.content = validated_data.get('content', instance.content)
        instance.created = validated_data.get('created', instance.created)
        return instance
```

Django model의 instance라면  **model**에도 동일하게.

```Python
def create(self, validated_data):
        return Comment.objects.create(**validated_data)

    def update(self, instance, validated_data):
        instance.email = validated_data.get('email', instance.email)
        instance.content = validated_data.get('content', instance.content)
        instance.created = validated_data.get('created', instance.created)
        instance.save()
        return instance
```

- deserializing을 할 때 `.save`로 validated **>**  instance 로 반환한다. 

  ```
  comment = serializer.save()
  ```

#####   Update or Create?

- data만 serlializer에 넣어주면 새 instance 생성
- instance와 함께 기존의 instance를 serlialzer에 넣어주면 update

```Python
#   .save() will create a new instance.
serializer = CommentSerializer(data=data)

#   .save() will update the existing `comment` instance.
serializer = CommentSerializer(comment, data=data)
```

#####   추가로 속성이 전달 될때

- Request data 이외의 다른 data 는 kwargs로 전달

  ```Python
  serializer.save(owner=request.user)
  ```

추가한 kwargs는 `.create()` 또는 `.update()` 호출시 validated_data에 포함된다 

#####   .save() Override

`.create()`또는 `.save()`  함수가 의미가 없을 시 override 하자

- Ex: email 객체 send하는 것으로 override

  ```python
  class ContactForm(serializers.Serializer):
      email = serializers.EmailField()
      message = serializers.CharField()

      def save(self):
          email = self.validated_data['email']
          message = self.validated_data['message']
          send_email(from=email, message=message)
  ```

###   Validation

Deserializing 할 때 validated data에 대한 항상 `is_valid()` 가 호출된다. 

- `serializer.is_valid()`>False 반환시 , `serlializer.errors`에서 에러 메세지를 확인 할 수 있다. 

#####   Field-level validation 

- serlializer class 에 `def validated_<field name>` 함수로field에 대한 검증을 custom할 수 있다. 

  >  `.clean_<field name>`과 유사하다

#####   Object-level validation

- 모든 field values를 하나의 arg 받아 검증

####   Partial updates(=patch)

객체, data, partial=True를 넣어주자

```Python
serializer = CommentSerializer(comment, data={'content': u'foo bar'}, partial=True)
```

###   Nested Objects

```Python
class UserSerializer(serializers.Serializer):
    email = serializers.EmailField()
    username = serializers.CharField(max_length=100)

class CommentSerializer(serializers.Serializer):
    user = UserSerializer()
    content = serializers.CharField(max_length=200)
    created = serializers.DateTimeField()
```

- field에 none value로 nested 허용 : `(required=False)`
- Field 하나에 list of items 로 nested 가능: `many=True`

##   ModelSerializer

model에 fields 대한 serlializer 가 자동으로 생성된다

- 모델에 근거하여 자동으로 set of fields 생성
- 자동으로 validator 생성 (ex: unique_together)
- 간단한 default method 를 포함한다 (ex: create, update)

```python
class AccountSerializer(serializers.ModelSerializer):
    class Meta:
        model = Account
        fields = ('id', 'account_name', 'users', 'created')
```

####   필드 정의

#####   특정 필드만 포함하기

```Python
class PostPhotoSerializer(serializers.ModelSerializer):
    class Meta:
        model = PostPhoto
        fields = ('post', 'photo',)
```

#####   모든필드 포함 가능

```Python
class AccountSerializer(serializers.ModelSerializer):
    class Meta:
        model = Account
        fields = '__all__'
```

