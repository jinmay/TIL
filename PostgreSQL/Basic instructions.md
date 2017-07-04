# 기본 사용법

- 클라이언트 접속 

```sql
psql <DB명>
```

- 데이터베이스 목록 확인

```sql
\l
```

- 데이터베이스 변경

```sql
\c <DB명>
```

- 데이터베이스 생성

```sql
create database <DB명>
```

- 데이터베이스 삭제

~~~sql
drop database <DB명>
~~~

- 스키마 목록 확인

```sql
\dn
```

* 클라이언트 종료

~~~sql
\q
~~~

* 테이블 정의

~~~python
CREATE TABLE <table_name> (
	column1 datatype,
    column2 datatype,
)
~~~

* 테이블 데이터 삭제

~~~python
DELETE from <table_name>
~~~

