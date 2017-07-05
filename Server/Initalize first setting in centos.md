# 가상환경 세팅하기

1. zsh 및 oh-my-zsh 설치

~~~shell
# yum 업데이트를 제일 먼저 해준다
yum update

# zsh 설치
yum install -y zsh

# shell 변경
chsh -s `which zsh`

# shell 변경 확인
echo $SHELL

# oh-my-zsh 설치
# 공식사이트를 참고하자
# wget / git 설치 필요
sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

# agnoster 테마 사용하기
# path : .zshrc 
ZSH_THEME="agnoster"
DEFAULT_USER="$USER"

# powerline font 설치하고 재시작 후 사용가능!!
~~~



2. python 3.6버전 설치하기

~~~shell
# gcc 컴파일러 설치
yum install -y gcc
~~~

약간의 시간이 걸린다

~~~shell
# python.org에서 설치하려는 파이썬 버전을 선택한뒤
# 링크를 복사해서 wget 이용해 서버에 설치
~~~

~~~shell
./configure
~~~

~~~shell
make
~~~

~~~shell
make install
~~~

만약 zlib 에러가 뜬다면

~~~shell
# example
./configure --prefix=/root/Python-2.7.8 --with-zlib-dir=/usr/local/lib
~~~

zlib 최신버전을 설치한 후 다시 ./configure 부터 해본다

혹은

~~~shell
# zlib error 일 때
sudo yum install -y zlib-devel

# pip로 패키지 설치시 ssl error 날 때
sudo yum install -y openssl-devel

# sqlite3 사용을 위해
sudo yum install -y sqlite-devel
~~~

설치 후 다시 ./configure 부터 해본다



3. PostgreSQL 설치

~~~shell
# centos에 이미 PostgreSQL은 설치되어 있어서 다른 패키지만 받으면 된다
sudo yum install postgresql-server postgresql-contrib

# 새로운 PostgreSQL DB cluster 생성
sudo postgresql-setup initdb

# By default, PostgreSQL does not allow password authentication.
# We will change that by editing its host-based authentication (HBA) configuration.
sudo vi /var/lib/pgsql/data/pg_hba.conf

##################################
host    all             all             127.0.0.1/32            md5
host    all             all             ::1/128                 md5
##################################

# Now start and enable PostgreSQL
sudo systemctl start postgresql
sudo systemctl enable postgresql

# 설치할 때 부여받은 postgres로 유저 전환
sudo su - postgres
~~~

4. virtualenv 설정하기

virtualenv 환경을 실행하지 않았을때는 패키지 관리를 하지 못하도록 한다

~~~shell
# virtualenv가 activate 일때만 pip 를 사용
export PIP_REQUIRE_VIRTUALENV=true
export PIP_DOWNLOAD_CACHE=$HOME/.pip/cache
~~~

그러나 전역적으로 패키지 관리가 필요한 경우도 있기 때문에 다음과 같이 설정해준다

~~~shell
# gpip로 전역적인 파이썬 패키지 관리를 이용할 수 있다
gpip(){
    PIP_REQUIRE_VIRTUALENV="" pip "$@"
}
~~~

