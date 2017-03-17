
# CSS
: HTML의 **스타일**을 지정하는 언어

## CSS 문법:
#### 기본형태:
``Selector {property: value;}``
#### CSS와 HTML연결: link tag 사용 href로 파일 위치 지정
```
<head>
<link rel="stylesheet" href="cssfile.css">
```
## CSS Selector(선택자)
#### Universal: *
html의 모든 요소에 적용하고 기본값을 정할때 사용
#### Tag selector:
tag name {

ex) `h1 {`,`p {`,`a {`,`tr{`, `td{`
#### Class selector: use PERIOD
`tag.class_name` (tag생략시 ->div)

ex) 
`.section{` -> `<div class="section"`

  `p.section` -> `<p class="section>`

#### ID selector: use #.
`tag#id_name` (tag생략가능)

**class보다 ID가 우선순위**   

ex) 
`#indent-title{` -> `<h1 id="indent-title"`

`p#indent-contents{` -> `<p id="indent-contents`
#### 그룹 선택자
여러 선택자로 같은 스타일 적용가능

ex) `#index-title, #index-contents{ ...`


### 복합 선택자(Combi Selector)
#### Child(자식 >)
ex) `section > ul`: section 바로 아래 레벨 ul

#### Descendant(하위  )
ex) `section ul`: section아래 모든 ul. 즉 조상 아래 모든 후손들...

#### Adjac. Sib(인접형제 +)
ex) `h1+ul{`:h1 바로 옆 아래 ul(같은 레벨 옆). 즉, 한 사람 기준 바로 옆(아래) 동생한명 선택

#### Gener. Sib(일반 형제 ~)
ex) `h1~ul{`:	한 사람 기준, 아래 모든 동생들
#### Attribute Selector
1. `E[attr]`:attr 포함 요소
2. `E[attr="val"]`: 정확히 속성이 val인 요소
3. `E[attr~="val"]`: 속성값이 val(공백분리)포함 요소
4. `E[attr!="val"]`: (공백분리)val또는 val로 시작
5. `E[attr^="val"]`: val로 시작
6. `E[attr$="val"]`: val로 끝
7. `E[attr*="val"]`: 그냥 어디든val 포함

#### Pseudo-classes/elements selector
1. `E::first-line`: 요소의 첫줄
2. `E::first-letter`:요소

의 첫 문자
3. `E::before`: 요소의 시작 지점 생성된 요소
4. `E:: after`: 요소의 끝지점 생성 요소
5. `E:link`: 방문하지 않은 링크
6. `E:visited`: 방문 후의 링크
7. `E:hover`: 커서가 올라갔을 때
8. `E:focus`: 포커스가 머무를 때

**before,after:Float clearfix때 사용**

## CSS우선순위
#### 특정도
**Important 절대적 >>|| inline > ID > Class > tag**

For example..

```
p {
	font-size:12px !important;
   }
```
: It overrides all!

## CSS typography
* color: #ccc
* font-family:"도움", "굴림, arial, ... 

 서체 없을 경우 순서대로 적용
* font-size: 13px, or 2rem 

 rem:은 부모와 상대적 비율
* font-style: italic, oblique..
* font-weight(굴기): bold, 700..
* line-height: 1.5(font size배수)
* text-decoration: underline, overline, line-through...
* text-align: left, center, right, justify (양쪽 정렬) 
* text-indent: 1em 들여쓰기
* text-transform: capitalization(첫글자 대문), uppercase, lowercase
* letter-spacing: 5px; (글자간 간격)
* word-spacing ( 단어간 간격)
* verical-align (요소간의 수직정렬): baseline, sub, super, top, text-top,bottom...
* white-space (줄바꿈): nowrap, pre, pre-line, pre-wrap(줄바꿈, 띄어쓰기, 자동줄) 

## CSS 배경 스타일
* background-color: #eee,#efefef, 
    rgb(x,y,z, alpha투명도)
* background-image: urla('../images/image1.png)

**url('..~') :상위 폴더 이동**
​		
* backgroud-repeat: no-repeat, repeat, repeat-x/y 

배경 이미지 반복 여부, 가로 세로.(이를 활용해 그라데이션 가능)

* background-position: 50x 16px, left/right top/bottom, center(좌우상하)..

2개의 값 x축, y축

* background-attachment: local, scroll(요소 안 고정), fixed(웹 페이지 상 고정)

배경 스타일 속기법의 EX

```
div{
	background: url('images.png') no-repeat scroll right 50% #eee
}
```
## CSS 테두리 스타일
#### 요소의 방향
- 상 우 하 좌 (위부터 시작해 시계방향)
- 스타일 지정시 두개 숫자: 상하 좌우 묶어서 지정됨

#### 구성 요소
- border-width
- border-style
- border-color

EX)

```
div{
	border:1px dotted red;
	}
```

