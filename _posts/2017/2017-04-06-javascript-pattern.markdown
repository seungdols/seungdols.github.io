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

##### eval() 피하기

보안상에도 좋지 않고, 주로 사용하지 않으므로 아예 쓰지 말자.
쓰는 코드를 발견할 경우 즉시, 변경하도록 하자.

특히, `setInterval(), setTimeout(), Function()` 생성자에 문자열을 넘기는 것도 eval()과 유사한 기능을 하므로, 지양하자.

```javascript
//안티패턴
setTimeout("seungdolsFunc()", 1000);
setTimeout("seungdolsFunc(1,2,3)", 1000);


//권장사항
setTimeout(seungdolsFunc, 1000);
setTimeout(funtion() {
    seungdolsFunc(1,2,3);
}, 1000);

```

##### parseInt()를 통한 숫자 변환

parseInt()를 사용하면 문자열로부터 숫자 값을 얻을 수 있다. 이 함수의 두번째 매개변수로 기수를 받는데, 생략하는 경우에 많지만 그럼 안된다. 파싱할 문자열이 0으로 시작할 경우 문제가 생길 수 있다.
일관성 없고, 예측을 벗어나는 결과를 피하려면 항상 기수 매개변수를 지정해주어야 한다.

```javascript
var strMonth = "06";
var month = parseInt(strMonth, 10);
```

## 3장 - 리터럴과 생성자

```javascript
var dog = {};
```

위는 빈 객체를 생성하는 객체 리터럴 표기법이다.
그러나, 참고해야 할 것은 자바스크립트에는 빈 객체라는 것은 없으며, 쉽게 말하는 의미이다.
간단한 객체 자체도, Object.prototype에서 상속받은 프로퍼티와 메서드를 가지고 있다.

```javascript
var dog = {gender: "M"};//권장 패턴

var cat = new Object(); //안티패턴
cat.gender = "W";
```

리터럴 표기법을 사용시, 유효범위 판별 작업도 하지 않는다. 생성자 함수 사용시에는 지역 유효범위에 동일한 이름의 생성자가 있을 수 있어, Object()를 호출한 위치에서부터 전역 Object 생성자까지 인터프리터가 쭉 거슬러 올라가면서 유효범위를 검색해야 한다.

##### 사용자 정의 생성자 함수

```javascript
var Person = function(name) {
    this.name = name;
    this.say = function() {
        return "I am " + this.name;
    };
}

var adam = new Person("Adam");
adam.say();
```

new 와 함께 생성자 함수를 호출하면, 함수 안에서 아래와 같은 일이 벌어진다.

* 빈객체가 생성되며, 이 객체는 this라는 변수로 참조할 수 있고, 해당 함수의 프로토타입을 상속받는다.
* this로 참조 되는 객 체에 프로퍼티와 메서드가 추가 된다.
* 마지막에 다른 객체가 명시적으로 반환되지 않을 경우, this로 참조된 이 객체가 반환된다.

코드의 내부적인 상황을 표현하면 이렇게 된다.
```javascript
var Person = function(name) {

    var this = {};

    this.name = name;
    this.say = function() {
        return "I am " + this.name;
    };

    return this;
}
```

##### 생성자의 반환 값

생성자의 반환값으로는 생성자 함수를 new와 함께 호출하면 항상, 객체가 반환 된다. 기본 값으로는 this호 참조되는 객체다.
함수내에 return문을 쓰지 않더라도 생성자는 암묵적으로 this를 반환한다.

생성자는 new와 함께 호출 될뿐이지, 다른 함수와 다를바가 없다. 그러나 new를 생략하면, 생성자 내부의 this 참조가 전역 객체인 window를 가리키므로 오류를 발생할 가능성을 높인다.

### 배열 리터럴

```javascript
var arr = new Array('seungdols','seungho','speed');//안티 패턴

var arr = ['seungdols', 'seungho','speed'];
```

참고로 자바스크립트에서 배열은 `object` 타입이다.
더군다나, new Array로 생성할 경우 오류가 발생할 요지가 있는데, 이는 인자로 숫자를 줄 경우이다.

```javascript
var arr = new Array(3);

console.log(arr.length);
```
인자로 숫자를 주면 배열의 길이가 인자로 준 숫자 값으로 배열을 만드는데, 각 원소는 `undefined`로 생성 된다.
특히나, 부동 소수점으로 인자를 줄 경우 더욱 문제가 심해진다.
부동 소수점을 인자로 줄 경우에는 `RangeError`가 발생한다.

### 정규 표현식 리터럴

```javascript
var re = '/\\/gm';

var reg = new RegExp('\\\\','gm');//안티 패턴
```

* g: 전역매칭
* m: 여러줄 매칭
* i: 대소문자 구분 없이 매칭

### 에러 객체

자바스크립트에는 `Error()`, `SyntaxError()`, `TypeError()`등 여러 가지 에러 생성자가 내장 되어 있으며, `throw`문과 함께 사용된다.
이 생성자들을 통해 생성된 에러 겍체들은 다음과 같은 프로퍼티를 가진다.

* `name`
 * 객체를 생성한 생성자 함수의 name 프로퍼티, 범용적인 'Error'일 수 도 있고, 'RangeError'와 같이 좀 더 특화된 생성자 일 수 있다.

* `message`
 * 객체를 생성할 떄 생성자에 전달된 문자열

```javascript
try {
    throw{
        name : "MyErrorType",
        message: "oops",
        extra: "This was rather embarrassing",
        remedy: genericErrorHandler
    };
} catch(e) {
    alert(e.message);

    e.remedy();
}
```

에러 생성자를 new 없이 일반 함수로 호출해도 new를 써서 생성자로 호출한 것과 동일하게 동작하여 에러 객체가 반환된다.
