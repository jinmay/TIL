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

~~~
pg_ctl -D /usr/local/var/postgres start
~~~

5. shut down server

~~~
pg_ctl -D /usr/local/var/postgres stop -s -m fast
~~~