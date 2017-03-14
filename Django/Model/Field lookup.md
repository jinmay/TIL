# Field lookup

> Field lookups are how you specify the meat of an SQL `WHERE` clause. They’re specified as keyword arguments to the `QuerySet` methods [`filter()`](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#django.db.models.query.QuerySet.filter), [`exclude()`](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#django.db.models.query.QuerySet.exclude) and [`get()`](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#django.db.models.query.QuerySet.get).





기본적인 사용법은 아래와 같다.    

```python
field__lookuptype = value # __ 는 double underscore
```





Example 

~~~ python
Reporter.objects.filter(full_name__startswith = 'John')
# Reporter 모델의 full_name 속성의 값이 'John'으로 시작하는 레코드를 찾음

Entry.objects.filter(pub_date__lte = '2006-01-01')
# Entry 모델의 pub_date 속성의 값이 '2006-01-01' 보다 작거나 같은 레코드를 찾음
# SQL로 바꿔보면 
#         SELECT * FROM blog_entry WHERE pub_date <= '2006-01-01';

~~~



## List

* contains : Case - sensitive

~~~python
Entry.objects.get(headline__contains = 'Lennon')
~~~

* icontains : Case - insensitive
* exact

~~~python
Entry.objects.get(id__exact=14)
Entry.objects.get(id__exact=None)
~~~

* iexact
* in : in a given list

~~~python
Entry.objects.filter(id__in=[1, 3, 4])
#
#
inner_qs = Blog.objects.filter(name__contains='Cheddar')
entries = Entry.objects.filter(blog__in=inner_qs)
~~~

* gt : Greater than
* gte : Greater than or equal to
* lt : Less than
* lte : Less than or equal to
* startswith

~~~python
Entry.objects.filter(headline__startswith='Will')
~~~

* istartswith
* endswith

~~~python
Entry.objects.filter(headline__endswith='cats')
~~~

* iendswith
* range

~~~python
import datetime

start_date = datetime.date(2005, 1, 1)
end_date = datetime.date(2005, 3, 31)
Entry.objects.filter(pub_date__range=(start_date, end_date))
~~~



* date
* year
* month
* day
* week_day
* hour
* minute
* second
* isnull