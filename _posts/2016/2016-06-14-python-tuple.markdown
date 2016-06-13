---
layout: "post"
title: "python tuple"
description: "python tuple 관련 함수에 대해 알아보자."
date: "2016-06-14 00:42"
tags: [python, tuple]
comments: true
---

##### 튜플(Tuple) 관련 함수

#### del
튜플 전체 삭제


```python
>>> mytuple = (1,2,3,4)
>>> del mytuple
>>> print(mytuple)
NameError: name 'mytuple' is not defined
```


#### in
튜플 내 해당 원소가 있는지 검사한다.

```python
>>> mytuple = (1,2,3,4)
>>> 1 in mytuple
True
```

#### len
길이를 반환한다.

```python
>>> len(mytuple)
4
```

#### 덧셈 연산자 +

튜플을 더한다.

```python
>>> mytuple
(1, 2, 3, 4)
>>> mytuple + (5,6)
(1, 2, 3, 4, 5, 6)
```

#### 곱셈 연산자 *
튜플을 곱한다.

```python
>>> mytuple * 2
(1, 2, 3, 4, 1, 2, 3, 4)
>>>
```
