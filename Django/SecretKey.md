# SecretKey

django 프로젝트를 새로 생성하게되면 기본적으로 환경설정에 대한 파일인 settings.py가 생성된다. 프로젝트의 보안과 관련된 항목에 **SECRET_KEY**가 있는데 이 와 관련하여 정리해보자

기본적으로 보안과 관련된 설정들은 public repo 에는 업로드 되어서는 안된다.~~(물론 지금까지 공개해왔다)~~

나의 경우 이미 시크릿 키를 처리하지 않은채 push했기 때문에 프로젝트를 새로 생성하던가 시크릿 키를 변경해야한다.

* 시크릿 키 변경

  1. 수동으로 시크릿 문자열 생성 [(코드출처)](https://github.com/honux77/inflearn-django/blob/master/script/genkey.py)

     ~~~python
     import string, random

     # Get ascii Characters numbers and punctuation (minus quote characters as they could terminate string).
     chars = ''.join([string.ascii_letters, string.digits, string.punctuation]).replace('\'', '').replace('"', '').replace('\\', '')

     SECRET_KEY = ''.join([random.SystemRandom().choice(chars) for i in range(50)])

     print(SECRET_KEY)
     ~~~

     2. SecretKey Generator 사용

     [주소](https://www.miniwebtool.com/django-secret-key-generator/)

     ​

* 시크릿 키 분리

SECRET_KEY 값을 분리하는 방법에는 주로 두 가지가 있다

1. 환경변수패턴

로컬 환경에서 한다고 가정한다면 각자 사용하는 쉘의 설정파일에 세팅해주면 된다(.zshrc / .bashrc)

~~~shell
# vi ~/.zshrc
export OUTER_SECRET_KEY='django_secret_key'

# 환경변수 확인
# source ~/.zshrc 또는 쉘을 재시작해야 할 수도 있다
echo $OUTER_SECRET_KEY
~~~

.zshrc 또는 .bashrc 에서 위와 같이 설정한 후 장고의 settings.py을 열어 시크릿 키를 변경해준다

~~~python
# settings.py
import os

SECRET_KEY = os.environ['OUTER_SECRET_KEY']
~~~





2. 비밀파일패턴

JSON 또는 YML 같은 설정파일에 키를 입력하여 관리하는 방식이다.

~~~json
# secret.json

{
    "OUTER_SECRET_KEY": "django_secret_key"
}
~~~

그리고 버전관리 시스템에 저장되지 않도록 **.gitignore**에 꼭 추가해 두고, 장고의  settings.py에 다음 코드를 추가한다

~~~python
import os, json
from django.core.exceptions import ImproperlyConfigured

# secret.json 파일의 위치
secret_file = os.path.join(BASE_DIR, 'secret.json')

with open(secret_file) as f:
  secret = json.loads(f.read())
  
def get_secret(setting, secret=secret):
  try:
    return secret[setting]
  except KeyError:
    error_msg = "Set the {} environment variable".format(setting)
    raise ImproperlyConfigured(error_msg)
    
SECRET_KEY = get_secret("SECRET_KEY")
~~~





-----------------

<참고>

[https://wayhome25.github.io/django/2017/07/11/django-settings-secret-key/](https://wayhome25.github.io/django/2017/07/11/django-settings-secret-key/)

[https://dayone.me/20WHzqH](https://dayone.me/20WHzqH)