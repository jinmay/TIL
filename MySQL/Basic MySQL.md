# Basic MySQL

* 설치하기

맥에서 brew를 이용하여 설치

~~~shell
brew install mysql
~~~

* mysql 서버 구동

~~~shell
mysql.server start
~~~

* mysql 서버 끄기

~~~shell
mysql.server stop
~~~

* 설치 후 root로 접속

~~~shell
mysql -u root
~~~

---------------

* 데이터 베이스 관리

1. 생성

~~~shell
CREATE DATABASE <db_name>;
~~~

2. 목록 보기

~~~shell
SHOW DATABASES;
~~~

3. 특정 데이터베이스 사용

~~~shell
USE <db_name>
~~~

4. 삭제

~~~shell
DROP DATABASE <db_name>
~~~

----------------------

* 테이블 관련

1. 테이블 목록

~~~shell
show tables;
~~~

2. 테이블 정보

~~~shell
describe <table_name>;
desc <table_name>;
~~~

3. 변경

~~~shell
# 속성 추가
alter table <table_name> add <속성명> <속성타입>;
alter table <table_name> drop <속셩명>;
alter table <table_name> change column <이전 속성명> <새 속성명 속성타입>;
alter table <table_name> modify column <속성명> <새 속성타입>;
alter table <table_name> rename <새 테이블명>;
~~~

4. 테이블 삭제

~~~shell
drop table <table_name>;
~~~

5. 데이터 삽입

~~~python
insert into <table_name> values(value1, value2);
insert into <table_name> (속성1, 속성2) values (값1, 값2);
~~~

-----------

#### python과 연결해보자

* 드라이버는 PyMySQL 사용

~~~shell
pip install pymysql
~~~

