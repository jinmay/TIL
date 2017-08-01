# crontab

**스케줄러**이다

작업을 고정된 시간, 날짜, 간격에 주기적으로 실행할 수 있도록 스케줄링하기 위해 사용한다

쉘 명령어들이 주어진 일정에 주기적으로 실행하도록 규정해놓은 **crontab (cron table)** 파일에 의해 구동된다

각각의 사용자들은 자신들만의 crontab 을 가질수 있고, 가끔은 /etc 또는 /etc의 하위 디렉터리에 시스템 관리자들만이 편집할 수 있는, 시스템 전반에 영향을 미치는 crontab 파일이 존재하는 경우도 있다고 한다



##### syntax

기본 문법은 아래와 같다

~~~shell
 # ┌───────────── min (0 - 59) 
 # │ ┌────────────── hour (0 - 23)
 # │ │ ┌─────────────── day of month (1 - 31)
 # │ │ │ ┌──────────────── month (1 - 12)
 # │ │ │ │ ┌───────────────── day of week (0 - 6) (0 to 6 are Sunday to Saturday, or use names; 7 is Sunday, the same as 0)
 # │ │ │ │ │
 # │ │ │ │ │
 # * * * * *  command to execute
~~~

**예시**

~~~shell
# 매일 20시 (오후 8시)에 export_dump.sh라는 쉘 프로그램을 실행
0 20 * * * /home/oracle/scripts/export_dump.sh

# 매주 일요일 0시 1분에 for_crawling.sh 쉘 프로그램을 실행
0 1 * * 0 /home/test_user/scripts/for_crawling.sh
~~~



##### 관련 명령어

* 예약작업 생성 및 수정

~~~shell
crontab -e
~~~

* crontab 작업 리스트 출력

~~~shell
crontab -l
~~~

