# Bootstrap 기초

Twitter 에서 개발한 오픈 소스 웹 디자인 프레임워크이다

공식 홈페이지 : [Bootstrap](http://getbootstrap.com)





## 적용하기

1. **CDN 이용**

example 

~~~html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <link rel="stylesheet" href="test.css">
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/css/bootstrap.min.css">
    </head>
    <body>
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>
        <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/js/bootstrap.min.js"></script>
    </body>
</html>
~~~

주의 : bootstrap 내부는 순수한 JavaScript가 아닌 jQuery로 구성되어있다. 그래서 반드시 bootstrap를 사용하기 전에 jQuery를 먼저 불러와야 한다!



2. **직접 link로 포함**

bootstrap 사이트에서 받은 js, css 파일을 각 폴더에 맞게 넣기

**bootstrap의 js 파일과 jQuery 파일은 <body> 태그 맨 마지막에 로드시키자**

