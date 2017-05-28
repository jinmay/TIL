# 파이썬3 설치

#### homebrew 이용해서 python3 설치

> brew를 이용하여 설치하면 최신 버전의 pip도 같이 설치된다
>
> 또한 root권한이 아니며 /usr/local/Cellar 밑에 설치가 된다
>
> 는 두 가지 장점이 있다

* Homebrew 설치

~~~shell
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
~~~

* python3 설치

~~~shell
brew install python3
# python3 이라고 명시해줘야 3버전대를 설치할 수 있다
~~~

#### virtualenv 환경 구성

프로젝트별로 다른 파이썬 버전을 구성할 수 있다

* pip통해 설치

~~~shell
pip3 install virtualenv
~~~

**virtualenv가 activate 된 상태에서만 pip가 실행되도록 설정**

bashrc나 zshrc에 다음과 같이 추가한다

~~~shell
export PIP_REQUIRE_VIRTUALENV=true
 
export PIP_DOWNLOAD_CACHE=$HOME/.pip/cache
~~~

**global 영역으로 pip 사용할 때**

~~~shell
gpip(){
  PIP_REQUIRE_VIRTUALENV="" pip "$@"
}
~~~

를 .zshrc 에 추가한다

이후에 gpip를 이용하여 전역적으로 pip를 사용할 수 있다







##### 참고

[http://knot.tistory.com/102](http://knot.tistory.com/102)

