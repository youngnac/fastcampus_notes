2017-01-25
#python 설정 (mac)

### ~/.zshrc 파일에 root설정

```
export PYENV_ROOT=/usr/local/var/pyenv
if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi
if which pyenv-virtualenv-init > /dev/null; then eval "$(pyenv virtualenv-init -)"; fi
```

>Install utilities to solve issues regarding arrow keys in cell
`brew install readline xz`

### Install python 3.4.3 using pyenv

`pyenv install 3.4.3`

#### Create virtual environment
>**pyenv virtualenv version#.# env_name**

`pyenv virtualenv 3.4.3 fc-python` 

#### Select the virtualenv as loca

`pyenv global 3.4.3`

*중간에 pyenv versions 치면 모든 버전 나옴*
#### Check which python version 
`python --version`

>후에 사용 폴더를 만든 후 그 폴더로 이동

#### Switch to virtualenv in local
`pyenv local fc-python`
 
> `$ l` 로 확인후 
> `python` 실행
> `exit()` 종료

####Interactive python shell iPython (install)
>`pip list` : 패키지 리스트 (`--format=colums` : 컬럼형태로 나옴)

`pip install ipython`

>pip란? 가상환경 안에서 파이썬 패키지 관리자
>


#PYTHON basic
##BASIC
###What is literal(리터럴)?
> 변하지 않는 고정된 데이터 자체의 표현
> (list is not literal)
 
- 정수형: 5, 19, 145
- 문자: '영나', 'time flies'
- 부동소수점: 1983.256923

###Expression(표현식)
>값을 의미하는 표현 (expressing values)

```python
>>> a = 3
>>> b = a + 6  #expression
>>> b
9
```
###Statement 
>값을 지니지 않고 목적 수행 
>for 문, del, print, if ...

```python
>>>	for char in 'fast':
 	 print(char)
f
a
s
t
```
##Variables 변수
>파이썬은 객체들로 이루어져있고 객체는 데이터의 형태를 결정하는 타입이다. 객체는 타입을 바꿀 수 없다. 
>
>따라서 변수를 선언하여 원하는 형태로 값을 할당하고 참조한다. 
>
> `=` assign 한다는 의미이며 `==` equal 을 뜻한다. 

```python 
var1 = 9 
var2 = var1
var2 
9  
var1 = 10 
var2 
9
```

>이때 var1의 값이 빠뀌어도 var2의 값이 바뀌지 않는 이유는 var1이 참조했던 곳을 var2에게 참조하라고 지시했으며 var1이 새로운 값을 참조하고 있다고 해서 var2가 참조하는 부분이 바뀌지 않기 때문이다. 
>변수에게 `=`assign value를 하게 되면 그 value를 갖는 것이 아닌 참조하는 것이다. 즉 참조하고 있는 객체의 메모리 상의 id를 갖는 것이다. 
#### id()
> `id(var1)` 을 활용해 그 주소값을 볼 수 있다. 
> 같은 곳을 참조하면 이 id 값들도 같다. 

#### type()
>`type()` 을 사용하여 변수의 타입, int, float, string 인지 확인 할 수 있다. 

```python
>>>	a = 123
>>>	type(a)
int
>>>b = 'sd'
>>>	type(b)
str
>>>c = 1.24
>>>	type(c)
float
>>>print(type(c)
<class 'float'>
```

>이때 class라고 하며 type을 나타내는데, 이 둘은 거의 같은 의미. 

###변수의 이름제한 
####사용 가능
- 소문자, 대문자 -> 유니코드
- 숫자
- underscore _
>name 은 숫자로 시작할 수 없으며, underscore로 시작할 때는 특병할 처리방법을 따르므로 일반적으로 사용하지 않음. 

#### Reserved 
>파이썬 내에서 구문을 정의하는데 사용하기 때문에 변수로 사용 불가

False, class, finally, is ,return, none, continue, for, lambda, try, True, def, from , nonlocal, while, and, del, global, not, with as, elif, if, or, yield, assert else, import, pass, break , except, in, raise 

### var = input()
>var에 변수를 입력받기 위해서는 `input()` 을 사용

