# clone 할 때

>  원격지로부터 clone 하고 git branch를 통해 브랜치 리스트를 보려고 했으나 달랑 master 브랜치밖에 딸려오지 않았다  
>
>  clone 하면 push 했던 브랜치까지 한꺼번에 오는 줄 알았는데 아니었다
>
>  이럴 땐
>
>  git checkout -t [원격저장소 branch 이름] 
>
>  하면 원격지의 branch를 가져오는 것과 동일한 기능한 한다고 한다



**git checkout -t [원격저장소의 branch 이름]**



찾아본 바 위 와 같이 원격저장소의 모든 브랜치가 한꺼번에 따라오지 않는 이유는

**로컬 브랜치와 원격저장소의 브랜치를 따로 관리하기 위함** 이라고 한다







**또 다른 Tip**

원격저장소의 브랜치 목록 확인

~~~shell
git branch -r
# remote의 약자가 아닐까 싶다
~~~

로컬환경의 브랜치 목록 확인

~~~shell
git branch -l
# local의 약자가 아닐까 싶다
~~~

모든 브랜치 확인

~~~shell
git branch -a
# all의 약자인 거 같다
~~~





참고

[https://blog.outsider.ne.kr/641](https://blog.outsider.ne.kr/641)