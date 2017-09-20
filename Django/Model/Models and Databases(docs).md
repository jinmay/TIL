# Models and Databases

각 모델은 하나의 DB 테이블과 매핑된다

기본: 

* django.db.models.Model의 파이썬 서브클래스이다
* 모델의 속성(attribute)는 데이터베이스 필드를 나타낸다
* 모델을 가지고, 장고는 자동으로 생성되는 database-access API 사용을 가능하게 해준다

예제

~~~python
from django.db import models

class Person(models.Model):
	first_name = models.CharField(max_length=30) 
    last_name = models.CharField(max_length=30)
~~~

first_name과 last_name은 Person 모델 클래스의 필드들이고 테이블의 컬럼이다

사용하는 데이터베이스 마다 다르겠지만 

~~~shell
CREATE TABLE myapp_person (
"id" serial NOT NULL PRIMARY KEY, "first_name" varchar(30) NOT NULL, "last_name" varchar(30) NOT NULL
);
~~~

와 같은 쿼리를 생성해 테이블을 만든다

주의점 : 

* myapp_person과 같은 테이블 명은 모델의 메타데이터로 부터 자동적으로 생성되지만, override 될 수 있다
* id필드 또한 자동으로 생성되며, override 가능
* 위 예시는 PostgreSQL 을 사용했다. 





## using model

모델을 정의했다면 장고에게 사용하겠다고 알려줘야 하는데, 모델명이 아닌 그 모델을 사용하는 app name을 적어주어야 한다

(정확히는 apps.py의 클래스? 를 넘겨주는 것이라고 한다. ex - myapp.apps.MyappConfig)

settings.py의 INSTALLED_APPS 에 추가하고 사용하도록 한다

~~~python
INSTALLED_APPS = [ 
#...
'myapp',
#...
]
~~~

추가했다면 마이그레이션 파일을 만들기 위해, python3 manage.py makemigrations myapp 을 실행할 수 있고 마이그레이션 반영을 위해 python3 manage.py migrate myapp 을 실행하면 된다

##### field

모델에서 유일하게 필수이면서 가장 중요한 것은 데이터베이스의 필드이다

필드는 클래스의 속성(attribute)이고, **clean, save, delete**와 같은 **model API**와 혼동되는 이름은 피하도록 한다

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

##### field type

각각의 필드는 적절한 Field class 의 인스턴스 이어야만 한다

장고는 아래의 몇가지를 정의하기 위해 필드 클래스 타입을 사용한다 : 

* 어떠한 데이터 타입을 저장할 지 정한다 - INTEGER / VARCHAR / TEXT
* form을 렌더링하기 위한 html widget - \<input type="text">
* 장고의 어드민에서 사용되는 최소한의 유효성 검사를 위해

장고는 빌트인 필드타입들을 가지고 있으며, 커스텀 필드도 생성 가능하다

##### field option

필드들은 특정한 argument을 필요로 하는 경우도 있다(예를들면 CharField()는 VARCHAR의 크기를 지정해야 하기 때문에 max_length라는 인자를 필요로한다)

그리고 모든 필드에서 사용 가능하며 optional한 argument도 있는데, 자주사용 되는 것들만 소개

* null - 기본값은 False이고, db에 값으로 NULL을 저장 (db관련이라고 생각)
* blank - 기본값은 False이고, form의 값으로 blank가 가능하다 (form validation 관련)

null과 blank는 엄연히 다른 것이니 혼동해서 사용하면 안된다

* choice - 기본 text 입력타입의 input이 아닌 선택지가 정해져있는 select box 가 기본 위젯이다

~~~python
YEAR_IN_SCHOOL_CHOICES = ( 
	('FR', 'Freshman'), 
	('SO', 'Sophomore'), 
	('JR', 'Junior'), 
	('SR', 'Senior'), 
	('GR', 'Graduate'), 
) 
~~~

튜플의 첫번 째 원소가 db에 저장되는 값이고, 두번 째 원소는 사용자에게 보여지는 값이다

~~~python
from django.db import models
class Person(models.Model): SHIRT_SIZES = (
        ('S', 'Small'),
        ('M', 'Medium'),
        ('L', 'Large'),
)
name = models.CharField(max_length=60)
shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)
~~~

