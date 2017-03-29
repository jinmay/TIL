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
* 자동으로 채워지는 필드 : name필드에 맞춰서 slug 필드의 내용이 채워짐

~~~python
prepopulated_fields = {
  'slug': ('name', )
}
~~~







### admin 적용방식

1. ~~~python
   # admin.py
   # register 함수 이용
   admin.site.register(Bookmark)
   ~~~

2. ~~~python
   # admin.py
   # 데코레이터 이용
   @admin.register(Bookmark)
   class BookmarkAdmin(admin.ModelAdmin):
       list_display = ['id', 'title']
   ~~~



### Custom actions

선택된 레코드들에 대해 한번에 액션을 주기위해서 자주 사용된다

**모든 액션은 첫 번째 인자로 request를 받고 두번째 인자로 queryset을 받아야 한다!!**

~~~python
@admin.register(Bookmark)
class BookmarkAdmin(admin.ModelAdmin):
    list_display = ['id', 'title']
    actions = ['make_something']
    
    def make_something(self, request, queryset):
        pass
    # 액션 이름은 메소드명과 같은데 바꿔주고 싶으면
    self.short_description = "Change status to something"
~~~

