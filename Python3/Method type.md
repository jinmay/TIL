# Method type

### 1. instance method

일반적인 클래스를 생생할 때의 메서드 타입

인스턴스 메서드의 첫 번째 매개변수는 **self**이고, 파이썬은 이 메서드를 호출할 때 **객체**를 전달한다

~~~python
class Person():
    def __init__(self, name):
        self.name = name
~~~

### 2. class method

클래스 전체에 영향을 미치는 메서드이다

클래스 정의에서 **@classmethod** 데코레이터를 가지고 있다면 이것은 클래스 메서드이다

첫 번째 매개변수는 클래스 자신이고 보통 **cls** 라고 표현한다(인스턴스 메소드에서 self 라고 표현하듯이)

~~~python
# example
# 객체가 얼마나 만들어졌는지에 대한 클래스
class A():
    count = 0
    def __init__(self):
        A.count += 1
        
    @classmethod
    def how_many(cls):
        print("{}".format(cls.count))
~~~

### 3. static method

클래스 정의에서 세 번째 타입인 static method 는 클래스나 객체에 영향을 미치지 못한다

단지 편의만을 위해 존재한다

**@staticmethod** 데코레이터가 붙어있고 첫 번째 매개변수로 self나 cls 가 없다

~~~python
class CoyoteWeapon():
    @staticmethod
    def commercial():
        print("blah blah")
        
CoyoteWeapon.commercial()
# "blah blah"
~~~

