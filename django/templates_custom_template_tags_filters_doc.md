[link](https://docs.djangoproject.com/en/1.10/howto/custom-template-tags/)

# Custom template tags and filters
> with the need of functionality that is not provided, you can **EXTEND the template engine** by **defining custom tags and filters** using python 

> then, use **`{% load %}`**

## Code Layout
- place to specify custom template tags and filter: inside a Django app
- Newly installed apps must contain a `templatetags` dir, at the same level as `models.py`, `views.py`, etc.
	* must be a Python package (with `\_\_init__.py`)
	* name of the module file is the name to lad the tags; thus should name that wont cause clash
	* ex) `templatetags/poll_extras.py`
- to LOAD the custom: `{% load module_name.py %}
	* ex) `{% load poll_extras %}`
- to be a **valid tag library**, module must contain a module-level variable named `register` that is a template; at the top of the module, add the following

	```python 
	from django import template
	register = template.Library()
	```

## Writing Custom Template Filters
- customer filters take on or two arguments
	1. the value of the variable (input) - does not have to a string
	1. the value of the argument - may have a default value or left out
- ex: ``{{ var|foo: "bar" }}: `foo` is the filter, and `var`(var) and `"bar"`(arg) are passed through `foo`
- template lang does not provide exception handing; thus it will occur a server error instead of raising any exception

-  **filter ex with 2 args** : Removes all values of arg ("0") from the given string (somevariable)

	```python
	def cut(value, arg):
		return value.replace(arg,'')
	```
	```python
	{{ somevariable|cut:"0" }}
	```
	
- filter ex with 1 arg : Converts a string into all lowercase
	```python
	def lower(value): #  Only one argument.
	    return value.lower()
	```

### Registering custom filters
-
**` django.template.Libary.filter()`**

- once you write you filter definition, register in you library instance
	* `register.filter('cut', cut)`
	* `register.filter('lower', lower)`

- `Libary.filter()` takes 2 args :
	1. name of the filter (a string)
	1. compliation function (python function; not the name of teh function as a string)

- may use `register.filter()` *as a decorator* :

	```python
	@register.filter(name = 'cut')
	def cut(value, arg):
		return value.replace(arg,'')
		
	@register.filter	# filter name not given
	def lower(value):
		return value.lower()
	```

- if name is NOT given at a decorator, django use the function's name as a filter name

### Template filters that expect strings
-
**`django.template,defaultfilters.stringfilter()`**

- setting ONLY STRINGS at first arg of the filter
- import `stringfilter` and use as a decorator

	```python
	from django import template
	from djangon.template.defaultfilters import stringfilter
	
	register = template.Library()
	# integers will just pass
	@register.filter
	@stringfilter
	def lower(value):
		return value.lower()
	```

#### Filters and auto-escaping
- auto-escaping behavior according to different types of strings
	* raw strings: if auto-escaping in effect; escaped and presented unchaged
	* safe strings: marked as safe from further escaping at output time; commonly used for output that contains raw HTML 
	* these strings are type of SafeData
- template filter code encounter one of two situations:
	1. filter does NOT introduce any HTML-unsafe chars:
		* auto-escaping done by **setting `is_safe` flag to True**
		
		```python
		@register.filter(is_safe=True)
def myfilter(value):
    return value
		```
		
		* if non-safe strings are passed, django automatically escape 
		* ex: just adding "xx" to values, this filter does not output dnagerouse HTML; thus, mark it with `is_safe`

		```python 
		@register.filter(is_safe=True)
