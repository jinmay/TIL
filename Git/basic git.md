# Git

###  버전관리 시스템

* 간략한 절차
  1. 새로운 파일이 생성되거나 기존파일이 수정된 상태
  2. **stage area**에 올려서 commit 대기상태로 만들기 ( git add)
  3. git commit
  4. repository에 commit이 된 결과들이 저장됨

### Manual

​	git <commend> <options> —help







## 제대로 써보자

* git directory 생성

~~~shell
git init
~~~

* 현재상태 확인

~~~shell
git status
~~~

* stage area로 올리기

~~~shell
git add <파일명>
# 모든파일에 적용
# git add .
~~~

* commit 하기

~~~shell
git commit -m "commit message"

# 주의
# 한번도 add 되지않은 파일은 git commit -am 'msg' 가 실행되지 않는다
~~~

~~~shell
# 옵션없이 그냥 commit 하면 편집기가 실행되면서
# 더 자세하게 메세지를 적을 수 있다
git commit
~~~

* 현재까지 커밋한 내역확인

~~~shell
git log
~~~

수정하고 add 하기 전 : 변경내용 확인

~~~shell
git diff
~~~

각 커밋은 고유한 ID를 가지고 있고 git log < commit id > 를 통해서 로그를 볼 수 있다

~~~shell
# 두 버전간의 차이점 비교
git diff <version1 id>..<version2 id>
~~~

모든 커밋사이의 소스상의 변경사항 출력

~~~shell
git log -p

# 이때 볼 수 있는 /dev/null의 의미는
# 이전 내역에서는 없었던 것을 말한다
~~~





#### git은 커밋함으로써 작업내용을 저장할 뿐 만 아니라 과거로 돌아갈수도 있다

대표적인 두 가지

1. reset - 초기화
2. revert - 취소하고 새로운 버전 생성





## branch

분기하다

작업도중에 분기하는 것을 뜻함



### 기본 사용법

* 새로운 브랜치 생성

~~~shell
git branch <name>
~~~

**새로운 브랜치를 생성하게 되면 현재 속해있는 브랜치의 상태를 그대로 가지게 된다**

브랜치 리스트 출력

~~~shell
git branch

# '*'로 표시되어 있는게 현재 브랜치
~~~

* 브랜치 변경

~~~shell
git checkout <name>
~~~



#### tip

~~~shell
# 브랜치 생성 후 자동전환
git checkout -b <name>

# 브랜치 삭제
git branch -d
~~~



* 브랜치 정보확인

~~~shell
git log --branches 
# 모든 브랜치의 로그 출력
# 그러나 브랜치를 구분해놓지 않음

git log --decorate
# 브랜치를 구분함

git log --graph
# 브랜치 분기상태를 그래프로 나타냄

git log --online
# 라인 하나로 알려줌

git log --branches --decorate --graph
~~~

* 브랜치 커밋 차이 이력

~~~shell
git log master..exp
# master에는 없고 exp에는 있는거

git log -p exp..master
# exp에는 없고 master에는 있으면서
# 소스상의 변경까지 출력
~~~

* 브랜치 합병 중 일어나는 몇가지 상황

  1. Fast_Forward : 커밋을 생성하지 않고 master 가 움직임
  2. recursive : 앞의 두 커밋을 가리키는 새로운 커밋이 생성

  ​