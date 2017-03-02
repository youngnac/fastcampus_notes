
- [QuerySet method reference](https://docs.djangoproject.com/en/1.10/ref/models/querysets/)

#QuerySet API reference

##QuerySet evaluation
* slicing
* repr() : immediately see your results when using the API interactively.
* len()
* list()
* exists()

```python
if some_queryset.filter(pk=entry.pk).exists():
    print("Entry contained in queryset")
```

* bool(), or, and, if
##QuerySet API
- `class QuerySet(model=None, query=None, using=None)`
- has two public attrs : `ordered` (`order_by=True` and default:F) and `db`

###Methods returning new QuerySets
####filter()
- new QuerySet **containing objects that match** with given parameters (`AND` , `OR` can be used)

```python
#AND filter
In [80]: Blog.objects.filter(post__title__contains = "yes", post__published_date__day=11)
Out[80]: <QuerySet []>
#Two filters acting on all() - OR filter
In [81]: Blog.objects.filter(post__title__contains = "yes").filter(post__published_date__day=11)
Out[81]: <QuerySet [<Blog: Beatles Dog>]>
```

####exclude()
- new q_set  containing objects that **DOES NOT match **with parameters (`AND` , `OR`)

```python
#AND
Entry.objects.exclude(pub_date__gt=datetime.date(2005, 1, 3), headline='Hello')
#OR
Entry.objects.exclude(pub_date__gt=datetime.date(2005, 1, 3)).exclude(headline='Hello')
```

####annotate(\*args,**kwargs)
- annotate each object in q_set with q_expression
	* q_expression: a simple value, a reference to a field on the model (or any related models), or an aggregate expression (averages, sums, etc.) 
- 주석을 달 듯, to each q\_set object, q_expression을 실행한 값을 주석처럼 달고 있어 그 값들을 불러올 수 있음...

```python
#q is generated as blog objects that has counts of entry..?
>>> q = Blog.objects.annotate(Count('entry'))
# The name of the first blog (q[0])
>>> q[0].name
'Blogasaurus'
# The number of entries on the first blog(q[0])
>>> q[0].entry__count
42
>>> q = Blog.objects.annotate(number_of_entries=Count('entry'))
# The number of entries on the first blog, using the name provided
>>> q[0].number_of_entries
42
```

####order_by(\*fields)
- return q_set ordered by ordering tuple given 
- order by descending pub_date and ascending headline
	* `Blog.objects.order_by('-pub_date', 'headline')`
- order by random: `order_by(?)`
- order (posts) by field in different model(blog's name): - use *TWO underscores*
	* Post.objects.order_by('blog__name')
- also possible to order a queryset by a related field, without incurring the cost of a JOIN, by referring to the _id of the related field????:

```python
# No Join
Entry.objects.order_by('blog_id')

# Join
Entry.objects.order_by('blog__id')
```
- may use `desc()` and `asc()` on the expression
	* `Entry.objects.order_by(Lower('headline').desc())
`
- if want NO ORDERING: `order_by()`; no parameters

####reverse()
- **reverse ordered** q_set's elements returned
- ex) return last 5 items in q_Set
	* `my_queryset.reverse()[:5]`
- must be **used on ordered** q_set (either model has ordering, or use order_by())

####distinct(*fields)
- return q_set that uses SQL `SELECT DISTINCT`
	* return q_set excluding repeated items (duplicates)

####values()
- returns q_set with dictionaries (used as iterable)
- key: attr names 

```python
In [76]: Post.objects.filter(id=1).values()
Out[76]: <QuerySet [{'comments_count': 10, 'modified_date': datetime.date(2017, 2, 10), 'rating': 0, 'content': '', 'title': 'First title', 'id': 1, 'published_date': None, 'pingbacks_count': 5, 'blog_id': 2}]>
```

####values_list(*fields, flat=False)
- return tuples (simiar to values() - but values() return dict)

```python
In [90]: Post.objects.filter(id=1).values_list()
Out[90]: <QuerySet [(1, 2, 'First title', '', None, datetime.date(2017, 2, 10), 10, 5, 0)]>
```
```python
In [93]: Post.objects.values_list('blog_id','title','blog__name')
Out[93]: <QuerySet [(2, 'First title', 'Beatles Dog'), (2, 'Yes P', 'Beatles Dog'), (2, 'Tmr P', 'Beatles Dog'), (2, 'Second', 'Beatles Dog')]>
```

- using `flat=True`: return single values, not tuples

```python
In [95]: Post.objects.values_list('blog__name',flat=True)
Out[95]: <QuerySet ['Beatles Dog', 'Beatles Dog', 'Beatles Dog', 'Beatles Dog']>

In [96]: Post.objects.values_list('blog__name',)
Out[96]: <QuerySet [('Beatles Dog',), ('Beatles Dog',), ('Beatles Dog',), ('Beatles Dog',)]>
```

####dates(field, kind, order='ASC')
- field: name of field that uses DateField (ex: pub_date, created_date)
- kind: 'year','month','day; returns of all distinct values

####datetimes(field_name, kind, order='ASC', tzinfo=None)
- similar to dates()
- uses datetime.datime objects

####none(): returns nothing, no query will be executed
```python
>>> from django.db.models.query import EmptyQuerySet
>>> isinstance(Entry.objects.none(), EmptyQuerySet)
True
```

####select_related() !!!!
- returns q_set that follows FK relatinoship; select additional related-object
```python
e = Entry.objects.select_related('blog').get(id=5)
```

####prefetch_related(*lookups)
- returns q_set that will automatically retrieve related objects for specific lookups in a single batches
- similar to `select_related` but different
	* select_related: work as it creates an SQL join and including the fields of related object; so it get the related objects in the same DB query; and it is limited to single-value relationships - FK and 1-1
- prefetch_related does a **separate lookup for each relationship**
- it allows to prefetch M-M and M-1 objects
- ex: `Pizza.objects.all().prefetch_related('toppings')`
	* ex implies that `self.toppings.all()` for each pizza; thus there is no need to go DB when self.toppings.all() is called

####extra()
- `extra(select=None, where=None, params=None, tables=None, order_by=None, select_params=None)`
- a hook for injecting specific clauses into SQL generated by a QuerySet
- **NOT portable** to other DB engines; use it as a LAST resort ( *avoid if possible* )
- to Use: Specify one or more of *params, select, where or tables*. None of the arguments is required, but you should use at least one.
	1. `select` : extra field in SELECT clause (sql)
	
		* `Entry.objects.extra(select={'is_recent': "pub_date > '2006-01-01'"})`
		* Entry object now has an extra attribute, `is_recent`, a boolean representing whether the entry’s pub_date is greater than 2006-1-1.
		
	1. where/tables
		
		* `Entry.objects.extra(where=["foo='a' OR bar = 'a'", "baz = 'a'"])`
		* non-explicit joins using where (AND)
		
	1. order_by
		
		* order the resulting q_set using some of the new fields or tables you have included via extra() use the order_by parameter to extra() and pass in a sequence of strings
		
	1. params
	
		* parameter that is a list of any extra parameters to be substituted.
		* `Entry.objects.extra(where=['headline=%s'], params=['Lennon'])`
	
####defer(*fields)
- when using the results of q_set where you **DO NOT want to retrieve certain fields**
- `Entry.objects.defer("headline", "body")`
- each deferred field still can be accessed one at a time
- can make multiple calls to `defer()` ; order does not matter
	* `Entry.objects.defer("body").filter(rating=5).defer("headline")`

- to clear defer:  `my_queryset.defer(None)` ; all fields loaded immeadiately
- NOT Possible to defer PK; espeically when using select_related (since it uses PK to retrieve related models)

####only(*fields)
- specifying the field that SHOULD NOT be deferred; kind of opposite of `defer()`
- `only(something)` == `defer(all other than something)`
- successive only()'s; only counts the last only()
- possible to combine defer() and only()
	* `Entry.objects.only("headline", "body").defer("body")`
	* above: everything except headline is deferred

####using(*fields)
- specifying which DB to be evaluated when using more than one DB
- `Entry.objects.using('backup')`

####select_for_update(nowwait=False)
- returns q_set that will lock rows until the end of the transaction, generating a `SELECT ... FOR UPDATE SQL` statement on supported databases.
- `entries = Entry.objects.select_for_update().filter(author=request.user)`
	* all matched entries are lock 'til the end of transaction block (other transactions CAN'T change/acquire them)

####raw(raw_query, params=None, translations=None)
- takes a raw SQL query, executes it, and returns a django.db.models.query.RawQuerySet instance
- RawQ_set instance can be iterated over like normal q_set to provide object instances
- [for more info](https://docs.djangoproject.com/en/1.10/topics/db/sql/)
- always triggers a new query and doesn’t account for previous filtering; thus, must not be called from Manager or from a fresh q_set

###Methods that do NOT return q_sets
> evaluate q_set and return SOMETHING other than q_set

####get(**kwargs)
- Returns the object matching the given lookup parameters

####create(**kwargs)
- method for creating an object and saving it all in one step
- NO NEED for save()
- below two are the SAME:

```python
p = Person.objects.create(first_name="YN", last_name="CHO")
p = Person(first_name="YN", last_name="CHO")
p.save(force_insert=True)
```

####get_or_create(defaults=None, \*\*kwargs)
- *look up an object* with the given kwargs (may be empty if your model has defaults for all fields), **create one if necessary.**
- defaults are optional fields, thus not passed as **kwargs

```python
(obj, created)=Person.objects.get_or_create(first_n="yn", last_n="cho")
```

####update_or_create()




















