# Templates overview

Django는 0개 또는 하나 이상의 template engine을 사용 할 수 있다  
장고의 기본 템플릿 엔진은 Django Template Languate(DTL)이라고 불리우며, 다른 인기있는 대안으로는 Jinja2 엔진이 있다 
또한 백엔드와 상관없이 템플릿은 로드하고 렌더하기 위한 standard API를 정의한다  
로딩은 템플릿을 찾는 것이고 렌더링은 템플릿 파일에 내용을 채우는 것을 의미한다



### Configuration

* 내장 템플릿 엔진

~~~python
django.template.backends.django.DjangoTemplates 
django.template.backends.jinja2.Jinja2
~~~

~~~python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            # ... some options here ...
        },
    },
]
~~~

**DIRS**는 장고가 우선적으로 찾아볼 템플릿 디렉토리들의 리스트  
**APP_DIRS에서 장고가 설치되어있는 app들의 템플릿 디렉토리를 검색할 것인지 설정할 수 있다



### Basic Django Template Language syntax

views.py 에서 context 변수(자료형 : dictionary)에 담겨져 템플릿 파일로 넘어온다

* 변수 사용법

~~~python
# views.py
context = {'first_name': 'John', 'last_name': 'Doe'}

# in template file
My first name is {{ first_name }} and last name is {{ last_name }}
~~~

* tag 사용법

~~~django
# {% 와 %} 로 감싸져야 한다
{% csrf_token %}
~~~

~~~django
# 인자를 받아들일 수 있는 태그
{% cycle 'odd' 'even' %}
~~~

~~~django
# 시작 태그와 끝 태그를 필요로 하는 경우
{% if user.is_authenticated %}Hello, {{ user.username }}.{% endif %}
~~~

* 필터

~~~django
# 인자를 필요로 하는 태그
{{ my_date|date:"Y-m-d" }}
~~~

~~~django

~~~
