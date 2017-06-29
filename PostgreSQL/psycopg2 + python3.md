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
cur.execute("CREATE TABLE test_table (title varchar, content text);") 

# data 입력
cur.execute("INSERT INTO test_table (title, content) VALUES (%s, %s)", ('hello' ,'qwerttyfdas'))
~~~

> cursor 란?
>
> \- 결과로 도출된 데이터에서 특정 위치, 특정 row를 가리킬 때 사용된다
>
> \- 많은 row 중 한 row를 가리키는 포인터라고 위키피디아는 설명한다

데이터 입력할 때 다음과 같은 방법이 선호된다

~~~python
cur.execute("INSERT INTO numbers VALUES (%d)", (42,)) # WRONG
cur.execute("INSERT INTO numbers VALUES (%s)", (42,)) # correct

cur.execute("INSERT INTO foo VALUES (%s)", "bar")    # WRONG
cur.execute("INSERT INTO foo VALUES (%s)", ("bar"))  # WRONG
cur.execute("INSERT INTO foo VALUES (%s)", ("bar",)) # correct
cur.execute("INSERT INTO foo VALUES (%s)", ["bar"])  # correct
~~~

~~~python
SQL = "INSERT INTO authors (name) VALUES (%s);" # Note: no quotes
data = ("O'Reilly", )
cur.execute(SQL, data) # Note: no % operator
~~~

