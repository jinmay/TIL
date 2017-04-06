# Install Ruby ( CentOS )

**임대중인 가상서버에 CentOS 7을 사용중이므로 centos에 루비를 설치해보자**

**RVM보다는 rbenv를 이용할 계획!!**





1. 필요한 패키지 설치

~~~shell
sudo yum update
sudo yum install git
sudo yum groupinstall -y 'development tools'
sudo yum install -y gcc-c++ glibc-headers openssl-devel readline libyaml-devel readline-devel zlib zlib-devel  sqlite-devel
sudo yum install glibc-devel libffi-devel
~~~

나중에 설치할 때 오류가 발생할 수 있으니 관련 패키지먼저 설치해준다





2. rbenv 설치

~~~shell
git clone git://github.com/sstephenson/rbenv.git ~/.rbenv
~~~

**~/.rbenv** 경로에 rbenv 를 설치한다

**다른 계정에서도 사용하기 위해 /usr/local/rbenv 에 설치하기도 한다**





3. ruby-build 설치

~~~shell
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
~~~

rbenv가 설치되있는 경로( ~/.rbenv )에 **plugins/ruby-bulid/ 디렉토리**에 ruby-build를 설치한다





4. 환경변수 설정

모든 계정에서 사용하려면 **/etc/profile.d/rbenv.sh**를 생성하여 적용하면되고

해당 유저만 사용하려고 할땐 각 계정의 **.bashrc ( .zshrc )**에 넣어주면 된다



* rbenv를 /usr/local/rbenv에 설치한 경우 /etc/profile.d/rbenv.sh 추가

```shell
export RBENV_ROOT="/usr/local/rbenv"
export PATH="$RBENV_ROOT/bin:$PATH"
eval "$(rbenv init -)"
```

* 그냥 .rbenv에 설치한 경우에는 rbenv path만 잡아주면 됩니다. (~/.bashrc)

```shell
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
```





5. Ruby 설치

~~~shell
# rbenv로 설치가능한 루비 리스트
rbenv install -l

# ruby 설치
rbenv install 2.4.1

# local 적용
rbenv local 2.4.1

# rehash
rbenv rehash

# 설치된 루비 목록
rbenv versions

# 버전 확인
rbenv versions
~~~

#### ruby 또는 gem을 새로 설치할때 마다 rbenv rehash를 해주어야 한다!!







### rails 설치하려면...

~~~shell
gem install rails
~~~