와 같이 사용하면 된다.

튜플의 두번 째 원소처럼 보여지는 값을 출력하고 싶다면 **get_fieldname_display()** 메서드를 사용하면 된다

~~~python
p = Person(name="Fred Flintstone", shirt_size="L")
p.save()

p.shirt_size # "L"
p.get_shirt_size_display() # "Large"
~~~

* default - 기본값을 정해준다, 값을 지정할 수도 있고 호출가능한 객체를 지정할 수도 있다. 만약 호출가능한 객체가 지정된다면 새로운 모델 객체가 생설될 때마다  호출될 것이다
* help_text - form 위젯에 출력될 help text 이다. 해당필드가 폼에 사용되지 않더라도 documentation 측면에서 좋은 습관일 것이다
* primary_key - True 라면 해당 필드가 pk가 된다. integer인 id필드에 자동적으로 적용이 되며, pk가 되는 필드를 바꾸는 것이 아니라면 수정할 필요는 없다

**기본적으로 primary_key 필드는 읽기전용이다.** 만약 이미 존재하고 있는 오브젝트의 pk값을 변경했다면 새로운 오브젝트가 생성 될 것이다

~~~python
from django.db import models

class Fruit(models.Model):
	name = models.CharField(max_length=100, primary_key=True)
~~~

~~~python
>>> fruit = Fruit.objects.create(name='Apple') 
>>> fruit.name = 'Pear'
>>> fruit.save()
>>> Fruit.objects.values_list('name', flat=True) ['Apple', 'Pear']
~~~

* unique - True 라면, 테이블 통틀어서 값이 유니크해야한다

위에서 제시한 옵션을 포함해서 다양한 것들이 있으니 official docs를 꼭 참고하자



##### automatic primary key field

~~~python
id = models.AutoField(primary_key=True)
~~~

장고는 기본적으로 위와 같은 필드를 제공한다. 자동으로 증가하는 pk값을 가진 id 필드이다

특정한 필드를 pk로 만들고 싶다면 해당 필드의 옵션에 **primary_key=True**를 지정하면 되는데, 이때에 id 필드는 생성되지 않는다

##### verbose field name

**ForeignKey, ManyToManyField, OneToOneField**를 제외하고 모든 필드는 **first positional argument**를 가진다.

verbose_name이 주어지지 않는다면 자동적으로 필드명을 가지고 별칭을 만든다(_가 있다면 빈칸으로 바꾼다)

~~~python
# verbose_name은 "person's first name" 으로 변경된다
first_name = models.CharField("person's first name", max_length=30)

# verbose_name은 "first name" 이다
first_name = models.CharField(max_length=30)
~~~

**ForeignKey, ManyToManyField, OneToOneField**의 첫번째 인자로는 **model class**가 와야하기 때문에 verbose_name을 지정하기 위해선 keyword argument를 사용해야 한다

장고가 알아서 바꿔주기 때문에 관례로 첫 글자를 대문자로 적지 않는다

~~~python
poll = models.ForeignKey(
  Poll,
  on_delete=models.CASCADE,
  verbose_name="the related poll", 
)

sites = models.ManyToManyField(Site, verbose_name="list of sites") 

place = models.OneToOneField
(
  Place, 
  on_delete=models.CASCADE, 
  verbose_name="related place",
)
~~~



##### relationship

**ForeignKey, ManyToManyField, OneToOneField**를 통해 일대다 / 다대다 / 일대일 관계를 정의할 수 있다

##### many-to-one relationships

일대다 관계를 설정하기 위해서 **django.db.models.ForeignKey**를 사용하고,  first positional argument로 관계맺을 모델의 클래스명을 적어준다

~~~python
from django.db import models

class Manufacturer(models.Model): 
  # ...
  pass

class Car(models.Model):
  manufacturer = models.ForeignKey(Manufacturer, on_delete=models.CASCADE) 
  # ...
~~~

위의 예시 코드를 보면, Manufacturer와 Car은 1:N 관계이다

