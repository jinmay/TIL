# PostgreSQL with CentOS7

최근 서버를 재설치하면서 식단 알리미의 DB를 바꾸었는데 CentOS에서 설치하면서 헤맸던 것을 정리해보자

일단 무슨 버전을 사용중인지 알아보자

~~~bash
yum list | grep ^postgresql
~~~

설치 확인 후 어디에 있는 지 확인하려면 아래와 같이 확인한다.

~~~bash
ls /usr/lib/systemd/system/po*
# postgresql.service가 있을것이다
~~~

**postgresql.service** 파일을 열어보면

> 포트번호 변경
>
> 데이터 저장경로 
>
> 실행파일 위치

와 같은 자세한 설정을 할 수 있다.

~~~python
# /usr/bin에 기본 명령어들이 위치해 있다
~~~

