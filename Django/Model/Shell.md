# Django shell

일반 python shell에 장고 프로젝트 설정이 로딩되지 않았기 때문에 장고 환경에 접근할 수 없다

따라서 python manage.py shell을 통해 파이썬 쉘을 이용하는게 일반적이다

> 이때 ipython[notebook] 이 설치되있다면 ipython 이 구동된다







python manage.py shell_plus로 장고 쉘을 실행할 수 있는데 필요한 모델들을 자동으로 import 해주기 때문에 편리하다  
(shell_plus 는 django_extensions에서 지원한다)

**python manage.py shell_plus —notebook**을 입력할 시 Jupyter notebook이 실행된다

* Jupyter notebook 을 실행하는 방법에는 두 가지가 있다
  1. jupyter notebook
  2. python manage.py shell_plus —notebook **현재 장고 프로젝트를 로딩**

shell_plus —notebook 은 현재 장고프로젝트를 자동으로 로딩해 주기 때문에 편리하다

