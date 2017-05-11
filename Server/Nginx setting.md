# Nginx 설치

**yum과 같은 패키지 매니저 말고 소스코드를 수동으로 다운로드해 컴파일 하는 경우**

> 저장소에 원하는 패키지가 없을수도 있어서.
>
> 주요 컴파일 옵션들을 다양하게 설정할 필요가 있어서.
>
> 레드마인 설치 용이

본격적인 설치에 앞서서 필요한 라이브러리에 대해 알아보자

* gcc
* pcre : 펄 호환 정규표현식
* zlib : 압축 
* openssl

~~~shell
yum install -y gcc

yum install -y pcre pcre-devel

yum install -y zlib zlib-devel

yum install -y openssl openssl-devel
~~~



1. repo. 등록

~~~shell
# vi /etc/yum.repos.d/nginx.repo

[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/OS/OSRELEASE/$basearch/
gpgcheck=0
enabled=1

# Replace “OS” with “rhel” or “centos”, depending on the distribution used, 
# and “OSRELEASE” with “5”, “6”, or “7”, for 5.x, 6.x, or 7.x versions, respectively.
# "os" 와 "osrelease" 에는 자신의 환경에 맞게 바꿔야함
~~~

repo. 에 대한 정보는 [here](http://nginx.org/en/linux_packages.html#stable)

~~~shell
yum install nginx
~~~

* 서버 구동

~~~shell
sudo systemctl start nginx
~~~

* 부팅 시 자동으로 구동

~~~shell
sudo systemctl enable nginx
~~~

* nginx 설정 문법 체크

~~~shell
nginx -t -c /etc/nginx/nginx.conf
~~~









<hr>

##### 참고 사이트

[http://altkeycode.tistory.com/10](http://altkeycode.tistory.com/10)