Car 모델이 Manufacturer 모델을 가지고 있는데 - 이것은 Manufacturer 이 여러개의 Car을 가질수 있고 반대로 Car은 하나의 Manufacturer 만을 가질 수 있음을 의미한다

또한 재귀적인 관계(스스로를 포함하는)와 아직 정의되지 않은 모델과의 관계도 정의할 수 있다

**필수는 아니지만 ForeignKey 필드의 이름은 해당 모델 클래스 이름의 소문자로 짓는 것을 추천한다**

~~~python
# 아래와 같이 ForeignKey 필드명을 지을수도 있지만
# manufacturer 처럼 짓는 걸 추천
class Car(models.Model):
company_that_makes_it = models.ForeignKey(
        Manufacturer,
		on_delete=models.CASCADE, )
# ...
~~~



##### many-to-many relationships

django.db.models.ManyToManyField 사용

~~~python
from django.db import models

class Topping(models.Model): 
  # ...
  pass

class Pizza(models.Model): 
  # ...
  toppings = models.ManyToManyField(Topping)
~~~

Topping 과 Pizza 는 N:M 관계이다.ManyToOneField과 마찬가지로 재귀적인 관계, 정의되지 않은 테이블과의 관계도 설정가능

또한 관습적으로 복수형으로 필드이름을 짓는다

다대다관계일 경우 Field는 어느쪽에 있어도 상관없지만 최대한 자연스러운 방향으로 설정하는 것이 좋다.  
**피자와 토핑의 예시로, 토핑이 피자에 속해있는 것이 더 자연스러운 생각이기 때문에 Pizza class에 toppings 필드를 두는 것이 자연스럽다.** 결과로 pizza form은 toppings select box 를 출력하게 된다



##### extra fields on many-to-many relationships (수정 필요)

조금 더 복잡한 경우가 있다

~~~python
from django.db import models 

class Person(models.Model):
	name = models.CharField(max_length=128)
    
	def __str__(self): 
		return self.name
      
class Group(models.Model):
	name = models.CharField(max_length=128)
	members = models.ManyToManyField(Person, through='Membership')
    
	def __str__(self): 
		return self.name
      
class Membership(models.Model):
    person = models.ForeignKey(Person, on_delete=models.CASCADE) 
    group = models.ForeignKey(Group, on_delete=models.CASCADE) 
    date_joined = models.DateField()
    invite_reason = models.CharField(max_length=64)
~~~

**ManyToManyField의 옵션에 through를 줌으로써 intermediate model을 생성할 수 있다**

intermediate model에 ForeignKey 필드 이외에 extra fields를 정의할 수 있다

intermediate model의 제한점 :

* 단 하나의 FK 또는 ManyToManyField.throuth_fields 를 통해 장고가 사용할 FK를 명시해주어야만 한다

만약 하나 이상의 또는 명시된 FK가 없다면 ValidationError이 발생한다

~~~python
>>> ringo = Person.objects.create(name="Ringo Starr") 
>>> paul = Person.objects.create(name="Paul McCartney") 
>>> beatles = Group.objects.create(name="The Beatles") 
>>> m1 = Membership(person=ringo, group=beatles,
	... date_joined=date(1962, 8, 16),
	... invite_reason="Needed a new drummer.")
>>> m1.save()
>>> beatles.members.all()
<QuerySet [<Person: Ringo Starr>]>
>>> ringo.group_set.all()
<QuerySet [<Group: The Beatles>]>
>>> m2 = Membership.objects.create(person=paul, group=beatles,
	... date_joined=date(1960, 8, 1),
	... invite_reason="Wanted to form a band.")
>>> beatles.members.all()
<QuerySet [<Person: Ringo Starr>, <Person: Paul McCartney>]>
~~~

일반적인 many-to-many 와는 다르게 **add(), create(), set()을 사용할 수 없다**

~~~python
>>> # The following statements will not work
>>> beatles.members.add(john)
>>> beatles.members.create(name="George Harrison") 
>>> beatles.members.set([john, paul, ringo, george])
~~~



##### one-to-one relationships

장고가 기본적으로 제공하는 accounts 앱과 Profile 앱의 관계를 생각하면 된다



##### models across files

