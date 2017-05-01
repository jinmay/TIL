# Bootstrap4 ruby gem

* 레일즈5와 부트스트랩4를 사용해보자

[bootstrap-rubygem github](https://github.com/twbs/bootstrap-rubygem)

1. Gemfile 에 bootstrap 추가

~~~shell
gem 'bootstrap', '~> 4.0.0.alpha6'
~~~

최소한 sprockets-rails v2.3.2. 가 설치되어있어야 한다

2. bundle install

~~~shell
bundle install 
~~~

추가된 bootstrap을 볼 수 있을것이다

3. app/assets/stylesheets/application.scss 의 내용을 수정해준다

**만약 scss가 아닌 css만 있다면 scss로 고쳐주고 수정한다**

~~~shell
# application.scss

@import "bootstrap";
~~~

**확장자가 .scss 인지 꼭 확인하자!**

그리고 *= require 와 *= require_tree 를 삭제한다

4. app/assets/javascripts/application.js 의 내용을 수정

~~~shell
//= require jquery
//= require bootstrap-sprockets
~~~

를 추가해준다

순서에 주의!!



5. **(선택사항)** Gemfile에 추가해준다

tether에 의존성을 가지고 있는 Tooltips 와 popovers 를 사용하기위해 추가

~~~shell
source 'https://rails-assets.org' do
  gem 'rails-assets-tether', '>= 1.3.3'
end
~~~

application.rb 에 이것또한 추가해준다

~~~shell
//= require jquery
//= require tether   # 추가된 파일
//= require bootstrap-sprockets
~~~

6. bundle install 로 마무리