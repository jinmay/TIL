# Bulit-in template tags and filters

## Tags

* autoescape

자동으로 이스케이프를 해준다  
escape 필터를 일일이 넣어준 것과 같은 효과를 보인다

~~~django
{% autoescape on %}
	{{ body }}
{% endautoescape %}
~~~

* block

템플릿 상속에 필요한 블록을 설정할 수 있다

~~~django
{% block title %}Index{% endblock title %}
~~~

* comment

comment tag는 중첩될 수 없다

~~~django
{% comment %}
	comment line
{% endcomment %}
~~~

* csrf_token

form tag와 함께 자주 쓰이며 Cross Site Request Forgeries를 방어하기 위함이다

~~~django
{% csrf_token %}
~~~

* extends

따옴표와 함께 사용되면 부모 템플릿을 상속받기 위함이다  
또한 변수와도 같이 사용될 수 있다..

~~~django
{% extends 'base.html' %}

{% extends variables %}
~~~

* for

파이썬의 for와 같다

~~~django
{% for post in all_post %}
	{{ post.title }}
{% endfor %}
~~~

~~~django
{% for x, y in points %}
    There is a point at {{ x }},{{ y }}
{% endfor %}
~~~

dictionary의 key와 value를 같이 사용가능

~~~django
{% for key, value in data.items %}
    {{ key }}: {{ value }}
{% endfor %}
~~~

for loop set

| Variable            | Description                              |
| ------------------- | ---------------------------------------- |
| forloop.counter     | The current iteration of the loop (1-indexed) |
| forloop.counter0    | The current iteration of the loop (0-indexed) |
| forloop.revcounter  | The number of iterations from the end of the loop (1-indexed) |
| forloop.revcounter0 | The number of iterations from the end of the loop (0-indexed) |
| forloop.first       | True if this is the first time through the loop |
| forloop.last        | True if this is the last time through the loop |
| forloop.parentloop  | For nested loops, this is the loop surrounding the current one |

example

~~~django
<input type="radio" id="choice{{ forloop.counter }}" name="choice" value="{{ choice.id }}" />
<label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label>
~~~

input 태그의 id와 label 태그의 "choice{{ forloop.counter }}"가 일치할때만 생성이 된다.

* for … empty 

~~~django
{% for post in all_post %}
	<p>{{ post.title }}</p>
{% empty %}
	<p>There are not posts available</p>
{% endfor %}
~~~

* if

boolean 연산도 가능하다!

~~~django
{% if athlete_list %}
    Number of athletes: {{ athlete_list|length }}
{% elif athlete_in_locker_room_list %}
    Athletes should be out of the locker room soon!
{% else %}
    No athletes.
{% endif %}
~~~

같이 사용가능한 operator

1. ==
2. !=
3. '>'
4. '<'
5. '>='
6. '<='
7. in
8. not in
9. is

~~~django
{% if somevar is True %}
  This appears if and only if somevar is True.
{% endif %}

{% if somevar is None %}
  This appears if somevar is None, or if somevar is not found in the context.
{% endif %}
~~~

10. is not

filter 와 같이사용 하기도 한다

~~~django
{% if messages|length >= 100 %}
   You have lots of messages today!
{% endif %}
~~~

* include

extends와 비슷한거 같은데 혼동하지 말자!!

~~~django
{% include 'foo/bar.html' %}
~~~

템플릿을 로드하고 **현재의 context 변수를 가지고 렌더링한다**



<참고>

> Context: variable `person` is set to `"John"` and variable `greeting` is set to `"Hello"`.
>
> Template:
>
> ```django
> {% include "name_snippet.html" %}
> ```
>
> The `name_snippet.html` template:
>
> ```django
> {{ greeting }}, {{ person|default:"friend" }}!
> ```
>
> You can pass additional context to the template using keyword arguments:
>
> ```django
> {% include "name_snippet.html" with person="Jane" greeting="Hello" %}
> ```
>
> If you want to render the context only with the variables provided (or even no variables at all), use the `only` option. No other variables are available to the included template:
>
> ~~~django
> {% include "name_snippet.html" with greeting="Hi" only %}
> ~~~



* load

custom template tag를 로드한다

~~~django
# somelibrary의 모든 custom filter 와 tag 를 로드
# package에 위치해있는 otherlibrary를 로드
{% load somelibrary package.otherlibrary %}
~~~

또한 아래와 같이 개별적으로 불러올 수 있다

~~~django
# somelibrary의 foo 와 bar 만 로드
{% load foo bar from somelibrary %}
~~~

* now

현재 시각과 날짜를 주어지는 필터의 형태로 출력할 수 있다

~~~django
{% now "jS F Y H:i" %}
~~~

* url

~~~django
# url-name 주소로 post.id 인자를 넘김
{% url 'url-name' post.id %}
~~~

매칭되지 않을시 NoReverseMatch error가 발생한다

조금 더 간략하게 쓰면

~~~django
{% url 'some-url-name' arg arg2 as the_url %}

<a href="{{ the_url }}">I'm linking to {{ the_url }}</a>
~~~

도 가능한데, 이때 {% url %} 의  scope 가 중요하다!

as 로써 생성된 var 의 범위는 {% url %} 태그가 있는 {% block %}에서만 사용될 수 있다  
원문 :  The scope of the variable created by the  `as var` syntax is the `{% block %}` in which the `{% url %}` tag appears.



물론 

~~~django
{% url 'myapp:view-name' %}
~~~

처럼 namespace 를 사용하여 reverse 하는게 편하고 이해하기 쉬운 코드인거 같다

* with

임시저장한다고 보면 된다(caching)

~~~django
{% with total=business.employees.count %}
    {{ total }} employee{{ total|pluralize }}
{% endwith %}
~~~

total 변수는 {% with %} 태그 내에서만 사용 가능하다

~~~django
{% with business.employees.count as total %}
~~~

또는 이렇게 약칭으로 사용하기도 한다