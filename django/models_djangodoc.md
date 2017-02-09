django document 
#Models:
- single, definitive source of infomration about your data
- contains fields and behaviours of the data
- each model maps to single DB table
- each model is a python class that subclasses
 `djdango.db.models.Model`
> So, when you create class in models.py in application:
 `class ClassName(models.Model)`
- Django automatically generates DB with queries.

###To use models: add 'myapp' in `INSTALLED_APPS`
##Fields:
- specified by class attributes
- DO NOT choose field names like clean, save or delete

###Field Types:
> Django uses the field class types to determine a few things:
>- column type: what kind of data to store
>- default HMLT widget to render a rodm field 
>- minimal validation requirements
###Field Options:
1. `CharField(max_length= #)` specifies the size of VARCHAR in DB
2. `null=` : if True: store empty values as NULL (default : false)
3. `blank=`: if Ture: left blank (default : false)
4. `choices`: an iterable of 2 suples to use as choices for this field. By default: form widget will be a select box instead of standard text field

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
##!!!verbose_name은 왜 사용하나??
> m-m,fk, 1-1일때 연결할때 이름 사용?

###Verbose Field names:
- except, m-m, 1-1,fk, other field type may take an optional first positional argument: verbose name
`first_name = models.CharField("person's first name", max_length=30)
`

-if not given, it automatically converts( _ -> space )

- however, m-m,1-1,fk cannot use verbose_name as first argument. **Their first argument must be a model class.**

##Relationship:
*###!!!models cascade*
###m-1 relationships
- must use: `django.db.models.ForeignKey`
- FoeignKey takes Class as its first positional argument: `manufacturer = models.ForeignKey(Manufacturer, on_delete=models.CASCADE) `
- most of times, (not required) use name of the model that you put it as the first positional argument of FK for Foriegnkey's name 

###m-m
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

- In this model, to create relationship,you cant simply use `add()`, `create()`,`set()`, `remove()`... You must use 'Membership' model.
- Instead, use `clear()`: remove all m-m relationship for an instance.

```python
>>> # Beatles have broken up
>>> beatles.members.clear()
>>> # Note that this deletes the intermediate model instances
>>> Membership.objects.all()
<QuerySet []>
```

###1-1
- use OneToOneField by including class attribute of your model
-  usefeul when an object "exxtends" another
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
- example: few csustom methods. Here, using 'property' method to implement attributes using method calls [more details](https://docs.python.org/3/library/functions.html#property)

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
- get_absolute_url(): figure out a URL of an object

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
            super(Blog, self).save(*args, **kwargs) # Call the "real" save() method.
```





