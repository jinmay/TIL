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



## Filters

* add

~~~django
{{ value | add:'2'}}
~~~

* addslash
* capfirst

첫 글자를 대문자화 한다

~~~django
{{ value | capfirst }}
# django => Django
~~~

* center


주어진 width(폭)에서 중앙정렬을 한다


~~~python
{{ value | center:'15' }}
# '        django        '
~~~

* ljust

좌측정렬

~~~django
{{ value | ljust: '10' }}
~~~

* rjust

우측정렬

~~~django
{{ value | rjust : '10' }}
~~~

* cut

인자로 들어오는 문자를 제거한다

~~~python
{{ value | cut:' ' }}
# value = 'First Second Third" -> 'FirstSecondThird'
~~~

* **date**

자주 사용되는 필터중에 하나인거 같다

[공식문서](https://docs.djangoproject.com/en/1.10/ref/templates/builtins/#date)를 참조하자

~~~django
{{ value | date:"Y d m"}}
~~~

| Format character | Description                              | Example output                           |
| ---------------- | ---------------------------------------- | ---------------------------------------- |
| a                | `'a.m.'` or `'p.m.'` (Note that this is slightly different than PHP’s output, because this includes periods to match Associated Press style.) | `'a.m.'`                                 |
| A                | `'AM'` or `'PM'`.                        | `'AM'`                                   |
| b                | Month, textual, 3 letters, lowercase.    | `'jan'`                                  |
| B                | Not implemented.                         |                                          |
| c                | ISO 8601 format. (Note: unlike others formatters, such as “Z”, “O” or “r”, the “c” formatter will not add timezone offset if value is a naive datetime (see [`datetime.tzinfo`](https://docs.python.org/3/library/datetime.html#datetime.tzinfo)). | `2008-01-02T10:30:00.000123+02:00`, or `2008-01-02T10:30:00.000123` if the datetime is naive |
| d                | Day of the month, 2 digits with leading zeros. | `'01'` to `'31'`                         |
| D                | Day of the week, textual, 3 letters.     | `'Fri'`                                  |
| e                | Timezone name. Could be in any format, or might return an empty string, depending on the datetime. | `''`, `'GMT'`, `'-500'`, `'US/Eastern'`, etc. |
| E                | Month, locale specific alternative representation usually used for long date representation. | `'listopada'` (for Polish locale, as opposed to `'Listopad'`) |
| f                | Time, in 12-hour hours and minutes, with minutes left off if they’re zero. Proprietary extension. | `'1'`, `'1:30'`                          |
| F                | Month, textual, long.                    | `'January'`                              |
| g                | Hour, 12-hour format without leading zeros. | `'1'` to `'12'`                          |
| G                | Hour, 24-hour format without leading zeros. | `'0'` to `'23'`                          |
| h                | Hour, 12-hour format.                    | `'01'` to `'12'`                         |
| H                | Hour, 24-hour format.                    | `'00'` to `'23'`                         |
| i                | Minutes.                                 | `'00'` to `'59'`                         |
| I                | Daylight Savings Time, whether it’s in effect or not. | `'1'` or `'0'`                           |
| j                | Day of the month without leading zeros.  | `'1'` to `'31'`                          |
| l                | Day of the week, textual, long.          | `'Friday'`                               |
| L                | Boolean for whether it’s a leap year.    | `True` or `False`                        |
| m                | Month, 2 digits with leading zeros.      | `'01'` to `'12'`                         |
| M                | Month, textual, 3 letters.               | `'Jan'`                                  |
| n                | Month without leading zeros.             | `'1'` to `'12'`                          |
| N                | Month abbreviation in Associated Press style. Proprietary extension. | `'Jan.'`, `'Feb.'`, `'March'`, `'May'`   |
| o                | ISO-8601 week-numbering year, corresponding to the ISO-8601 week number (W) which uses leap weeks. See Y for the more common year format. | `'1999'`                                 |
| O                | Difference to Greenwich time in hours.   | `'+0200'`                                |
| P                | Time, in 12-hour hours, minutes and ‘a.m.’/’p.m.’, with minutes left off if they’re zero and the special-case strings ‘midnight’ and ‘noon’ if appropriate. Proprietary extension. | `'1 a.m.'`, `'1:30 p.m.'`, `'midnight'`, `'noon'`, `'12:30 p.m.'` |
| r                | [**RFC 5322**](https://tools.ietf.org/html/rfc5322.html) formatted date. | `'Thu, 21 Dec 2000 16:01:07 +0200'`      |
| s                | Seconds, 2 digits with leading zeros.    | `'00'` to `'59'`                         |
| S                | English ordinal suffix for day of the month, 2 characters. | `'st'`, `'nd'`, `'rd'` or `'th'`         |
| t                | Number of days in the given month.       | `28` to `31`                             |
| T                | Time zone of this machine.               | `'EST'`, `'MDT'`                         |
| u                | Microseconds.                            | `000000` to `999999`                     |
| U                | Seconds since the Unix Epoch (January 1 1970 00:00:00 UTC). |                                          |
| w                | Day of the week, digits without leading zeros. | `'0'` (Sunday) to `'6'` (Saturday)       |
| W                | ISO-8601 week number of year, with weeks starting on Monday. | `1`, `53`                                |
| y                | Year, 2 digits.                          | `'99'`                                   |
| Y                | Year, 4 digits.                          | `'1999'`                                 |
| z                | Day of the year.                         | `0` to `365`                             |
| Z                | Time zone offset in seconds. The offset for timezones west of UTC is always negative, and for those east of UTC is always positive. | `-43200` to `43200`                      |

* default

값이 false 일때 주어진 인자를 기본값으로 사용한다

~~~django
{{ value | default: 'nothing' }}
~~~

* default_if_none

값이 none일때 주어진 인자를 기본값으로 한다

~~~django
{{ value | default_if_none : 'nothing' }}
~~~

* escape

이스케이프 문자

~~~django
< is converted to &lt;
> is converted to &gt;
' (single quote) is converted to &#39;
" (double quote) is converted to &quot;
& is converted to &amp;
~~~

~~~django
# autoescape가 꺼져있을때 따로 이스케이프 하는 법
{% autoescape off %}
    {{ title|escape }}
{% endautoescape %}
~~~

* escapejs

자바스크립트를 이스케이프 해준다

* filesizeformat
* first

첫 번재 인덱스 값을 출력

~~~django
{{ list | first }}
~~~

* last

마지막 인덱스의 값을 출력

~~~django
{{ list | last }}
~~~

* floatformat

[공식문서](https://docs.djangoproject.com/en/1.10/ref/templates/builtins/#floatformat)참고

* get_digit
* join

파이썬 문법 : str.join(list) 와 같다

~~~django
{{ value | join:"/" }}
# value가 ['a', 'b', 'c']라면
# 'a/b/c'
~~~

* length

length를 알려줌

~~~django
{{ value | length }}
# value = [1, 2, 3, 4, 5, 6]
# 값은 6
~~~

* length_is

argument와 length 값이 같으면 True를 리턴 아니면 False 리턴

~~~django
{{ value | length_is: '4' }}
~~~

* linebreaks

HTML파일에서의 개행은 브라우저가 해석하지 않는데 각각의 문단들을 \<p>로 감싸준다

~~~django
{{ value | linebreaks }}
~~~

* linebreaksbr

각각의 문단들을 \<br>로 감싸준다

~~~django
{{ value | linebreaksbr }}
~~~

* linenumbers

줄 번호와 함께 보여준다

~~~django 
{{ value | linenumbers }}
# one
# two

# 1. one
# 2. two
~~~

* lower

소문자로 바꿔준다

~~~django
{{ value | lower }}
~~~

* upper

대문자로 바꿔준다

~~~django
{{ value | upper }}
~~~



* make_list

리스트로 만들어준다

string일 경우에 한 글자씩 리스트로 들어가게 되며, integer일 경우 unicode string 으로 casting 되어 리스트로 리턴된다

~~~django
{{ value | make_list }}
~~~

* pluralize

복수형 접미사를 붙여준다. 기본값은 's'

~~~django
You have {{ num_message }} message{{ num_message | pluralize }}
~~~

복수형 접미사로 es를 써야할 땐

~~~django
You have {{ num_walruses }} walrus{{ num_walruses|pluralize:"es" }}

# y를 i로 고치고 es를 붙이는 경우
You have {{ num_cherries }} cherr{{ num_cherries|pluralize:"y,ies" }}.
~~~

* random
* slice

리스트 슬라이싱 하는것 과 비슷

~~~django
{{ some_list | slice: ':2' }}

# If some_list is ['a', 'b', 'c'], the output will be ['a', 'b'].
~~~

* slugify

slug화 시켜준다

~~~django
{{ value | slugify }}

# "joel-is-a-slug"
~~~

* striptags

HTML 태그를 제거해준다 ( 제대로 작동하지 않는 경우가 많다고 한다)

~~~ django
{{ value | striptag }}

# If value is "<b>Joel</b> <button>is</button> a <span>slug</span>",
# the output will be "Joel is a slug".
~~~

* time
* timesince
* timeuntil
* truncatechars

…을 만들어준다 ( 말줄임표 )

~~~django
{{ value | truncatechars: 9 }}
# 아홉글자 뒤에는 ...(말 줄임표) 사용
~~~

* truncatewords

단어를 기준으로 말 줄임표를 만든다

~~~django
{{ value | truncatewords: 2 }}
# 두 단어 뒤에는 ...(말 줄임표) 사용
~~~

* urlencode

URL에서 값들을 이스케이프 한다

~~~django
{{ value | urlencode }}
# If value is "https://www.example.org/foo?a=b&c=d",
# the output will be "https%3A//www.example.org/foo%3Fa%3Db%26c%3Dd".
~~~

* urlize

텍스트에서 클릭할수있는 이메일 또는 URL링크를 만들어준다

~~~django
{{ value | urlize }}
~~~

 하지만 지원되는 도메인이 한정적이다 : `.com`, `.edu`, `.gov`, `.int`, `.mil`, `.net`, and `.org`

* wordcount

단어의 갯수를 세준다

~~~django
{{ value | wourdcount }}
# value = first second 
# result = 2
~~~

