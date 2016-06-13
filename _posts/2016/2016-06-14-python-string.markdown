---
layout: "post"
title: "python string"
description: "python string 관련 함수에 대해 알아보자."
date: "2016-06-14 00:39"
tags: [python, string]
comments: true
---

##### 문자열(String) 관련 함수

#### count

문자열 내 매개변수로 주어진 문자의 횟수를 리턴한다. 이 함수는 문자열 비교 시 대소문자를 구별한다.

```python
>>> a = 'suengdols company'
>>> a.count('s')
2
>>> a.count('s', 4)
1
>>> a.count('s', 4, 10)
1
```

#### endswith

문자열이 접미사로 끝나면, True 리턴 아니면 False 리턴한다. 접미사는 둘 이상의 접미사를 결합한 튜플로 지정할 수도 있다.
이 함수는 대소문자를 구별한다.

```python
>>> a.endswith('company')
True
>>> a.endswith('company',3)
True
>>> a.endswith('company',0,3)
False
```

#### find/index
문자열에서 검색 문자열이 처음으로 나오는 인덱스를 리턴한다.
find()는 검색 문자열을 찾을 수 없을 때 -1을 리턴한다. index()는 검색 문자열을 찾을 수 없을 때 ValueError를 리턴한다.
이 함수는 대소문자를 구별한다.

```python
>>> a.find('com')
10
>>> a.index('com')
10
>>> a.find('comc')
-1
>>> a.index('comc')
Traceback (most recent call last):
  File "<pyshell#119>", line 1, in <module>
    a.index('comc')
ValueError: substring not found
```

#### isalnum
문자열에 최소 한개의 문자가 존재하며 모든 문자가 알파벳이나 숫자일 때 True 나머지는 False 리턴한다.

```python
>>> a.isalnum()
False
>>> '123asdf'.isalnum()
True
```

#### isalpha
문자열에 최소 한 개의 문자가 존재하고, 모든 문자가 알파벳일 때 True를 리턴한다.

```python
>>> 'asdf'.isalpha()
True
>>> 'as3adf'.isalpha()
False
```

#### isdigit
문자열에 최소 한 개의 문자가 존재하며 몬든 문자가 숫자일 때만 True를 리턴한다.

```python
>>> '1234'.isdigit()
True
>>> 'ad234'.isdigit()
False
```

#### islower
모든 문자열이 소문자일때 True를 리턴한다.

```python
>>> 'asdf'.islower()
True
>>> 'ASDF'.islower()
False
```

#### isspace
문자열에 최소 하나의 문자가 존재하고, 모든 문자가 공백문자일 때 True를 리턴한다.

```python
>>> ' '.isspace()
True
>>> 'a '.isspace()
False
```

#### istitle
문자열에 최소 한 개의 문자가 존재하고, 대소문자가 존재하지 않는 문자(숫자,공백) 다음에 나오는 대소문자 구별 가능 문자는 모두 대문자이고, 나머지 대소문자 구별 가능 문자는 소문자일 때, True를 리턴한다.

```python
>>> 'This Is A String'.istitle()
True
>>> 'This is a string'.istitle()
False
```

#### isupper
문자열에 대소문자를 구별할 수 있는 문자가 최소 한개 존재하고, 그런 문자가 모두 대문자일 때, True를 리턴한다.

```python
>>> 'ASDF'.isupper()
True
>>> 'asdf'.isupper()
False
```

#### join
매개변수로 전달된 데이터 항목들을 결합하되 각 항목은 구분자('-')로 구별한다.

```python
>>> sep = '-'
>>> t = ('a','b','c')
>>> sep.join(t)
'a-b-c'
>>> a = 'asdf'
>>> sep.join(a)
'a-s-d-f'
```

#### lower/upper
lower()의 경우 모든 문자를 소문자로 변환한 문자열을 리턴한다.
upper()의 경우 모든 문자를 대문자로 변환한 문자열을 리턴한다.

```python
>>> 'ASDF'.lower()
'asdf'
>>> 'asdf'.upper()
'ASDF'
```

#### replace
이 전 값을 새 값으로 대체한 문자열을 리턴한다. 횟수는 문자열 시작부터 횟수만큼만 대체한다.

```python
>>> 'asdfss'.replace('s','^')
'a^df^^'
>>> 'asdfss'.replace('s','^',2)
'a^df^s'
```

#### split
구분자를 기준으로 문자열을 쪼갠 후 결과를 단어 리스트로 리턴한다. 대소문자를 구별한다.

```python
>>> 'a g b'.split(' ')
['a', 'g', 'b']
```

#### splitlines
문자열에서 라인피드를 기준으로 문자열을 나눈 후 결과를 리스트로 반환한다.

```python
>>> 'asdfasdfasdf\nasdfasdfasdf'.splitlines()
['asdfasdfasdf', 'asdfasdfasdf']
```

#### startswith
문자열이 해당 접두사로 시작하면 True를 리턴한다.

```python
>>> 'seungdols company'.startswith('seun')
True
>>> 'seungdols company'.startswith('seun',0)
True
>>> 'seungdols company'.startswith('seun',0, 2)
```

#### strip
- 좌우 공백을 제거한다.

```python
>>> ' asdf '.strip()
'asdf'
```
