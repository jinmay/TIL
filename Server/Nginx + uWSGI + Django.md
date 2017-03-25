# Nginx + uWSGI + Django

#### 웹 서버 Nginx 와 Django 웹 어플리케이션을 연동하기 위함

현재 사용중인 환경

>  centos7
>
> python3.6
>
> Django1.10
>
> virtualenv
>
> Nginx





1. uWSGI 설치

virtualenv 가상환경이 활성된 상태에서

~~~shell
# Django 설치
pip3 install django

# 프로젝트 생성 후 이동
django-admin startproject test_project

# uWSGI 설치
pip3 install uwsgi
~~~

uwsgi가 작동하는지 test

~~~shell
# uwsgi 작동 테스트
def application(env, start_response):
	start_response('200 OK', [('Content-Type', 'text/html')])

	return [b"Hello world"]
~~~

~~~shell
uwsgi --http :8000 --wsgi-file test.py
# curl http://127.0.0.1:8000 으로 터미널 상에서 확인 가능
~~~

hello world 가 제대로 출력된다면

#### Web Client <-> uWSGI <-> Python

이 통신하고 있다고 보면 된다

uWSGI로 변경시키면

~~~shell
# 프로젝트 이름.wsgi
uwsgi --http :8000 ---module test_project.wsgi
~~~

#### Web Client <-> uWSGI <-> Django

가 통신하고 있다





## Nginx setting

virtualenv 폴더로 이동해 test_project_nginx.conf 를 작성한다

~~~shell
# nginx.conf
upstream django {
# connect to this socket
# server unix:///tmp/uwsgi.sock;    # for a file socket
server 127.0.0.1:8001;      # for a web port socket
}

server {
    # the port your site will be served on
    listen      8000;
    # the domain name it will serve for
    server_name localhost;   # substitute your machine's IP address or FQDN
    charset     utf-8;

    #Max upload size
    client_max_body_size 75M;   # adjust to taste

    # Django media
    location /media  {
		    alias /root/desktop/vir/first/media;      # your Django project's media files
    }

    location /static {
		    alias /root/desktop/vir/first/static;     # your Django project's static files
    }

    # Finally, send all non-media requests to the Django server.
    location / {
	    uwsgi_pass  django;
	    include     /root/desktop/vir/first/uwsgi_params; # or the uwsgi_params you installed manually
    }
}
~~~

##### 각자 환경에 맞게 바꿔야 한다

~~~python
STATICROOT = os.path.join(BASE_DIR, 'static/')
~~~

~~~shell
python manage.py collectstatic
~~~