## CSS Box Model
![](http://www.corelangs.com/css/box/img/box-model.png) 
#### Margin
- margin> border> padding> contents: 이 4요소가 박스의 형태를 구성한다.

- **margin**은 실제 개체를 선택시 포함되지 않음
- 또한, 두 요소의 마진이 (요소1의 bottom과 요소2의 top)이 겹치면 둘중 **큰값만** 적요됨으로 한쪽에 몰아 설정후 패딩으로 처리한다. 

#### Padding
- 보더안의 내부 여백

### Box설정
```
div{
	width: 200px;
	height: 50px;
	padding: 10px;
	border: 1px dotted red;
	margin: 4px;
	}
```
* `box sizing: border-box;`: 패딩과 보더값을 제외하고 요소의 너비 재적용

## CSS List Style
* `list-style-type: disc, circle, square, lower roman`...bullet 타입을 결정한다.
* `list-style-image: url(images.png)` 이미지로 bullet을 설정 가능하다.

## CSS Table Style
* `border-collapse: collapse` border를 합친다
* `border-spacing: 2px` 2px만큼을 테이블 셀 사이에 공백을 준다
* `empty-cells: hide;` 빈셀 공백으로 둔다
* `table-layout: auto`; 내용물에 따라 테이블이 유동적으로 움직임


## CSS Display
![display속성](https://i.stack.imgur.com/mGTYI.png)
#### Display: block

* span의 인라인 속성이지만 블록 속성으로 전환

```
span{ 
display: block;
}
```
* 블록 속성을 인라인으로도 전환 가능

```
 div{ 
display: inline;
}
```

* 인라인이면서 블록과 같이 높이/상하 값을 갖을 수 있음(높이, 상하 패딩, 마진값 설정)

```
span{ 
display: inline-block;
	}
```
#### Display: others...
* `display: none`:화면X, 공간X
* `visibility: hidden`:화면X, 공간 O
* `overflow: hidden`: 넘침 숨김
* `overflow: visible`:박스 밖으로 보임
* `overflow: auto`: 넘치면 스크롤
* `overflow: scroll`: 항상 스크롤

----------------------

## CSS Float
**개체를 띄움**

단락과 이미지를 함께 넣을때 사용 가능.

* Float:right

오른쪽으로 띄움. 하지만 이미 존재하는 윗쪽의 개체가 블록일 경우 올라갈 수 없음. 


	​```
	E{
	float: right
	}
	​```


* float속성 해제: float 속성과 겹칠시 (예- 그림 아래로 텍스트가 들어간다거나...) clear(left,both,right)을  사용하여 속성 해제.

 ```
 p{
 clear: both
 }
 ```

CSS Float Layout

* float frame안에 자식 요소들로 float 개체들을 넣는다

 ```
 <div class="floatframe">
   <div class="float-unit"></div>
   <div class="float-unit"></div>
   <div class="float-unit"></div>
   <div class="float-unit"></div>
 </div>
 ```

* CSS에선 모든 개체들을 float해준다.
* 이때, 부모 요소의 높이가 인식이 되지 않아  개체들을 다 담고 있지 않은 모습을 보일때는 아래의 방법 중 택1:
1.  Html float unit들 아래:
    `<br style="clear: both">`
2.  float frame css에 overflow숨기기:
    `.floatframe{overflow: auto;
    overflow: hidden;}`
3.  float 요소들의 부모 요소에도 float 설정
4.  :after 가상선택자 이용하여 부모요소에게 아래와 같은 설정을 준다. 

  ```
  content: “ “;		display: block;		height: 0;  
  clear: both;
  ```

## CSS Position
* position: static (기본), relative (top, right, left, bottom 설정 상대적), fixed(창 기준), absolute (부모 기준)

### 가운데 정렬
##### 좌우 가운데 정렬
특정 넓이 값을 준 후, margin 값으로 정렬한다. 

* `margin: 0 auto;`

##### 가로세로 가운데 정렬
1. 부모요소의 height 와 line-height를 같은 값을 준다. 
2. 포지션 값으로 설정하는 방법으로는...

 ```
 position: asbolute;
 width: 100px;
 height: 100px;
 top: 50%;
 left: 50%;
 transform: translate(-50%, -50%);		
 ```

# SASS
**Syntactically Awesome Stylesheet**
: CSS의 전처리기 pre-processor

Sass는 다양한 형태의 CSS로 변환가능하다

1. nested: 인덴트로 들여쓰며...
2. compact: 한줄 한줄
3. expanded: 인덴트 없이 줄줄이
4. compressed: 일자로 쭉

## SASS style
#### 주석
`//주석` 또는 `/* 주석내용 */` 사용한다. 

**`//주석` 는 CSS에서는 보이지 않는다.**

#### Nested
{} 안에...하위 선택자.

```
selector{	
	width: 100px;
	>selector{
		color: red;
	}
	selector{
		font-size: 12px;
	}
}
```
#### 부모 참조 &
자신의 부모선택자를 참조한다

* 아래의 예시는 a:hover를 뜻함.

```
a{
	color: red;
	&:hover{
		color: blue;
	}
}
```

#### 선택자 상속 @extend
`@extend selector` 특정 selector의 속성들을 상속한다. 

#### 변수 $name
변수를 설정하여 변수이름을 속성의 값으로 나타내면 수정하기 용이하다. 

`$btn-color: red;` `$title-font-weight: bold;`

별도의 페이지에 변수들을 선언하고 불러올때는  현재 파일을 기준으로 import해야한다: `@import 'variables'`



​	


