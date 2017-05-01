# Introduction to migrations

* 자주 사용되는 명령어

  1. makemigrations
     * 모델에 대한 변경을 토대로 새로운 마이그레이션 파일을 생성한다
     * migrations 이름을 바꾸고 싶다면  

  ~~~python
  python manage.py makemigrations --name <changed_my_model> <your_app_label>
  ~~~

  ​

  1. migrate

     * 마이그레이션 적용과 적용한 것을 취소하는 역할을 수행한다

  2. showmigrations

     * 프로젝트의 마이그레이션 상태를 보여준다

     ~~~shell
     python manage.py showmigrations 

     # 뒤에 appname을 적으면 해당 app애대한 내용만 출력
     python manage.py showmigrations blog
     ~~~

     ​

  3. sqlmigrate

     * 해당 마이그레이션에 대한 SQL문을 보여준다

       ~~~shell
       # 사용법
       python manage.py sqlmigrate blog 0012 # 앱 이름 / 마이그레이션 파일명을 지정해준다
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





