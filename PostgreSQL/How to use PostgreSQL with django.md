# How to use PostgreSQL with django

In **settings.py**

~~~python
# 집에서 공부할때 쓰던 세팅
# 실 서비스 배포하고 운영할땐 다르게 해야한다
# 시에라 사용할때 기준..!

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2', # psycopg2는 설치되어 있는 상태야 함
        'NAME': '<database_name>',
        'USER': '<user_name>',
        'PASSWORD': '',
        'HOST': 'localhost',
        'PORT': '',
    }
}
~~~

