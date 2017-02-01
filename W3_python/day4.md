2017-1-26
###QnA from last lecture
string.replace("~", "new~")
이는 새로운 문자열을 return하는 것이지 본 string은 바뀌지 않음

```python
>>>	a = 'sdfgh'
>>>	a.replace('sd','xy')
'xygh'
>>>	a
'sdfgh'
```

a = 'sdfgh'
##조건문: if, elif, else

```python
>>>if is_holiday == True:
		print('Good')
 	else:
     	print('Bad')

>>> if is_holiday:
		print('Good')
	else:
		print('Bad')   
```
```python
>>>	if vacation >=7:
		print('Good')
	elif vacation >=5:
		print('Normal')
	else:
		print('bad')
```

####조건표현식
`참일경우 if 조건식 else 거짓일 경우`

```python
print('good') if vacation >=7 else print('normal') if vacation >=5 else print ('bad')
```


##for 문 (조건에 따른 순회)
>squence 데이터 순회할 때 사용
>
>iterable: sequences(list,) 숫자 range, tuple, dictionary, strings, set
>>dictionary의 key / value / 모두통째로 순회할때
>>
>>- dict.keys()

```python
>>>	for i in colors.keys():
		print(i)
red
white
blue
black
yellow
```
		
>>- dict.values()
>>- dic.items()

```python
>>>	for i in colors.items():
     	print(i)
```
###기본틀
```python 
for 항목 in 순회가능객체:
	실행 codes...
```
###range(#): 0 ... #-1
###range(1,#): 1 ... #-1

#####구구단
```python
>>>	for i in range(2,10):
	     print('{}단시작'.format(i))
	     for s in range(1,10):
	     	k = i*s
	     	print('%s * %s = %s' % (i,s,k))
		print('\n')
```

```python
for item in zip(fruits, colors):
    ...:     print(item)
    ...:     print(type(item))
    ...:     print(item[0])
    ...:     print(item[1])
    ...:
('apple', 'red')
<class 'tuple'>
apple
red
('kiwi', 'yellow')
<class 'tuple'>
kiwi
yellow
('peach', 'blue')
<class 'tuple'>
peach
```

####Break and Continue
>- break: 중도에 중단

```python
for i in range(5):
    ...:     print(i)
    ...:     if i == 4:
    ...:         break
    ...:
0
1
2
3
4
```

>- continue: skip하고 계속

```python
for i in range(9):
    ...:     print(i)
    ...:     if i == 4:
    ...:         continue
    ...:
    ...:
0
1
2
3
4
5
6
7
8
```

####ZIP
>여러 sequence 동시 순회 가능

```python
>>>	result = zip(colors, fruits)
>>>	print(type(result))
<class 'zip'>


>>>	list(result)
[('red', 'apple'), ('yellow', 'kiwi'), ('blue', 'peach')]
```
##While: 조건이 참일 때까지 반복

```python
while 조건:
	조건이 참일 경우 실행문
	조건이 거짓일 때까지 반복
```

##List comprehension
>iteralbe 객체로부터 파이썬 자료구조를 만드는 방법.

```python
>>>	[item for item in range(1,6)]			#list에 숫자 담기
[1, 2, 3, 4, 5]
>>>	[item*3 for item in range(4)]			#3곱해서 담기
[0, 3, 6, 9]

>>>	[item for item in range(20) if item %3 == 0]
							#list에 숫자담는데 3의 배수로만
[0, 3, 6, 9, 12, 15, 18]

>>>	for color in colors:
     for fruit in fruits:
         com_list.append((color, fruit))


>>>	com_list
[('red', 'apple'),
 ('red', 'kiwi'),
 ('red', 'peach'),
 ('yellow', 'apple'),
 ('yellow', 'kiwi'),
 ('yellow', 'peach'),
 ('blue', 'apple'),
 ('blue', 'kiwi'),
 ('blue', 'peach')]

```
####Set comprehension
`표현식 for 표현식 in interable`
####Generator comprehension
`표현식 for 표현식 in iterable`
> tuple 생성 X, 리스트 형태로 생성 O

##Tuple은 generator 생성 (다시 돌려줘야 하는 것)
```python
>>>g = (item for item in range(10))
>>>	for i in g:
    ...:     print(i)
    ...:
0
1
2
3
4
5
6
7
8
9
```

실습
2번:
`[(i,j) for i in range(7) for j in range(4)]`

3번

```python
>>>	for i in range(7):
         if i %2 !=0:
             continue
         for j in range(4):
             print((i,j))
   
[(x,y) for x in range(7) if x%2 == 0 for y in range(4)]
```
4번

```python
for i in range(1001,2001,2):
	k = k+i
	print(k)
```

5.

```python
for i in range(2,10):
	for j in range(1,10):
	gugu.append('{} x {} = {}'.format(i,j,i*j))

for i in range(1,100):
	if i %7 == 0 or i % 9 ==0:
	list79.append(i)
     
for i in range(2,10):
	for j in range(2,10):
    	print('{} {} = {}'.format(i,j, result[index]))
		index +=1
```

6.

```python
k=[]
for x in range (1,100):
	if x%63 == 0:
		k.extend(list([x]))
	elif x%7==0:
     ...:         k.extend(list([x]))
     ...:     elif x%9 == 0:
     ...:         k.extend(list([x]))
     ...: print(k)
     
```

##ENUMERATE

```python
for index, x in enumerate(range(2,10)):
     ...:     print('index : {}, value : {}'.format(index,x))
     ...:
index : 0, value : 2
index : 1, value : 3
index : 2, value : 4
index : 3, value : 5
index : 4, value : 6
index : 5, value : 7
index : 6, value : 8
index : 7, value : 9

for x in enumerate(range(3,9)):
     ...:     print('{}'.format(x))
     ...:
(0, 3)
(1, 4)
(2, 5)
(3, 6)
(4, 7)
(5, 8)
```
##함수
>반복적인 작업을 코드화하여 재사용 가능하게 한 것.

```python
def 함수명(매개변수[parameter]):
	동작
```
####parameter와 argument의 차이
> 함수 생성시 함수에게 전달되어 온 변수: parameter 
> 
> 함수 호출 시 전달하는 변수: argument

###함수를 argument로 전달하기 (연습)
```python
>>> def print_call_func():
    print('call func')

>>> def execute_arg_func(func):
     func()
>>> execute_arg_func(print_call_funct)
call func

```

my codes:

```
>>> def call_func():
     print('call_func')
>>> def func1():
     print(call_func())

>>> func1()
call_func
None

```

###내부함수
```python
def outer(val):
     def inner():
         return val.upper()
     return inner()
     
def outer(val):
     def inner(string):
         return string.upper()
     return inner(val)    
     
```
####default parameter
> parameter선언시 default 값을 미리 지정하여, 따로 주어지지 않을 경우 항상 그 값을 사용.

```python
>>>	def multi(arg1, arg2=None):		# arg2에 값이 지정되지 않을 경우 항상 none
		if arg2 is None:
        	result = int(arg1)*int(arg1)
        	return result
	    else:
        	result = int(arg1)*int(arg2)
        	return result
    	print(result)
```
#####default parameter는 실행될 때마다 계산되는 것이 아니라 정의 될때 계산되어 사용
```python
>>> def return_list(value, result=[]):
		result.append(value)
		return result

'''
default 는 맨처음에 정의되고 실행될때 계속 기존의 값을 갖고 있기에 list안의 아이템이 쌓임
'''
>>> return_list('apple')
['apple']
>>> return_list('banana')
['apple', 'banana']
```

