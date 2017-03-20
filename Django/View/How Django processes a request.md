 # How Django processes a request

1. 일반적으로 settings.py의 **ROOT_URLCONF**에 정의된 값으로 결정되지만,  HttpRequest 객체가 (middleware에 의해 설정된) urlconf를 가지고 있다면 그것의 값으로 대체된다
2. Django는 파이썬 모듈과 urlpatterns를 로드한다  
   (urlpatterns 는 url() 인스턴스의 파이썬 리스트이다)
3. 각각의 URL 패턴을 매칭해보며 요청된 url과 매칭되는 첫 번째 것에서 멈춘다
4. url()를 실행
5. 매칭되는 url string 이 없다면 error를 발생시킴





~~~python
from django.conf.urls import url

from . import views

urlpatterns = [
  url(r'^(?P<bookmark_slug>[-\w]+)/$', views.BookmarkDV, name="detail"),
]
~~~

* leading slash 를 사용할 필요는 없음  
  ^/bookmark (X) | ^bookmark(O)
* url()의 **r**은 필수적으로 사용 될 필요는 없지만 사용하기를 매우 권장함  
  파이썬에게 **raw string**이라고 알리는 것이며 string 속에서 이스케이프 되는 것이 없게끔 만든다  
  (이스케이프 문자가 적용되지 않고 그대로 출력된다)





**captured arguments are always string**

예를 들어  

~~~python
url(r'^(?P<bookmark_slug>[-\w]+)/$', views.BookmarkDV, name="detail"),
~~~

와 같이 url pattern이 있다면 **<bookmark_slug>**라는 이름으로 BookmarkDV 뷰 함수로 넘어가게 되는데  
string 의 자료형으로 넘어간다  
(slug 가 숫자와 매칭되더라도 views로 넘어갈 땐 문자열로 넘어간다)



### Django provides tools for performing url reversing

* Django는 **url reversing**을 수행할 tool을 제공한다
  1. In tamplate, **url template tag**
  2. In python code, **reverse()** function
  3. In higher level code related to handling of URLs of Django model instances, get_absolute_url() method





