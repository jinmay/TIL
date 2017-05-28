* SlugField

~~~python
class SlugField(max_length=50, **options)
~~~

admin.py 에서 prepopulated_fields 와 자주 사용된다

따로 정의하지 않는다면 max_length 는 100으로 그리고 Field.db_index 는 True로 설정된다

~~~python
# 필드옵션
allow_unicode # 유니코드 사용가능
~~~







