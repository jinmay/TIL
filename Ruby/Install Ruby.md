# Install Ruby

> Ruby를 설치하는 데에는 크게 두 가지가 있다
>
> 1. RVM ( Ruby Version Manager )
> 2. rbenv

RVM의 경우 과거에 주로 사용됐지만 환경변수를 많이 건드린다는 단점으로 인해서 사용빈도가 줄어들고 있다고 한다

따라서 rbenv를 이용하여 맥에 ruby를 설치해보자

> rbenv 또한 두 가지 설치방법이 있는데
>
> 1. git을 통해
>
> 2. Mac이라면 homebrew를 통해 설치할 수 있다
>
>    (python의 virtualenv와 비슷한거 같다)







#### git을 이용하여 설치하게 되면 별도의 수작업이 필요하므로 brew로 설치해보자

1. Homebrew 최신화

~~~shell
brew update
~~~

2. rbenv 와 ruby-build 설치

~~~shell
brew install rbenv ruby-build
~~~

3. 설치가 완료된 후에는 아래와 같은 커맨드를 한번 입력해준다

~~~shell
eval "$(rbenv init -)"
~~~

* 터미널을 실행할 때 마다 위와 같은 명령어를 입력하기 보다는 shell profile에 등록을 해 두는 편이 낫다

~~~shell
if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi

# 또는
echo 'if which rbenv > /dev/null; then eval "$(rbenv init - )"; fi' >> ~/.zshrc
~~~

* rbenv 를 통해 설치할 수 있는 목록

~~~shell
rbenv install -l

# 시스템에 설치된 항목 list
rbenv versions
~~~

4. 폴더별로 사용할 ruby version 지정

~~~shell
# 폴더로 이동한 뒤
rbenv local <version>

# 전역적으로 사용하고 싶으면
rbenv global <version>

# ruby version 확인
ruby -v
~~~

5. RVM과 다르게 bundler가 기본으로 설치되지 않기때문에 설치해준다

~~~shell
gem install bundler
rbenv rehash
~~~

5. rails 설치

~~~shell
gem install rails <version>
~~~

