day1 date: 2017-1-9 
#HTML

###Use Markdown
Links for more details:

1. [Wiki] (<https://en.wikipedia.org/wiki/Markdown>)

2. [github] (<https://guides.github.com/features/mastering-markdown/>)





## Abbreviation syntax
* 참고 링크 [Emmet] (<http://docs.emmet.io/abbreviations/syntax/>)  
	
#### Elments
>: element의 이름으로 html tags생성

ex) `<div>` 또는 `p` -> `<div></div>` `<p></p>`

#### Nesting Operators
#####Child > 
>: Nest inside

ex) `div>ul>li` ->
```<div><ul><li></li></ul></div>
```
#####Sibling +
>: Same Level

ex) `div+p+bq`->
`<div></div><p></p><blockquote></blockquote>`
#####Climb-up ^
>: 상위레벨로 복귀

ex)`p>span+em^<span>`->
```
<p>
 	<span></span>
 	<em></em>
	 </p>
	 <span>
	 </span>
```
	 
		 
**여러번 사용도 가능 ^^^**

ex) `div+div>p>span+em^^^bq`
	
```
<div></div>
<div>
 <p>	
  <span></span><em></em>
 <p>
</div>
<blockquote></blockquote>
```


#####Muliplication * 
>: 같은 element 반복

ex) `ul>li*5`

```		 
		 <ul>
		 	<li></li>
		 	<li></li>
		 	<li></li>
		 	<li></li>
		 	<li></li>
		 </ul>
```

#####Grouping ()
>: 그룹핑

ex) `div>(header>ul>li*2>a)+footer>p`

```		 
		 <div>
		 	<header>
		 		<ul>
		 			<li><a></a></li>
		 			<li><a></a></li>
		 		</ul>
		 	</header>
		 	<footer>
		 		<p></p>
		 	</footer>
		 </div>
```
		 
**()안에 (), *와도 같이 사용가능**

ex) `(div>dl>(dt+dd)*3)+footer>q`

```	
		 <div>
		 	<dl>
		 		<dt></dt>
		 		<dd></dd>
		 		<dt></dt>
		 		<dd></dd>
		 		<dt></dt>
		 		<dd></dd>
		 	</dl>
		 </div>
		 <footer>
		 	<p></p>
		 </footer>
```
	
####Attribute operators
>요소들의 attrs 설정 (id, class, title etc)

#####ID 와 Class
>: # -> ID and . -> Class

>: elem#id -> id="" and elem.class -> class=""

ex) `div#header + div.page + div#footer.class1.class2.class3`

```		 
		 <div id="header"></div>
		 <div class="page"></div>
		 <div id="footer" class="class1 class2 class3></div>
```
		 
#####Custom Attributes
: elem[attr]
	ex) `td[title ="hello world" colspan=3]`
	
		 `<td title = "hello world" colspan ="3"></td>`
	
* 여러 attrs을 하나의 []안에 넣어도 무관
* "" = ''
* attr에 따른 value 지정하지 않아도 attr="" 으로 생성

#####Item numbering $
ex) `ul>li.item$*5`

```		
		 <ul>
		 	<li class="item1></li>
		 	<li class="item2></li>
			<li class="item3></li>
			<li class="item4></li>
		 	<li class="item5></li>
		 </ul>
```
		 
* item$$$*4 - 자릿수 001, 002 003...

######Numbering base와 오름/내림차순 변경
: @ 사용
ex)` ul>li.item$@-3*5` (base:3,4,5,6,7 -:reverse)

```
		 <ul>
		 	<li id="item7"></li>
			<li id="item6"></li>	
			<li id="item5"></li>
			<li id="item4"></li>
			<li id="item3"></li>
		 </ul>
```
		 
####Text {}
`{txt}`사용하여 txt삽입

ex) `a{Click me}` -> `<a href="">Click me</a>`
		
ex) `a{click}+b{here}` -> `<a href="">click</a><b>here</b>`

ex) `a>{click}+b{here}` -> `<a href="">click<b>here</b></a>`

* 단, +로 연걸되지 않고 단독으로 상속 시에는 

ex) `a{click}` = `a>{click}` -> `<a href="">click</a>`
		
ex) `p>{Click}+a{here} + {to continue}` -> `<p> Click <a href=""> here</a> to continue </p>`


day2 date: 2017-1-10 

#HTML (cont)
## Block과 Inline(요소의 형태)
####block
:width = 전체너비

`<h>`, `<p>`, `<div>`
####Inline
:width = text length, 줄바꿈 x

`<strong>`, `<a>`, `<span>`

####Layout
``<div>
<p>블록 내부<span> 인라인</span>블록 내부</p>
</div>``
	
##HTML tags
####Headline
`h1`,`h2`,...,`h6`

`h1`:biggest, `h6`:smallest

