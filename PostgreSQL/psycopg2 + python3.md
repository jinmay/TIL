# psycopg2 + python3

가상환경에서 psycopg2를 설치하자

~~~shell
pip3 install psycopg2
~~~

host : PostgreSQL server ip

dbname : 접속할 db name

user : db에 접속할 user

password : 접속할 user의 password

port : db에 접속할 port (default : 5432)

* 기본연결

~~~python
import psycopg2

# db에 연결하기
conn = psycopg2.connect("dbname=test user=postgres")

# cursor 생성
cur = conn.cursor()

# 명령 실행 : 새 테이블 만들기
cur.execute("CREATE TABLE test (id serial PRIMARY KEY, num integer, data varchar);")

# 
~~~

> cursor 란?
>
> \- 결과로 도출된 데이터에서 특정 위치, 특정 row를 가리킬 때 사용된다
>
> \- 많은 row 중 한 row를 가리키는 포인터라고 위키피디아는 설명한다
>
> 

