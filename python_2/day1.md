2017-1-31 day4

##Scope (영역)
- 함수마다 독립적인 함수 scoff를 갖는다
- global scope: 메인 프로그램(현재 동작하는 프로그램의 최상위 위치)의 영역
- local scope: 전역 스코프 내부에서 독립적인 영역을 갖고 있는 경우

```python
def change_global_champion():
	print('before change_global_champion : {}'.format(champion))
	champion = 'Ahri'
	print('after change_global_champion : {}'.format(champion))
#함수내부에서 champion을 쓰려고 하는데 할당하기 전에 참조되어 에러. 따라서 두번째줄 주석처리하면 함수 에러 없어짐.

def change_global_champion():
	#print('before change_global_champion : {}'.format(champion))
	champion = 'Ahri'
	print('after change_global_champion : {}'.format(champion))

```

####global
로컬 스코프에서 글로벌 스코프의 변수를 변경해야 한다면, 해당 변수가 로컬 스코프에 생성되는 것이 아닌 글로벌 영역에 이미 존재하는 값을 사용함을 명시해주어야 한다

####nonlocal
상위중 가장 가까운

##global keyword와 argument 전달 차이
```python
global_level = 100
def arg_level_add(value):
	#인자로 전달받은 값을 value라는 parameter로 사용 value parameter의 값을 30 가중 ->출력
	value += 30
	print('arguments_level_add, value: %s' % value)

def global_level_add():
	#global scope의 global level변수의 값을 30증가 후 출력
	global global_level
	global_level += 30
	print('global_level_add, value: %s' % global_level)

def show_global_level():
	#global scope의 global_level변수값을 출력
	print('global_level: %s' % global_level)

show_global_level()
arg_level_add(global_level)
show_global_level()
global_level_add()
show_global_level()
```

```
global_level: 100
arguments_level_add, value: 130
global_level: 100
global_level_add, value: 130
global_level: 130
```
> value에 global_level을 전달 즉 argument로 전달한 경우, 같은 객체를 가리키는 글로벌 변수 global_level과 parameter인 value가 존재한다.
이 때, parameter인 value의 값을 변경하는 것은 global_level에는 영향을 주지 않아, arg_level_add(global_level)=130이지만, 이 후의 global_level=100으로 유지된다. 


###list변수가 전달된다면.

```python
global_level = [100]
def arg_level_add(value):
    value[0] += 30
    print('arguments_level_add, value: %s' % value)

def global_level_add():
	global global_level
    global_level[0] += 30
    print('global_level_add, value: %s' % global_level)

def show_global_level():
    print('global_level: %s' % global_level)

show_global_level()
arg_level_add(global_level)
show_global_level()
global_level_add()
show_global_level()
```
return: 

```
global_level: [100]
arguments_level_add, value: [130]
global_level: [130]
global_level_add, value: [160]
global_level: [160]
```

##Lambda function
>using lambda is a single line expressino
>cannot contain 반복문이나 조건문 등의 제어문은 포함될 수 없다.

```python
import string
for char in string.ascii_lowercase:
    if char>'i':
        print(char.upper())
    else:
        print(char)


for char in string.ascii_lowercase:
    print((lambda x : x.upper() if x > 'i' else x)(char))
```

##closure
>함수가 정의된 환경
>
>파일이 여러개일 경우 각 파일이 모듈 역할을 한다.
>
>각 모듈은 독립저인 환경을 갖고 각자의 영역을 전역영역으로 사용.

module\_a 와 module\_b
>module\_a로부터 module\_b로 print_level()을 불러온다. -> module\_a.print\_level()

```python
#module_a
level = 100
def print_level():
    print(level)
```
```python
#module_b
import module_a
level = 50
def print_level():
    print(level)

module_a.print_level()
print_level()
```

####내부함수의 클로져
>outter안에 inner함수를 변환하여 결과를 func에 할당
>inner함수를 통째로 반환하기 때문에 func이 갖는 variable은 함수객체.

```python
level = 0
def outter():
    level = 50
    def inner():
        nonlocal level		#상위 outter에서 정의된 level을 가져오고, 증가시킴
        level += 3
        print(level)
    return inner

func = outter()
func()
func()
func()
print(func)
func2 = outter()
func2()
func3 = func
func3()
```
> 실행하면,,, 53 56 59 <function outter.<locals>.inner at 0x10bde7840> 53 62
> func3 = func을 가르키기때문에 계속해서 값이 누적되어 반환된다. 

###decorator
> 함수를 받아 다른 함수를 반환하는 함수
> 
> 기존의 함수에 변화를 주지 않고 새로운 함수를 이용

1. 기능을 추가할 함수를 인자로 받음
2. 데코레이터 자체에 추가할 기능을 함수로 정의
3. 인자로 받은 함수를 데코레이터 내부에서 적절히 호출
4. 위 2가지를 행하는 내부함수를 반환

예제: multi와 divide함수를 제대로 프린트 하기 위한 함수 데코레이터 print_args를 만들어본다. 

```python
def print_args(func):
    def inner_func(*args, **kwargs):
        print('args:',args)
        result = func(*args, **kwargs)
        return result
    return inner_func

@print_args
def multi(arg1, arg2):
    result = arg1*arg2
    print(result)
    return result

@print_args
def divide(arg1, arg2):
    result = arg1 / arg2
    print(result)
    return result

multi(3, 5)
divide(10,2)
```
result : 
```
args: (3, 5)
15
args: (10, 2)
5.0
```

###generator
: generates sequence data

>different from sequence: it only carries a routine for generating a sequence
>
>함수를 통해 생성하고 `yield`를 반복문 내부에 사용해야 함
>
>제네레이터는 마지막으로 호출한 위치(항목)에 을 기억하고 있으며, 한 번 순회할 때 마다 그 다음 값을 반환한다.


```python 
>>>	def range_gen(num):
		i = 0
		while i < num:
			yield i
			i += 1
		
>>>	gen = range_gen(10)		#0에서 9까지의 시퀀스 gen 생성
>>>	gen_list = [item for item in gen]

>>>	for i in gen_list:
		print(i)	
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
>>> gen_list
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```
##Seqeuntial Search
```python
def search(key, source):
    length = len(source)
    index = 0
    while index < length:
        cur_char = source[index]
        if cur_char == key:
            return index
        index +=1
    return 0
```