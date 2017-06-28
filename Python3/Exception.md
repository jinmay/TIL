# 예외 처리

 기본 형태 : 

~~~python
try:
    pass
except:
    pass
~~~

실패할 수 있는 코드를 실행했을 때 모든 잠재적인 에러를 방지하기 위해서 적절한 **예외 처리**가 필요하다

간단한 예제 : 

~~~python
num_list = [1, 2, 3]
position = 5

num_list[position] # 리스트의 인덱스보다 크기 때문에 IndexError이 발생한다
~~~

~~~python
num_list = [1, 2, 3]
position = 5

try:
    num_list[position]
except:
    print("IndexError appear") # 에러가 있다면 예외가 발생하고 이와 같이 출력
~~~

위 와 같이 인자가 없는 except문은 모든 예외 타입에 대해서 예외처리를 하겠다는 의미이다

하지만 두 개 이상의 예외가 발생하면 각각의 예외 핸들러를 제공하는 것이 가장 좋은 방법이다

~~~python
except 예외타입 as 이름
~~~

~~~python
num_list = [1, 2, 3]
position = 5

while True:
    value = input('insert: ')
    if value == 'q':
        break
    try:
        position = int(value)
        print(num_list[position])
    except IndexError as err:
        print("bad index")
    except Exception as e:
        print("something goes wrong")
~~~

위의 예제는 사용자로부터 리스트의 인덱스를 받고 예외처리를 하는 코드이다

err 변수에 IndexError 예외를, e 변수에 다른 기타 예외를 저장한다



# 예외 커스텀

지금까지 알아본 예외처리는 파이썬 표준 라이브러리에 이미 정의되어 있는 것이다

필요한 예외 처리를 선택해서 사용할 수 있을 뿐만 아니라, 특별한 상황에 발생할 수 있는 예외를 처리하기 위해 예외 타입을 직접 정의할 수 있다

**예외는 클래스고, Exception 클래스의 자식이다** 

~~~python
class UppercasException(Exception):
    pass

words = ['fasd', 'fads', 'FASD']
for word in words:
    if word.isupper():
        raise UppercaseException(word)
~~~





