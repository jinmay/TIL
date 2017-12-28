# npm

node package manager

노드를 설치할 때 함께 설치되며 각종 패키지들을 설치하고 관리하는 데 도움을 주는 도구이다.

~~~bash
node -v # 노드 버전 확인
npm -v # npm 버전 확인
~~~

npm를 이용하여 패키지를 설치하는 가장 간단한 방법은

* npm install \<package name>

프로젝트 루트의 **node_modules** 이라는 디렉토리에 모든 로컬 모듈이 설치된다.

프로젝트에 설치하고 사용하는 패키지를 의존성이라고 부른다. 프로젝트의 규모가 커지면 사용한 패키지도 늘어나기 때문에 **의존성 관리**가 필요하기 마련인데 **package.json**을 통해 손쉽게 할 수 있다.

* package.json

직접 만들어도 되지만 아래와 같은 명령어를 통해 손쉽게 생성할 수 있다.

~~~bash
npm init
~~~

