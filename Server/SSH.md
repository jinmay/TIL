# SSH

1. 공개키 / 개인키 생성하기

~~~bash
mkdir ~/.ssh
cd ~/.ssh
ssh-keygen -t rsa

# ssh-keygen 실행시
# 키 이름과 비밀번호를 입력하라고 하는데
# 엔터 누르면서 넘어가도 된다
~~~

ssh-keygen 명령은 **id_rsa(개인키)** / **id_rsa.pub(공개키)** 쌍을 생성한다.

2. 공개키 인증파일(authorized_keys)에 등록

~~~bash
# ~/.ssh에 authorized_keys 파일이 있는 지 확인 후 진행
# 그리고 공개키 파일의 내용을 authorized_keys에 입력
cat id_rsa.pub >> authorized_keys

# 확인
cat authorized_keys
~~~

* 원격 서버에서 접속을 허용할 클라이언트의 공개키(id_rsa.pub)를 저장해야한다.

