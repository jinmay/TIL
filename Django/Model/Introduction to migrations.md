# Introduction to migrations

* 자주 사용되는 명령어

  1. makemigrations

     migrations 이름을 바꾸고 싶다면  

     ~~~python
     python manage.py makemigrations --name <changed_my_model> <your_app_label>
     ~~~

     ​

  2. migrate

  3. showmigrations

  4. sqlmigrate

     ~~~python
     python manage.py sqlmigrate <app_label> <migration_name>
     ~~~



## Backend support

1. PostgreSQL  

> the only caveat is that adding columns with default values will cause a full rewrite of the table, for a time proportional to its size.

> it’s recommended you always create new columns with `null=True`, as this way they will be added immediately.

##### 유의점

* 디폴트 값과 함께 column을 저장하는 건 테이블 전체에 대해서 rewrite을 실행한다.  
  그래서 null = True로 새 column을 생성하는 것이 권고사항!!



2. MySQL
3. SQLite





