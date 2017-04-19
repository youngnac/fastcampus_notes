2017-2-3
[mylink]([]()Findall)
[j](## SPlit)

# Regular Expresison

> 특정한 패턴에 일치하는 복잡한 문자열을 처리
> 
> 표준 모듈 `re`를 사용해서 정규표현식을 사용할 수 있다. (파이썬 내장)
## Match
>일반적으로 match는 소스의 시작부분에서 부터 찾는다 .
>
>>p: pattern
>>m: match

```python
import re
source = 'Lux, the Lady of Luminosity, Lux is back'
p = re,compile('Lux')
m = re.match(p, source)
```
>시작부분 부터 찾기 때문에 문자열 사이에 Lady가 있으면 못 찾는다. 따라서 어느 문자열이 여러개가 올 수 있다는 의미로 `.*`를 앞에 붙인다.

```python
p2 = re.compile('.*Lady')
m2 = re.match(p2, source)
```
## Search
> 첫 번째 일치하는 패턴 찾기

```python
p3 = re.compile('Lady')
m3 = re.search(p3, source)
if m3:
    print(m3.group())
```

## Findall
>일치하는 모든 패턴 찾아 list로 반환

```python 
>>> m4 = re.findall(p3, source)
>>> if m4:
    	print(m4)
['Lady', 'Lady']
```

## Split


## 정규 표현식의 패턴
### \b \B
[stack overfloow](http://stackoverflow.com/questions/6664151/difference-between-b-and-b-in-regex)


# 예외처리

# file
## offset
```python
f = open('skill.txt','wt')

In [14]: size = len(skill)

In [15]: print(size)
75

In [16]: offset = 0

In [17]: chunk = 30

In [18]: while True:
    ...:     if offset > size:
    ...:         break
    ...:     f.write(skill[offset:offset+chunk])
    ...:     offset += chunk
    ...:

In [19]: skills
----------------------------------------------------------------
NameError                      Traceback (most recent call last)
<ipython-input-19-1a21abac8244> in <module>()
----> 1 skills

NameError: name 'skills' is not defined

In [20]: skill
Out[20]: 'Illumination\nLight Binding\nPrismatic Barrier\nLucent Singularity\nFinal Spark'

In [21]: size
Out[21]: 75

In [22]: f.close()

```














