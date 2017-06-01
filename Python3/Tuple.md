# Tuple

불변 시퀀스 / 하지만 단순한 불변 시퀀스만을 의미하는 것은 아니다

필드명이 없는 레코드로 사용하기도 한다

#### 레코드로 사용된 튜플

~~~python
lax_coordinates = (33, 34)
city, year, pop = ('Tokyo', 2003, 32450)
~~~

#### 튜플 언패킹

병렬 할당을 할때 자주 사용된다.

병렬 할당이란 반복형 데이터를 변수로 구성된 튜플에 할당하는 것이다

~~~python
a = (33, 34)

b, c = a

print(b, c)
~~~

또한 임시 변수를 사용하지 않고 두 변수의 값을 교환할 수 있다

~~~python
a, b = b, a
~~~

**\* 사용법 **

함수를 호출할 때 인수앞에 *을 붙여 언패킹할 수 있다

함수에서 호출자에 여러 값을 간단히 반환하는 기능이다

~~~python
divmod(30, 9) 
# (3, 3) 

t = (30, 9)
divmod(*t)
# (3, 3)

first, second = divmod(*t)
# first = 3, second = 3
~~~

**\_ 와 같은 더미 변수를 플레이스홀더로 사용해서 필요로하지 않는 부분은 언패킹할 때 무시할 수 있다**



#### 초과항목을 잡기 위해 * 사용

~~~python
a, b, *c = range(5)
# a = 0
# b = 1
# c = [2, 3, 4]

a, b, *c = range(3)
# (0, 1, [2])

a, b, *c = range(2)
# (0, 1, [])
~~~

병렬 할당의 경우 *는 단 하나의 변수에만 적용할 수 있는데, 어떠한 변수에도 가능하다!

~~~python
a, *b, c, d = range(5)
# (0, [1, 2], 3, 4)

*a, b, c, d = range(5)
# ([0, 1], 2, 3, 4)
~~~

#### 이름이 있는 튜플

~~~python
# 클래스명과 필드명의 리스트 - 2개의 매개변수 필요
from collections import namedtuple
City = namedtuple('City', 'name country population coordinates')
tokyo = City('Tokyo', country = 'JP', population = 23, coordinates = (123, 312))

tokyo.coordinates
# (123, 312)
tokyo[1]
# 'JP'


City._fields
# ('name' ,'country', 'population', 'coordinates')

LatLong = namedtuple('LatLong', 'lat long')
delhi_data = ('Delhi NCR', 'IN', 21, LatLong(312, 321))
delhi = City._make(delhi_data)
delhi_asdict()
# OrderedDict([('name', 'Delhi NCR'), ('country', 'IN'), ('population', 21), ('coordinates', LatLong(lat=23, long=32))])
~~~

또한 named tuple은 튜플에서 상속받은 속성 외에 다른 속성도 가지고 있다

1. _fields : 클래스의 필드명을 담고있는 튜플
2. _make(iterable) : iterable 한 객체로부터 named tuple 을 만든다
3. _asdict() : named tuple 객체에서 만들어진 collections.OrderedDict 객체를 반환한다

> 참고
>
> Dict.keys() : key 출력
>
> Dict.values() : value 출력
>
> Dict.items() : (key, value) 튜플 출력

