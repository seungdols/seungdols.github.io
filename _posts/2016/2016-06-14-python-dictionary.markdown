---
layout: "post"
title: "python dictionary"
description: "python dictionary 관련 함수에 대해 알아보자."
date: "2016-06-14 00:45"
tags: [python, dic, dictionary]
comments: true
---

##### 사전(Dictionary DataType) 관련 함수

#### clear
- 사전에 저장된 데이터를 날린다.


```python
>>> dic1 = {1:'one', 2:'tow'}
>>> print(dic1)
{1: 'one', 2: 'tow'}
>>> dic1.clear()
>>> print(dic1)
{}
```

#### del
- 사전 전체 삭제


```python
>>> dic1 = {1:'one', 2:'tow'}
>>> del dic1
>>> print(dic1)
Traceback (most recent call last):
  File "<pyshell#95>", line 1, in <module>
    print(dic1)
NameError: name 'dic1' is not defined
```

#### get
- key를 이용해 값을 가져온다.


```python
>>> dic1 = {1:'one', 2:'tow'}
>>> dic1.get(1)
'one'
```

#### in
- 사전에 존재하는지 검사한다.


```python
>>> 1 in dic1 # 키 기반 검사
True
>>> 'one' in dic1.values() # 값 기반 검사
True
```

#### items

- 사전을 리스트로 반환한다.


```python
>>> dic1.items()
dict_items([(1, 'one'), (2, 'tow')])
```

#### keys
- 키를 리스트로 반환한다.


```python
>>> dic1.keys()
dict_keys([1, 2])
```

#### len
- 사전의 길이를 반환한다.


```python
>>> len(dic1)
2
```

#### update
- 사전의 키-값 쌍들을 다른 사전에 추가한다. * 중복은 제거한다.


```python
>>> dic1 ={1:'one',2:'two'}
>>> dic2 ={1:'one',3:'three'}
>>> dic1.update(dic2)
>>> print(dic1)
{1: 'one', 2: 'two', 3: 'three'}
>>> print(dic2)
{1: 'one', 3: 'three'}
```

#### values
- 사전의 모든 값을 리스트로 반환한다.


```python
>>> dic1.values()
dict_values(['one', 'two', 'three'])
>>>
```
