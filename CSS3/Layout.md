	# Layout

공간을 분할할 때는 대부분 이러한 순서를 거친다

1. 웹 페이지 구상
2. 구성 영역 분리
3. **구성 영역을 행 단위로 분리**
4. 나누어진 행의 내부 요소를 분리



## 초기화

모든 HTML 페이지의 첫 번째 스타일시트는 초기화 코드로 시작하며 모든 웹 브라우저에서 동일한 출력을 만드는 데 사용된다

* reference

[Eric Meyer's Reset CSS](http://meyerweb.com/eric/tools/css/reset)

[HTML5 Doctor CSS Reset](http://html5doctor.com/html-5-reset-stylesheet)

[YUI 3 Reset CSS](http://developer.yahoo.com/yui/3/cssreset)



example

~~~css
* {
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Times New Roman', sans-serif;
}

li {
  list-style: none;
}

/*a 태그의 밑줄을 제거해 준다*/
a {
  text-decoration: none;
}

/*a 태그를 걸게되면 img 태그에 테두리가 표시되는데 이를 없애기위해 사용*/
img {
  border: 0;
}

~~~



