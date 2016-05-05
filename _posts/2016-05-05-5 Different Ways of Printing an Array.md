---
layout: post
title: "5 Different Ways of Printing an Array"
description: "Use Array of Java"
tags: [java, array]
comments: true
---


이것은 자바 프로그래머들을에게  가장 자주 질문들 중 하나다.

자바에서 배열의 원소를 출력하는 방법에 대해 다른 5가지를 보여준다.

```java
  Arrays.toString(arr);
```

```java
  for ( int n : arr )
  System.out.println(n + " ,");
```  

```java
  for(int i = 0; i < arr.length; i++){
  System.out.print(arr[i] + ",");
  }
```  

```java
  System.out.List(Arrays.asList(arr));
```

```java
  Arrays.asList(arr).stream().forEach(s -> System.out.println(s));
```

확실히 첫번째가 충분히 간결하다.

만약 배열이 다차원 배열이라면, 그것은 배열의 원소들을 포함한다.

우리는 Arrays.deelToString ()을 사용할 수 있다.

```java
  System.out.println(Arrays.deepToString(arr));
```

[기사 본문](http://www.programcreek.com/2015/03/print-an-array-in-java)
