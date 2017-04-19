#  Responsive Web
####  HTML의 CSS 속성의 너비 값을 조절하며 layout을 바꾸는 것.

##  Viewport
스마트폰에서 내용이 표시되는 영억
모바일의 viewport width: 980px;

###  Viewport tag
`<meta name='viewport' content='width=device-width, initial-scale=1'>`

>width값 외에도, height, user-scalable (확대/축소, yes/no), intial- scale (1~10), minimum-scale, maximum-scale과 같은 속성들을 줄 수 있다. 

###  Media queries
접속 장치에 따라 CSS style을 주는 모듈

`@meida [only | not] 미디어유형 [and 조건(복수)]`

ex)

- `@media screen (min-width: 1024px)` : 최소크기가 1024px 이상 (PC)
- `@media screen (max-width: 1023px)` : 너비 1023px 이하 (태블릿)
- `@media screen (max-width: 768px)` : 너비 768px 이하일 때 (모바일)
- `@media screen (max-width: 320px)` : 너비 320px 이하일 때 (모바일, 아이폰5이하)

###  콘텐츠 1개 보이기 2개 보이기...

>반응형 웹에서 콘텐츠의 사이즈를 줄이거나 늘려 작은 뷰포트에서 잘 보이게 하려면 width를 조정하는데 이를 비율로 지정해주면 된다. 
>웹상에서 4개씩 보이는 이미지를 뷰포트의 넓이가 줄어들었을 때 하나씩 보이게하려면 width=100%를 각 이미지에 주면 화면에 꽉차게 나오게된다. 
>이를 응용해 width=50%로 지정하면 이미지가 2개씩 보이게 된다. 
>
>넓이의 %를 줘 활용할 수 있다.