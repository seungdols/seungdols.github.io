---
layout: "post"
title: "Java-Obj Equality and Obj Identity"
description: "Java를 사용하는 신입 개발자가 익혀야 할 것"
date: "2016-05-28 13:02"
tags: [java, programming]
comments: true
---

### Java를 사용하는 신입 개발자가 익혀야 할 것.

#### 바로, Object Equality and Object Identity의 차이를 아는 것이다.

이 것이 왜 중요할까?

바로, 우리가 흔히 사용하기 때문이기도 하고, 가장 중요한 객체 지향의 개념이기 때문이다.

이를 예시로 들자면 자바의 문자열로 예시를 들 수 있다.

```java
public class ObjExe {
    public static void main(String[] args) {
        String str1 = "str";
        String str2 = new String("str");
        String str3 = str1;

        if(str1 == str3)
            System.out.println("1: true");
        if(str1 == str2)
            System.out.println("2: true");
        if(str2 == str3)
            System.out.println("3: true");

        if(str1.equals(str2))
            System.out.println("4: true");
        if(str2.equals(str3))
            System.out.println("5: true");

    }
}
```

위 코드의 출력을 예상해보자. 어떤 출력이 나올까?

정답은 바로, 아래와 같다.

```text
1: true
4: true
5: true
```

왜 그런지 아는가? Object Identity는 객체의 동일성을 말하며, Object Equality는 객체의 동등성을 말한다.

고로, 동일성은 참조가 같은지?를 말하는 것이고, 동등성은 값이 같은지?를 말한다.

위 코드에서는 결국, 값은 모두 같다. "str"이란 문자열이란 값은 같다.
하지만, 동일성에 대해서는 다르다.

객체 리터럴로 생성한 문자열과 new 연산자를 통해 생성한 문자열 객체는 동일한 객체가 아니다.

그러기때문에 1,4,5번만 if문의 참이 인정이 된다.

equals()의 경우 해당 문자열의 값이 같은지를 비교한다.

==의 경우 해당 객체들이 동일한지를 비교한다.

이 차이를 계속적으로 학습하고, equals(), hashcode() overriding에 대해서도 공부하는 것이 좋을 것 같다.

쉽게 결론을 내보도록 하자.

1. Object Identity : 두 개의 Obj가 같은 메모리 영역의 객체를 가리킨다.
2. Object Equality : 두 개의 Obj를 .equals() 메소르로 비교하였을때, 참 값이 나오는 경우를 말한다.

모든 내용은 오류를 포함하고 있을 수 있습니다. 항상 태클과 지적 감사합니다.

조언은 더 나음으로 향하는 길이라 생각합니다.
