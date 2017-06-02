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