` var = input('please enter #: ")`

##Numerics 숫자
###Arithmetic operations

연산자|설명|example
---|---|---
//	|정수나누기:나머지 버리고 정수만|10//3=3
%| 나머지만 보이기| 7%5=2
** |지수| 2**3 =8
+=| 자기 자신에 더하기| a += 3 -> a= a+3
*=| 자기에 곱| a *= 3 -> a= a * 3

####우선순위
>괄호를 사용하지 않아도 우선수위에 따라 계산된다. 

```
3+6*2
15
```

>하지만 가독성을 위해 ()를 사용

```
3+(6*3)
20
```

###base 진수
- 2진수: **0b**11 = 3 (**b**inary)
- 8진수: **0o**10 = 8 (**o**cta)
- 16진수: **0x**A=10 (he**x**a)

####2진<->10진 변환
2진->10진: `int('####',2)`
10진->2진: `bin(#)`


###int(), float(): type변환
>`int(35)`, `float(35)` 와 같이 type을 바꿔줄 수 있다. 

##문자열
###문자열 표현 "~" '~' "'~'~~~"
>작은따옴표 `''` 또는 큰따옴표 `""` 사용하여 문자를 나타낸다. 
>여러줄은 `''' line1 line2 line3 '''` 세개씩 사용하여 감싸준다. 
>이는 `'line1\nline2\nline3\n'`과 같다고 볼 수 있다. 
>`\n` 줄바꿈이다. 이러한 것들을 escape codes라고 한다. 

Escape code|설명
---|---
\a|beep
\t|tab
\n|line change
\ | \ 입력
\'|' 입력
\"|" 입력

###Indexing 인덱스 연산
```python
>>>	a = "korea"
>>>	a[0] 
'k'
>>>	a[-1]
'a'
>>>	a[:]
'korea'
```

####Slicing var[start:end-index전:step]
```python
>>>a = "asdfghjk"
>>>	a[0:3]
'asd' #0,1,2
>>>	a[1:6:3]
'sg'
>>>	a[6:0:-2]
'jgd' #거꾸로도 가능
```

###str.split('나누고싶은지점') : 문자열 -> list
```python
>>>	a = "sfd@asdf@asdfhg@onfb"

>>>	a.split('@')
['sfd', 'asdf', 'asdfhg', 'onfb']
```