####Line Breaks(줄바꿈)
* `<p>~~~~</p>`

    ***공백***
 
  `<p>~~~</p>`
  
두개의 `<p>`태그 사이에는 공백이 존재

* `~~~~~~<br>`

 ` ~~~~~~<br>`
 
`<br>`은 바로 아래 줄이 옴. 바로 줄 바꿈

#####ETC
* `<hr>`:수평선
* `<blockquote></blockquote>`:인용문
* ``<pre>
    def pretag_test();
		val ='pretag'
		</pre>``
	:preformatted texts
* strong, b, em ,i, mark :강조, 볼드, 이탈릭,하이라이트

####Anchor: a tag

`<a href="이동할 페이지 주소" 
	target="_blank"` or `"_self "` 이동페이지 열리는 창 `"
	title="마우스 올리변 보이는 제목> 링크걸리는 text</a>`
####Image: img tag

`<img src="경로" width="##", height="##px" alt="이미지 설명, 대체txt">`
####Lists: ol,ul

* ordered list(숫자, 알파벳 등):
	
```
<ol type="숫자,문자", start="시작숫자" reversed(역순)>
	<li>first</li>
	<li>second</li>
</ol>
```
	
* Unordered list(그냥 bullet):

```
<ul>
   <li>item1<li>
	<li>item2<li>
<ul>
```
	
####Description List: dl
```
<dl>
	<dt>HTML목록의 개념</dt>
		<dd>hyper text mkup lang개념 정의</dd>
		<dd>웹페이지 구현 맠업 언어:개념 정의</dd>
		
	<dt>CSS</dt>
		<dd>Cascading Style Sheet</dd>
		<dd>HTML형태 지정 언어</dd>
</dl>
```

##Table 
####기본구조

`table>tr>th*3^(tr>td*3)*2`: 3 X 3 테이블
	
```
<table>
		<tr>
			<th></th> *3
		</tr>
		<tr>
			<td></td> *3
		</tr>
		<tr>
			<td></td> *3
		</tr>
</table>
```

####테이블의 큰 구조

```
<table>
	<thead>
		<tr>
			<th></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td></td>
		</tr>
	</tbody>
	<tfoot>
		<tr>
			<td></td>
		</tr>
	</tfoot>
</table>
```

**thead와 tfoot은 한번씩만 선언될 수 있다.**

####셀 병합

`<td colspan="병합할 셀 수">`: column(열) 병합됨(<-->)
	
`<td rowspan="병합할 셀수"`: row (행) 병합
	
`<colgroup span="병합 열 수"></colgroup>`: 특정 열, 행 속성 쉽게 접근

####행의 구조

- th: table heads
- table heads: 첫 행
- table body: 내용 행
- table foot: 도표하단 행,겷과 행

####colgroup: 열 그룹

`<colgroup><col/><col/></colgroup>`	
:이를 사용하여 특정 열 또는 열의 그룹에 쉽게 속성을 줄 수 있다. 


##Form	
###Form 요소
form tag: 브라우저에서 서버로 data전송 위한 tag (ID,pwd,성별, 생일 등...)

``` 
<form action="" method="get">
	<label for="username">ID</label>
	<input type=text name="username">
</form>
```

####두가지 method:

1. **get: url** 에 표시(**never for credentials**)
	- url안에 data 삽입해서 전송됨

2. **post: url** 과 별도로 전달(보안성)

###Form - action
####Input tags:
* <input type=text, password, 
		radio, checkbox, button, file, 
		submit, reset, hidden ...
		
* id="username", "password" 
	  radio, checkbox...>
	
**input의 요소:**

* 값을 갖는다
* form에 영향
* 같은 이름의 radio는 동시에 체크 불가

####Label tag:
* `<label for="a">a</label>
	  <input type="text id="a">`

label for =~~ 와 input id=~~ 연결하면 해당 요소를 정확히 가르킴.

####Select tag:
```
<select multiple="multiple">
	<option value="a">A</option>...
</select>
```
: 보통 select로 1개 선택, multiple일 경우 ctrl+shift로 다수 선택 가능

* `<select size ="option 몇개 보이기"></select>`

####Optgroup tag:
```
<optgroup label="Fruits">
	<option....
	...
</optgroup>
<optgroup label="animals>
	<option....
	...
</optgroup>
```
	
**option들을 카테고리로 묶는다.**

####Button tag:
```
<button type="submit, reset, button"> text goes here</button>
```
:`<input type="button">`을 대체 가능

####Field Set, Legend tag:
* Field set: form을 담는다
* Legend: fieldset의 제목
 
###Class와 ID
####ID: 
페이지에서 딱 한 번 사용하며 unique한 특성 나타냄

* ex: id=chatper1, chpater2

	
####Class:
여러번 사용 가능, 범요적인 부분 나타냄

* ex: class= "chapter"로 chap1, chap2 다 명시















