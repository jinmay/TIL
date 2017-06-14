# Basic CentOS

* 프롬프트

~~~
# -> root 
$ -> 일반사용자
~~~

* 시작과 종료

~~~shell
# 종료
shutdown -P now
halt -p
init 0
# 종료시간 설정
shutdown -P +20 # 20분 후에 종료, 사용자에게 메세지도 날라감
shutdown -k +15 # 15분 후에 종료한다고 메세지는 남지만, 실제로 종료 X 

# 재부팅
shutdown -r now
reboot
init 6

# 사용자 로그아웃
logout
exit
~~~

* 런 레벨

~~~shell
0 - shutdown
3 - 텍스트 모드 다중 사용자 모드
5 - 그래픽 모드 다중 사용자 모드
6 - reboot
~~~

* 기본 명령어

~~~shell
# 디렉토리 안의 파일 목록
ls
ls -a # 숨김폴더 포함 확인
ls -l # 자세히, 리스트처럼 나열
ls -al # 처럼 조합해서 사용 가능

# 디렉터리 이동(change directory)
cd

# print working directory 현재 디렉터리
pwd

# 크기가 0인 파일 생성
touch a.txt

# 복사
cp a.txt b.txt

# 이동 또는 이름변경
mv a.txt abc.txt

# 디렉터리 생성
mkdir # make directory

# 디렉터리 삭제 (디렉터리가 비어있어야 한다)
rmdir

# 텍스트 파일 화면에 출력
cat

# 텍스트파일 앞 10행 / 뒤 10행 출력
head # 앞 10행
tail # 뒤 10행

# page 단위로 출력
more a.txt # 페이지 전환은 space bar

# 속성? 확인
file a
file /var/log/ # directory

# clear
clear
~~~

* 사용자와 그룹

리눅스는 다중 사용자 시스템이다(Multi-user system)

**root**라는 수퍼유저가 있고, 모든 작업을 할 수 있는 권한을 가지고 있다

모든 사용자는 **하나 이상의 그룹**에 속해 있고, **/etc/passwd**에서 확인할 수 있다

~~~shell
# vi /etc/passwd

first:x:1002:1002::/home/first:/bin/bash
# 사용자명:패스워드:유저ID:그룹ID:전체이름(없어도 된다):home 디텍터리:로그인 쉘
# 패스워드는 /etc/shadow 파일에서 따로 관리한다

# 그룹 파일
# vi /etc/group
root:x:0:nginx
# 그룹명:패스워드:그룹ID:그룹에 속한 사용자명
~~~

사용자는 **/etc/paswd**

그룹은 **/etc/group**

패스워드는 **/etc/shadow** - 암호화 되어있다

유저를 생성할 때 그룹이 따로 생성하지 않으면 자동으로 유저이름과 같은 그룹을 생성하고 포함시킨다

~~~shell
# 유저 생성
useradd <name>

# 패스워드 지정 및 변경
## root는 모든 사용자의 패스워드 변경가능
## 일반사용자는 본인의 패스워드만 변경가능
passwd <name>

# 사용자 속성 변경
usermod <name>

# 유저 삭제
## -r 옵션을 추가하면 홈 디렉터리까지 삭제
userdel <name>

## 유저 생성시 옵션
-u : id 지정
-g : 그룹 지정
-d : 홈 디렉터리 지정
-s : 쉘 지정

# 유저의 패스워드를 주기적으로 변경하도록 설정
chage 2 <name>

# 현재 유저가 속한 그룹을 출력
groups

# 그룹 생성
groupadd <name>

# 그룹 속성 변경
groupmod 

# 그룹 삭제
groupdel <name>

# 그룹 패스워드 설정
gpasswd <name>
~~~



* 파일 소유 / 허가권 / 링크

##### 파일유형

~~~shell
- : 일반파일
d : 디렉터리
l : 링크
~~~

##### 허가권 

read / write / execute 의 첫글자로 표시

소유자 / 그룹 / 기타사용자 순서

숫자(8진수)로 표현가능

~~~shell
drw-r--r--
# 디렉터리이고 소유자는 read, write 가능
# 그룹은 read만 가능
# 기타사용자는 read만 가능

