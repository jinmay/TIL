# 유용한 함수

* map(function, iterator, ...)

iterator 의 모든 item 들을 function 에 적용하고 list 형태로 리턴한다

~~~python
# 문자형 숫자가 있는 리스트
num_list = ['1', '2', '3', '4', '5']

map(int, num_list) # 를 하면 num_list 의 item 들이 int() 함수를 적용받아 int화 된 숫자 리스트를 리턴한다
~~~

