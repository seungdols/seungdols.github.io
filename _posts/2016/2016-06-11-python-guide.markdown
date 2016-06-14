---
layout: "post"
title: "Python Guide"
description: "python에 대해 배우자"
date: "2016-06-11 00:22"
tags: [python, python-install]
comments: true
---


### Python 설치

[anaconda package](https://www.continuum.io/downloads#34)를 설치하는게 윈도우 환경에서는 수월하다.

혹 python.org에서 설치하고자 하신다면, 수동으로 python path를 잡아줘야 하고, pip 설치도 해야한다. 그런데 이게 여간 까다로운 작업이 아니며, 굉장히 난해하다. 더군다나 이제 막 프로그래밍에 입문했다면 더더욱. 고로, **아나콘다**를 이용해 python을 설치하도록 하자.

### Python 문법

#### 변수

파이썬에서의 변수는 동적 타이핑이므로 할당하면 알아서 Type을 지정해준다. 우선 파이썬에서 변수명에 사용 가능한 문자는 알파벳 대·소문자, 숫자, 밑줄(_)이다.

단, 변수명은 숫자로 시작할 수 없다. 특히, 변수명에 파이썬 언어에서 사용하는 Reserved Word의 경우 사용 불가능하다.
```python
userName = 'seungdols'
username = 'seungdols' #위 두 변수는 다른 변수이다.`
```
#### 할당

할당은 `=`으로 변수에 값을 대입한다고 생각하면 된다.

```python
userAge , userName = 30, 'seungdols' #여러 변수를 한 번에 할당도 가능하다.
```

#### 기본 연산자


기본 연산자로는 `+,-,*,/,//,%,**`이 존재한다.
기본적인 연산자는 기본 수학과 동일하고, `//`의 경우는 정수 나눗셈(내림 나눗셈)이라하고, `%`의 경우 나머지 연산자, `**`의 경우 제곱 연산을 뜻한다.

```python
>>> x , y = 5, 7
>>> x+y
12
>>> x-y
-2
>>> x/y
0.7142857142857143
>>> x//y
>>> x**y
78125
>>> x*y
35
>>>
```

추가로 += , -=, *= 3가지의 할당 연산자가 더 있다. 각각 의미는 아래 코드로 확인하자. x = x + y와 x+=y와 같은 의미이다.

```python
>>> x+y
12
>>> x+=y
>>> print(x)
12
>>> x - y
5
>>> x-=y
>>> print(x)
5
>>> x*y
35
>>> x*=y
>>> print(x)
35
>>> x = x * y
>>> print (x)
245
>>>
```

#### 데이터 타입

```python
int() , float(), str() # 세가지의 타입 변환 내장함수가 존재한다.
```

##### 정수

정수는 소수부가 없는 숫자를 말한다.

```python
>>> x+y
12
>>> x+=y
>>> print(x)
12
>>> x - y
5
>>> x-=y
>>> print(x)
5
>>> x*y
35
>>> x*=y
>>> print(x)
35
```

### 부동소수점(실수) 

실수는 소수부가 포함된 수들을 말하며, 정수보다 표현 범위가 크다.

```python
>>> f = 1233454.345345345
>>> print (f)
1233454.345345345
```

### 문자열

문자열은 `', "`로 감싸진 문자들의 나열을 말한다.

```python
a = 'seungdols'
print(a)
seungdols
```

### 리스트

리스트는 보통 서로 연관된 데이터의 모음이다. 비슷한 종류의 데이터를 별도의 변수에 저장하는 대신 리스트에 저장하면 편하다.

```python
userAges = [23,44,23,45,40,50]
userAges = []#이렇게 빈 리스트도 생성 가능하다.

userAges[0]#index는 0부터 시작한다.
userAges[-1]#reverse도 지원하며, -1이 가장 마지막 인덱스와 같다.
```

### 리스트 슬라이싱

list[시작:끝]을 하게 되면 slice라는 기법인데, 시작index부터 끝index까지의 리스트 범위를 사용할 수 있다. 단, 끝 index는 포함하지 않는다. 즉, 마지막 index-1까지만 포함 된다는 의미이다.

```python
userAges[0:5]
[23, 44, 23, 45, 40]
```
```python
userAges * 2
[23, 44, 23, 45, 40, 50, 23, 44, 23, 45, 40, 50]#리스트를 2번 곱한 것도 가능
userAges[0] = userAges[0] + 10 #리스트 원소내 값 수정도 가능하다.
print(userAges)
[33, 44, 23, 45, 40, 50]
```

### 튜플

아래와 처럼 튜플은 ()를 이용하여 생성하고, 리스트와 다른 점은 불변식이라는 점이다. 초기선언 이후 값을 변경 할 수 없다. 원소에 접근하는 것은 리스트와 같이 인덱스로 접근 한다.

```python
tup = (1,2,3,4,5)
tup
(1, 2, 3, 4, 5)
tup[0]
1
tup[0] = 2
Traceback (most recent call last):
  File "<pyshell#37>", line 1, in <module>
    tup[0] = 2
TypeError: 'tuple' object does not support item assignment>>>
```

### 사전

dictionary type의 경우는 Java의 HashMap과 비슷하다고 볼 수 있고, Key : value 형태를 갖는다. 특히 한 사전 데이터 타입 안에 같은 key가 중복 되어서는 안 된다. dict() 메소드로도 생성 할 수 있다.

```python
dic = {1:'one', 2:'two'}
dic[1]
'one'
dic[0]
Traceback (most recent call last):# key 0인 것이 없으니 오류가 난다.
  File "<pyshell#40>", line 1, in <module>
    dic[0]
KeyError: 0
```

#### Python 기초

조건

기본적인 비교연산자는 다른 언어들과 동일하다.

```python
5 != 2
True
>>> 5> 2
True
>>> 2 <5
True
>>> 5>= 5
True
>>> 2 <= 5
True
>>> 2 <= 2
True
```

### if / elif

C/C++, Java에서 쓰는 else if를 쓰지 않고, elif를 쓰는 것이 좀 차이가 있다.
그리고 아시겠지만, 파이썬은 {}를 쓰지 않고, indent 들여쓰기를 가지고 블록을 구분한다.

`if 조건 : code elif 조건 : code else : code`

```python
if True:
    print('a')
elif True:
    print('b')
else:
    print('c')
a
```

### 입력

input 함수를 쓰면 되고, 2.x 경우 raw_input()을 사용하는 것으로 알고 있다.

`userInput = input('Input Number : ')`

### 인라인 if문

`code A if condition else code B` 로 이루어지며, if 조건이 참이면 앞의 코드를 실행하고 그게 아니면 else 문장을 실행한다.

```python
>>> a
5
>>> a = 6 if a == 10 else 2
2
```

### 반복문

- list/tuple을 순회하는 iterator라고 보면 쉽다.


```python
li = [1,2,3,4,5]
for a in li:
    print(a)

1
2
3
4
5
```

- enumerate 함수를 이용하면 index를 추출 할 수 있다.

- range 함수를 이용하면, 자유롭게 반복을 구사할 수 있다.

- range(start, end, [step]) step : 증가치로 볼 수 있다.

- while 문 , 반복문에서 break와 continue를 사용할 수 있다.

### Try/Except

기존 코드를 try 영역에 두고, 예외 발생할 경우 처리할 코드를 except영역에 두면 된다. `try: except: `  기본 적인 Error의 경우는 다음과 같다. ` IOError: ImportError: IndexError: KeyError: NameError: TypeError: `  자세한 것은 파이썬 홈페이지에서 확인 할 수 있다.

```python
 try:
    answer = 12/0
    print(answer)
except:
    print("Error")

Error
```

### 함수

코드를 함수로 정의 해두고 사용하면 편리해진다. 그리고 코드의 관리 차원에서도 좋고, 호출하기도 쉽다. 그렇지 않으면, 찾을 때도 번거롭고, 사용하기에도 불편함이 따른다. 그래서 보통 함수단위로 작성하는 버릇이 좋다.

```python
def 함수명(매개변수):

	return [표현식]


def hello():
    print('hello')

>>> hello()
hello
```

### 모듈 추가하기

`import module name`  이렇게 한다면 모듈명.함수명으로 호출을 해야 하지만 `from module name import function name` 이렇게 하면 아래와 같이 randint를 바로 사용할 수 있게 된다.

```python
import os
from random import randint
a = randint(1,5)
>>> a
4
os.path
```

### 파일 입·출력

파일 쓰기모드 'w' - 파일이 존재하지 않으면, 생성한다.

```python
file = open('text.txt','w')
file.write('File IO Test')
12
>>> file.close()
```

파일 읽기 모드 'r' - 파일이 없으면, 오류를 발생한다.

```python
file = open('text.txt','r')
>>> line = file.read()
>>> >>> line
'File IO Test'
>>> file.close()
```
파일 추가모드 'a' - 파일의 끝부터 차례로 추가시킨다.

```python
file = open('text.txt','a')
>>> for line in range(1, 10):
    file.write('\n line '+str(line))
>>> file.close()
```

위 코드의 파일 다시 읽기

```python

file = open('text.txt','r')
>>> for line in file:
    print(line)

File IO Test

 line 1

 line 2

 line 3

 line 4

 line 5

 line 6

 line 7

 line 8

 line 9>>> file.close()
```

`파일 읽기/쓰기 모드 'r+'가 존재한다. 바이너리의 경우 'rb' , 'wb'가 존재한다.`

버퍼를 이용하여 파일 읽기

```python
file = open ('text.txt','r')
>>> data = file.read(10)#10글자씩만 읽어들인다고 지정한다.
>>> while len(data):
    	print(data)
    	data = file.read(10)
```
