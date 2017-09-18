# Amazon linux account settings

AWS에서 인스턴스를 생성하고 ssh를 통해 접속하려고 하면 root가 아닌 ec2-user로 접속해야하는데,

일단 공부의 편의를 위해서 root로 접속하는 방법을 알아보자.

* 일단 ssh접속을 위해 ec2-user로 접속한다

~~~shell
# /etc/ssh/sshd_config 파일을 수정한다
sudo vi /etc/ssh/sshd_config
~~~

~~~shell
#PermitRootLogin yes
라고 주석처리 되어있는 것을 주석해제
~~~

~~~shell
sudo cp .ssh/authorized_keys /root/.ssh/
sudo service sshd restart
~~~

를 하면 로그아웃 했다가 root계정으로 ssh를 통해 접속 할 수 있다

