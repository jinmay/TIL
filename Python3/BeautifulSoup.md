# BeautifulSoup

계층적인 HTML문서를 parsing 하기위한 라이브러리

> requests를 통한 응답 : "페이지 소스보기"를 통해 HTML 확인
>
> 개발자도구에서의 소스는 각각의 브라우저가 해석한 DOM Tree 내역이다
>
> 그래서 개발자도구를 통해 보는 소스와 서버가 최초로 응답한 (또는 requests을 통해 얻은) 소스가 다들 수 있다!



* 복잡한 HTML 문자열에서 정보를 가져오는 방법

1. 정규표현식
   * 가장 빠른 처리가 가능하지만 정규표현식 규칙을 만드는 것이 번거롭고 어렵다
   * 간혹 필요함
2. HTML Parser 라이브러리 활용
   * DOM Tree을 탐색하는 방식이므로 쉽다
   * 예를 들면 BeautifulSoup4 / lxml



~~~python
# 간단한 bs4 example
from bs4 import BeautifulSoup
soup = BeautifulSoup('http://www.naver.com', 'html.parser') # url과 parser종류 설정

for li in soup.select('li'):
  print(li)
~~~

HTML Parser 로는 대표적으로 두 가지를 사용한다

 	1. html.parser
     - python으로 작성된 파서
     - 외부 라이브러리를 사용 할 수 없거나 간단하게 사용하고자 할때 자주 이용
     - bs4 내장파서
	2. lxml
    * 외부 C 라이브러리
    * html.parser 보다 유연하고 빠르다
    * 제대로 HTML로 마크업이 안 되어있을 경우 사용하곤 한다



#### 태그를 찾는 방법

마찬가지로 두 가지가 있다

1. find 함수를 통해 하나씩 찾기
2. 태그 관계를 지정하여 찾기 - CSS Selector

