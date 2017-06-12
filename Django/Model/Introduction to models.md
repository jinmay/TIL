# Introduction to models

* 각 모델은 django.db.models.Model 의 파이썬 서브클래스이다

### Quick example

**first_name**고 **last_name**을 가지는 **Person **모델을 생성

~~~python
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length = 100)
    last_name = models.CharField(max_length = 100)
~~~

first_name과 last_name을 모델의 Person 모델의 필드

각 필드는 클래스 속성으로서 정의되고 데이터베이스의 컬럼에 해당한다

위의 모델을 sqlmigrate를 이용하여 SQL문을 보면 아래와 같을 것이다

~~~sql
CREATE TABLE myapp_person (
    "id" serial NOT NULL PRIMARY KEY,
    "first_name" varchar(30) NOT NULL,
    "last_name" varchar(30) NOT NULL
);
~~~

테이블 이름인 myapp_person 은 모델의 meta class로 부터 도출되지만 재정의 할 수 있다  
기본은 **appname_modelname** 이다

id필드 또한 자동으로 생성되며, 재정의 가능하다

### Using models

일단 모델을 만들었다면, 사용하기 위해서 장고에게 알려야 한다.

방법은 해당 models.py를 포함하고 있는 app을 프로젝트의 settings.py에 추가하는 것이다.

~~~python
INSTALLED_APPS = [
    #...
    'myapp',
    #...
]
~~~

INSTALLED_APPS 리스트에 app을 추가하면 된다.

추가했다면 manage.py migrate 하는 것을 잊지말자. 추가적으로 변경사항 반영을 위해 makemigrations이 필요할 수도있다.

### Fields

모델에서 가장 중요한 부분인 필드는 - 그리고 모델에서 유일한 필수 요소인 - 데이터베이스 필드의 리스트이다

Example) 

~~~python
from django.db import models

class Musician(models.Model):
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    instrument = models.CharField(max_length=100)

class Album(models.Model):
    artist = models.ForeignKey(Musician, on_delete=models.CASCADE)
    name = models.CharField(max_length=100)
    release_date = models.DateField()
    num_stars = models.IntegerField()
~~~



### Field types

모델의 각 필드는 적잘한 필드 클래스의 인스턴스가 돼야한다

장고는 아래 세 가지를 정의하기 위해서 필드 클래스 타입을 사용한다

1. 컬럼 타입 (interger, text, varchar etc)
2. 폼 필드를 렌더링 할때 사용할 기본적인 HTML 위젯
3. 장고의 admin과 html form 에서 사용 될 최소한의 validation(유효성검사)