다른 앱의 모델도 사용할 수 있다. 

~~~python
from django.db import models
from geography.models import ZipCode # 경로 설정에 유의하자

class Restaurant(models.Model): 
  # ...
  zip_code = models.ForeignKey( ZipCode,
    on_delete=models.SET_NULL, blank=True,
    null=True,
)
~~~



##### field name restrictions

* 파이썬의 예약어 사용 불가 - SyntaxError raise
* 두 개 이상의 언더스코어(_) 사용 불가 - 장고의 쿼리 lookup syntax이기 때문

장고가 테이블명과 필드명을 자동으로 이스케이핑 하기 때문에 join, where, select 와 같은 SQL 예약어는 사용 가능하다. 



#### Custom field types

존재하고 있는 모델 필드가 사용목적에 잘 맞지 않다던가 덜 일반적인 db 컬럼타입을 사용하고 싶을때 커스텀 필드를 만들 수 있다. 

##### Meta options

모델 클래스 내에 **Meta class**를 정의한다

~~~python
from django.db import models

class Ox(models.Model):
  horn_length = models.IntegerField()
  
  class Meta:
    ordering = ["horn_length"]
    verbose_name_plural = "oxen"
~~~

해당 테이블에 대한 부가정보를 저장하며 필수요소는 아니다



##### model attributes

모델에서 가장 중요한 것은 **Manager**이다. 데이터베이스 쿼리 수행일 할 수 있는 인터페이스를 제공하며 db로 부터 인스턴스를 가져온다. 커스텀 매니져를 사용하는 것이 아니라면 기본 매니져의 이름은 **objects**이고, **모델의 인스턴스가 아니라 모델 클래스만을 통해서 접근할 수 있다**

##### model methods

특정한 모델 인스턴스에서 사용할 수 있는 모델 메서드를 정의할 수 있다

비즈니스 로직을 한 곳(모델)에서 관리할 수 있는 좋은 테크닉이다 

~~~python
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
          
    @property
    def full_name(self):
	    "Returns the person's full name."
    	return '%s %s' % (self.first_name, self.last_name)
~~~

각 모델에 기본으로 생성되는 메서드들이 있는데 override 가능하다. 그 중에서 거의 항상 사용되는 두 가지가 있다 : 

* \__self__()

모델 인스턴스가 문자열로 표현되어야 할때 사용한다. 주로 콘솔이나 어드민에서 자주 사용된다

* get_absolute_url

장고에게  특정 object의 URL을 어떻게 계산할 지 말해준다. 



##### overriding predefined model methods

커스텀하고 싶은 일련의 데이터베이스 수행을 요약한 모델 메서드 셋이 있으며, 특히 **save()와 delete()** 메서드를 재정의하는 빈도가 높다. 빌트인 메서드를 재정의하는 클래식한 케이스로는 특정 오브젝트를 저장할 때 추가 행동들을 덧붙이는 것이다. 

~~~python
from django.db import models

class Blog(models.Model):
  name = models.CharField(max_length=100)
  tagline = models.TextField()
  
  def save(self, *args, **kwargs):
    do_something()
	super(Blog, self).save(*args, **kwargs)  # Call the "real" save() method. 
    do_something_else()
~~~

~~~python
from django.db import models

class Blog(models.Model):
	name = models.CharField(max_length=100) 
    tagline = models.TextField()
	
    def save(self, *args, **kwargs):
		if self.name == "Yoko Ono's blog":
			return # blog name이 "Yoko Ono's blog"인 경우 save() 호출불가
        else:
			super(Blog, self).save(*args, **kwargs) # Call the "real" save() method.
~~~

**super()를 호출하는 것은 매우 중요하다.** - 원래의 save 함수를 호출하지 않으면 작동하지 않기 때문이다

또한 **인자를 넘겨주는 것도 중요한데** **\*args / \*\*kwargs 를 사용하게 되면 문제없이 함수를 정의할 수 있다** 



##### executing custom SQL

모델과 모듈 레벨의 메서드에 커스텀 SQL문을 작성할 수 있다



##### model inheritance

장고에서의 모델 상속은 파이썬에서의 일반적인 클래스 상속처럼 작동한다. 

