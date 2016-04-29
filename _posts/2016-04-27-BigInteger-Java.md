---
layout: post
title: "Use BigInteger of Java "
description: "자바에서 큰 정수형을 사용하고 싶을 때, BigInteger를 사용하자"
tags: [java, programming]
---
###Java BigInteger class 사용하기

주로 int 형 타입을 사용하게 되는데, 이 정수형 타입은 허용 가능한 범위가 존재합니다.

그럴때는 Java 언어에서 지원하는 BigInteger Class, BigDecimal Class를 사용할 수 있습니다.

해당 클래스 또한 표현 가능한 범위가 있는 것으로 알고 있는데, 대략 100억은 가볍게 표현 가능합니다.

사용하는 방법은 간단합니다.
(참고로 Class가 무엇인지, API가 무엇인지는 알고 계셔야 합니다.)

```java
 public void longNumberSum(long x, long y)
    {
        BigInteger a = BigInteger.valueOf(x);
        BigInteger b = BigInteger.valueOf(y);
        BigInteger result = a.add(b);//Class 이므로 메소드 형태로 가감승제 외 다른 기능을 제공합니다.

        System.out.println(result);
    }
```
예를 들어 인자로 x, y를 입력하는데 두 수의 크기가 long이고, 덧셈을 한다면 오버플로우(Overflow)를 발생할 수 있으습니다.

오버플로우는 연산간의 오작동인데, 이를 악용한 해킹 기법도 존재하므로 없어야 하는 프로그램 상의 버그입니다.

위처럼 사용하면 되고, BigInteger.Zero/One/Ten 같은 Immutable한 미리 정의 된 값들도 있으니 상수 값을 사용한다면 바로 사용하시면 될 것 같습니다.


[BigInteger Class API](https://docs.oracle.com/javase/7/docs/api/java/math/BigInteger.html)
