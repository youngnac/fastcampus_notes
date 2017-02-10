django document 
#Models:
- single, definitive source of information about your data
- contains fields and behaviors of the data
- each model maps to single DB table
- each model is a python class that subclasses `djdango.db.models.Model`
	* So, when you create class in models.py in application: `class ClassName(models.Model)`
- Django automatically generates DB with queries.

###To use models: add 'myapp' in `INSTALLED_APPS`
##Fields:
- specified by class attributes
- DO NOT choose field names like clean, save or delete

###Field Types:
Django uses the field class types to determine a few things:

- column type: what kind of data to store
- default HMLT widget to render a rodm field 
- minimal validation requirements

###Field Options:
1. `CharField(max_length= #)` specifies the size of VARCHAR in DB
2. `null=` : if True: store empty values as NULL (default : false)
3. `blank=`: if Ture: left blank (default : false)
4. `choices`: an iterable of 2 suples to use as choices for this field. By default: form widget will be a select box instead of standard text field

(요소가 tuple인 tuple)

```python
from django.db import models

class Person(models.Model):
    SHIRT_SIZES = (
        ('S', 'Small'),
        ('M', 'Medium'),
        ('L', 'Large'),
    )
    name = models.CharField(max_length=60)
    shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)
```
****
```python
>>> p = Person(name="Fred Flintstone", shirt_size="L")
>>> p.save()
>>> p.shirt_size
'L'
>>> p.get_shirt_size_display()
'Large'
```

