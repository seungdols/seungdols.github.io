---
layout: "post"
title: "python-list"
description: "python list 함수들에 대해 알아보자."
date: "2016-06-14 00:32"
tags: [python, list]
comments: true
---

##### 리스트(List DataType) 관련 함수

#### append

- 매개변수로 주어진 원소를 추가한다.

```python
>>> myList =['a','b','c','d']
>>> myList.append('e')
>>> print (myList)
['a', 'b', 'c', 'd', 'e']
```

#### del
- 해당하는 원소를 삭제한다.


```python
>>> del myList[0]
>>> print(myList)
['b', 'c', 'd', 'e']
>>> del myList[0:2]
>>> print(myList)
['d', 'e']
```

#### extend
- 리스트를 확장한다.


```python
>>> li1= [ 1,2,3]
>>> li2= [4,5,6]
>>> li1.extend(li2)
>>> print (li1)
[1, 2, 3, 4, 5, 6]
```

#### in
- 리스트 내에 해당하는 원소가 있는지 검사한다.


```python
>>> 1 in li1
True
```

#### insert
- 해당하는 인덱스 지점에 값을 삽입한다.

```python
>>> li1.insert(0,0)
>>> print(li1)
[0, 1, 2, 3, 4, 5, 6]
```

#### len
- 리스트 길이를 반환한다.


```python
>>> print(len(li1))
7
```

#### pop
- 매개변수로 주어진 원소를 제거하고, 해당 원소를 반환한다.


```python
>>> ret = li1.pop(2)
>>> print (ret)
2
>>> li1
[0, 1, 3, 4, 5, 6]
>>> print(li1.pop())
6
```

#### remove
- 매개변수로 주어진 해당 원소를 삭제한다.

```python
>>> li1
[0, 1, 3, 4, 5]
>>> li1.remove(3)
>>> li1
[0, 1, 4, 5]
```

#### reverse
- 리스트를 역순으로 뒤집는다.


```python
>>> li1.reverse()
>>> print(li1)
[5, 4, 1, 0]
```

#### sort
- 리스트를 정렬한다.

```python
>>> li1.sort()
>>> print(li1)
[0, 1, 4, 5]
```

#### sorted
- 원본 리스트는 정렬되지 않지만, 정렬된 리스트를 반환한다.


```python
>>> li = [5,4,2,5,-3,3]
>>> li2 = sorted(li)
>>> li2
[-3, 2, 3, 4, 5, 5]
>>> li
[5, 4, 2, 5, -3, 3]
>>>
```

#### 덧셈 연산자 +
- 리스트를 결합한다. 곱셈 연산자도 가능하다.


```python
>>> li1 + li2
[0, 1, 4, 5, 4, 5, 6]
>>> li1
[0, 1, 4, 5]
>>>
```
