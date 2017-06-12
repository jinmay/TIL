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