###'연결고리'.join(LIST) : list -> 문자열
```python
>>>	b = a.split('@')

>>>	b
['sfd', 'asdf', 'asdfhg', 'onfb']

>>>	' '.join(b)
'sfd asdf asdfhg onfb'
```
###대소문자변환 *ex) 문자열var.upper()*
```python
>>>	a = "Like"

>>>	a.upper()
'LIKE'

>>>	a.lower()
'like'

>>>	a.title()
'Like'

>>>	a.capitalize()
'Like'

>>>	a.swapcase()
'lIKE'
```
> 이 외에도 .casefold() .center() 등 많다. [참고링크](https://docs.python.org/3/library/stdtypes.html#string-methods)

###문자열 format
format|설명|example
---|---|---
%s| string|
%d| decimal
%x| hexadecimal
%o| octal
%f| float
%e| Exponent notation|`%.2e' % a`->`'1.23e+02'`
%g|10진 부동소수점수 혹은 지수로 나타낸 부동소수점수|`'%.2g' % a`->'1.2e+02'`
%%|literal %

```python
>>> '%s and %s' % ('cat', 'dog')
'cat and dog'

>>> %d / %d : %d' % (4, 2, 2)
'4 / 2 : 2'

>>>	d =37
>>>	s =4.98
>>>	'%d %f' % (d, s)
'37 4.980000'
>>>	'%d %f' % (s,d)
'4 37.000000'
```

###정렬 기준
`%[정렬기준(-,없음)][전체글자수].[문자길이 또는 소수점 이후 문자길이][변환타입]`
>'% (-: 왼쪽정렬) 전체글자#.문자#or소수점자리수_변환타입'

```python
>>>	s =4.98
>>>'%13.3f' % (s)
'        4.980'
>>>	'%-12.5s' % ('happybirthday')
'happy       '
```

###새스타일 '{}{}'.format(var1,var2)
```python 
>>>	'{}{}'.format('apple','kiwi')
'applekiwi'

>>>	'{1} {0}'.format('apple','kiwi') #index 사용하여 위치 선정
'kiwi apple'

>>>	dict = {'a':12,'b':34,'c':'cat'}
>>> '{0[a]} {0[b]} {0[c]} {1}'.format(dict, 'WPS') #dictionary 사용해서 키-벨류 이용
'12 34 cat WPS'

>>>'{:d} {:f}'.format(4.09,4)
#ValueError: Unknown format code 'd' for object of type 'float', deciaml에 float값을 줄순 없다. 

>>>	'{:d} {:f}'.format(4,4.09)
'4 4.090000'

```
####새 스타일 '{:_fillings_> or < or ^_#_type}.format(var)

```python
>>> s = 4
>>> '{:10d}'.format(s)		#10자리, decimal, default=우측정렬
'         4'
>>> '{:10>d}'.format(s)		#위와 같음
 
>>>	'{:<10d}'.format(s)		#좌측정렬
'4         '
>>>'{:^5d}'.format(s)		#가운데 정렬
'  4  '
>>> {:@<5d}'.format(s)		#좌 정렬 그리고 채우기
'4@@@@'
```

##Sequence: 문자열, ' ' , List[ ] , Tuple ( )
> index를 사용하여 seqeunce안의 항목에 접근
> 문자열, 튜플 수정 불가
###LIST
>순차적 data, 내부항목 변경 가능

```python 
list_1 = [] 		#empty list created
list_2 = list{}  	#empty list created
list_1 = ['a', 'b', 'c']
list_2 ['cat', 'dog', 'bird']
```

#####문자열->LIST
```python
>>>	a = 'abcdefg'
>>> a = list(a)
['a', 'b', 'c', 'd', 'e', 'f', 'g']
```

####Slicing, element change using index
```python
>>>	a
['a', 'b', 'c', 'd', 'e', 'f', 'g']

>>>	a[0]
'a'

>>>	a[5]
Out[16]: 'f'

>>>	a[1::2]
['b', 'd', 'f'

>>>	a[1:4] = ['g','k','l']
>>>	a
['a', 'g', 'k', 'l', 'e', 'f', 'g']
```
###LIST 함수
####LIST.extend(or append).list2 append
####LIST.insert(index 'element to insert')
####LIST.remove('element')
#### del LIST[#]
extend 리스트안으로 추가
append 리스트안에 집합체로 리스트 추가

```python
>>>	a
['a', 'g', 'k', 'l', 'e', 'f', 'g']
>>>b=['kiwi','apple']

>>>	a.extend(b)			#extend하면 리스트 a 안으로 들어가 일부가 된다
['a', 'g', 'k', 'l', 'e', 'f', 'g', 'kiwi', 'apple']

>>>	a.append(b) 		#append하면 리스트 통째로 삽입
['a', 'g', 'k', 'l', 'e', 'f', 'g', ['kiwi', 'apple']]

>>>	a.insert(0, 'peach')		#0번째 자리에 peach, 즉 맨앞에 넣겟다.
>>>	a
['peach', 'a', 'g', 'k', 'l', 'e', 'f', 'g']

>>>	a.remove('kiwi')			#list a안의 'kiwi' 삭제
>>>	a
['peach', 'a', 'g', 'k', 'l', 'e', 'f', 'g', 'apple']

>>>del a[3]		#list a의 4번쨰 학목 지우기. 'k' 지우기
>>>	a
['peach', 'a', 'g', 'l', 'e', 'f', 'g', 'apple']
```

####LIST.pop(index#)
```python 
>>>	a
['a', 'l', 'c']

>>>	a.pop()		#끝요소 빼내고 지우기
'c'
>>>	a
['a', 'l']

>>>	a.pop(0)		#index0, 즉 맨앞 빼내고 지우기
'a'
>>>	a
['l']
```

####LIST.index('element')
####'element' in LIST -> T or F
####LIST.count('element')
```python
>>>	a = ['red','pink','blue', 'red']
>>>	a.index('pink')		#리스트안에서 'pink'의 인덱스 값을 찾아라
1

>>>'blue' in a		#리스트안에 'blue'가 있는지
True				#있음
>>>	'grey' in a
False				#'grey'는 없음

>>>	a.count('red')		#리스트에 'red' 몇개?
2
```
	

####LIST.sort() 
>리스트가 sort되어 새리스트가 됨

```python
a = ['peach', 'a', 'g', 'l', 'e', 'f', 'g', 'apple']
>>>	a.sort()
>>>a
['a', 'apple', 'e', 'f', 'g', 'g', 'l', 'peach']
```

>> sorted(a) 그냥 sort되서 보여주기만 새리스트로 바로 전환은 안됨

```python
a = ['a','l','c']
>>> sorted(a)
['a', 'c', 'l']
>>>	a	
['a', 'l', 'c'] 			#a 는 계속 sort 되어 있지 않은 상테/
```
####**항목 추가** LIST.append('element')

```python 
a = ['a','l','c']
>>>	a.append('hot')
>>>	a
['a','l','c','hot']

>>>a.extend('hot')		#extend는 글자 하나하나 따라서 append를 사용
['a','l','c','h','o','t']
```

####리스트끼리 병합 LIST.extend(LIST2)

```python 
>>>	a = ['a','b','c']
>>>	b = ['x','y','z']
>>>	a.extend(b)
>>>	a
['a', 'b', 'c', 'x', 'y', 'z']
```

### Tuple
>리스트와 비슷 그러나 내부항목 삭제,수정 불가 (extend, append 불가)
>리스트보다 적은 메모리 사용

#####Tuple Unpacking
```python 
>>>a = ('6', '3')
>>>	x, y = a
>>>	x
'6'
>>>	y
'3'
```
####List <-> Tuple
```python

>>>	a = ['a', 'b', 'c', 'x', 'y', 'z', 's', 'd']
>>> a = tuple(a)
>>>	a 
('a', 'b', 'c', 'x', 'y', 'z', 's', 'd')
>>>b = list(a)
>>>b
['a', 'b', 'c', 'x', 'y', 'z', 's', 'd']
```
##Dictionary
key-value 형태로 항목을 갖는 자료구조
>dictionary_name = {'key':value, 'key2':value2 ...}

####dict_name['key']: Using Key, find value, change value, add key-value

```python 
>>>	num_wheels = {'car':4, 'bike':2}
>>>	num_wheels['car'] 		#'car'의 키값찾기
4

>>>num_wheels['car'] = 8		#'car'의 키 값 8로 바꾸기
>>>num_wheels['plane'] = 0		#'plane': 0 추가하기
>>> num_wheels
{'bike': 2, 'car': 8, 'plane': 0}
```
####dict_name.update(dict2): 다른 dict와 합치기.
```python
>>>	num_wing = {'bird':2}
>>>	num_wheels.update(num_wing)
>>>	num_wheels
{'bike': 2, 'bird': 2, 'car': 8, 'plane': 0}
```
####dict.key(), dict.value(), dict.items()
> 모든 키 값, 벨류 값, 키-벨류

```python 
>>>	num_wheels.values()
dict_values([2, 8, 0, 2])

>>>	num_wheels.items()
dict_items([('bird', 2), ('car', 8), ('plane', 0), ('bike', 2)])

>>>	num_wing.keys()
dict_keys(['bird']
```

###Set
> 키만 있는 dictionary, 중복 값 불허

```python
>>>	set(num_wheels)
{'bike', 'bird', 'car', 'plane'}

>>>	set('abfdsasdbvewxwefds')
{'a', 'b', 'd', 'e', 'f', 's', 'v', 'w', 'x'}
```

####집합연산
연산자|설명
---|---
\|	|	Union
&	|	Intersection
\-	|	Difference
^	|	대칭차집합(Exclusive) = union - intersection 
<=	|	Subset
<	|	Proper subset
\>=	|	Superset
\>	|	Proper superset


