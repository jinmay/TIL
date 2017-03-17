# Position property

* position

~~~css
static
/*태그가 위에서 아래로 순서대로 배치*/

relative 
/*초기 위치 상태에서 상하좌우로 위치 이동*/

absoulte
/*절대적 위치좌표 설정*/

fixed
/*화면을 기준으로 절대적 위치좌표 설정*/
~~~



* z-index : 숫자가 클수록 태그가 앞으로 올라온다

~~~css
/*z-index가 큰 #div1 사각형이 위로 올라와 #div2 사각형을 가리게 된다*/

#div1 {
  width: 100px; height: 100px;
  background: red;
  z-index : 999
}

#div2 {
  width: 100px; height: 100px;
  background: blue;
  z-index: 1
}
~~~



## 위치 속성과 관련된 중요한 공식!!!

~~~html
<!-- example.html -->

<body>
  	<p>Dummy Dummy Dummy Dummy Dummy</p>
	
  	<div class="wrap">
	    <div class='box'></div>
	    <div class='box'></div>
	    <div class='box'></div>
	</div>
  
	<p>Dummy Dummy Dummy Dummy Dummy</p>
</body>
~~~

~~~css
/*example.css*/

.wrap {
    position: relative;
    border: 1px solid black;
}

.box {
    width: 100px; height: 100px;
    position: absolute;
}

.box:nth-child(1) {
    background: red;
    left: 10px; top: 10px;
    z-index: 999
}

.box:nth-child(2) {
    background: green;
    left: 50px; top: 50px;
    z-index: 2
}

.box:nth-child(3) {
    background: blue;
    left: 90px; top: 90px;
    z-index: 1
}


p {
    border: 1px solid black;
}
~~~

위의 example.html 에는 두 가지 문제가 있다

>1. div 태그가 자리를 차지하지 못해서 p 태그가 붙어있다
>2. .box 가 자신의 부모(.wrap)를 기준으로 위치를 잡지 못한다

position 속성에 absolute 키워드를 적용하면 부모 태그가 영역을 차지하지 않는다. 따라서 자손의 position 속성에 absolute가 적용될 경우에는 부모 태그에 몇가지 처리가 필요하다

> 1. 자손의 position 속성에 absolute 키워드를 적용하면 부모의 position 속성에 height 속성을 입력한다

~~~ css
.wrap {
  height: 100px;
}
~~~

위 의 처리를 할 경우 부모 태그가 영역을 차지하게 된다

> 2. 자손의 position 속성에 absolute 키워드를 적용하면 부모의 position 속성을 relative 키워드로 지정한다

~~~css
.wrap {
  height: 100px;
  position: relavite;
}
~~~

이렇게 하면 자손 태그가 부모태그의 위치를 기준으로 절대좌표를 잡는다





## overflow property

내부의 요소가 부모의 범위를 벗어날 때 어떻게 처리할지 지정하는 속성이다

~~~css
hidden
/*영역을 벗어나는 부분을 보이지 않게 만든다*/

scroll
/*영역을 벗어나는 부분을 스크롤로 만든다*/
~~~



## float property

부유하는 대상을 만들 때 사용한다

~~~css
자주쓰는 키워드는 
left
right
~~~

초기에는 img 태그에 적용하는 것을 기본으로 만들어 졌으나 현대에는 레이아웃 구성할 때 자주 사용된다  

주의 : 여러태그에 right 키워드를 적용하는 경우 - 첫 번째 위치한 태그일수록 오른쪽으로 간다

~~~css
<div class='box'>1</div>
<div class='box'>2</div>

###################################################

.box {
    width: 100px; height: 100px;
    background: red;
    margin: 10px;
    padding: 10px;

    float: right;
}
~~~

#### 자손에 float 속성을 적용하면 부모의 overflow 속성에 hidden 키워드를 적용한다!!  

~~~css
body {
    width: 960px;
    margin: 0 auto;
}

#aside {
    width: 200px;
    float: left;
}

#section {
    width: 760px;
    float: left;
}

#wrap {
    overflow: hidden;
}

~~~





**Tip** : width 와 margin 값을 이렇게 주게되면 자동으로 중앙정렬 한다.(reset)

~~~css
body {
  width: 960px;
  margin: 0 auto;
}
~~~

