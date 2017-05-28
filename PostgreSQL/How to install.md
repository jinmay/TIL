# Install

### How to install 

1. Homebrew update

~~~
brew update
~~~

2. Install postgresql

~~~
brew install postgresql
~~~

3. create database

~~~
initdb /usr/local/var/postgres -E utf8
~~~

4. start server

~~~shell
pg_ctl -D /usr/local/var/postgres start
~~~

설치하고 위와 같이 입력하면 실행하는 데 까지는 문제가 없으나

psql로 접속하려고 하면 로그인 계정이 없다고 문제가 발생한다

이때 psql postgres 로 하니까 postgres table 로 접속이 되는 듯 하다

5. shut down server

~~~
pg_ctl -D /usr/local/var/postgres stop -s -m fast
~~~
