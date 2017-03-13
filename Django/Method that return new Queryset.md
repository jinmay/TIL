# Method that return new Queryset

* filter()
* exclude()
* annotate()
* order_by()

~~~python
Entry.objects.filter(pub_date__year=2005).order_by('-pub_date', 'headline')

# 다른 model에서 정렬하려면 modelname__fieldname 으로 정의한다
Entry.objects.order_by('blog__name', 'headline')

# order_by()가 여러개 있을땐 가장 마지막의 것만 영향을 준다
# order_by('pub_date')만 작동함
Entry.objects.order_by('headline').order_by('pub_date')
~~~

* reverse()

> Django doesn’t support that mode of access (slicing from the end), because it’s not possible to do it efficiently in SQL.

django는 python과는 다르게 뒤에서부터 시작하는 인덱스 슬라이싱을 지원하지 않는다. (e.g. seq[-5:])

* distinct()
* values() - returns dictionaries

```python
# This list contains a Blog object.
>>> Blog.objects.filter(name__startswith='Beatles')
<QuerySet [<Blog: Beatles Blog>]>

# This list contains a dictionary.
>>> Blog.objects.filter(name__startswith='Beatles').values()
<QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>

Blog.objects.values()
# <QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>
Blog.objects.values('id', 'name')
# <QuerySet [{'id': 1, 'name': 'Beatles Blog'}]>
```

* values_list() - returns tuples

* dates(field, kind) - Field가 DateTimeField 이거나 DateField 일 때,

  > year / month / day

~~~python
>>> Entry.objects.dates('pub_date', 'year')
[datetime.date(2005, 1, 1)]

>>> Entry.objects.dates('pub_date', 'month')
[datetime.date(2005, 2, 1), datetime.date(2005, 3, 1)]

>>> Entry.objects.dates('pub_date', 'day')
[datetime.date(2005, 2, 20), datetime.date(2005, 3, 20)]

>>> Entry.objects.dates('pub_date', 'day', order='DESC')
[datetime.date(2005, 3, 20), datetime.date(2005, 2, 20)]

>>> Entry.objects.filter(headline__contains='Lennon').dates('pub_date', 'day')
[datetime.date(2005, 3, 20)]
~~~

* datetimes(field_name, kind) - models should be DateTimeField model

> year / month / day / hour / minute / second 

* none()
* all()
* select_related()
* prefetch_related()
* extra()







# Methods that do not return Querysets

* get() 

>매칭되는 object가 없을 때 **DoesNotExist** Exception 
>
>여러개의 objects가 매칭될 때 **MultipleObjectsReturned** Exception

* create()
* get_or_create()

~~~python
obj, created = Person.objects.get_or_create(
    first_name='John',
    last_name='Lennon',
    defaults={'birthday': date(1940, 10, 9)},
)
~~~

> 조건에 해당하는 object를 찾으면 tuple 과 False 를 리턴하고  
> 여러개가 매칭되면 **MultipleObjectsReturned**을 리턴하고  
> 매칭되는 게 없다면 defaults에 따라 객체를 생성 후 저장하고 True를 리턴한다.
>
> 이때, key가 겹친다면 IntegrityError 가 발생함

* update_or_create()
* count()

~~~python
Post.objects.count()
# Post model의 레코드 갯수를 리턴함(int형)
~~~

* latest(field_name)
* earliest(field_name)
* first()
* last()
* aggregate()
* exists()
* update()
* delete()