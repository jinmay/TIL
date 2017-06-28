# 데코레이터

**First class function**와 **closure**를 먼저 이해해야 한다

데코레이터는 클로저와 그 형태가 매우 비슷하다. **함수를 인자로 받는 것이 큰 특징이다**

예제 : 앞 뒤로 수평선을 출력하는 데코레이터

~~~python
# decorator
def deco(func):
    def inner(some):
        print('--------------------')
        func(some)
        print('--------------------')
    return inner

# 인자를 출력하는 함수
def print_something(something):
    print(something)
    
# 수동으로 데코레이터 사용
test = deco(print_something)
test('hello world')

# --------------------
# hello world
# --------------------
# 를 출력한다
~~~

하지만 위 와 같이 수동으로 사용하지 않고 '@' 심볼과 데코레이터 함수의 이름을 붙여쓰는 구문을 사용한다

~~~python
@deco
def print_something(something):
    print(something)
    
print_something('hello world')
# --------------------
# hello world
# --------------------
# 를 출력한다
~~~

> 클래스 형태의 데코레이터도 가능하다
>
> (추가 예정)