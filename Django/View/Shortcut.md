# Shortcut

* render()

인자로 주어진 템플릿과 dictionary 형태의 context를 결합하고 HttpResponse 객체를 리턴한다  

~~~python
return render(request, 'poll/index.html', context)
~~~

> Required arguments
>
> * request
> * template_name

> Optional arguments
>
> * context 
>
>   템플릿 파일로 넘겨줄 변수이며, dictionary 형식이어야 한다
>
> * content_type
>
> * status
>
>   The status code for the response. (Default = 200)
>
> * using
>
>   template name to use





* render_to_response()

미래에 없어질 것 같으니 사용하지 않는 걸 추천



* redirect()

HttpResponseRedirect 를 리턴한다

> 인자가 될 수 있는 항목들 :
>
> 	1. Model : model's get_absolute_url() will be called
> 	2. reverse()
> 	3. ….

~~~python
from django.shortcuts import redirect

def my_view(request):
    ...
    object = MyModel.objects.get(...)
    
    # 리다이렉트를 하기 위해서 MyModel 모델의 객체 'object'의 get_absolute_url 이 호출된다
    return redirect(object)
~~~





* get_object_or_404()

get 메소드를 통해 **단 하나의 레코드를 찾거나** 조건에 매칭되지 않을때 DoseNotExist 익셉션 대신 **Http404 를 발생한다**

> arguments
>
> 1. 모델, manager, QuerySet 인스턴스
> 2. 조건

~~~python
from django.shortcut import get_object_or_404

def my_view(request):
	 try:
        my_object = MyModel.objects.get(pk=1)
     except MyModel.DoesNotExist:
        raise Http404("No MyModel matches the given query.")
~~~

일반적으로 첫 번째 인자로써 Model 이 가장 빈도가 높지만, **QuerySet instance** 또한 사용될 수 있는 걸 잘 알아두자!!!

~~~python
queryset = Book.objects.filter(title__startswith='M')
get_object_or_404(queryset, pk=1)

# 한 줄로 쓰면
get_object_or_404(Book, title__startswith='M', pk=1)

# Model.manager 또한 사용 가능!!!
get_object_or_404(Book.dahl_objects, title='Matilda')

# related manager도 사용가능!!!
author = Author.objects.get(name='Roald Dahl')
get_object_or_404(author.book_set, title='Matilda')
~~~



* get_list_or_404()

filter() 의 결과로써 **list 를 리턴하거나 ** 조건에 매칭되지 않을때 빈 리스트 대신 **Http404를 발생시킨다**

>  arguments
>
> 1. 모델, manager, QuerySet 인스턴스
> 2. 조건

~~~python
from django.shortcuts import get_list_or_404

def my_view(request):
    my_objects = get_list_or_404(MyModel, published=True)
~~~



