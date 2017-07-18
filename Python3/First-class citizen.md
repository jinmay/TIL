# 일급 객체와 클로져

파이썬 - 모든 것은 객체다

* 함수를 변수에 할당할 수 있다
* 함수의 인자로 사용 가능하다
* 함수의 리턴값으로 사용 가능하다



1. 함수를 변수에 할당

~~~python
def print_something(something):
    print(something)
    
a = print_something
a('hello world') # 'hello world' 출력
~~~

()가 있으면 그 함수를 호출함을 의미하고, ()가 없는 함수는 함수 그 자체이다



2. 함수의 인자로 사용

~~~python
def run_func(func, *args):
    func(*args)
    
def sum_args(*args):
    print(sum(args))
    
    
run_func(sum_args, 1, 2, 3, 4, 5) # 15 출력
~~~



3. 함수의 리턴값으로 사용 : **내부함수 ->  클로져**

* 내부함수 - 함수 안에 또 다른 함수 정의

루프나 코드 중복을 피하기 위해 사용하곤 한다(함수 내에 어떤 복잡한 작업을 한 번 이상 수행할 때 유용하게 사용)

~~~python
def outer(a, b):
    def inner(c, d):
        print(sum(c, d))
    return inner(a, b)

outer(1, 2) # 3 출력
~~~

* 클로져 - 내부함수처럼 행동. 다른 함수에 의해 동적으로 생성되며 바깥 함수로부터 생성된 변수값을 변경하고 저장할 수 있다

~~~python
def outer(a, b):
    def inner():
        print(a + b)
    return inner

print1 = outer(1, 2)
print1() # 3 출력
~~~

내부 함수가 외부 함수의 변수를 기억하고 있다 

외부함수는 내부함수를 리턴한다

바깥 함수로부터 생성된 변수 값을 알고 있는 함수



### 클로저 - 사용되는 곳

* 주로 : 내부 데이터를 은닉할 때 사용
  * private 데이터를 만들 때 사용
  * 변수명을 통해서 데이터를 직접 통제하는 것이 아니라 변수에 접근하는 메소드를 갖고있는 것
* 전역 변수를 사용하지 않으려고 할 때
* 객체로 만드는 것이 비효율적일 때. 메소드가 하나 밖에 없는 객체같은 경우 클로저를 활용