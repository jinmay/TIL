# iterable 과 iterator

#### iterable 과 iterator 설명

- iterable

멤버를 하나씩 차례로 순회할 수 있는 object

예로는 list / str / tuple

```python
for num in range(1, 11):
    print(num)
    
# range()가 만들어준 객체가 iterable 한 객체이기 때문에 
# for 문 과 함께 사용가능했다
```

또한 non-sequence 타입인 dictionary와 file 로 iterable 하다고 볼 수 있다

```python
x = {'a': 'first', 'b': 'second'}

for word in x:
    print(word) # 키 출력
    
# non-sequence 타입인 dictionay도 iterable 한 객체
```

그리고

\__iter()__ 나 \__getitem()__ 메소드가 정의되어 있는 class도 iterable 하다고 할 수 있다.

> 참고
>
> zip()과 map() 내장함수는 iterable 한 인자를 받는다

- iterator

next() 메소드로 순차적으로 호출 가능한 object

next() 로 다음 데이터를 불러올 수 없는 경우(예를 들면, 시퀀스의 가장 마지막 데이터 일 때) **StopIteration exception**을 일으킨다



### 하지만 iterable한 object라고해서 꼭 iterator 인 것은 아니다

```python
x = [1, 2, 3]

next(x)
# TypeError 발생 - list object is not an iterator
# 리스트는 iterable 하지만 iterator는 아니다
```

**iter()**라는 내장함수를 통해서 iterable 한 object를 iterator 로 바꿀 수 있다

```python
a = iter(x)

next(a) # 1
next(a) # 2
next(a) # 3
next(a) # StopIteration Exception
```

그렇다면 list와 tuple 같은 iterable 하지만 iterator가 아닌 object들은 for 문과 쓰였을 때 순차적으로 접근 가능했던 것인가?

**답은 for 문으로 반복하는 동안 python 내부에서 임시로 iterator로 자동 변환했기 때문이다**