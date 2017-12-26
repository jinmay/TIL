# git config

### git의 설정인 gitconfig에 대해서 알아보자

보통은 각 계정의 홈 디렉토리에 **.gitconfig** 라는 파일로 설정한다.

전역적으로 설정하는 경우와 각 git repository 마다 설정하는 경우가 있으며, git 전반적으로 필요한 것들은 전역적으로 하곤한다.

1. 사용자 이름

~~~bash 
git config --global user.name "<Username>"
~~~

2. 사용자 이메일

~~~bash
git config --global user.email <User_email>
~~~

3. branch 색상을 자동으로 설정

~~~bash
git config --global color.branch auto
~~~

4. git diff 색상을 자동으로 설정

~~~bash
git config --global color.diff auto
~~~

5. 대화식 command의 폰트색상을 자동으로 설정

~~~bash
git config --global color.interactive auto
~~~

6. git status 명령어 입력시 폰트 색상 자동설정

~~~bash
git config --global color.status auto
~~~

7. commit 시 편집기를 **vim**으로 설정

~~~bash
git config --global core.editor "vim"
~~~

8. git merge 툴을 **vimdiff**로 설정

~~~bash
git config --global merge.tool vimdiff
~~~

