# Django 배포하기

[장고 배포 튜토리얼](http://uwsgi-docs.readthedocs.io/en/latest/tutorials/Django_and_nginx.html)의 내용을 이해해보자

#### Setting up Django and your web server with uWSGI and nginx

이 튜토리얼은 장고를 배포하려는 유저들을 위한 것이다. uWSGI와 Nginx와 함께 Django를 셋업 하는 것부터 시작해보자

> **note**
>
> 이 튜토리얼은 사용하고 있는 시스템에 대한 몇가지 가정을 필요로 한다
>
> Unix-like 시스템
>
> 장고 1.4 혹은 이후 버전 - wsgi 모듈을 자동적으로 만들어줘서 더 편리하다

#### Concept

웹 서버는 바깥세계와 마주한다. 또한 HTML, 이미지, 스타일시트와 같은 파일들을 파일 시스템으로 부터 직접적으로 서빙한다. 그러나 웹 서버는 장고 어플리케이션과 직접 연결될 수 없다.

웹 서버 게이트웨이 인터페이스 - WSGI - 는 이와 같은 작업을 한다. WSGI는 파이썬 표준이다

이번 튜토리얼에서 Unix socket을 생성하기 위해 uWSGI를 사용할 것이고, WSGI 프로토콜을 이용해서 웹 서버에게 응답을 제공할 것이다. 모든 과정을 도식화 하면 이러할 것이다.

```
the web client <-> the web server <-> the socket <-> uwsgi <-> Django
```

#### Before you start setting up uWSGI

uWSGI를 세팅하기에 앞서서 꼭 가상환경과 Django 를 설치하도록 하자

### virtualenv

Make sure you are in a virtualenv for the software we need to install (we will describe how to install a system-wide uwsgi later):

```
virtualenv uwsgi-tutorial
cd uwsgi-tutorial
source bin/activate
```

### Django

Install Django into your virtualenv, create a new project, and `cd` into the project:

```
pip install Django
django-admin.py startproject mysite
cd mysite
```

#### About the domain and port

Django의 기본 runserver 가 그러하듯, 8000번 포트를 사용할 것이다.

## Basic uWSGI installation and configuration

#### Install uWSGI into your virtualenv

```
pip install uwsgi
```

가상환경에 uwsgi를 설치하자

물론 uWSGI를 설치하는 방법에는 여러가지가 있지만, 이 방법도 다른 것과 마찬가지로 좋다. pip가 설치되어 있어야 한다. 

#### Basic test

아래와 같이 test.py를 생성하자 : 

```python
# test.py
def application(env, start_response):
    start_response('200 OK', [('Content-Type','text/html')])
    
    return [b"Hello World"] # python3
    #return ["Hello World"] # python2
```

uWSGI를 실행하자  :

```
uwsgi --http :8000 --wsgi-file test.py
```

위의 커맨드 옵션의미 :

- `http :8000`: http 프로토콜을 8000번 포트로 이용
- `wsgi-file test.py`: test.py 라는 파일은 로드

8000번 포트를 통해서 브라우저에게 직접적으로 'hello world'라는 문자열을 서빙할 것이다. 들어가보자

```shell
# 나의 경우는 임대한 서버에서 진행하고 있기 때문에 curl 을 이용할 계획
curl 'http://localhost:8000'
```

'hello world'가 출력된다면 아래와 같이 통신을 하고있는 것이다

```
the web client <-> uWSGI <-> Python
```

#### Test your Django project

이제 우리는 uWSGI를 이용해서 같은 작업을 해 볼 것이다. 그러나 test.py대신에 장고 프로젝트를 직접 돌릴것이다.

```
python manage.py runserver 0.0.0.0:8000
```

장고 프로젝트가 작동된다면, uWSGI를 실행한다 : 

```
uwsgi --http :8000 --module mysite.wsgi
```

- `module mysite.wsgi`: 모듈을 로드한다

결과를 확인해보자. **It works** 가 제대로 출력된다면 **uWSGI가 가상환경으로 부터 Django application을 서빙하고 있는 것 이고**, 아래와 같이 통신을 하고 있는 것 이다

```
the web client <-> uWSGI <-> Django
```

우리는 일반적으로 uWSGI와 직접 통신하지는 않는다. 클라이언트와 uWSGI 사이에서 역할을 하는 것이 웹서버이다.

### Basic NginX

#### Install nginx

```shell
# NginX 설치 방법인데 이렇게 패키지 매니저를 통해 설치하는 것 보다
# 소스코드를 다운로드하여 수동으로 설치하자
sudo yum install nginx
sudo /etc/init.d/nginx start    # 서버 구동
```

웹 브라우저의 80번 포트를 이용해 통신함으로써 엔진엑스가 구동되고 있는지 확인해보자 - 작동된다면 "Welcome to nginx" 메세지가 출력 될 것이다. 아래와 같이 소통하고 있음을 의미한다.

```
the web client <-> the web server
```

이미 80번 포트를 사용중이라면 nginx 포트설정을 바꿔줘야한다. 이번 튜토리얼에서는 8000번 포트를 사용 할 것이다.

> 참고
>
> * 서버 구동 
>
> ~~~shell
> service nginx start
> # 또는
> sudo systemctl start nginx
> ~~~
>
> * 에러 발생시
>
> ~~~shell
> # [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
> # 와 같은 에러 발생은 80번 포트를 이미 사용하고 있기 때문에 발생함
> # 포트번호를 변경하거나 80번 포트를 사용하고 있는 프로세스를 kill..?
>
> sudo fuser -k 80/tcp
> ~~~

### Configure nginx for your site

nginx 디렉토리에서 이용 가능한 **uwsgi_params**을 필요로 할 것 이다. [https://github.com/nginx/nginx/blob/master/conf/uwsgi_params](https://github.com/nginx/nginx/blob/master/conf/uwsgi_params) 에서 복사한 뒤 사용하자

이제 **mysite_nginx.conf** 라는 이름으로 설정파일을 만들고 그 안에 다음과 같이 적는다 : 

```shell
# mysite_nginx.conf

# the upstream component nginx needs to connect to
upstream django {
    # server unix:///path/to/your/mysite/mysite.sock; # for a file socket
    server 127.0.0.1:8001; # for a web port socket (we'll use this first)
}

# configuration of the server
server {
    # the port your site will be served on
    listen      8000;
    # the domain name it will serve for
    server_name .example.com; # substitute your machine's IP address or FQDN
    charset     utf-8;

    # max upload size
    client_max_body_size 75M;   # adjust to taste

    # Django media
    location /media  {
        alias /path/to/your/mysite/media;  # your Django project's media files - amend as required
    }

    location /static {
        alias /path/to/your/mysite/static; # your Django project's static files - amend as required
    }

    # Finally, send all non-media requests to the Django server.
    location / {
        uwsgi_pass  django;
        include     /path/to/your/mysite/uwsgi_params; # the uwsgi_params file you installed
    }
}
```



위 와 같은 설정파일은 nginx가 파일시스템으로 부터 media file과 static file을 서빙하라고 말한다. 또한 장고의 개입을 필요로 하는 request를 처리한다. 좀 더 큰 스케일의 배포를 위해서 static/media 파일들을 처리하는 서버와 장고 어플리케이션을 관리하는 서버를 두는 것이 관습이지만, 지금은 이정도면 충분하다.



심볼릭 링크를 만들자 : 

```shell
sudo ln -s ~/path/to/your/mysite/mysite_nginx.conf /etc/nginx/sites-enabled/
```

> 참고
>
> * 심볼릭 링크 만들때 에러가 난다면 [여기](http://stackoverflow.com/questions/17413526/nginx-missing-sites-available-directory)를 참고하자

###  Deploying static files

nginx를 구동하기 전에, static folder에 장고의 static 파일들을 모아야 한다. 가장먼저 해야 할 것은 mysite 프로젝트의 settings.py 를 수정하는 것이다.

```shell
# 알아서 수정하자
STATIC_ROOT = os.path.join(BASE_DIR, "static/")
```

그리고 collectstatic 을 실행한다 : 

```
python manage.py collectstatic
```

### Basic nginx test

nginx를 재시작한다 : 

```
sudo /etc/init.d/nginx restart

or 

service nginx restart
```

미디어 파일이 제대로 서빙됐는지 확인하기위해, `/path/to/your/project/project/media directory` 경로에 media.png 이미지를 추가해보자. - 만약 작동한다면, nginx가 제대로 서빙하고 있다는 것을 의미한다.

It is worth not just restarting nginx, but actually stopping and then starting it again, which will inform you if there is a problem, and where it is.

## nginx and uWSGI and test.py

nginx가 test.py의 "hello world"를 출력하도록 해보자 : 

```
uwsgi --socket :8001 --wsgi-file test.py
```

다른옵션을 사용했다는 것을 제외하면 이전에 했던것과 매우 비슷하다

- `socket :8001`: use protocol uwsgi, qort 8001

nginx가 8001번 포트를 가지고 uWSGI와 통신하는 동안에 바깥에서는 8000번 포트를 가지고 통신한다

[http://example.com:8000/](http://example.com:8000/)

```
the web client <-> the web server <-> the socket <-> uWSGI <-> Python
```

Meanwhile, you can try to have a look at the uswgi output at [http://example.com:8001](http://example.com:8001/) - but quite probably, it won’t work because your browser speaks http, not uWSGI, though you should see output from uWSGI in your terminal.

8001번 포트로 요청을 하면 비어있는 응답을 한다

## Using Unix sockets instead of ports

지금까지 간단하기 때문에 TCP 포트 소켓을 이용했다. 그러나 Unix 소켓을 이용하는 게 더 낫다 - 오버헤드가 줄어들기 때문에

`mysite_nginx.conf`파일을 아래와 같이 수정하자 : 

```shell
server unix:///path/to/your/mysite/mysite.sock; # for a file socket
# server 127.0.0.1:8001; # for a web port socket (we'll use this first)
```

nginx를 재시작한다

그리고 uWSGI를 다한다 : 

```shell
uwsgi --socket mysite.sock --wsgi-file test.py
```

### If that doesn’t work

에러로그를 확인해보자.(/var/log/nginx/error.log)

```shell
connect() to unix:///path/to/your/mysite/mysite.sock failed (13: Permission denied)
```

nginx가 사용하기 위해서 소켓에 대한 권한 설정을 해주어야 한다.

Try:

```
uwsgi --socket mysite.sock --wsgi-file test.py --chmod-socket=666 # (very permissive)
```

or:

```
uwsgi --socket mysite.sock --wsgi-file test.py --chmod-socket=664 # (more sensible)
```

nginx가 소켓에 적절하게 읽고 쓰기를 가능하게 하기 위해서, nginx 그룹에 사용자를 추가해야한다.

**You may also have to add your user to nginx’s group (which is probably www-data), or vice-versa, so that nginx can read and write to your socket properly.**

> 참고
>
> permission deny 문제를 해결하지 못한 채 밑에 있는 .ini file 을 만들고 했더니 됐다.
>
> 이유를 모르겠음

## Configuring uWSGI to run with a .ini file

uWSGI에 사용했던 것 처럼 동일한 옵션을 사용할 수 있다. 이러한 방법은 설정을 다루는 데 있어서 더 편리하다

``mysite_uwsgi.ini``라는 이름의 파일을 만들어보자

```shell
# 각자의 환경에 맞게 바꿔줘야 한다
# mysite_uwsgi.ini file
[uwsgi]

# Django-related settings
# the base directory (full path)
chdir           = /path/to/your/project
# Django's wsgi file
module          = project.wsgi
# the virtualenv (full path)
home            = /path/to/virtualenv

# process-related settings
# master
master          = true
# maximum number of worker processes
processes       = 10
# the socket (use the full path to be safe
socket          = /path/to/your/project/mysite.sock
# ... with appropriate permissions - may be needed
# chmod-socket    = 664
# clear environment on exit
vacuum          = true
```

그리고 이 파일을 이용해서 uwsgi를 구동하자 : 

```
uwsgi --ini mysite_uwsgi.ini # the --ini option is used to specify a file
```

Once again, test that the Django site works as expected.

## Install uWSGI system-wide

지금까지 uWSGI는 가상환경에서만 사용했는데 배포를 위해서는 system-wide 한 설치를 필요로 한다

Deactivate your virtualenv :

```
deactivate
```

uWSGI를 system-wide 하게 설치 :

```
sudo pip install uwsgi

# Or install LTS (long term support).
pip install https://projects.unbit.it/downloads/uwsgi-lts.tar.gz
```

uWSGI 위키는 설치절차를 제공한다 : [installation procedures](https://projects.unbit.it/uwsgi/wiki/Install)

uWSGI를 system-wide 하게 설치하기 이전에, 어느 버전을 설치할 것인지와 어떤 방식으로 설치하는 게 가장 적절할 것인지에 대해 고민해보는 것이 좋다

uWSGI 구동 : 

```
uwsgi --ini mysite_uwsgi.ini # the --ini option is used to specify a file
```