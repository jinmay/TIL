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

ForeignKey / ManyToManyField / OneToOneField 필드를 제외하고 각각의 필드 타입은 





* ​