5. default: value 또는 callable object
6. !!!!help_text: extra "help" text displayed with the form widget.  It’s useful for documentation even if your field isn’t used on a form.? (필드셋)
7. [others see](https://docs.djangoproject.com/en/1.10/ref/models/fields/#common-model-field-options)

###Automatic Primary Key Fields:
- by default, each model get auto-incrementing primary key:`id = models.AutoField(primary_key=True)
`
###flat=True

```python
In [6]: Person.objects.values_list('name')
Out[6]: <QuerySet [('youngna',), ('kent',), ('smith',)]>
#list에 바로
In [7]: Person.objects.values_list('name', flat=True)
Out[7]: <QuerySet ['youngna', 'kent', 'smith']>

```

####verbose_name은 왜 사용하나??
> in admin page, if you want to display more detailed names...

###Verbose Field names:
- except, m-m, 1-1,fk, other field type may take an optional first positional argument: verbose name
`first_name = models.CharField("person's first name", max_length=30)
`

-if not given, it automatically converts( _ -> space )

- however, m-m,1-1,fk cannot use verbose_name as first argument. **Their first argument must be a model class.**

#Relationship:
*###!!!models cascade*
##m-1 relationships
- must use: `django.db.models.ForeignKey`
- FoeignKey takes Class as its first positional argument: `manufacturer = models.ForeignKey(Manufacturer, on_delete=models.CASCADE) `
- most of times, (not required) use name of the model that you put it as the first positional argument of FK for Foriegnkey's name 

##m-m
[ref](https://docs.djangoproject.com/en/1.10/ref/models/fields/#manytomany-arguments)

- when contructing m-m, `ManyToManyField` must be put in one of the models. Usually, the object that will be edited on a form. (pizza - topping, Pizza has ManyToManyField)
- for complex M-M that has associate data in relationship btw two models:
	* use `through` to point to the model that acts as intermediary
- example: here Group and Person is M-M and Membership is an intermediary(intermediate model). Then, P-Mem:1-M, and G-Mem: 1-M. Since Group specifies `Through='Mempership'`, it is the source model.

```python
from django.db import models

class Person(models.Model):
    name = models.CharField(max_length=128)

    def __str__(self):              # __unicode__ on Python 2
        return self.name

class Group(models.Model):
    name = models.CharField(max_length=128)
    members = models.ManyToManyField(Person, through='Membership')

    def __str__(self):              # __unicode__ on Python 2
        return self.name

class Membership(models.Model):
    person = models.ForeignKey(Person, on_delete=models.CASCADE)
    group = models.ForeignKey(Group, on_delete=models.CASCADE)
    date_joined = models.DateField()
    invite_reason = models.CharField(max_length=64)
```

- in Membership model, if you have more than one foreign key and through field is not specified, you must put through fields so that django knows what to connect with. 
- when there is one more
- In this model, to create relationship,you cant simply use `add()`, `create()`,`set()`, `remove()`... You must use 'Membership' model.
- Instead, use `clear()`: remove all m-m relationship for an instance.

```python
>>> # Beatles have broken up
>>> beatles.members.clear()
>>> # Note that this deletes the intermediate model instances
>>> Membership.objects.all()
<QuerySet []>
```
###m-m (idol example)
- uses intermediate model 
- intermediary allows to store relationship-related data (ex: idol - group: date joined, recommender)

```python 

class Idol(models.Model):
    name = models.CharField(max_length=100)
    
class Group(models.Model):
    name = models.CharField(max_length=100)
    members = models.ManyToManyField(
        Idol,
        # notifies that R of (Idol, Group) is connected by Memberhsip
        through='MemberShip',
        # if one of classes uses more than two fields, it must specifiy through fields so that django know what to relate in DB 
        # since there is a recommender
        # we need to specify we are using group and idol
        through_fields=('group', 'idol'),
    )

class MemberShip(models.Model):
    group = models.ForeignKey(Group)
    idol = models.ForeignKey(Idol)
    # 추천인이라는 Target모델에 대한 추가 필드가 있을 경우
    # 이 경우에도 through_fields를 정의해줘야함
    date_joined = models.DateTimeField()
    recommender = models.ForeignKey(
        Idol,
        null=True,
        blank=True,
        related_name='recommender_membership_set'
    )
```
- from created memership object, to reach idol's name, use TWO undersocres

```python
In [38]: Membership.objects.values_list('idol__name', flat = True)
Out[38]: <QuerySet ['reina', 'liz', 'nana']>
In [42]: nana.group_set.all()
Out[42]: <QuerySet [<Group: ocara>]>
In [43]: nana.membership_set.values()
Out[43]: <QuerySet [{'group_id': 1, 'idol_id': 8, 'recommender_id': 7, 'id': 5, 'date_joined': datetime.datetime(2016, 10, 29, 6, 0, tzinfo=<UTC>)}]>
In [44]: nana.membership_set.values_list('group__name','date_joined')
Out[44]: <QuerySet [('ocara', datetime.datetime(2016, 10, 29, 6, 0, tzinfo=<UTC>))]>
In [40]: g.members.values_list('name', flat=True)
Out[40]: <QuerySet ['reina', 'liz', 'nana']>
In [108]: nana.recommender_membership_set.values()
Out[108]: <QuerySet []>
In [111]: after.membership_set.values_list("recommender")
Out[111]: <QuerySet [(None,), (None,), (None,), (None,), (None,), (None,), (None,)]>
In [117]: nana.membership_set.filter(date_joined__gt = "2015-2-2")
Out[117]: <QuerySet [<Membership: nana in ocara, When?: 2016-10-29 06:00:00+00:00>]>

```
##1-1
- use OneToOneField by including class attribute of your model
-  usefeul when an object "extends" another
-  (might want to use inhertance in case of an implicit 1-1 relationship)
- example: Place and Restaurant

```python 
from django.db import models

class Place(models.Model):
    name = models.CharField(max_length=50)
    address = models.CharField(max_length=80)

    def __str__(self):              # __unicode__ on Python 2
        return "%s the place" % self.name

class Restaurant(models.Model):
    place = models.OneToOneField(
        Place,
        on_delete=models.CASCADE,
        primary_key=True,
    )
    serves_hot_dogs = models.BooleanField(default=False)
    serves_pizza = models.BooleanField(default=False)

    def __str__(self):              # __unicode__ on Python 2
        return "%s the restaurant" % self.place.name

class Waiter(models.Model):
    restaurant = models.ForeignKey(Restaurant, on_delete=models.CASCADE)
    name = models.CharField(max_length=50)

    def __str__(self):              # __unicode__ on Python 2
        return "%s the waiter at %s" % (self.name, self.restaurant)
```
###1-1 recursive:
- instructor, instance of Person, can be created and other Person's instance can have "instructor" even though they are all instances of Person. 

```python
class Person(models.Model):
        # to make recusive 1-1 relationship: connect itself with Fk as 'self'
    instructor = models.ForeignKey(
 		  'self',
        verbose_name='담당 강사',
        #Person's instances do not need instructor. 
        #Thus, it is left Null=T and blank =t
        null=True,
        blank=True,
        on_delete=models.SET_NULL,
        related_name='student_set',
    )
```
###Field name restriction
- DO NOT USE TWO UNDER SCORES (not allowed, one of the  query lookup sytnaxes)
- CANNOT be an Python RESERVED word (pass,,,)

##Model Methods
- custom methods: add custom "row-level" functionality to you objects
- manager methods:  intended to do 'table-wide; things (every model class has object(manager, you can rename it using: `new_name = models.Manager()`
	* example: Person.people.all() provde a list of all person objects
	
	```python
	from django.db import models
	class Person(models.Model):
    	#...
    	people = models.Manager()
	```

- **Model Methods** acts on a particular model instance
- example: few custom methods. Here, using 'property' method to implement attributes using method calls [more details](https://docs.python.org/3/library/functions.html#property)

```python
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    birth_date = models.DateField()

    def baby_boomer_status(self):
        "Returns the person's baby-boomer status."
        import datetime
        if self.birth_date < datetime.date(1945, 8, 1):
            return "Pre-boomer"
        elif self.birth_date < datetime.date(1965, 1, 1):
            return "Baby boomer"
        else:
            return "Post-boomer"

    def _get_full_name(self):
        "Returns the person's full name."
        return '%s %s' % (self.first_name, self.last_name)
    full_name = property(_get_full_name)
```

- \_\_str()__ : returnds a unicode representation of any object. displayed as a plain string.
- get\_absolute_url(): figure out a URL of an object

###Overriding predefiend model methods:
- override save(), delete() by calling "superclass method"

```python
from django.db import models

class Blog(models.Model):
    name = models.CharField(max_length=100)
    tagline = models.TextField()

    def save(self, *args, **kwargs):
        if self.name == "Yoko Ono's blog":
            return # Yoko shall never have her own blog!
        else:
            super().save(*args, **kwargs) # Call the "real" save() method.
```
##Model Inheritance
- similar with python
- base class must subclass djang.db.models.Model
- before creating Model: decide
	1. if you want your parent models to be models in their own right
	2. or if you want to make your parent model to be a holder for common information and only to be visible through child models
##3 Possible Inheritances:
-
1. Abstract base classes: parent class just holds information so that you do  not type out same information for each child model
2. Multi-table inheritance: subclassing an existing model and want each model to have its own DB table
3. Proxy models: want to just modify the python-level behavior (NOT changing the models fields) 

###Abstract Base Classes:
-

- common information holder
- put **`abstract=True`** in Meta class > it wont create DB table
- but still adds fields to child class
- child and this parent class cannot have name field names

####Meta InheritanceQQQQ???
- when abstact base class is created: any Meta inner class in base class is available as an attribute
- child also automaticallyinherits parent's meta and can extends on it as its Meta class by subclassing parents meta
* children of abstract class do not become abstract class. 
* MUST USE **`abstract=True`** if you want to make children class as asbtract class

##related\_name / related\_query_name: (M-M and FK related...)
- since fields in base class are inherited by child classes, base's related_name and related_query_name conflicts with those of childs. Thus, it is IMPORTANT TO  add labels `%(app_label)s` and `%(class)s`
- `&(class)s` : replaced by the lower-cased name of the child class that the field is used in
- `%(app_label)s` : replaced by the lower-cased name of the app the child class is contained within
- example
>app:common/models.py

```python
from django.db import models

class Base(models.Model):
    m2m = models.ManyToManyField(
        OtherModel,
        related_name="%(app_label)s_%(class)s_related",
        related_query_name="%(app_label)s_%(class)s",
    )
	class Meta:
        abstract = True
        
class ChildA(Base):
    pass    
```

* in this case: ChildA's reverse name is `commons_child_related` and related_qurey_name is `commons_childas`
- you can customize (app_labe) and (class) parts
###Multi-talbe Inheritace:
-
- each models in hierarchy is a model itself (own DB)
- own DB and be queried out and created individually
- Inheritance relatinoship: links btw child and its parents (auto 1-1 field)

###META and Multi-table Inheritance:
- since child of multi-table inheritance inherits all behaviors of each parent: no need to inherit parent's meta class (if does: contradictory) > thus **NO Access** to base meta
- HOWEVER: sometimes you want to specify `ordering` or `get_latest_by` attribute since child inherits them by default
- example: Parent model has `class Meta` with `ordering = ['horn length']` and you dont want your child to have it

```python 
class ChildModel(ParentModel):
    # ...
    class Meta:
        # empty out ordering attr of child
        ordering = []
```

####Inheritance and Reverse Relations (m-m field and fk field)
- again, when putting  m-m and fk field in subclasses of the parent model, MUST SPECIFY `related_name`

####Specifying the parent link field (????)

- Django automatically creates a `OneToOneField` linking your child class back to any non-abstract parent models.
-  want to control the name of the attribute linking back to the parent, you can create your own `OneToOneField` and set `parent_link=True` to indicate that your field is the link back to the parent class.

##Proxy Models:
- use Proxy Model by declaring `proxy = True` in Meta class of Child class
- To change the python behavior of a model, such as..
	1. to change default manager
	2. add a new method
- allows you to create, delete and update instances of the proxy model and all the data will be saved as if you were using the original 
- you can change things like default model ordering or default manager in proxy w/o having to alter original
- ex) normal Person queries will be unordered and OrderedPerson queries will be ordered by last_name.

	
```python
class OrderedPerson(Person):
    class Meta:
        ordering = ["last_name"]
        proxy = True
```
####Base Class Restrictions
- Proxy model **must inherit from One NON-abstract** model class (NOT ALLOWED from Multiple non-abs)
- Possible to i**nherit from multiple Abstract** model classes and multiple Proxy models that share a common Non-abstract parent class

####Proxy Model Managers
- usually if no specified managers for proxy model, by default, proxy model inherits managers form its parent
- Defining a manager on the proxy model allows to have default manager and can still access to those of parents
- example: Proxy model class (MyPerson) of parent model class (Person) is defined its new manager : `objects = NewManager()`

```python 
class MyPerson(Person):
    objects = NewManager()

    class Meta:
        proxy = True
```

###Difference btw Proxy Inheritance and Unmanaged Models
- Unmanaged model: shadows an existing model and may add python methods. HOWEVER: very repetitive and fragile to keep both copies (Meta.db_table) in sync
	* useful for modeling db views and tables not under control of Django
	* `Meta.managed=False`
- Proxy model: behave exactily like the model that is proxied. SO, they are ALWAYS in sync with parent
	* useful when keeping all the same fields as its original
	* `Meta.proxy=True`

##Multiple Inheritance
- class model can inherit from multiple parent models
- when inheriting from multiple parents, Meta of the frst parenet is used (others are ignored)
- When making inheritance from multiple parents:
	* use explicit AutoField 

```python
class Article(models.Model):
    article_id = models.AutoField(primary_key=True)
    ...
class Book(models.Model):
    book_id = models.AutoField(primary_key=True)
    ...
class BookReview(Book, Article):
    pass
```

	* or use common ancestor to hold the AutoField

```python 
class Piece(models.Model):
	 ...
class Article(Piece):
    ...
class Book(Piece):
    ...
class BookReview(Book, Article):
	 ...
```

##Field name when Inheritance
- No Overriding is allowed when inheriting Non Abstrat model base class
- Overriding is allowed when inheriting from Abstract
	* use: `field_name = None` to remove

##Organizing models in a package
- when there are many models, organize them in separate files within a `models` package (must create \_\_init__.pu to make a directory a package
- then in \_\_init__.py import models
- `myapp/models/__init__.py`:

```python 
from .organic import Person
from .synthetic import Robot
```
