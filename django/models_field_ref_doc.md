- [Field types](https://docs.djangoproject.com/en/1.10/ref/models/fields/) 

##Field options
###Null
-
- True : **store empty** as NULL in db (default: False)
- **Do NOT** use `NULL` **on strings** (storing NULL and empty string together)
- SET `blank=True` for strings and non-string-based fields if you want to allow empty values in froms
- for boolean: use `NullBooleanField`
- check if you want to store

###blank
-
- True: **allowed to be blank ('')** (d: False)
	* for **both numbers/strings**, any input section can be left blank
- null > db-related vs. blank > validation-related
- True: form validation allows empty values
- False: requires field

###choices
-
- iteralbe (list/tuple) consisting itself of iterables of exactly TWO items (e.g. [(A, B), (A, B) ...])
- if given: by default, widget is a select box
- best to define choices inside a model class,
- (A,B) 
	* A: actual value to be set on the model
	* B: human readable anme

```python 
from django.db import models

class Student(models.Model):
    FRESHMAN = 'FR'
    SOPHOMORE = 'SO'
    JUNIOR = 'JR'
    SENIOR = 'SR'
    YEAR_IN_SCHOOL_CHOICES = (
        (FRESHMAN, 'Freshman'),
        (SOPHOMORE, 'Sophomore'),
        (JUNIOR, 'Junior'),
        (SENIOR, 'Senior'),
    )
    year_in_school = models.CharField(
        max_length=2,
        choices=YEAR_IN_SCHOOL_CHOICES,
        default=FRESHMAN,
    )

    def is_upperclass(self):
        return self.year_in_school in (self.JUNIOR, self.SENIOR)
```

- tuple in tuple in choices

```python
#first element 'Audio' : name to group
#iterable second element: 
	#a value & a human-readable name for an option
MEDIA_CHOICES = (
    ('Audio', (
            ('vinyl', 'Vinyl'),
            ('cd', 'CD'),
        )
    ),
    ('Video', (
            ('vhs', 'VHS Tape'),
            ('dvd', 'DVD'),
        )
    ),
    ('unknown', 'Unknown'),
)
```
- Retrieve method

```python
>>> get.media_choices_display()
```
- to make one of blank: `blank = False` or put `(None, "YOUR STRING(none)")` as an option(otherwise it shows "------"

###db_column
- name of DB column to use for this field
- if not given, use field name as DB column

###db_index
- True: db's index will be created

###db_tablespace (name of talbe for field's index)
- if `db_tablespace = "INDEX TBSPACE NAME"`
- tablespace is set to `'tables'`: `class TablespaceExample`'s data will be stored in `'tables'` table
- index of the model will be created for name and edges by `db_tablespace="indexes"`. They are also stored in `"indexes"` talbe

```python
class TablespaceExample(models.Model):
    name = models.CharField(max_length=30, db_index=True, db_tablespace="indexes")
    data = models.CharField(max_length=255, db_index=True)
    edges = models.ManyToManyField(to="self", db_tablespace="indexes")

    class Meta:
        db_tablespace = "tables"
```
###default
- default for field
- either value or callable object (def...)
- CANT be mutalbe objects (model instance, list, set)
- CANT be lambdas b.c lambdas cant be serialized by migrations

###editalbe
- False: NOT displayed in admin nor ModelForm

####ModelForm: (django provides...)
- a *helper class* that lets you **create a Form class** from a Django model
- allows you to use fields already created in models

```python 
from django.forms import ModelForm
from .models import Post

class PostForm(ModelForm):
    class Meta:
        model = Post
        fields = ['author','title','content','published_date']
```
###error_messages
- override the default messages that the field will raise
- pass **as a dictionary** with **keys** matching you error messages
- keys: *null, blank, invalid, invalid_choice, unique, and unique_for_date*

###primary_key:
- True: this fields is pk for a model
- `id = models.AutoField(primary_key=True)`
	* implies `null=False` and `unique=True`
- otherwise: automatically add an AutoField to hold the primary key

###unique
- True: must be unique throughout a table
- enforced at **DB level** and by **model validation**

###unique_for_date???
- Set it to the name of a DateField or DateTimeField to require that this field be unique for the value of the date field.
- ex: titles on certain pubdate m
```python
class Example(models.Model):
    title = models.CharField(unique_for_date = 'pub_date')
    pub_date = models.DateTimeField()
```




















