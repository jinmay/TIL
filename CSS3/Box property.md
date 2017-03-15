# 박스 속성

**width**와 **height**는 비교적 무난하니 넘어가자



## border

테두리와 관련 돼 있는 속성이다

~~~css
div {
  border: 1px solid red;
  /* 	  두께 모양   색상 */
}
~~~

* border-style
* border-color
* border-width(두께)
* border-radius : 둥근 테두리 만들기

~~~css
div {
  border-radius: 50px 40px 30px 20px;
  /* 왼쪽위 오른쪽위 오른쪽아래 왼쪽아래 */
}
~~~







## padding

**border**를 기준으로 안쪽 여백을 의미한다

~~~css
div {
  padding: 1px 1px 1px 1px; /* 롱핸드 방식 : 시계방향 순서 , 상우하좌 */
  padding: 1px 1px; /* 상하, 좌우 */
  padding: 1px; /* 상하좌우 */
  
  /* 따로 지정가능 */
  padding-top: 10px; 
}
~~~





## margin

**border**를 기준으로 바깥 여백을 의미하고 실제 박스의 크기에는 영향을 미치지 않는다

~~~css
div {
  margin: 1px 1px 1px 1px; /* 롱핸드 방식 : 시계방향 순서 , 상우하좌 */
  margin: 1px 1px; /* 상하, 좌우 */
  margin: 1px; /* 상하좌우 */
  
  /* 따로 지정가능 */
  margin-top: 10px; 
}
~~~

