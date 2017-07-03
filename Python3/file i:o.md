# 파일 입출력

기본형태 

~~~python
f = open('file name', 'mode')
~~~

f -  파일 객체

file name - 파일 문자열 이름

mode - 파일 타입과 무엇을 할지

##### mode

r - 파일 읽기

w - 파일 쓰기

x - 파일 쓰기(파일이 존재하지 않을 경우)

a - 파일 추가(파일이 존재하면 기존의 내용의 뒤에 추가)

-----------------------

t(또는 아무것도 명시하지 않음) - 텍스트 타입

b - 이진 타입

### 텍스트 파일 쓰기

**write()**

~~~shell
f = open('test.txt', 'w')
f.write('first sentence')
f.close() # 꼭 닫아주어야 한다
~~~

텍스트 파일을 쓰는 방법 

1. write()
2. print()

위의 예제를 **print()**을 통해서 쓰려면

~~~python
f = open('test.txt', 'w')
print('first sentence', file=f)
f.close()
~~~

기본적으로 print()는 각 인자 뒤에 스페이스를, 끝에 줄바꿈을 추가한다



### 텍스트 파일 읽기

**read(), readline(), readlines()**

1. read()

인자 없이 호출하여 한 번에 전체 파일을 읽을 수 있다

엄청 큰 파일을 read()로 읽으면 메모리 소비가 심하므로 주의!!

~~~python
f = open('test.txt', 'r')
contents = f.read()
f.close()

print(contents)
~~~

2. readline()

파일을 라인 단위로 읽는다

~~~python
f = open('test.txt', 'r')
a = f.readline()
f.close()

print(a) # 첫번째 줄만 읽어서 출력
~~~

3. readlines()

힌 번에 모든 라인을 읽고, 한 라인으로 된 문자열들의 리스트를 반환한다

~~~python
f = open('test.txt', 'r')
a = f.readlines()
f.close()

for line in a:
    print(line, end='')
~~~



### CSV

Comma-Seperated-Values

* 쓰기 예제

~~~python
import csv

lists = [
    ['a', 'first'],
    ['b', 'second'],
    ['c', 'third'],
    ['d', 'fourth'],
    ['e', 'fifth'],
    ['f', 'sixth']
]

with open('test2.txt', 'w') as f:
    csvout = csv.writer(f)
	csvout.writerows(lists)
~~~

* 읽기 예제(List)

~~~python
import csv

with open('test2.txt', 'r') as f:
    var = csv.reader(f)
    lists = [row for row in var]
~~~

* 읽기 예제(Dictionary)

~~~python
import csv

with open('test2.txt', 'r') as f:
    var = csv.DictReader(f, fieldnames = ['alphabet', 'number'])
    aaa = [row for row in var]
~~~

