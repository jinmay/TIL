# 모델 매니저

model manager란? 장고의 ORM을 통해서 모델에 쿼리를 날라게 되는데 이때 model manager라는 DB와 연동하는 인터페이스를 통해서 수행하게 된다

각 모델 클래스에 대한 기본 모델 매니저를 제공하며, 커스텀할 수도 있다.

**커스텀 모델 매니저를 만들어보자**

~~~python
from django import db

class PublishedManager(models.Manager):
    
    use_for_related_fields = True
    
    def published(self, **kwargs):
        return self.filter(pub_date__lte = timezone.now(), **kwargs)
    
class FlavorReview(models.Model):
    review = models.CharField(max_length = 100)
    pub_date = models.DateTimeField()
    
	# 커스텀 모델 매니저 추가
    objects = PublishedManager()
~~~

~~~python
# 사용법
FlavorReview.objects.count()
FlavorReview.objects.published().count()
~~~

