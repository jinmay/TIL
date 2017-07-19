# get/set 속성값과 property

자바를 예로 들면 외부로부터 바로 접근할 수 없는 private 객체 속성이 있다

이 때에 private 한 속성을 읽고 쓰기 위해서 **getter**과 **setter** 메서드를 사용한다

하지만 파이썬에서는 모든 속성과 메서드는 public 하기 때문에 getter / setter 메서드를 필요로 하지 않는다

만약 속성에 직접 접근하는 것이 부담스럽다면  getter 과 setter 메서드를 작성하여 이용하면 되는데, pythonic 하게 사용하기 위해서 **property**를 사용해보자!!

~~~python
# 외부에서 hinnden_name 속성에 직접 접근하는 것을 막기위해
# getter / setter 메서드를 정의
# 그리고 마지막으로 이 메서드들을 name 속성의 property로 정의
class Person():
    def __init__(self, name):
        self.hidden_name = name
    def get_name(self):
        print("getter")
        return self.hidden_name
    def set_name(self, name):
        print("setter")
        self.hidden_name = name
    name = property(get_name, set_name)
~~~

사용

~~~python
a = Person('a')
a.name
# 'getter'
# a

a.get_name()
# 'getter'
# a

a.name = 'b'
# 'setter'

a.set_name = 'dd'

a.name
# 'getter'
# dd
~~~

프로퍼티를 정의하는 또 다른 방법은 **데코레이터**를 사용하는 것이다

##### getter 메서드에는 @property

##### setter 메서드에는 @name.setter

~~~python
class Person():
    def __init__(self, name):
        self.hidden_name = name
    @property
    def name(self):
        print('getter')
        return self.hidden_name
    @name.setter
    def name(self, name):
        print('setter')
        self.hidden_name = name
~~~

지금까지 완벽하지는 않지만 숨겨진hidden_name 속성을 호출했다

**파이썬은 클래스 정의 외부에서 볼 수 없도록 하는 속성에 대한 naming convention이 있다**

**속성 이름 앞에 더블 언더스코어 / double underscore / __ 를 붙이면 된다**

~~~python
class Person():
    def __init__(self, name):
        self.__name = name
    @property
    def name(self):
        print('getter')
        return self.__name
    @name.setter
    def name(self, name):
        print('setter')
        self.__name = name
~~~

~~~python
a.__name 
# AttributeError를 발생시킨다
# 이 속성이 외부코드에서 발견할 수 없도록 이름을 맹글링(mangling) 한 것이다
~~~

**맹글링(mangling)이란, 컴파일러나 인터프리터가 변수/함수명을 그대로 사용하지 않고 일정한 규칙에 의해 변형시키는 것을 말한다)**