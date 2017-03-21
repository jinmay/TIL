# Request and response objects



## HttpRequest objects

페이지 요청이 오면 Django는 요청에 대한 메타데이터를 포함하는 HttpRequest 객체를 생성한다. 그리고 적절한 view를 호출하며 첫 인자로 HttpRequest 를 보낸다. 각각의 view는 HttpRequst 객체를 리턴해야 한다.



####  Attributes

* HttpRequest.method

~~~python
if request.method == 'get':
    do_something()

elif request.method == 'post':
    do_other_thing()
~~~

* HttpRequest.GET

HTTP GET 파라미터로 주어진 자료형 dictionary 처럼 생긴 객체

* HttpRequest.POST

HTTP POST 파라미터로 주어진 자료형 dictionary 처럼 생긴 객체 (providing that the request contains form data)  

* HttpRequest.scheme
* HttpRequest.body
* HttpRequest.path
* HttpRequest.path_info
* HttpRequest.encoding
* HttpRequest.content_type
* HttpRequest.content_params
* HttpRequest.COOKIES
* HttpRequest.FILES
* HttpRequest.META
* HttpRequest.resolber_match





## QueryDict objects

In an HttpRequest object, the GET and POST attributes are instances of django.http.QueryDict, a dictionary-like class customized to deal with multiple values for the same key.

~~~python
>>> QueryDict('a=1&a=2&c=3')
<QueryDict: {'a': ['1', '2'], 'c': ['3']}>
~~~





## HttpResponse objects

Django에 의해서 자동으로 만들어지는 HttpRequest 객체와는 다르게, HttpResponse 객체는 사용자의 의지로 생성할 지 안할지 정할 수 있다. 

~~~python
from django.http import HttpResponse

httpresponse_object = HttpResponse('You can write strings')
~~~

#### Attributes

* HttpResponse.content
* HttpResponse.charset
* HttpResponse.status
* HttpResponse.reason_phrase
* HttpResponse.streaming
* HttpResponse.closed





## JsonResponse objects

Json response를 생성하는데 돕는 HttpResponse 의 서브클래스이다.