parent  model이 child model을 통해추상적으로 공통된 정보를 가진 모델인지, 아니면 일반 모델과 같은지 생각해보아야 한다

장고에는 세 가지 스타일의 모델 상속이 있다 : 

* Abstract base model 
* multi-table inheritance
* proxy model



##### abstract base classes

abstract base class는 공통되는 정보를 여러 모델에 입력해야 할 때 유용하다. 필드를 정의하고 **class Meta에 abstract=True** 라고 정의해주면 된다. 그렇다면 이 모델은 어떤 데이터베이스 테이블도 생성하지 않게 된다. 대신에, 다른 모델의 base 모델로서 사용될 때, abstract model의 필드들은 child model의 필드에 더해지게 된다. 이 때에 child model과 abstract model에 같은 이름의 필드가 있다면 에러가 발생하게 된다(장고는 예외를 발생시킨다)

~~~python
from django.db import models

class CommonInfo(models. Model):
  name = models.CharField(max_length=100) 
  age = models.PositiveIntegerField()
  
  class Meta:
    abstract = True
    
class Student(CommonInfo):
  home_group = models.CharField(max_length=5)
~~~

위의 예시에서 Student 모델은 세 개의 필드를 가지게 된다: name / age / home_group

CommonInfo 모델은 Class Meta의 abstract = True로 인해 장고의  일반적인 모델로서 사용되지 않는다 - 어떠한 데이터베이스 테이블도 만들지 않고, 모델 매니저도 없고, 인스턴스도 생성할 수 없다.



##### Meta inheritance

만약 child 모델에서 Class Meta를 정의하지 않는다면 parent 모델의 Meta를 자동으로 상속하게 된다. parent 모델의 Meta를 상속받음과 동시에 확장하고 싶다면 **Class Meta를 상속받아야 한다**

~~~python
from django.db import models

class CommonInfo(models.Model):
  # ...
  class Meta:
    abstract = True
    ordering = ['name']
    
class Student(CommonInfo):
  # ...
  class Meta(CommonInfo.Meta):
    db_table = 'student_info'
~~~

**abstract 모델의 child 모델은 자동적으로 abstract 되지 않는다.** 물론 abstract 모델로부터 상속받은 모델을 abstract 모델로 만들 수는 있다. **중요한건 abstract 모델로 만들고 싶다면 꼭 Meta class에서 abstract = True를 명시적으로 적어주어야 한다는 것이다**



##### Be careful with 'related_name' and 'related_query_name'

ForeignKey와 ManyToManyField에서 'related_name'과 'related_query_name'을 이용할때에 **명확한 reverse name**또는 **명확한 query name**을 적어주어야 한다. 

~~~python
from django.db import models

class Base(models.Model):
m2m = models.ManyToManyField(
				OtherModel, related_name="%(app_label)s_%(class)s_related", 			
  				related_query_name="%(app_label)s_%(class)ss",
				)

class Meta: 
  abstract = True
  
class ChildA(Base):
  pass

class ChildB(Base):
  pass
~~~



##### multi-table inheritance

모델 상속의 두 번째 타입은

~~~python
from django.db import models

class Place(models.Model):
	name = models.CharField(max_length=50) 
	address = models.CharField(max_length=80)
    
class Restaurant(Place):
	serves_hot_dogs = models.BooleanField(default=False) 
	serves_pizza = models.BooleanField(default=False)
~~~

Restaurant에 자동적으로 OneToOneField()가 아래와 같이 생성이 되며 관계를 가지게 된다

~~~python
place_ptr = models.OneToOneField( 
  	Place, 
  	on_delete=models.CASCADE, 
  	parent_link=True,
)
~~~



비록 데이터가 Restaurant에 있다고 하더라도 Place의 내용을 접근할 수 있다

~~~python
Place.objects.filter(name='example')
Restaurant.objects.filter(name='example') # Restaurant가 Place를 상속받고 있기 때문에 가능
~~~

모델이름을 소문자로 적음으로써 Place 객체로부터 Restaurant 객체를 얻을 수 있다

~~~python
p = Place.objects.first()
p.restaurant
~~~



