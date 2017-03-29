# Migrations

##### 주요 명령어

* makemigrations

  ​	마이그레이션 파일 생성

* migrate

  ​	생성된 마이그레이션 파일 DB에 반영







#### Forward / Backward

현재 마이그레이션 진행상황에 따라 정방향과 역방향 마이그레이션 두 가지 상황이 생기게 된다

———zero---------

bookmark/0001

bookmark/0002

bookmark/0003

bookmark/0004  <— 현재 마이그레이션 상태

bookmark/0005

bookmark/0006

——end------------

1. 현재 0004까지 마이그레이션 된 상태일때,
   * python manage.py migrate 0005 를 입력하게 되면 **0005까지 정방향 마이그레이션을 하게 된다**(0006은 미반영)
   * python manage.py migrate 0003 을 입력하게 되면 **0004는 취소가되며 0003까지만 migrate 하게된다**
2. 모든 마이그레이션 취소는,
   * python manage.py migrate <app name> zero
   * 위 의 경우는 python manage.py migrate bookmark zero 하게 되면 모든 마이그레이션이 취소된다



### <지정된 마이그레이션 파일> 이 <현재 위치한 마이그레이션 파일> 보다 

이후라면, **정방향 마이그레이션**이,

이전이라면, **역방향 마이그레이션(롤백)**이 수행된다





### models 클래스의 모든 필드에서 공통적으로 사용될 수 있는 옵션은 *blank*와 *null* 이다.

black는 form validation과 관련이 있으며 null은 DB 옵션이라고 생각하면 된다





기존 모델에 필수필드를 추가하게 되면

1. 값을 입력
2. 모델 클래스를 수정하여 디폴트 값을 설정

하라고 경고한다

그 이유는 기존에 있던 레코드들에는 새로 추가된 필수필드가 없었기 때문인데 경고문과 같이

1. 원래 있던 레코드의 새로운 필드에 값을 추가하거나
2. 모델 클래스를 수정하여 default 값을 넣어줘야 한다 