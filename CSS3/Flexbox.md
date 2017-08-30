# Flexbox

09년에 나왔다고는 하지만 17년이 되도록 존재조차 몰랐던 Flexbox에 대해서 알아보자

플렉스박스는 CSS의 display 속성을 flex로 줌으로써 html의 레이아웃을 표현하는 방식이라고 한다

~~~css
display: flex;
~~~

첫째로 flexbox 레이아웃을 구성할 때는 부모요소에 flex를 지정해야 한다

~~~css
.container {
    display: flex;
}
~~~

~~~html
<div class="container">
    <div class="box"></div>
    <div class="box"></div>
    <div class="box"></div>
</div>
~~~

display: flex 로 인해서 div.container 는 flex container 가 되었으며 컨테이너 안의 div.box 들은 flex 항목이 된 것이다

