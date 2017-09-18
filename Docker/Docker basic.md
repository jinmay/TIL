# Docker basic

도커의 기본에 대해서 알아보자

우선 자주 사용하는 리눅스 배포판인 CentOS7 에서 설치한다

* 설치 on CentOS 7

~~~shell
sudo yum install -y docker # 안정화버전 설치
~~~

설치하는 방법에는 여러가지가 있지만 일단 가장 쉬운 yum 을 통해 설치한다

도커에서 중요한 핵심인 **도커 엔진**을 이루는 것에는 **도커 이미지**와 **도커 컨테이너**가 있다.

이 중에서 도커 컨테이너를 다루는 기본적인 사용법을 알아보자

* 도커 버전 확인

~~~shell
docker -v
# Docker version 17.03.2-ce, build 7392c3b/17.03.2-ce
~~~

도커의 업데이트 속도는 매우 빠르다고 한다. 빠르고 사소한 버전 차이로 인해서 중요한 기능을 사용할 수 없는 일도 발생할 수 있으니 버전 체크를 먼저 확인하는 것이 좋다

* 컨테이너 생성 및 실행

~~~shell
docker run -i -t centos
~~~

centos 이미지를 이용해 컨테이너를 생성하고 실행까지 하는 커맨드이다. **run** 커맨드는 컨테이너를 실행하는 명령어이며 **-i -t** 욥션에 주목하자. 두 옵션은 컨테이너와 상호 입출력을 가능하게 하는 기능을 가진다. 

만약 컨테이너를 생성하기 위한 이미지가 없다면 자동으로 pull 을 통해 이미지를 먼저 받고 생성 및 실행의 과정을 거친다

> -i : 상호 입출력
>
> -t : tty를 활성화하여 bash shell 사용

또한 docker run 명령어에서 위의 두 옵션 중 하나라도 사용하지 않았다면 bash shell이 정상적으로 작동하지 않는다

* 컨테이너 내부에서 빠져나오기

~~~shell
# 컨테이너 정지 후 빠져나오기
exit
ctrl + d

# 컨테이너를 정지하지 않고 빠져나오기
ctrl + p, q
~~~

컨테이너에서 나오는 방법에는 두 가지가 있다. 첫번째로 실행중인 컨테이너를 정지 후 빠져나오는 방법이고, 두 번째로는 실행중인 컨테이너를 정지하지 않은 채로 나오는 방법이다. 후자의 방법을 실제 어플리케이션 개발하는 목적으로 자주 사용한다고 한다. 

* 이미지 내려받기

~~~shell
docker pull <docker_image_name>
# docker pull centos:7
~~~

해당 이미지를 도커 공식 이미지 저장소에서 내려받는다

* 이미지 목록

~~~shell
docker images
~~~

도커 엔진에 존재하는 도커 이미지의 목록을 출력한다

* 도커 이미지 삭제

~~~shell
docker rmi <name>
~~~

* 컨테이너 생성

~~~shell
docker create -i -t --name example_img centos

# 컨테이너명 재설정
docker rename <name>
~~~

> —name 옵션은 컨테이너의 이름을 지정할 수 있다

run 명령과 비교했을때 차이점이 있다. run 명령은 컨테이너를 생성함과 동시에 내부로 들어가지만, **create** 명령은 컨테이너를 생성만 할 뿐이다. 따라서 컨테이너에 **attach / start** 명령을 통해 직접 들어가주어야 한다

create 명령어는 도커 이미지를 pull 한 뒤에 컨테이너를 생성만 할 뿐 docker start와 docker attach를 실행하지 않는다.

* 컨테이너 시작

~~~shell
docker start example_img
~~~

* 컨테이너 정지

~~~shell
docker stop example_img
~~~

* 컨테이너의 내부로 들어가기

~~~shell
docker attach example_img
~~~

* 컨테이너 목록 확인

~~~shell
docker ps # 실행중인 컨테이너만 표시
docker ps -a # 생성된 모든 컨테이너 표시
~~~

* 컨테이너 삭제

~~~shell
docker rm <name>
# docker rm -f <name>
~~~

한번 삭제한 컨테이너는 살릴 수 없기 때문에 조심해야 한다!!

또한 실행중인 컨테이너는 바로 삭제할 수 없다. 삭제하기 위해서는 컨테이너를 정지시키거나 **-f 옵션을 주어 강제로 삭제해야 한다**

* 모든 컨테이너 삭제

~~~shell
docker container prune
~~~

* 컨테이너 상태 확인

~~~shell
docker stats <name>
~~~