# 숫자로 표현해보면
# 644
# 420 400 400
~~~

##### 허가권 변경

chmod

~~~shell
chmod 777 sample.txt
~~~

심볼릭 링크로 변경도 가능하다

~~~shell
###
user : u
group : g
odd users: o

추가 : +
삭제 : -
###

# 유저와 그룹의 read / write 권한을 +(추가하겠다)
chmod ug+rw sample.txt
~~~



##### 소유권 변경

chown

~~~shell
chown <new onwer> <file>
~~~

chgrp

~~~shell
chgrp <new group> <file>
~~~

소유자와 소유그룹을 한꺼번에 변경 

~~~shell
chown <new owner>.<new group> <fil
~~~



##### 링크

하드링크와 심볼릭링크 두 가지가 있다

심볼릭 링크는 윈도우즈의 바로가기 아이콘과 개념이 비슷하다고 보면 된다

> 링크를 생성하게 되면 파일에 대한 정보를 저장하고 있는 inode을 생성하게 된다.
>
> 하드링크의 경우 inode가 원본 파일 데이터를 바로 가리키게 되며
>
> 심볼릭링크의 겨우 inode가 원본 파일 포인터를 가리키게 되어
>
> "심볼릭 => inode => 원본파일 포인터 => 원본파일" 의 순서를 가지게 된다

하드링크는 사용하는 일이 별로 없으며, 심볼릭 링크에 대해서 알아두자!!

하드링크 생성 : ln

심볼릭링크 생성 : ln -s 

inode 확인 : ls -il

**하드링크와 원본파일의 inode는 같고 심볼릭링크의 inode만 다르다**

이때에 원본파일의 경로를 바꿔서 실행해 보면

**하드링크는 정상적으로 작동하고, 심볼릭링크는 작동하지 않는다!!**

* 패키지 설치

##### YUM

Yellowdog Updater Modified

