# Retrieving objects

* ### Retrieving all objects

~~~django
all_entries = Entry.objects.all()
~~~

It returns **Queryset**





* ### Specific objects with filters

~~~python
# filter(**kwargs) 
Entry.objects.filter(pub_date__year=2006)

# exclude(**kwargs)

# get()
~~~

**filter()**는 항상 **Queryset**을 리턴하는 반면에  
**get()**은 **a single object**를 리턴한다



매칭되는 것이 없을때,   
**filter()**는 **empty Queryset**  /  **get()**은 **DoesNotExist Exception**을 발생시킨다  
한 개 이상 매칭될때는 **MultipleObjectsReturned** 가 발생한다







* ### Chaining filters

~~~python
Entry.objects.filter(
     headline__startswith='What'
 ).exclude(
     pub_date__gte=datetime.date.today()
 ).filter(
     pub_date__gte=datetime(2005, 1, 30)
 )
~~~

*Filtered query sets are unique*

~~~python
q1 = Entry.objects.filter(headline__startswith="What")
q2 = q1.exclude(pub_date__gte=datetime.date.today())
q3 = q1.filter(pub_date__gte=datetime.date.today())
~~~

*Lazy queryset*

```python
q = Entry.objects.filter(headline__startswith="What")
q = q.filter(pub_date__lte=datetime.date.today())
q = q.exclude(body_text__icontains="food")
print(q)
```

> Though this looks like three database hits, in fact it hits the database only once, at the last line (`print(q)`). In general, the results of a [`QuerySet`](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#django.db.models.query.QuerySet) aren’t fetched from the database until you “ask” for them.



## Q object

여러개의 매칭 조건을 주고 싶을때 사용한다  

~~~python
from django.db.models import Q

Blog.objects.filter(Q(id__in=[1, 2]) & Q(name__contains='first'))
~~~



## Retrieving span relationships

~~~python
Question.objects.filter(choice__choice_text__icontains = 'game')
# 모델명은 소문자로 적어야 한다
~~~

