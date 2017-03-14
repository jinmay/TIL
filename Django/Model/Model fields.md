# Model fields

#### example

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



#### Fields type

> Each field in your model should be an instance of the appropriate [`Field`](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.Field) class.

1. Column type (e.g. Integer, Text)
2. HTML Widget when rendering a form field (e.g. <input type="text">)
3. MInimal validations requirements





#### Field options

* null ( default = false )
* blank ( default = false )

>  [`null`](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.Field.null) is purely database-related, whereas [`blank`](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.Field.blank) is validation-related. If a field has [`blank=True`](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.Field.blank), form validation will allow entry of an empty value. If a field has [`blank=False`](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.Field.blank), the field will be required.

* choice

~~~python
from django.db import models

class Person(models.Model):
    SHIRT_SIZES = (
        ('S', 'Small'),
        ('M', 'Medium'),
        ('L', 'Large'),
    )
    name = models.CharField(max_length=60)
    shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)
~~~

​	first element = the value in database  
​	second element = the value that will be displayed

* help_text
* unique - [refer](https://docs.djangoproject.com/en/1.10/ref/models/fields/#common-model-field-options)
* primary_key
* verbose_name

~~~python
first_name = models.CharField("person's first name", max_length=30)
~~~





#### Relationships

1.  ForeignKey ( ManyToOne )

~~~python
from django.db import models

class Manufacturer(models.Model):
    # ...
    pass

class Car(models.Model):
    manufacturer = models.ForeignKey(Manufacturer, on_delete=models.CASCADE)
    # ...
~~~

2. ManyToMany

~~~python
from django.db import models

class Topping(models.Model):
    # ...
    pass

class Pizza(models.Model):
    # ...
    toppings = models.ManyToManyField(Topping)
~~~

> It doesn’t matter which model has the [`ManyToManyField`](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.ManyToManyField), but you should only put it in one of the models – not both.

3. OneToOne







### Meta

[Model option references](https://docs.djangoproject.com/en/1.10/ref/models/options/)

Give model metadata by using inner class class Meta

~~~python
from django.db import models

class Ox(models.Model):
    horn_length = models.IntegerField()

    class Meta:
        ordering = ["horn_length"]
        verbose_name_plural = "oxen"
~~~





#### Model method

> Model methods should act on a particular model instance

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

    def _get_full_name(self):
        "Returns the person's full name."
        return '%s %s' % (self.first_name, self.last_name)
    full_name = property(_get_full_name)
~~~





## Model Inheritance

* Abstract base classes

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

Student model has three fields : **name**, **age**, **home_group**  
And CommonInfo model doesn't generate a database table or have a manager



* Meta Inheritance

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

> If a child class does not declare its own [Meta](https://docs.djangoproject.com/en/1.10/topics/db/models/#meta-options) class, it will inherit the parent’s [Meta](https://docs.djangoproject.com/en/1.10/topics/db/models/#meta-options). If the child wants to extend the parent’s [Meta](https://docs.djangoproject.com/en/1.10/topics/db/models/#meta-options) class, it can subclass it

 



#### Organizing models in a package

> Remove `models.py` and create a `myapp/models/`directory with an `__init__.py` file and the files to store your models. You must import the models in the `__init__.py` file.



For example, if you had `organic.py` and `synthetic.py` in the `models` directory:

myapp/models/ ____init____.py

~~~python
from .organic import Person
from .synthetic import Robot
~~~

