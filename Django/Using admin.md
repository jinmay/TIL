# How to use admin.py usefully 

* 레코드 리스트 항목 지정

~~~python
list_display = ['question_text', 'created_by']
~~~

* Question 과 Choice 를 한 화면에서 수정하기

~~~python
class ChoiceInline(admin.TabularInline): # 테이블 형식으로 나타내기
    
class ChoiceInline(admin.StackedInline): # 밑으로 나열됨
    model = Choice
    extra = 2 # Question 변경 화면에서 보일 Choice의 갯수
    
class QuestionAdmin(admin.ModelAdmin):
    inlines = [ChoiceInline]
    

~~~

* 필드순서 변경

~~~python
# admin 페이지에서 Question 테이블 수정할 때
fields = ['created_by', 'question_text']
~~~

* 필드 분리하기

~~~python
fieldsets = [
  ('Question statement', {'fields': ['question_text']}),
  ( 'Date Information', {'fields': ['created_by']}),
]
~~~

* 필드 접기

~~~python
fieldsets = [
  ('Date Information', {'fields': ['question_text'], 'classes': ['collapse']}),
]
~~~

* list_filter : UI 화면에 필터 사이드 바 추가

~~~python
list_filter = ['pub_date']
~~~

* search_fields : UI화면에 검색박스 추가

~~~python
search_fields = ['question_text']
~~~