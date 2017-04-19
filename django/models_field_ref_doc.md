- [Field types](https://docs.djangoproject.com/en/1.10/ref/models/fields/) 

## Field options
### Null
-
- True : **store empty** as NULL in db (default: False)
- **Do NOT** use `NULL` **on strings** (storing NULL and empty string together)
- SET `blank=True` for strings and non-string-based fields if you want to allow empty values in forms
- for boolean: use `NullBooleanField`
- check if you want to store

### blank
-
- True: **allowed to be blank ('')** (d: False)
	* for **both numbers/strings**, any input section can be left blank
- null > db-related vs. blank > validation-related
- True: form validation allows empty values
- False: requires field

### choices
-
- iteralbe (list/tuple) consisting itself of iterables of exactly TWO items (e.g. [(A, B), (A, B) ...])
- if given: by default, widget is a select box
- best to define choices inside a model class,
- (A,B) 
	* A: actual value to be set on the model
	* B: human readable name

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
# first element 'Audio' : name to group
# iterable second element: 
	# a value & a human-readable name for an option
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

### db_column
- name of DB column to use for this field
- if not given, use field name as DB column

### db_index
- True: db's index will be created

### db_tablespace (name of table for field's index)
- if `db_tablespace = "INDEX TBSPACE NAME"`
- tablespace is set to `'tables'`: `class TablespaceExample`'s data will be stored in `'tables'` table
- index of the model will be created for name and edges by `db_tablespace="indexes"`. They are also stored in `"indexes"` table

```python
class TablespaceExample(models.Model):
    name = models.CharField(max_length=30, db_index=True, db_tablespace="indexes")
    data = models.CharField(max_length=255, db_index=True)
    edges = models.ManyToManyField(to="self", db_tablespace="indexes")

    class Meta:
        db_tablespace = "tables"
```
### default
- default for field
- either value or callable object (def...)
- CANT be mutable objects (model instance, list, set)
- CANT be lambdas b.c lambdas cant be serialized by migrations

### editable
- False: NOT displayed in admin nor ModelForm

#### ModelForm: (django provides...)
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
### error_messages
- override the default messages that the field will raise
- pass **as a dictionary** with **keys** matching you error messages
- keys: *null, blank, invalid, invalid_choice, unique, and unique_for_date*

### primary_key:
- True: this fields is pk for a model 
- `id = models.AutoField(primary_key=True)`
	* implies `null=False` and `unique=True`
- otherwise: automatically add an AutoField to hold the primary key

### unique
- True: must be unique throughout a table
- enforced at **DB level** and by **model validation**

### unique\_for_date
- Set it to the name of a DateField or DateTimeField to require that this field be unique for the value of the date field.
- ex: titles on certain pubdate m
```python
class Example(models.Model):
    title = models.CharField(unique_for_date = 'pub_date')
    pub_date = models.DateTimeField()
```
- this is at a **validation level** (NOT DB level)

### unique\_for_month
### unique\_for_year
-
- these two are just like unique for date, only range difference

### validators
-
- list of validators to run for this field

## Field types

### AutoField
-
- an IntegerField that increments automatically according to available IDs
- ex: `id = models.AutoField(primary_key=True)`

### BigAutoField
-
- similar with `AutoField` but allows more larger #  ` ~ (64-bit-integer)

### BigIntegerField
-
- 64-bit integer like IntegerField (-9223372036854775808 ~ 9223372036854775807)

### BinaryField
-
- field to store raw binary data 
- only supports bytes
- NOT possible to filter a queryset (???)
- NOT possible to include a BinaryField in a ModelForm

### BooleanField
-
- True/False
- in widget: Checkbox
- to accept *null* : use **`NullBooleanField`**

### CharField
-
- string field from small - large
- for *Larger*: use **`TextField`**
- default widget: TextInput
- Required: **`max_length`**

### CommaSeparatedIntegerField (???)
-
- field of integers separated by commas

### DateField
-
- date, represented by in Python by **datetime.date** instance

####  DateField.auto_now (whenever save)
- set the field to **now every time the object is saved**
-  “last-modified” timestamps

#### DateField.auto_now_add (when created-just once)
-  set the field to now when the object is first created
-  Useful for creation of timestamps
-  IF want to modify this field: 
	* from datetime.date.today(), `DateField: default=date.today` 
	* from django.utils.timezone.now(), `DateTimeField: default=timezone.now`

### !! auto_now and auto_now_add cause the field to have **`editable=False` & `blank=True`**

### DateTimeField(auto_now=False, auto_now_add=False)
-
- default *form widget* : **`TextInput`**

### DecimalField
-
- required: **`max_digits=None, decimal_places=None`**
- `DecimalField(max_digit= 4 , decimal_paces = 3`: represents number upto 9 with decimal places of 3 (3.456)
- default : `NumberInput` when `localize=False`
- otherwise,`TextInput`

### DurationField
-
- store periods of time: 
- modeled by timedelta: `class datetime.timedelta(days=0, seconds=0, microseconds=0, milliseconds=0, minutes=0, hours=0, weeks=0)`

### EmailField
- validates email inputs
- `email = models.EmailField(unique=True, blank=False)`

### FileField **
-
- **A file-upload field**
#### FileField.upload_to
- a way of setting the upload directory and file name
- TWO possible ways (both pass values to `Storage.save()`

```python
class MyModel(models.Model):
    #  uploaded to MEDIA_ROOT/uploads
    upload = models.FileField(upload_to='uploads/')
    #  saved to MEDIA_ROOT/uploads/2015/01/30
    upload = models.FileField(upload_to='uploads/%Y/%m/%d/')
```
- `upload_to` can be a callable which means a function; it req two args: instance, filename

	1. instance: where the `FileField` is defined
	1. filename originally given to the file

```python
def user_directory_path(instance, filename):
    #  file will be uploaded to MEDIA_ROOT/user_<id>/<filename>
    return 'user_{0}/{1}'.format(instance.user.id, filename)

class MyModel(models.Model):
    upload = models.FileField(upload_to=user_directory_path)
```
	
#### FileField.storage

### FieldFile
To access FileField: FieldField is given as a proxy for accessing the underlying file
#### FieldFile.name
-  name and path from the root of storage
#### FieldFile.size
#### FieldFile.url
- read only property to access files' relative URL
#### FieldFile.open (close(), save(), delete())
- `save()` : file's name and contents go to storage class for the field
	* two *req* args : **name** and **content**
	* constructing a File in 2 ways
	
		1. from Existing python file:
		
		```python
		from django.core.files import File
#  Open an existing file using Python's built-in open()
f = open('/path/to/hello.world')
myfile = File(f)
		```
		
		2. from a python string

		```python
		from django.core.files.base import ContentFile
myfile = ContentFile("hello world")
		```

### FilePathField
-
`FilePathField(path=None, match=None, recursive=False, max_length=100,...)`
- CharField limited to filenames in a certain directory on filesystem
- 3 args; first one( `path` ) is required

#### FilePathField.path
- The absolute filesystem path to a directory from which this FilePathField should get its choices. 

```python
from django.conf import settings

class Foo(models.Model):
    audio = models.FilePathField(path=settings.FILE_PATH_FIELD_DIRECTORY)
```

#### FilePathField.match (optional)
- regular expression that filters filenames
- applied to base filename (NOT full)

#### FilePathField.recursive (optional)
- T or F: default=F
- True: all subdirectories of path are included

#### FilePathField.allow_files
- True: folders in specified location are included  ( `allow_files` = True does the same)

### FloatField
-
- floating point number represented by float instance
- `localize = False` and default: NumberInput; otherwise, TextInput

### ImageField
-
- `class ImageField(upload_to=None, height_field=None, width_field=None, max_length=100, **options)`
- inherits all attrs and methods from FileField
- TWO extras: `height_field` and `width_field`
- req [Pillow]library(https://pillow.readthedocs.io/en/latest/)
- in DB: created as **`varchar`** columns with default of `max_length=100` (can be overridden)

### IntegerField
-
- Values from -2147483648 to 2147483647
- default: NumberInput

#### PositiveIntegerField
- accepts positive integers (0 ~ 2147483647)

#### SmallIntegerField
- -32768 to 32767 

#### PositiveSmallIntegerField
- 0 to 32767

### GenericIPAddressField
-
- `class GenericIPAddressField(protocol='both', unpack_ipv4=False, **options)`
- an IPv4 of IPv6 address in string (default widget: TextInput)

#### GenericIPAddressField.protocol
- 'both' (default), 'IPv4' or 'IPv6'

#### GenericIPAddressField.unpack_ipv4
- used when protocol is set to **'both'**
- **Unpacks IPv4** mapped addresses like "::ffff:192.0.2.1." into "192.0.2.1." 

### NullBooleanField
- accepts Null and True/False ( `BooleanField(null=True)` )

### SlugField
-
- short label for something, containing only letters, numbers, underscores or hyphens
- generally used in URLs
- can specify `max_length` (default: 50)
- implies: `index=True`
- sometimes in admin : slugfield is prepopulated...

```python
class ArticleAdmin(admin.ModelAdmin):
    prepopulated_fields = {"slug": ("title",)}
```

### TextField
-
- large text field
- default form widget: Textarea

### TimeField
-
- time represented by a `datetime.time` in python
- the **same auto-population** options as **`DateField`**
- default widget: TextInput

### URLField
-
- `class URLField(max_length=200, **options)`
- A CharField for a URL

### UUIDField 
-
- store universally unique identifiers
- [what is it?] (https://en.wikipedia.org/wiki/Universally_unique_identifier)
- good alternative to AutoField for primary_key
```python
import uuid
from django.db import models

class MyUUIDModel(models.Model):
    id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
```

## Relationship Fields
### ForeignKey
-
- `class ForeignKey('_othermodel_', on_delete=... , options..)`
- req one positional (first) arg which is the Model that it has relationship with 
- may have recursive by putting `'self'` at the first positional arg

```python
class Car(models.Model):
    manufacturer = models.ForeignKey(
        'Manufacturer',
        on_delete=models.CASCADE,
    )
 #  if FK's relationship with another app use: 'production.Manufacturer'
```
- DB index is automatically created on FK (to disable: db_index=False)

### Arguments for FK
#### on_delete: when object by FK is deleted
* CASCADE : **also deletes** the object containing the FK
* PROTECT : **prevent deletion**
* SET_NULL : set FK null; only possible if null=True
* SET_DEFAULT : set FK to default value; default must be set to use
* SET() : value passed to something... it can be a callable

```python 
def get_sentinel_user():
    return get_user_model().objects.get_or_create(username='deleted')[0]

class MyModel(models.Model):
    user = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.SET(get_sentinel_user),
    )
```

* DO_NOTHING : **no action** (raise Integrity Error)

#### limit_choices_to
- **sets a limit to available choices** for this field when field is rendered *using ModelForm or the admin *
- ex: on ModelForm: list only Users with `is_staff=True`

```python
staff_member = models.ForeignKey(
    User,
    on_delete=models.CASCADE,
    limit_choices_to={'is_staff': True},
)
```

- ex: `limit_choices_to =  _callable_`

```python
def limit_pub_date_choices():
    return {'pub_date__lte': datetime.date.utcnow()}

limit_choices_to = limit_pub_date_choices
```

#### related_name
- name to use **for the relation** from the related object back to this one
- `related_name='+'` : django WILL NOT create backward relation to this model

#### related_query_name (from target)
- name to use **for the reverse filter** name from the target (defaults to related_name)

#### to_field
- field on related object that the relation is to
- by default: django uses PK 

#### swappale ???
- Controls the migration framework’s reaction if this FK is pointing at a swappable model.

### ManyToManyField
- `class ManyToManyField(othermodel, options... )`
- req ONE positional arg: the class to which the nodel is related

### Arguments for MTMField
-
- similar with FK (limit_choices_t0, related_name, related_query_name)

#### symmetrical
- ONLY used in MTM to 'self'

#### through
- specify **intermediary** with `through = 'some_table'`

#### through_fields
- if intermediary has many FK relation: source must specify two fields that make relationship with the source
- it must be in s tuple form: ('field1'z, 'field2'`)
- ex: `through_fields=('group', 'person')`

#### db_table
- name of the table to create for storing the many-to-many data

#### db_constraint 
- whether or not there should be constraint for intermediary (default: T)
- False: bad for data integrity

#### swappale

### OneToOneField
-
- similar to FK with `unique=True` but reverse side return directly a single object
#### related_name
- if not set: django will **use lower-case** name

#### parent_link
- True and used in a model inheriting from concrete model: indcates that this field should be used as the **link back to the parent class**

## Field API Ref
## Field Attribute reference