##### meta and multi-table inheritance

multi-table 상속관계에서, child 클래스의 Meta 를 부모로부터 상속받는 것은 적합하지 않다. 모든 메타 클래스의 속성들은 이미 부모 클래스에 적용되어 있고, 그것들을 다시 정의하는 것은 일반적으로 모순되는 행동이다. (반대로 abtract base class 에서는 사용된다)

그렇기 때문에 child model은 부모의 모델의 meta에 접근해서는 안된다. 그러나 아래와 같은 이유로 인해서 일반적이지는 않지만 상속받는 경우가 있다

~~~python
class ChildModel(ParentModel):
  pass 
  class Meta:
	# 부모 모델의 정렬효과를 무효화 한다
    ordering = []
~~~

##### inheritance and reverse relations

멀티테이블 상속은 부모와 자식 모델을 연결하기 위해 암묵적인 OneToOneField를 사용하기 때문에, 

##### specifying the parent link field

장고는 child 모델을 non-abstract 모델과 연결하기 위해 내부적으로 OneToOneField를 사용하게 된다. child 모델에 부모 모델과 연결할때 사용되는 onetoone 필드가 생성되며 이름은 자동적으로 지어지게 되는데 **parent_link=True 옵션을 사용한 OneToOneField 를 이용함으로써 필드명을 지정할 수 있다**

##### proxy model

멀티테이블 상속을 하게 될때 새로운 테이블이 생성된다. 이것은 서브클래스가 부모클래스에 없는 데이터 필드를 저장할 장소를 필요로 하기 때문이다. 

> 원래 모델의 프록시 모델을 만든다고 생각하면 될 것 같다. 

프록시 모델의 인스턴스를 생성, 삭제, 수정할 수 있으며 마치 원래 모델을 이용하는 것처럼 데이터는 저장될 것이다. 두 모델의 차이점은 원래 모델의 변경 없이 추가적으로 정렬한다던가 기본 매니저를 변경한다던가 할 수 있다는 것이다. 

~~~python
from django.db import models

class Person(models.Model):
	first_name = models.CharField(max_length=30) 
    last_name = models.CharField(max_length=30)
    
class MyPerson(Person): 
  class Meta:
	proxy = True
	
    def do_something(self): 
      	# ...
		pass
~~~

프록시 모델은 Meta 클래스에 **proxy=True**를 정의함으로써 만들 수 있다. 위 의 예시에서는 새로운 메소드를 추가하였다.

MyPerson 모델은 부모 모델인 Person 과 똑같이 행동한다. Person의 인스턴스에 접근하는 것처럼 MyPerson의 인스턴스에도 엑세스할 수 있다.

~~~python
p = Person.object.first()
mp = MyPerson.object.first()
~~~

또한 기존의 정렬을 바꾸기 위해서 프록시 모델을 사용하기도 한다. 

~~~python
class OrderedPerson(Person):
  class Meta:
    ordering = ['first_name']
    proxy = True
~~~

##### querysets still return the model that was requested

Person 모델에 대한 쿼리셋은 그 모델(Person)의 타입을 리턴한다. (Person 모델에 대해 쿼리를 생성했을때 절대로 MyPerson 모델 오브젝트를 리턴할 수 없다)

##### base class restrictions(수정)

프록시 모델은 반드시 non-abstract 모델로부터 상속을 받아야 한다. 프록시 모델이 다른 데이터베이스 테이블의 행 사이에서 어떠한 관계도 찾을 수 없을때 multiple non-abstract 모델로부터 상속 받을 수 없다.

##### proxy model manager

프록시 모델에 대한 어떠한 매니저도 정의하지 않았다면 부모 모델로부터 모델 매니저를 상속받는다. 만약 프록시 모델에서 모델 매니저를 정의했다면 부모 모델에서 정의된 매니저가 있을지라도 새로 정의된 매니저가 디폴트 매니저로서 역할을 수행한다

~~~python
from django.db import models 

class NewManager(models.Manager):
  # ...
  pass

class MyPerson(Person):
	objects = NewManager()
   	class Meta: 
      proxy = True
~~~

##### differences between proxy inheritance and unmanaged model