장고는 이미 여러개의 빌트-인 필드타입을 가지고 있다. [ref](https://docs.djangoproject.com/en/1.11/ref/models/fields/#model-field-types)



### Field options

각각의 필드는 각 필드에 맞는 인자를 필요로한다.

> 예를 들면, **CharField(그리고 그것의 서브클래스)**는 저장하는 VARCHAR 데이터베이스 필드의 사이즈를 결정하는 max_length 인자를 필요로한다.

또한 모든 필드 타입에 이용가능한 max_length 와 같은 argument들이 있다

모든 필드 옵션 참고 - [Model field reference](https://docs.djangoproject.com/en/1.11/ref/models/fields/#common-model-field-options)



그 중에서 자주 쓰이는 것들에 대해서 알아보자

* null - True 일때 장고는 데이터베이스에 NULL로서 empty value를 저장한다. 기본값은 False 이다

- blank - 빈 값이 저장되는 걸 허용. 기본값은 False

>null 과 blank의 차이를 구별해야 한다. **null**은 순수하게 데이터베이스와 관련된 속성이지만, 반면에 **blank**는 유효성 검사와 관련된 속성이다. 즉 number = models.IntegerField(blank = True) 인 경우, number 필드는 필수필드가 아니게 된다.

* choice

choice가 주어진다면 기본 HTML위젯은 일반 텍스트 필드 대신에 선택박스로 바뀌게 되고 choice의 튜플에 정의된 값만 선택할 수 있다.

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

('S', 'Small') -> 첫 번째 S가 실제 데이터베이스에 저장되는 값이고  
두 번째 Small이 폼에 표시되는 값이다

폼에 보여지는 값을 보고 싶을땐 모델의 인스턴스의 get_status_display()를 이용하여 접근하면 된다

* default - 기본값을 설정한다. 값 또는 호출가능한 오브젝트도 사용될 수 있다.

- help_text - 사용자들에게 이해를 돕기위한 폼 텍스트에 표시되는 텍스트
- primary_key - True일 경우, 해당 해당 필드는 pk가 된다. 굳이 True로 두지 않을 경우 기본적으로 IntegerField가 pk가 된다. [Automatic primary key fields](https://docs.djangoproject.com/en/1.11/topics/db/models/#automatic-primary-key-fields)

**pk필드는 read-only 필드이다**. 이미 존재하는 오브젝트의 id를 변경하고 저장하려 한다면 새로운 오브젝트가 생성된다.

~~~python
from django.db import models

class Fruit(models.Model):
    name = models.CharField(max_length=100, primary_key=True)
    
    
    
fruit = Fruit.objects.create(name='Apple')
fruit.name = 'Pear'
fruit.save()
Fruit.objects.values_list('name', flat=True)
# ['Apple', 'Pear']
~~~



- unique - True인 필드는 테이블 전체적으로 유일해야 한다

#### Automatic primary key fields

~~~python
id = models.AutoField(primary_key=True)
~~~

위 와 같은 primary_key 값이 True인 id 필드가 암묵적으로 생성된다



### Verbose Field names

ForeignKey / ManyToManyField / OneToOneField 필드를 제외하고 각각의 필드 타입은 positional argument를 선택적으로 가지고 있다 - verbose name이라 부른다.

만약 verbose name이 주어지지 않는다면, 장고는 자동적으로 필드의 속성 이름을 사용 할 것이다  
이 때, 언더스코어(_)는 띄어쓰기로 변한다

~~~python
# verbose name : person's first name
first_name = models.CharField("person's first name", max_length = 20)

# verbose name : first name
first_name = models.CharField(max_length = 20)
~~~

**ForeignKey / ManyToManyField / OneToOneField**는 첫 번째 인자로 모델 클래스를 필요로하고,  verbose_name은 keyword argument로 사용한다

~~~python
poll = models.ForeignKey(
	Poll,
    verbose_name = "the related poll",
)

sites = models.ManyToManyField(Site, verbose_name = "list of sites")
~~~

verbose_name의 첫 글자를 대문자로 사용하지 않는 것이 관습이다. 장고는 자동적으로 첫 글자를 대문자화 한다



### Relationships

RDB의 장점은 서로가  관계맺는 것에 있다. 장고는 세 가지의 일반적인 데이터베이스 관계를 가지고있다.

* many-to-one
* many-to-many
* one-to-one

1. Many-to-one relationships

many-to-many 관계를 정의하기 위해서 django.db.models.ForeignKey를 사용한다. 단지 필드 타입을 사용하는 것처럼 사용하기만 하면된다 - 관계맺을 모델 클래스를 포함하면서.

**ForeignKey**는 positional argument를 필요로 한다 

~~~python
from django.db import models

class Manufacturer(models.Model):
    # ...
    pass

class Car(models.Model):
    manufacturer = models.ForeignKey(Manufacturer)
    # ...
~~~

위의 예시에서, Car model은 Manufacturer 을 가지고 있다 - 즉, Manufacturer는 여러개의 Car를 가질 수 있지만 Car는 단 하나의 Manufacturer를 가질 수 있다

또한 재귀적인 관계와 아직 정의되지 않은 모델과의 관계를 생성할 수 있다

**필수는 아니지만 ForeignKey 필드명은 소문자를 사용하는 걸 권장한다**





2. Many-to-many relationships

many-to-many 관계를 정의하기 위해서 **ManyToManyField**를 사용한다.

또한 positional argument를 필요로 한다

~~~python
from django.db import models

class Topping(models.Model):
    # ...
    pass

class Pizza(models.Model):
    # ...
    toppings = models.ManyToManyField(Topping)
~~~

위의 예시코드에서, Topping은 여러개의 Pizza를 가질 수 있고 Pizza도 여러개의 Topping를 가질 수 있다

재귀적 관계와 아직 정의되지 않은 모델과의 관계를 만들 수 있다

**필수는 아니지만 ManyToMany 필드명은 복수형으로 사용하는 걸 권장한다**

 ManyToManyField가 어느 모델클래스에 있는 지 상관없지만 한 군데에서만 사용해야 한다

하지만 위의 예시처럼 피자가 여러개의 토핑을 가지는 게 더 자연스러운 흐름이기 때문에 "natual함"을 고려하는게 좋다

추가적으로 필수적이지 않은 argument를 더 가질 수 있다 - [ref](https://docs.djangoproject.com/en/1.11/ref/models/fields/#manytomany-arguments)



#### Extra fields on many-to-many relationships

피자와 토핑의 관계처럼 간단한 many-to-many필드를 사용할 때는 일반적인 ManyToManyField로 충분하다

하지만 가끔 두 모델사이 관계의 정보를 필요로 할 때가 있다.

예를 들면 사람과 그룹의 관계이다. person은 multiple group에 속할 수 있고 group에는 multiple person이 존재 할 수있다. 이러한 상황일때 추가적인 필드를 생성할 수 있다(intermediate model이라 부르는 것 같다). intermediate model은 **through** argument 를 사용함으로서 관계될 수 있다.

~~~python
from django.db import models

class Person(models.Model):
    name = models.CharField(max_length = 20)
    
class Group(models.Model):
    name = models.CharField(max_length = 20)
    members = models.ManyToManyField(Person, through = 'Membership')
    
class Membership(models.Model):
    person = models.ForeignKey(Person)
    group = models.ForeignKey(Group)
    date_joined = models.DateField()
    invite_reason = models.CharField(max_length = 64)
~~~

위 코드는 Person과 Group 두 모델이 어떻게 관련되어 있는지 알 수 있다

intermediate model을 정의할때 어떠한 모델들이 관련되어 있는지 명확하게 정의해야 한다

intermediate model에는 몇 가지 제한적인 점이 있다

* 오직 하나의 foreignkey 필드만 사용할 수 있거나 여러 foreignkey를 사용할 땐 through를 써야만 한다  
  **만약 through 없이 두 개 이상의 ForeignKey 필드가 있다면 Validation error이 발생한다**
* For a model which has a many-to-many relationship to itself through an intermediary model, two foreign keys to the same model are permitted, but they will be treated as the two (different) sides of the many-to-many relationship. If there are *more* than two foreign keys though, you must also specify `through_fields` as above, or a validation error will be raised.
* When defining a many-to-many relationship from a model to itself, using an intermediary model, you *must* use[`symmetrical=False`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.ManyToManyField.symmetrical) (see [the model field reference](https://docs.djangoproject.com/en/1.11/ref/models/fields/#manytomany-arguments))



**일반적인 many-to-many 필드와는 다르게 add() / create() / set() / remove() 을 사용할 수 없다**

하지만 clear() 메소드는 many-to-many 관계를 지우는데 사용될 수 있다

일단 intermediate model의 인스턴스를 생성함으로서 many-to-many  관계를 만들었다면, query를 사용할 있다

단지 일반적인 many-to-many 관계처럼 사용하면 된다

~~~python
Group.objects.filter(members__name__startswith = 'Paul')
~~~

intermediate model을 사용할때 또한 쿼리를 날릴 수 있다

~~~python
Person.objects.filter(
	group__name = "The Beatles",
    membership__date_joined__gt = date(1961, 1, 1)
)
~~~

membership의 정보에 접근하고 싶다면 Membership model에 직접적으로 쿼리를 하면 된다

~~~django
ringos_membership = Membership.objects.get(group = beatles, person = ringo)

ringos_membership.date_joined
ringos_membership.invite_reason
~~~



3. One-to-one relationships

one-to-one  관계를 정의하기 위해서 OneToOneField를 사용한다

한 오브젝트의 primary key를 확장하는 데에 가장 좋은 방법이다

OneToOneField 또한 positional argument 하나를 필요로 한다

예를 들어, address / phone_number 를 가지고 있는 place 데이터베이스를 만들었고, 그 위에 restaurant 를 만들고 싶다면 코드를 반복하는 대신에 OneToOneField를 사용하면 된다

ForeignKey 처럼, 재귀적 관계로 정의 가능하다

example - [참고](https://docs.djangoproject.com/en/1.11/topics/db/examples/one_to_one/)

~~~python
from django.db import models

class Place(models.Model):
    name = models.CharField(max_length = 50)
    address = models.CharField(max_length = 80)
    
class Restaurant(models.Model):
    place = models.OneToOneField(
        Place,
    	primary_key = True,    
  	)
    serves_hot_dogs = models.BooleanField(default = False)
    server_pizza = models.BooleanField(default = False)
    
class Waiter(models.Model):
    restaurant = models.ForeignKey(Restaurant)
    name = models.CharField(max_length = 50)
~~~

parent_link argument를 받을 수 있다  
과거에 OneToOneField는 자동적으로 primary key가 됐지만 지금은 아니다  
그러므로 지금은 여러개의 OneToOneField를 가지는 것이 가능하다

### Models across files

다른 app의 모델과 relate 할 수 있다. 이렇게 하기 위해서는 models.py의 맨 위에서 임포트를 해야 한다

~~~python
from django.db import models
from geograpy.models import Zipcode

class Restaurant(models.Model):
    zip_code = models.ForeignKey(
    	Zipcode,
        blank = True,
        null = True,
    )
~~~



### Field name restrictions

두 가지의 필드명 제한점이 있다

1. 파이썬 예약어는 사용될 수 없다. 파이썬 문법에러를 일으키기 때문이다
2. 필드명은 2개 이상의 언더스코어(_)를 가질 수 없다. 장고 쿼리 룩업 신택스 에러를 일으키기 때문이다

**필드명은 데이터베이스의 컬럼명과 일치할 필요는 없다** - [참고(db_column)](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.Field.db_column)

**join, where, select 와 같은SQL 예약어는 필드명으로 사용가능 하다.** 장고는 데이터베이스의 테이블명과 컬럼명을 자동으로 모두 이스케이프 하기 때문이다. 



### Custom field types

커스텀 필드를 만들 수 있다. [Write custom fields](https://docs.djangoproject.com/en/1.11/howto/custom-model-fields/)



### Meta options

Class meta 를 사용함으로서 메타데이터를 줄 수 있다

~~~python
from django.db import models

class Ox(models.Model):
	horn_length = models.IntegerField()
    
    class Meta:
        ordering = ['horn_length']
        verbose_name_plural = 'oxen'
~~~

모델의 메타데이터란 필드 이외의 모든 것들이라고 이해하면 된다 - 정렬옵션(ordering), 테이블명(db_table) 과 같은 -

 Class Meta는 필수 요소는 아니다

가능한 메타 옵션 리스트 - [Model option reference](https://docs.djangoproject.com/en/1.11/ref/models/options/)



### Model attributes

**모델의 가장 중요한 속성은 Model manager 이다**

모델 매니저는 모델의 인스턴스를 취득하고 데이터베이스 쿼리 오퍼레이션을 가능하게 만드는 중요한 속성이다. 커스텀되지 않았다면 기본 매니저명은 **objects**이다. **매니저는 오직 모델 클래스만을 통해서 사용 가능하다. 모델의 인스턴스는 매니저를 사용할 수 없다**



### Model methods

커스텀 메소드를 모델에 정의할 수 있다. Manager는 table-wide thing하게 사용되는 반면, 모델 메소드는 특정한 모델 인스턴스에서만 사용한다. 모델 메소드는 로직처리를 한 장소 - 모델 - 에서 처리할 수 있는 효과적인 방법이다

예를 들어, 아래와 같은 모델은 몇몇의 커스템 모델 메소드를 가진다

~~~python
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length = 20)
    last_name = models.CharField(max_length = 20)
    birth_date = models.DateField()
    
    def baby_boomer_status(self):
		"Returns the person's baby-boomer status."
        import datetime
        if self.birth_date < datetime.date(1945, 8, 1):
            return "pre-boomer"
        elif self.birth_date < datetime.date(1965, 1, 1):
            return "Baby boomer"
        else:
            return "Post boomer"
        
        
    @property
    def full_name(self):
        "Returns the person's full name."
        return "%s %s" % (self.first_name, self.last_name)
~~~

Property에 대한 설명은 [여기](https://docs.djangoproject.com/en/1.11/glossary/#term-property)에 있다

모델에 자동으로 생성되는 모델 메소드들이 있다. 아래에는 오버라이딩 해서 자주 사용되는 메소드들을 소개하겠다

* __str__

~~~python
def __str__(self):
    return self.first_name
~~~

어떠한 오브젝트이던 유니코드 representation을 리턴하는 파이썬 매직 메소드이다

파이썬과 장고는 모델 인스턴스가 plain string로써 보여질 필요가 있을때 \__str__ 메소드를 사용한다

보통 콘솔이나 어드민 페이지에서 모델 인스턴스를 표시하기 위해 사용한다

* get_absolute_url

~~~python
def get_absolute_url(self):
~~~

디테일한 오브젝트의 URL을 알아내기 위해 사용한다. 유일하게 식별할 수 있는 URL을 가진 모든 오브젝트는 get_absolute_url 메소드를 정의해야만 한다.



### Overriding predefined model methods

커스텀 하기 원하는 database behavior을 포함한 모델 메소드도 있다. 특히, **save()** / **delete()** 의 방식을 변경하곤 한다

~~~python
from django.db import models

class Blog(models.Model):
    name = models.CharField(max_length=100)
    tagline = models.TextField()
    
    def save(self, *args, **kwargs):
        do_something()
        super(Blog, self).save(*args, **kwargs) # 진짜 save 메소드를 호출한다
        do_something_else()
~~~

~~~python
from django.db import models

class Blog(models.Model):
    name = models.CharField(max_length=100)
    tagline = models.TextField()
    
    def save(self, *args, **kwargs):
        if self.name == "Yoko's blog":
            return # name이 Yoko's blog 라면 절대로 save 할 수 없다
        else:
            super(Blog, self).save(*args, **kwargs)
~~~

**super**의 사용은 굉장히 중요하다. 만약 super를 호출하지 않으면 위 와 같은 경우 save 가 작동하지 않을 것이다

(super를 사용하지 않으면 데이터베이스에도 접근하지 않고 아무런 기능도 하지 않을것이다)

또한 **\*args**와 **\**kwargs**의 사용도 매우 중요하다



>**오버라이딩된 모델 메소드는 bulk operation에 사용될 수 없다**



### Executing custom SQL

커스텀 SQL문을 작성하는 것 또한 일반적인 패턴이다. 더 자세한 raw SQL의 내용은 [여기](https://docs.djangoproject.com/en/1.11/topics/db/sql/)를 참고하자



###  Model inheritance 

모델상속은 파이썬의 클래스 상속과 비슷하다. 마찬가지로 django.db.models.Model 를 사용한다

장고에서 가능한 세 가지 스타일의 모델 상속이 있다.

1. 각각의 child model에서 정의하지 않기 위해서 사용하는 경우. 이러한 경우는 독립정으로 사용되지 않는다. 

   [Abstract base class](https://docs.djangoproject.com/en/1.11/topics/db/models/#abstract-base-classes)

2. 이미 존재하는 모델의 서브클래스 혹은 각각의 모델이 그 자신의 데이터베이스 테이블을 가지기 원할때 사용.

   [Multi-table inheritance](https://docs.djangoproject.com/en/1.11/topics/db/models/#multi-table-inheritance)    

3. 모델의 파이썬 레벨의 행동을 수정하고 싶을 때(어떠한 방식으로도 모델필드를 변경하지 않으면서)

[Proxy model](https://docs.djangoproject.com/en/1.11/topics/db/models/#proxy-models)



### Abstract base classes

Abstract base classes 는 common info를 여럿 모델에 적용하고 싶을때 유용하게 사용된다. base class를 정의하고 abstract = True 를 Class Meta:에 두면된다. **Abstract 모델은 데이터베이스 테이블을 생성하지 않는다**

대신에 다른 모델에 대한 base class로서 사용될 때 그것의 필드는 상속받는 클래스의 필드에 더해 질 것이다

abstract class와 상속받는 class가 같은 필드명을 가지고 있다면 에러를 발생시킨다

~~~python
from django.db import models

class CommonInfo(models.Model):
    name = models.CharField(max_length=100)
    age = models.PositiveIntegerField()
    
    class Meta:
        abstract = True
        
class Student(CommonInfo):
    home_group = models.CharField(max_length=5)
~~~

Student 모델은 name / age / home_group 세 필드를 가질 것이다. CommonInfo 모델은 abstract 가 True 이기때문에 장고의 일반적인 모델로서 사용되지 않는다. manager 와 table 을 생성하지 않는다. 또한 인스턴스를 생성할 수 없고 직접적으로 save를 할 수 없다.



### Meta inheritance

abstract base model이 생성되었을 때, 