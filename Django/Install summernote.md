# Summernote 설치

**Summernote는 간편한 WYSIWYG 에디터이다**

### Setup

1. pip로 간편하게 설치 할 수 있다

~~~shell
# pip3 와 같이 버전 조심!
pip install django-summernote
~~~

2. settings.py의 **installed_apps**에 추가

~~~python
'django_summernote'
~~~

3. 프로젝트의 urls.py 수정

~~~python
url(r'^summernote/', include('django_summernote.urls'))
~~~

4. 첨부파일을 위한 적절한 MEDIA_URL 설정

~~~python
MEDIA_URL = 'media'
~~~

5. 마이그레이션

~~~python
python manage.py migrate
~~~



### 적용법

* admin 페이지에 적용하기

~~~python
# summernote admin 로드
from django_summernote.admin import SummernoteModelAdmin
from .models import SomeModel

# Apply summernote to all TextField in model.
class SomeModelAdmin(SummernoteModelAdmin):  # instead of ModelAdmin
    ...

admin.site.register(SomeModel, SomeModelAdmin)
~~~

* forms

~~~python
from django_summernote.widgets import SummernoteWidget, SummernoteInplaceWidget

# Apply summernote to specific fields.
class SomeForm(forms.Form):
    foo = forms.CharField(widget=SummernoteWidget())  # instead of forms.Textarea

# If you don't like <iframe>, then use inplace widget
# Or if you're using django-crispy-forms, please use this.
class AnotherForm(forms.Form):
    bar = forms.CharField(widget=SummernoteInplaceWidget())
~~~

