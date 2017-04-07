---
layout: "post"
title: "javascript-pattern"
description: "JavaScript Patterns 리뷰"
date: "2017-04-06 17:39"
tags: [book, javascript]
comments: true
---

# [책] JavaScript Patterns


## 2장 기초

### 전역변수 최소화

전역변수의 문제점은 자바스크립트 어플리케이션이나 웹페이지 내 모든 코드 사이에서 공유가 된다.
모든 전역변수는 동일한 전역 네임스페이스 공간 안에 존재하게된다.

그래서 무조건 var  키워드를 변수 선언시 사용하는 것이 좋다.
1. 자바스크립트에서는 var 키워드를 쓰지 않고도 변수 생성이 가능하다.
2. 암묵적 전역이라는 개념이 존재하여, 선언 하지 않고 사용할 경우 명시적으로 선언한 전역변수와 다를바 없다.

```javascript
function foo() {
    var a = b = 0;
}
```

위처럼 코드를 작성하면, b는 전역변수가 된다. 평가 방식이 우측에서 좌측으로 이동한다.

var선언을 빼먹음으로써 부족용이 2가지 있다.

1. var를 사용하여 명시적으로 선언된 전역 변수는 삭제 할 수 없다.
2. var를 사용하지 않고, 생성한 암묵적 전역변수는 삭제 할 수 있다.

##### 단일 var 패턴

* 함수에서 필요한 변수를 한군데서 찾을 수 있다.
* 변수를 선언하기 전에 사용할 때 발생하는 로직상의 오류를 막아준다.
* 변수를 먼저 선언한 후에 사용해야 한다는 사실을 상기시킨다.
* 코드량이 줄어든다.

```javascript
function funcexmaple() {
    var a = 1,
        b = 2,
        sum = a + b,
        myObject = {},
        i,
        j;
}
```

초기 값없이 선언된 변수들을 모두 undefined라는 값으로 초기화 된다.

##### 암묵적 타입캐스팅 피하기

자바스크립트는 변수를 비교할 떄 암묵적으로 타입캐스팅을 실행한다. 그런 이유로 `false == 0` 이나 """== 0과 같은 비교가 true를 반환한다.
암묵적 타입 캐스팅을 피하기 위해서는 항상 표현식의 값과 타입을 모두 확인하는 `===`, `!==` 연산자를 사용해야 한다.

```javascript
var seungdols = 0;
if (seungdols === false) {

}

//anti-patterns
if (seungdols == false) {

}
```
