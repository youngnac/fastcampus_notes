[link to doc](https://docs.djangoproject.com/en/1.10/ref/forms/api/)

# Forms API
## Bound and Unbound forms
- form : either **bound** to a set of data, or **unbound**
	- bound to a set of data: form **can validate the data** and **render the form as HTML**  with data displayed in the HTML
	- unbound: CANNOT validate, but can still redner the blank as HTML

- Create UNBOUND Form instance: ` f = PostForm()`
- create BOUND Form, pass data as a dictionary to Form's first parameter
	* key: field names, attributes in Form class
	* values: data to be validated by Form

	```python
	data = {'subject': 'hello',
...         'message': 'Hi there',
...         'sender': 'foo@example.com',
...         'cc_myself': True}
>>> f = ContactForm(data)
	```

### Form.is_bound
-
- to check if Form is bounded: `form.is_bound`

```python
>>> f = ContactForm()
>>> f.is_bound
False
>>> f = ContactForm({'subject': 'hello'})
>>> f.is_bound
True
```

* !!: to change the data somehow, or to bind an unbound Form instance, MUST create another Form instance

## Using forms to validate data
### Form.clean()
-
- to add custom validation for fields that are interdependent: `clean()`

### Form.is_valid()
-
- to run validation: `is_valid` > return BOOLEAN
	* if some field are `blank=False`, Form of data given as '' will return False when `f.is_valid`
	
### Form.errors
-
- `f.errors` will return dictionary of error messages stored in a list

```python
>>> f.errors
{'sender': ['Enter a valid email address.'], 'subject': ['This field is required.']}
```
#### Form.errors.as_data()
- returns a dict that maps fields to their original ValidationError instances

```python
>>> f.errors.as_data()
{'sender': [ValidationError(['Enter a valid email address.'])],
'subject': [ValidationError(['This field is required.'])]}
```
#### Form.errors.as_json(escape_html=False)
- errors serialized as JSON
- by default: does not escape its ouput

```python
>>> f.errors.as_json()
{"sender": [{"message": "Enter a valid email address.", "code": "invalid"}],
"subject": [{"message": "This field is required.", "code": "required"}]}
```

#### Form.add_error(field, error)
- add errors to specific fields from within the Form.clean() method, or from outsidte the form altogether( ex: from view)
- value = None; Form.non_field_errors()
- error argument must be a simple string, or instance of [ValidationError](https://docs.djangoproject.com/en/1.10/ref/forms/validation/# raising-validation-error)
- removes the relevant field from cleaned_data

#### Form.has_error(field, code = None)
- **returns a boolean** designating whether a field **has an error** *with a specific code*
- code:None > return True if the field has no errors at all

#### Form.non_field_errors()
- returns the list of errors from Form.errors that aren’t associated with a particular field, including ValidationErrors that are raised in Form.clean() and errors added using Form.add_error(None, "...")

## Dynamic initial values
### Form.initial
- to declare the initial value of form fields at runtime
- ex: fill in a username field with username of current session
- to declare initial value:
	* `Form(initial = {'field_name': 'values'})`

```python
>>> f = ContactForm(initial={'subject': 'Hi there!'})
```
- only displayed **for unbound forms**
- not used as fallback values if particular value is NOT given

## Checking which form data has changed
### Form.has_changed()
- to check if form data has been changed from initial data
- `f.has_changed()` > returns boolean 
- to compare, reconstruct with original data:

	```python
	>>> f = ContactForm(request.POST, initial=data)
	>>> f.has_changed()
	```
- above ex will return True if request.POST data differs from initial

### Form.changed_data
- returns a list of fields' names whose values in the form's bound data differ from `initial`
- **f.changed_data**
- if nothing has changed, returns empty list

	```python
	>>> f = ContactForm(request.POST, initial=data)
	>>> if f.has_changed():
	...     print("The following fields changed: %s" % ", ".join(f.changed_data))
	```

## Accessing the fields from the form
### Form.fields
- access fields of Form instance
- f.fields['field_name']
- to changed the label for field in a table:
	* `f.fields['name'].label = "Username"`
- DO NOT alter `base_fields` attribute; it will affect all subsequent instances...


## Accessing "clean" data
### form.cleaned_data
- each field in form is responsible for "cleaning", resulting in consistent output

```
f.cleaned_data
{'cc_myself': True, 'message': 'Hi there', 'sender': 'foo@example.com', 'subject': 'hello'}
```

- Charfield, EmailField: always cleans the input into Unicode string
- if data is NOT valid, `cleaned_data` only contains valid fields
- if `form.is_valid = True` and there is blank values, still returns all data ad cleaned_data

## Outputting forms as HTML
- to render itself as HTML

1. just `print(f)`; same as `f.as_table()`
1. `f.as_p()` : renders form as a series of <p> tags
1. `as_ul()`: renders as a series of <li> tags

## Styling required or erroneous form rows
### Form.error_css_class
### Form.required_css_class
- to add class attributes to required rows or to rows with errors

```python
class ContactForm(forms.Form):
    error_css_class = 'error'
    required_css_class = 'required'
```
## Configuring from elements' HTML id attr and <label> tags
### Form.auto_id
- by default, form rendering methods include 
	* HTML id attrs on the forms elementts
	* The corresponding <label> tags around the labels
- `f = ContactForm(auto_id=False)` : the form output will NOT include <label> tags nor id attrs
### Form.label_suffix
- translatable **string** that will be **appended after any label name** when a form is rendered

```python
>>> f = ContactForm(auto_id='id_for_%s', label_suffix=' ->')
>>> print(f.as_ul())
<li><label for="id_for_subject">Subject -></label> <input id="id_for_subject" type="text" name="subject" maxlength="100" required /></li>
```

- !!! label suffix is **added only** if the **last character** of the label **isn’t a punctuation **character (in English, those are ., !, ? or :)

### Form.use_required_attribute
- when True, required form fields will have required HTML attrs

### Notes on field ordering
- as_p(), as_ul(), as_table() show fields in the order that you define initially in your form class

**To REORDER fields in HTML output:**
### Form.field_order
- field_order is a **list of field names**, the fields are ordered as specified by the list and *remaining fields are appended according to the default order*.

#### Form.order_fields(field_order)
- rearrange the fields any time using order_fields() with a list of field names as in field_order.

## How errors are displayed
- HTML output will include the validation errors as a `<ul class="errorlist">`
- can custom error list (pass it to alternate class)

## More ouput (other than as_p, as_ul...)
### class BoundField
- to  access a sinlgle field of a Forn instance, use dictionary lookup syntax (use field's name as key)

```python
>>> form = ContactForm()
>>> print(form['subject'])
```
- to get all fields, iterate using for ~ in form

### Attributes of BoundField
-
#### BoundField.auto_id
- HTML ID attr; returns an empty string if Form.auto_id=False
#### BoundField.data
- for unbound, return `None`

```python
>>> b_form = ContractForm(data = {'message': 'hello~'})
>>> print(b_form['message'].data)
hello~
```

#### BoundField.field
- form Field instance from the form that this BoundField wraps

#### BoundField.form
- Form instance this BoundField is bound to

#### BoundField.help_text
- help_text of the field

#### BoundField.html_name
- name used in the widget's HTML name attri; takes the form prefix into account

#### BoundField.id_for_label
- to render the ID of this field

#### BoundField.is_hidden
- return True if BoundField's widget is hidden

#### BoundField.label
- label of the field; used in label_tag()

#### BoundField.name
- name of the field in the form

## Methods of BoundField
### BoundField.as_hidden(attrs=None, **kwargs)[source]
- returns a string of HTML for representing this as an <input type="hidden">.

### BoundField.as_widget(widget=None, attrs=None, only_initial=False)[source]
- renders the field by rendering the passed widget
- any HTML attributes passed as attrs
- no widget, then the field’s default widget will be used

### BoundField.css_classes()
- to *indicate* **required** form fields or fields that contain **errors**

```python
>>> f['message'].css_classes()
'required'
```

### BoundField.label_tag(contents=None, attrs=None, label_suffix=None)
- To separately render the label tag of a form field

```python
>>> f = ContactForm(data={'message': ''})
>>> print(f['message'].label_tag())
<label for="id_message">Message:</label>
```

### BoundField.value()
- to render the raw value of this field as it would be rendered by a Widget

## Binding upload files to a form
- forms that have FileField and ImageField field
- to UPLOAD files, <form> element must correctly define the enctype as "multipart/form-data"
	
	```html
	<form enctype="multipart/form-data" method="post" action="/foo/">

	```

- then, need to bind the file data; 
	* ex : to include an ImageField called mugshot, we need to bind the file data containing the mugshot image
	
	```python
	>>> from django.core.files.uploadedfile import SimpleUploadedFile
>>> data = {'subject': 'hello',
...         'message': 'Hi there',
...         'sender': 'foo@example.com',
...         'cc_myself': True}
>>> file_data = {'mugshot': SimpleUploadedFile('face.jpg', <file data>)}
>>> f = ContactFormWithMugshot(data, file_data)
	```
	
- practically: specify request.FILES as the source of file data

	```python
	>>> f = ContactFormWithMugshot(request.POST, request.FILES)
	```