def add_xx(value):
    return '%sxx' % value
		```
		
	1. manually escape by filter code when necessary:
		* when new HTML markup introduced in the result
		* to mark a string safe, use: `mark_safe()` `from django.utils.safestring`
		* BUT must ensure if a string is REALLY SAFE with filter that operate in templates when auto-esc is either on or off to make things
		* when start writing and registering the filter, set `needs_autoescape=True` to let filter know the current auto-esc state
		
		```python
		from django import template
		from django.utils.html import conditional_escape
		from django.utils.safestring import mark_safe

		register = template.Library()

		@register.filter(needs_autoescape=True)
		def initial_letter_filter(text, autoescape=True):
		    first, other = text[0], text[1:]
		    if autoescape:
		        esc = conditional_escape
		    else:
		        esc = lambda x: x
		    result = '<strong>%s</strong>%s' % (esc(first), esc(other))
		    return mark_safe(result)
    ```

### Filters and time zones
-

- custom filter operating on `datetime` objects
- register it with `expects_localtime=True`: convert the value to current time zone before filtering

	```python
	@register.filter(expects_localtime=True)
	def businesshours(value):
    	try:
       	 return 9 <= value.hour < 17
    	except AttributeError:
        	 return ''
	```

## Writing Custom Template Tags
> tags are complex and filters since they can do anything

### Simple tags
**`django.template.Library.simple_tag()`**

- takes function that accepts any number of args, wraps it in a render function and registers it with the template system (`register = template.Library()`)

```python
import datetime
from django import template

register = template.Library()

@register.simple_tag
def current_time(format_string):
    return datetime.datetime.now().strftime(format_string)
```

- [link to strftime()](https://docs.python.org/3/library/time.html# time.strftime)
- our function is **passed the current value** of the variable *if the* *arg was a template variable*
- `simple_tag` passes it output through `conditional_escape()` if template context is in autoescape mode 
- when you template tag **needs access to the current context**, use **`takes_context=True`** arg when registering
	* first arg must be context
	* [more on takes_context](https://docs.djangoproject.com/en/1.10/howto/custom-template-tags/# howto-custom-template-tags-inclusion-tags)

```python
@register.simple_tag(takes_context=True)
def current_time(context, format_string):
    timezone = context['timezone']
    return your_get_current_time_method(timezone, format_string)
```

- you may custom tag's name using `name="something"`

```python
register.simple_tag(lambda x: x-1, name = 'minusone')

@register.simple_tag(name='minustwo')
def somefuncton(value):
	return value -2
```

- simple tags with multiple positional or kwargs:

```python
@register.simple_tag
def my_tag(a, b, *args, **kwargs):
    warning = kwargs['warning']
    profile = kwargs['profile']
    ...
    return ...
```

* in template: 
	* `{% my_tag 123 "abcd" book.title warning=message|lower profile=user.profile %}`
- possible to **store tag results using `as`** rather than directly outputting

```python
{% get_current_time "%Y-%m-%d %I:%M %p" as the_time %}
<p>The time is {{ the_time }}.</p>
```

### Inclusion tags
**`django.template.Library.inclusion_tag()`**

- displays some data by rendering another template
- register in `template.Library` 
- ex: want to show all chocies in the poll

```python
{% show_results poll %}
```
- this directs to 

```python
@register.inclusion_tag('results.html')
def show_results(poll):
	choices = poll.choice_set.all()
    	return {'choices': choices}
```

- `takes_context=True`: no need to pass args to template tag
- sometimes you want to load the tag whenever that context contains certain word in specific html : ex - link and title in 'link.html'

```python
@register.inclusion_tag('link.html', takes_context=True)
def jump_link(context):
    return {
        'link': context['home_link'],
        'title': context['home_title'],
    }
```

- then:
	* instead of :`Jump directly to <a href="{{ link }}">{{ title }}</a>`
	* just load tag: `{% jump_link %}` > without any args, this custom tags has automatic access to the context

### Assignment tags:
**`django.template.Library.assignment_tag()`**

- ease the creation of tags setting a variable in the context
- works the **same way as `simple_tag()`** except it **stores the tag's result in specified context variable **instead of directly outputting it
- use `as`

```python
@register.assignment_tag
def get_current_time(format_string)
	return datatime.datetime.now().strftime(format_string)
```

- then, in template:

```html
{% get_current_time "%Y-%m-%d %I:%M %p" as the_time %}
<p>The time is {{ the_time }}.</p>
```


















	

