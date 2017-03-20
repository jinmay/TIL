# View functions

**Web request**와 **Web response**를 처리한다



Returning errors

~~~python
from django.http import HttpResponse

def my_view(request):
    # ...

    # Return a "created" (201) response code.
    return HttpResponse(status=201)
~~~

모든  HTTP response code가 일반적이진 않기때문에 HttpResponse 의 subclass 로써 제공되는 것에는 한계가 있다  
하지만 status 를 통해 각 상황에 맞는 HttpResponse를 할 수 있다.



### Http 404 exception

404는 매우 흔하기 때문에 편리한 exception으로 제공된다

~~~python
from django.http import Http404
from django.shortcuts import render
from polls.models import Poll

def detail(request, poll_id):
    try:
        p = Poll.objects.get(pk=poll_id)
    except Poll.DoesNotExist:
        # DEBUG = True 일때 아래와 같은 404메세지 출력
        raise Http404("Poll does not exist")
    return render(request, 'polls/detail.html', {'poll': p})
~~~

장고가 404를 리턴할때 커스터마이징 된 페이지를 출력하고 싶으면 HTML Template 을 **404.html** 라는 이름으로 생성한다  
저장 경로는 템플릿 트리의 최상위에 위치시키며, **DEBUG = False** 일때 실행된다.





DEBUG = True 일때 인자로 넘겨진 메세지가 출력된다. 일반적으로 이러한 메세지는 production level 로는 옳지 않으며 디버깅을 목적으로 사용되는 거 같다



