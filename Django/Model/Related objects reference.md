# Related objects reference

* 1 : N 관계 - reporter.article_set 

~~~python
from django.db import models

class Reporter(models.Model):
    # ...
    pass

class Article(models.Model):
    reporter = models.ForeignKey(Reporter, on_delete=models.CASCADE)
~~~





* M : N 관계 

  1. pizza.toppings
  2. topping.pizza_set

  ~~~python
  class Topping(models.Model):
      # ...
      pass

  class Pizza(models.Model):
      toppings = models.ManyToManyField(Topping)
  ~~~

  ​