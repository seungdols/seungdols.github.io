---
layout: post
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

자바스크립트는 변수를 비교할 떄 암묵적으로 타입캐스팅을 실행한다.
그런 이유로 `false == 0` 이나 """== 0과 같은 비교가 true를 반환한다.
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

parseInt()를 사용하면 문자열로부터 숫자 값을 얻을 수 있다.
이 함수의 두번째 매개변수로 기수를 받는데, 생략하는 경우에 많지만 그럼 안된다.
파싱할 문자열이 0으로 시작할 경우 문제가 생길 수 있다.
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

리터럴 표기법을 사용시, 유효범위 판별 작업도 하지 않는다.
생성자 함수 사용시에는 지역 유효범위에 동일한 이름의 생성자가 있을 수 있어, Object()를 호출한 위치에서부터 전역 Object 생성자까지 인터프리터가 쭉 거슬러 올라가면서 유효범위를 검색해야 한다.

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

---

## 4장 - 함수

* 함수는 일급객체이다.
* 함수는 유효범위를 가진다.
* 런타임 시점에 생성 가능하다.
* 변수에 할당 가능하며, 다른 변수에 참조를 복사 할 수 있고, 확장 가능하고, 몇몇 특별한 경우를 제외하면, 삭제 할 수 있다.
* 다른 함수의 인자로 전달할 수 있고, 다른 함수의 반환 값이 될 수 있다.
* 자기 자신의 프로퍼티와 메서드를 가질 수 있다.

```javascript
//기명 함수 표현식 (named function expression)
var add = function add(a,b) {
    return a + b;
};

//(무명)함수 표현식 or 익명 함수 (anonymous function)
var add = function(a,b) {
    return a + b;
};
```
위 둘의 차이점은 별다른 것은 없고, 함수 객체의 name property가 빈문자열이 된다는 차이 밖에 없다.

```javascript
//함수 선언문(function declaration)
function add(a,b) {
    //
}
```

함수 선언문은 전역 유효범위나 다른 함수의 본문 내부, 즉 '프로그램 코드'에서만 쓸 수 있다.

```javascript
callMe(function() {
    //unnamed function expression
});

callMe(function me() {
    //named function expression
});

var seungdolsObject = {
    say : function() {
        //function expression
    }
}
```
위의 예 같은 경우에는 함수 선언문을 쓸 수 없다.

### 함수 호이스팅

```javascript
function foo() {
    alert('global foo');
}
function bar() {
    alert('global bar');
}

function hoistMe() {
    console.log(typeof foo);//function
    console.log(typeof bar);//undefined

    foo();//local foo
    bar();//TypeError

    function foo() {
        alert('local foo');
    }

    var bar = function() {
        alert('local bar');
    };
}
hoistMe();
```
결국 선언된 위치와 상관 없이 끌어 올려지며 전역의 foo, bar를 덮어쓴다.

그런데, 지역변수 foo()는 나중에 정의되더라도 상단으로 호이스팅 되어 정상 작동하나, bar()의 정의는 호이스팅 되지 않고, 선언문만 호이스팅 된다.

때문에 bar()의 정의가 나오기 전까지는 undefined 상태이고, 따라서 함수로 사용할 수 없다.
또한 선언문 자체는 호이스팅 되어 유효범위 체인 내에서 전역 bar()도 보이지 않는 이유이다.

* 함수는 객체다.
* 함수는 지역 유효범위를 제공한다.

### 콜백 패턴

콜백 패턴은 이를테면, 함수의 특징 중 하나인 `다른 함수의 인자로 전달 할 수 있다.`를 활용한 방법이다.

```javascript
function requestSpec(callback) {
    //working
    callback();
}
```
위와 같은 코드를 콜백 패턴이라 말하는데, 요즘은 사실 콜백 헬 패턴이 될 가능성이 높아서 꺼리는 패턴이긴 하다.

#### 콜백과 유효범위

콜백이 일회성의 익명함수나 전역 함수가 아니고, 객체의 메서드인 경우도 많은데, 이 때, 콜백 메서드가 자신이 속한 객체를 참조하기 위해 `this`를 사용하면, 예상치 않게 동작할 수 있다.

```javascript
var seungdolsApp = {};
seungdolsApp.color = "green";
seungdolsApp.paint = function(node) {
    node.style.color = this.color
};

var findNodes = function (callback) {
    if(typeof callback === "function") {
        callback(found);
    }
};
```
`this.color`가 정의 되지 않아 예상대로 동작 할 수 없다. 그리하여 동작 할 수 있도록 변경한다.

```javascript
var findNodes = function (callback, callback_obj) {
    if(typeof callback === "function") {
        callback.call(callback_obj,found);
    }
}

findNodes(seungdolsApp.paint, seungdolsApp); -> findNodes('paint', seungdolsApp);//이렇게 바꿀 수 있다.
var findNodes = function (callback, callback_obj) {
    if (typeof callback === 'string') {
        callback = callback_obj[callback];
    }
    if (typeof callback === "function") {
        callback.call(callback_obj,found);
    }
};
```

##### 함수 반환하기

함수는 객체이기 때문에 반환 값으로 사용될 수 있다.
즉, 함수의 실행 결과로 꼭 어떤 데이터 값이나, 배열을 반환할 필요는 없다는 뜻이다.

```javascript
var setup = function() {
    alert(1);
    return function() {
        alert(2);
    };
};

var my = setup();
my();
```

##### 자기 자신을 정의하는 함수

함수는 동적으로 정의 할 수 있고, 변수에 할당 할 수 있다.
새로운 함수를 만들어 이미 다른 함수를 가지고 있는 변수에 할당하면, 새로운 함수가 이전 함수를 덮어 쓰게 된다.

```javascript
var scareMe = function() {
    alert('Boo');
    scareMe = function () {
        alert('Double Boo');
    };
};

scareMe();
scareMe();
```

이 패턴은 lazy function definition이라고도 불리는데, 최초 사용 시점 전까지 함수를 완전 정의 하지 않고 있다가 호출된 이후에는 더 게으르게 동작하기 때문이다.

장점으로는 함수가 어떤 초기화 준비 작업을 단 한 번만 수행하는 경우 유용하다.
단점으로는 자기 자신을 재정의한 이후에는 이전에 사용한 함수에 추가 되었던 프로퍼티를 모두 찾을 수 없다.

```javascript
var seungdols = function() {
    console.log('Boo');
    seungdols = function () {
        console.log('Double Boo');
    };
};
seungdols.property = 'property';

var seungdols_company = seungdols;

var seungho = {
    nick: seungdols
};

seungdols_company();
seungdols_company();
console.log(seungdols_company.property);

seungho.nick();
seungho.nick();
console.log(seungho.nick.property);

seungdols();
seungdols();//undefined
```

#### 즉시 실행 함수

즉시 실행 함수 패턴은 함수가 선언되자마자 실행되도록 하는 문법이다.

```javascript
(function() {
    console.log('watch out');
}());
```

JSLint는 위 패턴을 선호하는데, 보통 아래와 같이 사용해도 무방하다.


```javascript
(function() {
    console.log('watch out');
})();
```

##### 즉시 실행 함수의 매개변수

```javascript
(function(name) {
    console.log('My ' + name);

}('seungdols'));
```

매개변수도 전달을 할 수 있으나, 되도록 즉시 실행 함수에 많은 매개변수를 전달 하는 것은 좋지 않다.

장점으로는 전역 변수를 남기지 않고, 많은 양의 작업을 가능하도록 해준다.
선언 된 모든 변수는 스스로를 호출 하는 (self-invoking) 함수의 지역 변수가 되어 임시 변수가 전역 공간에 할당 되지 않는다.

#### 즉시 객체 초기화

즉시 객체 초기화 패턴이라 불리며, 즉시 실행 함수와 유사하다.

```javascript
({
    maxHeight: 1330,
    maxWidth : 780,
    getWidth : function() {
        return this.maxWidth;
    },
    getHeight : function() {
        return this.maxHeight;
    },
    init : function() {
        console.log(this.getWidth());
        console.log(this.getHeight());
    }

}).init();
```

일회성 작업에 적합하고, `init()`이 완료되고 나면, 객체에 접근 할 수 없다. 만약 객체의 참조를 유지하고 싶다면, `init()` 마지막에 `return this;`를 추가 하면 된다.

장점으로는 단 한 번의 초기화 작업을 실행 하는 동안 전역 네임스페이스 영역을 보호 할 수 있다.

단점으로는 자바스크립트 압축 도구가 즉시 실행 함수 패턴에 비해 덜 효율적인 압축을 한다는 것이다.

#### 메모이제이션 패턴

함수는 객체여서 프로퍼티를 가질 수 있는데, 이를 이용하여 함수에 프로퍼티를 추가하여 결과를 캐시하면, 다음 호출 시점에 복잡한 연산을 반복하지 않아도 되는 이점을 얻을 수 있다. 이를 Memoization이라 부른다.

```javascript
var myFunc = function(param) {
    if(!myFunc.cache[param]) {
        var result = {};
        //working
        myFunc.cache[param] = result;
    }
    return myFunc.cache[param];
}

myFunc.cache = {};
```

물론, 더 많은 매개변수나 복잡한 타입을 갖는다면, JSON 문자열로 직렬화하고, 이 문자열을 cache 객체에 키로 사용 할 수 있다.

단, 직렬화 하면 객체를 식별 할 수 없게 되는 단점이 존재한다.

#### 설정 객체 패턴

설정 객체 패턴은 좀 더 깨끗하게 API를 제공하는 방법이다.

초기에는 어떤 함수가 단순 했다고 가정하자.

```javascript
function addPerson(first, last) {}

function addPerson(first, median, date ,last) {}
```

이렇게 유지보수 하다보면, 매개변수가 늘어 날 수 있다.
이 때 사용하는 방법이 설정 객체 패턴이다.

```javascript
var conf = {
    first: 'seungdols',
    median: 'seungseung',
    last: 'jojoldu'
};

addPerson(conf);
```

* 매개변수와 순서를 기억하여 매칭 할 필요 없다.
* 선택적인 매개변수 사용이 가능하다.
* 가독성이 좋다.
* 매개변수 추가/삭제가 용이하다.
* 매개변수 명을 외워야 하는 단점 존재한다.
* 프로퍼티의 이름은 압축이 되지 않는다.

#### 커리

##### 함수적용

순수한 함수형 프로그래밍 언어에서 함수는 불려지거나 호출된다고 표현하는 것보다 적용 된다고 표현한다.
자바스크립트에서도 `Function.prototype.apply()`를 사용하면 함수를 적용할 수 있다.

```javascript
var sayHi = function(who) {
    return "Hello" + (who? "," + who : "") + "!";
};
sayHi();
sayHi('world');

sayHi.apply(null, ["hello"]);
```

예제에서 확인이 가능하듯이 함수의 호출이나, 함수의 적용의 결과는 같다.
`apply()`는 2개의 매개변수를 받는데, 첫번째는 함수내 `this`와 바인딩할 객체이고, 두번째는 배열 또는 인자로 함수 내부에서 배열과 비슷한 형태의 `arguments` 객체로 사용한다.

```javascript
var seungdols = {
    sayHi: function(who) {
       return "Hello" + (who? "," + who : "") + "!";
    }
};

seungdols.sayHi('world');
sayHi.apply(seungdols, ['company']);
```

위 처럼 함수가 객체의 메서드일 때의 예시이다.
`seungdols`를 전달하게 되면, 해당 함수내의 this는 `seungdols`에 바인딩이 된다.

원래는 apply로 함수를 적용하는 방법이 더 직관적일 수도 있다는 생각이 든다...물론, 인자를 하나만 가지고도 해결 할 수 있다면, `call()` 메서드도 있다는 것을 기억하자!!

```javascript
sayHi.apply(seungdols, ['company']);
sayHi.call(seungdols, 'company');
```

위 처럼 `call()`을 이용하는 것이 더 편리할 수도 있다.

##### 부분 함수 적용

함수의 일부만 적용하는 것이 가능할까? 수학에서도 볼 법한 내용이지만, 실제로 가능하다.

```javascript
var add = function (x , y) {
    return x + y;
}

add.apply(null, [5,4]);

var newadd = add.partialApply(null, [5]);
newadd.apply(null, [4]);
```
위 처럼 partialApply() 메소드가 있다는 걸로 가정하고, 5 + 4를 부분적으로 적용 할 수 있는 함수를 머릿속으로 생각만 해보자.
이러한 방법을 써먹을 수 있을까? 자바스크립트에서는 자유도가 높기에 이러한 기능을 하는 함수를 구현 할 수 있다.
함수가 부분적인 적용을 이해하고 처리 할 수 있도록 만드는 과정을 **커링**이라고 한다.

#### 커링

다른 함수형 언어에서는 커링이 언어 자체에 내장 되어 모든 함수가 커링 된다. 자바스크립트는 약간의 기교를 추가하면 커링이 가능하게끔 할 수 있다.

```javascript
function add(x,y) {
    var oldx = x, oldy = y;
    if (typeof oldy === 'undefinded') {
        return function (newy) {
            return oldx + newy;
        };
    }
    return x + y;
}

typeof add(5);
add(3)(4);

var add2000 = add(2000);
add2000(10);
```

위 코드를 조금만 살펴보면, add 함수에서 리턴하는 내부 함수에 클로저가 생성 된다. 부분 적용이 없이 값이 둘 다 전달 되면, 단순히 두 값을 더한다.
실제로 위 코드를 조금 더 간략하게 만들 수는 있다.

```javascript
function add(x,y) {
    if(typeof y === 'undefined') { //부분 적용
        return function(y) {
            return x + y;
        };
    }
    return x + y;//전체 인자 적용
}
```

이러한 방식으로 커링을 만들어 볼 수 있다.

심화로 범용적으로 사용 할 수 있는 커링 함수를 만들어 본다면, 아래와 같다.

```javascript
function schonfinkelize(fn) {
    var slice = Array.prototype.slice,
    stored_args = slice.call(arguments, 1);
    return function () {
        var new_args = slice.call(arguments),
        args = stored_args.concat(new_args);
        return fn.apply(null, args);
    };
}
function add (x,y) {
    return x + y;
}
var newadd = schonfinkelize(add, 5);
newadd(4);

schonfinkelize(add, 1)(3);
//2단계 커링
var addOneCurry = schonfinkelize(add, 3);
addOne(10,20,30,40);
var addTwoCurry = schonfinkelize(addOneCurry, 2,3,4,5,);
addTwoCurry(45,4);
```

#### 4장 요약

* API패턴 : 함수에 더 좋고 깔끔한 인터페이스를 제공할 수 있게 돕는다.
    * 콜백패턴 : 함수를 인자로 전달한다.
    * 설정 객체 : 함수에 많은 수의 매개변수를 전달할 떄 통제를 벗어나지 않도록 해준다.
    * 함수 반환 : 함수의 반환 값이 또 다시 함수 일 수 있다.
    * 커링 : 함수와 매개변수 일부를 물려받는 새로운 함수를 생성한다.
* 초기화 패턴
    * 즉시 실행 함수 : 정의되자 마자 실행
    * 즉시 객체 초기화 : 익명 객체 내부에서 초기화 작업을 구조화하고, 다음 즉시 호출 할 수 있는 메서드를 제공한다.
    * 초기화 시점의 분기 : 최초 코드 실행 시점에 코드를 분기하여, 어플리케이션 생명 주기 동안 계속해서 분기가 발생하지 않도록 막는다.
* 성능 패턴
    * 메모이제이션 패턴 : 함수 프로퍼티를 사용해 계산 된 값을 저장하고, 그 값을 사용한다.
    * 자기선언 함수 : 자기 자신을 덮으씀으로 두번 째 호출 이후부터 작업량을 줄이는 용도

---

## 5장 Patterns

#### 네임스페이스 패턴

네임스페이스 패턴을 사용함으로써 전역 변수를 줄 일 수 있는 패턴으로 흔히 사용한다.

```javascript
var SH = {};

SH.Parent = function () {};
SH.Child = function() {};

SH.seungdols = 1;
SH.hello_module = {};
SH.hello_module.korean_hello = {};
SH.hello_module.korean_hello.data = {HI: '안녕하세요 ?', Hello: '안녕?'};
SH.hello_module.english_hello = {};
```

전역 네임스페이스 객체의 이름은 보통 어플리케이션 이름이나 라이브러리의 이름, 도메인명, 회사 이름 중에서 선택하여 사용하곤 한다.
보통 전역 객체 이름은 모두 대문자로 쓰는 명명 규칙을 사용한다.

장점

* 코드 내의 이름 충돌을 방지해준다.
* 전역변수를 줄일 수 있다.

단점

* 모든 변수, 함수에 접두어를 붙여야 한다.
* 전역 인스턴스는 하나이므로, 일부분이 수정되면, 전역 인스턴스를 수정 하게 된다.
* 이름이 중첩되고, 길어져 가독성은 나빠질 수 있다.

##### 범용 네임스페이스 함수

```javascript
var SH = {}; //이렇게 쓰면, 기존에 있는 이름과 충돌 가능성이 높다.

if (typeof SH === 'undefined') {//개선안
    var SH = {};
}

var SH = SH || {};
```

기존 코드를 망가뜨리지 않고, 네임스페이스를 만드는 네임스페이스 함수를 구현 할 수 있다.

```javascript
var SH = SH || {};
SH.namespace = function(ns_string) {
    var parts = ns_string.split('.'),
        parent = SH,
        i;

    if (parts[0] === "SH") {
        parts = parts.slice(1);
    }
    for (i = 0; i < parts.length; i+= 1) {
        if (typeof parent[parts[i]] === "undefined") {
            parent[parts[i]] = {};
        }

        parent = parent[parts[i]];
    }
    return parent;
};
```

위 함수를 가지고 네임스페이스를 안전하게 만들 수 있다.

```javascript
SH.namespace("SH.modules.hello");
```

#### 비공개 프로퍼티와 메서드

자바스크립트에는 private, protected, public 프로퍼티와 메서드를 나타내는 별도 문법적인 방법이 없으나, 클로저를 이용해 비공개 멤버를 구현 할 수 있다.

```javascript
function private_member() {
    var sh = "seungdols";
    this.getSH = function() {//특권 메서드(privileged method)
        return sh;
    };
}

var seungdols = new private_member();

console.log(seungdols.sh);//접근 안됨.
console.log(seungdols.getSH);//접근 됨.
```

객체 리터럴로 만드는 방법

```javascript
var myobj = (function () {
    var name = "seungdols";

    return {
        getName: function() {
            return name;
        }
    };
}());
```

자바스크립트에서 클로저를 활용한 비공개 멤버를 쉽게 구현 할 수 있다.

비공개 멤버의 허점

* 파폭 초기 버전 일부는 eval() 함수에 두 번째 매개변수를 전달 할 수 있게 되어 있어 비공개 유효범위에도 접근 할 수 있다.
* 특권 메서드에서 비공개 변수의 값을 바로 반환할 경우 참조가 반환되어 외부 코드에서 비공개 변수 값을 수정할 수 있게 된다.

생성자를 사용해 비공개 멤버를 만들 경우 생성자를 호출하여 새로운 객체를 만들기 때문에 비공개 멤버가 매번 재생성 된다는 단점이 있다.
이를 해결하려면, 공통 프로퍼티와 메서드를 생성자의 prorotype 프로퍼티에 추가해야 한다. 이렇게 하면, 감춰진 비공개 멤버들도 모든 인스턴스가 함께 쓸 수 있다.
이를 위해서는 , 생성자 함수 내부에 비공개 멤버를 만드는 패턴과 객체 리터럴로 비공개 멤버를 만드는 패턴을 함께 써야 한다.

```javascript
function Seungdols() {
    var name = "seungdols"
    this.getName = function() {
        return name;
    };
}

Seungdols.prototype =(function() {
    var browser = "Chrome";
    return {
        getBrowser: function() {
            return browser;
        }
    };
}());

var sh = new Seungdols();
console.log(sh.getName());
console.log(sh.getBrowser());
```

##### 비공개 함수를 공개 메서드로 노출시키는 방법

노출 패턴은 비공개 메서드를 구현하면서 동시에 공개 메서드로도 노출하는 것을 말한다.

```javascript
var SH;

(function () {
    var isEmpty = false;

    function isEmpty() {
        //
    }

    function isNotEmpty() {
        //
    }

    SH = {
        isEmpty: isEmpty,
        isNotEmpty: isNotEmpty
    };
}());
```

마지막 부분에서 접근을 허용해도 괜찮은 부분에 대해 SH 객체에 추가해준다. 즉, SH를 통해서는 공개 메서드가 되나, 다른 부분에서는 비공개 멤버가 된다.

#### 모듈 패턴

늘어나는 코드를 구조화하고, 정리하는데 도움되는 패턴이다. 이 패턴을 이용하면, 개별적인 코드를 느슨하게 결합시킬 수 있다.
모듈패턴은 여러개의 패턴을 조합한 것을 말한다.

* 네임스페이스 패턴
* 즉시 실행 함수
* 비공개 멤버와 특권 멤버
* 의존 관계 선언

```javascript
SEUNGDOLS.namespace('SEUNGDOLS.utilites.ArrayUtils');

SEUNGDOLS.utilities.ArrayUtils = (function() {
    //의존 관계
    var obj = SEUNGDOLS.utilities.object,
        lang = SEUNGDOLS.utilities.lang,

        array_string = "[object Array]",
        tos = Object.prototype.toString;

    //비공개 메서드
    //...

    //var 선언 끝

    //일회성 초기화 실행
    //...

    //공개 API
    return {
        //1

        //2
    }
}());
```

모듈 패턴은 점점 늘어가는 코드를 정리 할 때 널리 사용하며, 추천 되는 방법이다.


참고
* [영문서이나 괜찮은 문서](http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html)


#### 모듈에 전역변수 가져오기

모듈을 감싼 즉시 실행 함수에 인자를 전달하는 형태가 있는데, 보통 전역 변수에 대한 참조 또는 전역 변수 자체를 전달한다.
이렇게 전달을 하게 되면, 탐색작업이 좀 더 빨라진다.

```javascript
SEUNGDOLS.utilities.module = (function (app, global) {
    //
    //
}(SEUNGDOLS,this));
```


#### 샌드박스 패턴

네임스페이스 패턴의 단점을 해결해주는 패턴이다. 더군다나 전역 네임스페이스를 보호해주는 역할을 한다. (아직 이해가 잘...)

```javascript
function Sandbox() {
    var args = Array.prototype.slice.call(arguments),
        callback = args.pop(),
        modules = (args[0] && typeof args[0] === "string" ) ? args : args[0],
        i;

        //함수가 생성자로 생성되도록 보장
        if(!(this instanceof Sandbox)) {
            return new Sandbox(modules, callback);
        }

        //this에 필요한 프로퍼티들을 추가
        this.a = 1;
        this.b = 2;

        // core this 객체에 모듈 추가
        if( !modules || modules === "*" || modules[0] === "*") {
            modules = [];
            for (i in Sandbox.modules) {
                if (Sandbox.modules.hasOwnProperty(i)) {
                    modules.push(i);
                }
            }
        }

        //필요한 모듈들 초기화
        for ( i = 0; i < modules.length; i+= 1) {
            Sandbox.modules[modules[i]](this);
        }

        callback(this);
}

Sandbox.prototype = {
    name : "seungdols Application",
    version : "1.0",
    getName : function () {
        return this.name;
    }
};

Sandbox('dom','hello', function() {
    console.log('hello');
});

```

#### 스태틱 멤버

스태틱 프로퍼티와 메서드란 인스턴스에 따라 달라지지 않는 프로퍼티와 메서드를 말한다.
자바스크립트 언어에서는 스태틱 멤버를 지칭하는 문법이 지원 되지 않는다. 하지만, 생성자에 프로퍼티를 추가하여 구현할 수 있다.

##### 공개 스태틱 멤버

```javascript
var Gadget = function() {};

Gadget.isShiny = function() {
    return "you bet";
};

Gadjet.prototype.setPrice = function(price) {
    this.price = price;
};
```

위처럼 코드를 작성하면, 위험한 점이 존재하게 된다. Gadget.isShiny()를 호출 하면, 내부의 this는 Gadget 생성자를 가리키지만, 아래 코드에서는 위험하다.

```javascript
var iphone = new Gadget();
iphone.setPrice(1000);

typeof Gadget.setPrice;
typeof iphone.isShiny;
```
스태틱 메서드가 인스턴스를 통해 호출 했을 때도 동작한다면, 편리한 경우가 있을 수 있는데, 이 때는 프로토타입에 메서드를 추가 하는 것만으로도 가능하다.
그러나, 문제가 포함되어 있다.

스태틱 메서드 안에서 this를 사용할 떄 주의해야 한다. Gadget.isShiny()를 호출했을 떄는 isShiny()는 가젯 생성자를 가리키지만, iphone, isShiny()를 호출 했을 때는 this가 iphone을 가리키게 된다.

##### 비공개 스태틱 멤버

* 동일한 생성자 함수로 생성된 객체들이 공유하는 멤버다.
* 생성자 외부에서는 접근할 수 없다.

먼저 클로저 함수를 만들고, 비공개 멤버를 이 함수로 감싼 후, 이 함수를 즉시 실행한 결과로 새로운 함수를 반환하게 한다.

```javascript
var Gadget = (function() {
    var counter = 0;
    return function() {
        console.log(counter += 1);
    };
}());
```

실제로 카운터 값을 인스턴스끼리 공유하는 것을 확인 할 수 있다.

#### 체이닝 패턴

객체에 연쇄적으로 메서드를 호출 할 수 있도록 하는 패턴이다.

* 코드량이 줄어 들고, 간결해진다.
* 디버깅이 어렵다.

양날의 검인듯 하지만, 알아 두면 좋을 듯한 패턴이다.
메서드에서 return 할 때에 this를 리턴하면 체이닝이 가능해진다.

#### method() 패턴

더글라스 크락포드가 고안한 클래스 기반의 언어에 적응한 사람들이 메서드를 구현하는 방식을 고안했다. 허나 이 방법을 추천하지는 않는다.

생성자 본문 내에 인스턴스 프로퍼티를 추가 할 수 있다. 그러나 this에 인스턴스 메서드를 추가하게 되면, 인스턴스 마다 메서드가 재생성 되어 메모리를 잡아 먹어 비효율적이다.

```javascript

if (typoef Function.prototpye.method !== "function") {
    Function.prototype.method = function(name, implementation) {
        this.prototype[name] = implementation;
        return this;
    };
}
```
