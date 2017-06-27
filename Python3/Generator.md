# 제너레이터

파이썬의 시퀀스를 생성하는 객체이다

전체 시퀀스를 한 번에 메모리에 생성하고 정렬할 필요 없이, 아주 큰 시퀀스를 순회할 수 있다.

제너레이터를 순회할 때마다 마지막으로 호출된 항목을 기억하고 다음 값을 반환한다

return 문으로 반환하는 것이 아닌 **yield 문으로 값을 반환한다**

**간단하게 iterator를 생성해주는 것이라고 보면 된다**

~~~python
# example

def test(n):
    i = 0
    while i < n:
        yield i # 함수 종료가 아닌 계속 유지되면서 i 값을 리턴한다
        i += 1
        
foo = test(3)
for x in foo:
    print(x)
# 0
# 1
# 2 
~~~

~~~python
# 나만의 range함수
def xrange(start=0, stop=10, step=1):
    i = start
    while i < stop:
        yield i
        i += step

foo = xrange(1, 11)
for num in foo:
    print(num)
# 1 ~ 10까지 출력
~~~

제너레이터 객체든 제너레이터 표현식의 객체든 한 번 사용하면 다시 사용할 수 없다

* generator expression

~~~python
foo = (num for num in range(1, 11))

for x in foo:
    print(x)
# 1 ~ 10 출력
# 그리고 다시 foo 객체를 사용하려고 하면 비어있기 때문에 불가
~~~



### generator를 사용하는 이유

* 메모리를 효율적으로 사용할 수 있다

list와 generator 비교

~~~python
import sys

s1 = [num for num in range(1, 10)]
s2 = [num for num in range(1, 1000)]
v1 = (num for num in range(1, 10))
v2 = (num for num in range(1, 1000))

# sys.getsizeof() 결과
192
9024
88
88
~~~

list의 경우 사이즈가 커질수록 메모리 사용량이 늘어난다. 하지만 generator의 경우 사이즈가 커진다고 해도 차지하는 메모리는 동일하다. list 와 generator 의 동작방식의 차이 때문에 발생하는 현상이라고 한다

> list는 list에 속한 모든 데이터를 메모리에 적재하기 때문에 list의 크기만큼 메모리를 차지하고 있으며, generator의 경우 next() 메소드를 통해 차례로 값에 접근할 때마다 메모리에 적재하는 방식이다

따라서 list의 사이즈가 클 수록 generator이 더 효율적이다