> rpm의 패키지 의존성 문제를 해결할 수 있다
>
> 인터넷을 통해 필요한 패키지를 repository에서 자동으로 다운로드 하고 설치한다
>
> **저장소의 url은 /etc/yum.repos.d/** 이다

~~~shell
# 기본 설치
yum install
yum -y install <package name> # 사용자의 확인을 모두 yes 간주하고 설치

# 업데이트 목록 확인
yum check-update

# 업데이트
yum update <name>

# 삭제 
yum remove <name>

# 정보확인
yum info <name>

# 패키지 그룹 설치
yum groupinstall <package group name>

# 패키지 리스트 확인
yum list <package name>

# 저장소 목록 지우기
## 간혹 yum에 문제 있을때 사용
yum clean all
~~~

* 파일 압축, 묶기

##### 압축

xz와 bz2가 압축률이 좋다

압축을 하면 원본파일은 없어진다

 압축과 묶는 것은 다르다

##### 묶기

tar

동작 : c - 묶기 / x - 풀기 / t - 경로확인

옵션 : f - 파일 / v - 과정보이기 / J - (tar + xz) / z - (tar + gzip) / j - (tar + bzip2)

~~~shell
# 묶기
tar cvf sample.tar file1 file2

# 묶기 + xz 압축
tar cvfJ sample.tar.xz file1 file2 file3

# tar 풀기
tar xvf sample.tar

# tar 풀기 + 압출 해제
tar xvfJ sample.tar.xz
~~~

* 파일 찾기

find 경로 옵션 조건 action : 기본파일 찾기

> 옵션
>
> -name / -user / -newer / -perm / -size
>
> action
>
> -print(default) / -exec(외부명령 실행)

~~~shell
find /etc -name "*.conf"

find /bin -size +10k -size -100k

# /home 디렉터리에 있는 모든 .swp 파일을 찾아서 지운다
# -exec \; 사이에 있는 명령을 수행
find /home -name "*.swp" -exec rm {} \;
~~~

which - PATH에 설정된 디렉터리만 검색

whereis - 실행 파일, 소스, man 페이지 파일까지 검색

locate - 파일 목록 데이터베이스에서 검색

* cron과 at

##### cron

주기적으로 반복되는 일을 자동으로 실행하기 위해 설정

관련 데몬은 crond 

파일은 /etc/crontab

~~~shell
# 매 시간 1분마다 curl localhost 실행
01 * * * * root curl localhost

# 새일 3시 2분마다 실행
02 3 * * * root curl localhost

# 매월 1일 4시 42분에 실행
42 4 1 * * root curl localhost
~~~

##### at

일회성 작성 예약

~~~shell
at <time>

# 내일 새벽 3시
at 3:00am tomorrow

# 지금으로부터 1시간 후
at now + 1 hours

를 입력하면 실행할 명령어를 적는 프롬프트가 나오고
입력 후 
컨트롤 + D

예약 리스트 확인 : at -l
취소 : atrm <작업번호>
~~~

* 네트워크 관련 명령어

##### nmtui

네트워크와 관련된 대부분의 작업을 수행

> 자동 IP주소 또는 고정 IP주소 사용결정
>
> IP주소, 서브넷 마스크, 게이트웨이 정보 입력
>
> DNS 정보입력
>
> 네트워크 카드 드라이버 설정
>
> 네트워크 장치 설정

텍스트 기반으로 작동한다

##### systemctl

systemctl <start / stop / restart / status> network

네트워크 설정을 변경한 후, 시스템에 적용을 하기위해 수행해야 함!

##### ifdown / ifup

네트워크 장치를 on / off 한다

~~~shell
ifup eno12312
ifdown eno2312312
~~~

##### ifconfig

IP주소 설정 정보 출력

##### nslookup

DNS 서버 테스트

##### ping

네트워크상에서 응답하는지 테스트

<hr>

/etc/sysconfig/network - 네트워크의 기본적인 정보가 설정되어 있는 파일

/etc/sysconfig/network-scripts/ifcfg-<장치이름> - 장치에 설정되어 있는 네트워크 정보가 모두 들어있는 파일

/etc/resolv.conf - DNS 서버의 정보 및 호스트 이름이 있는 파일

/etc/hosts - 현재 컴퓨터의 호스트 이름 및 FQDN이 들어있는 파일

* 파이프, 필터, 리다이렉션

##### 파이프

명령어를 연결해준다

|

~~~shell
ls -l /etc | more
~~~

##### 필터

필터링해줌

파이프와 같이 자주 사용된다

~~~shell
ps -ef | grep bash
~~~

##### 리다이렉션

표준 입출력의 방향을 바꾸어 줌

~~~shell
ls -l > list.txt

sort < list.txt > out.txt
~~~

* 프로세스, 데몬

프로세스 : 메모리에 로딩되어 활성화 된 것

포그라운드 프로세스 : 눈에 보이는 프로세스(대부분의 응용프로그램)

백그라운드 프로세스 : 눈에 보이지 않는 프로세스(백신, 서버 데몬)

프로세스 번호 : 각 프로세스가 가지고 있는 고유번호

작업 번호 : 현재 실행되고 있는 백그라운드 프로세스의 순차번호

부모 프로세스 : 모든 프로세스는 부모 프로세스를 가지고 있고 부모 프로세스를 kill 하면 자식 프로세스도 자동으로 kill 됨

##### ps

현재 프로세스의 상태 확인

주로 

~~~shell
ps -ef | grep <process name>
~~~

을 사용

##### kill

프로세스 강제 종료

~~~shell
kill -9 <process id>
~~~

##### pstree

부모 프로세스와 자식 프로세스의 관계를 트리 형태로 출력

* 서비스와 소켓

##### 서비스

시스템과 독자적으로 구동되어 제공하는 프로세스

예를 들면 = 웹 서버(httpd) / db 서버( mysqld) / ftp 서버(vsftpd) 등이 있다

실행과 종료는 'systemctl start / stop / restart <name>' 으로 자주 사용

##### 소켓

서비스는 항상 가동되지만, 소켓은 외부에서 특정 서비스를 요청 할 경우에만 systemd 가 구동시키는것 이다

요청이 끝나면 소켓도 종료