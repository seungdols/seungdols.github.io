---
layout: "post"
title: "java generics vs c# generics"
description: "Java의 제네릭의 지우개 기능에 대해 알아보자"
date: "2016-06-14 00:20"
tags: [java, c#, generics]
comments: true
---

### [Java Erasure Generic](http://seungdols.tistory.com/421) vs C# Reification Generic

Complie Time이 지나 Java의 경우 ByteCode로 변환 될 경우 Generic에 관한 Type Info가 날라간다. 즉,

```java
List <Orange> = new ArrayList<Orange>();
List <Apple> = new ArrayList<Apple>();
```

위의 Type 정보는 `Byte Code`를 확인해보면 아래와 같이 **타입 정보**가 사라진다는 것을 알 수 있다.

```java
List <> = new ArrayList<>();
List <> = new ArrayList<>();
```

제네릭이 **컴파일 타임의 Type Check에 대한 기능**을 기본적으로 가지게 되는데 `Java의 경우 컴파일 트릭`에 불과하다.

```java
public class GenericsErasure {

    public static void main (String[] args) {

        List<Apple> appleList = new ArrayList<> ();
        List<Orange> orangeList = new ArrayList<> ();

        Apple apple = new Apple ();
        add (apple);

        add (orangeList);
    }
    public static <E>void add (E fruit)
    {
        if(fruit instanceof Apple)
        {
            System.out.println ("Apple");
        }
        else if(fruit instanceof List<?>)
        {
            System.out.println ("Apple List");
        }

    }

}

class Apple
{

}
class Orange
{

}
```

위 코드의 실제 결과

```bash
Apple
Apple List
```

즉, 이 것은 버그이다. List<Apple>이여야지만 동작해야하는데 orangeList를 전달해도 appleList로 동작하게 되는 것이다.

List<?> 즉 List 타입의 와일드카드를 도입하므로 `Type Checking`에 Bug가 생긴다.


`List<Apple> 입력시 컴파일 에러가 발생된다.`

C#의 경우 컴파일 되고나면, `IL (Intermediate Language)`로 번역이 되는데, 이 번역된 코드는 `CLR ( Common Language Runtime )` 위에서 동작하게 된다.

그런데 이 IL 코드에는 `Generic Type Info`가 존재한다. 결과적으로 제네릭에 관한 구체적인 정보를 컴파일 이후 코드에도 존재하게 된다.

그리하여 Java Generic의 한계와 단점은 다음과 같다.

  * 타입 조사 (Introspection)
  * 오버로드 (Overload)
  * 객체 생성 (Instantiation)
  * 리플렉션 (Reflection)
  * 성능 (Performance)



### 예외


실행시간 예외 ( Runtime Exception )

  * NullPointerException
  * IndexOutOfBoundsException

검사된 예외 ( Checked Exception )

  * Complie Time에 검사를 하므로 코딩하는 시점에서 강제로 예외처리.
  * Call Site에서 반드시 Try-Catch 구문을 사용해야 함.

이 두 예외에 관해서는 언어 설계자의 철학이 달라서라고 할 수 있다.


앤더스 하일스 버그

> C#에서 검사된 예외는 확장성과 호환성에 문제를 가지므로 제외 시켰으며, Try-catch • Try-finally 구문 둘 중에서 1:9의 비율로 Try-finally 구문을 많이 사용해야 한다.

제임스 고슬링

> 예외는 발생한 곳에서 가장 가까운 지점에서 해당 예외를 처리 해야한다. 모든 예외처리가 실행 시간에 예외라면 프로그램 종료 말고, 어떻게 예외를 처리를 하겠는가?라고 했다고 한다.

제임스 고슬링의 경우 `간결성의 원칙`과 로보틱스의 주된 관심사로 인한 철학을 가진 사람이기에 Java 설계시에도 그런 영향을 정말 많이 준 것으로 보인다.



#### 타입 시스템 ( Type System )

`Java`

* 참조타입 (Reference Type)
* 원시타입 (Primitive Type)

`C#`

* 참조타입 (Reference Type)
* 값 타입 (Value type)


#### Java8의 특징

* 람다 표현 ( Lambda Expression )
	- 내부적으로는 실제 구현 객체로 변함.
* 함수 인터페이스 (Functional Interface) = SAM(Single Abstract Method)
* 메서드 참조 (Method Reference)
    - 함수포인터와 비슷함
* 스트림 (Stream) API
* 디폴트 메서드 ( Default Method )
	- C#의 Extension Method 개념을 유사하게 지원함.C#의 Extension Method 개념을 유사하게 지원함.
    - 기존 Interface rule을 변경하여 Interface 내에 Body를 포함하는 Method 추가 가능.

#### C#의 특징

* 람다 (Lambda)
  - delegate로 변함
* 대리자 (delegate)
  - Function Pointer와 비슷함
* [링큐 (LinQ)](###LinQ)
* 확장 메서드 (Extension Method)

### LinQ

에릭 마이어 - 크래커출신 MS사에서 앤더스 하일스 버그와 같이 C#을 작업함.

* 링큐를 가능하게 만든 C#에서 기본적으로 지원하는 문법 기능

    - 질의 표현 (Query Expression)
    - 암묵적 타입 변수 (Implicitly typed Variables)
    - 객체와 컬렉션 초기자 (Object and Collection Initializers)
    - 익명 타입 (Anonymous Types)
    - 확장 메소드 (Extension Methods)
    - 람다 표현식 (Lambda Expression)
    - 자동 구현 속성 (Auto-Implemented Properties)



출처 : [MVA 임작가님 - C# 세미나](https://mva.microsoft.com/ko/training-courses/-c--11209?l=hXTcSPHBB_6704984382)
