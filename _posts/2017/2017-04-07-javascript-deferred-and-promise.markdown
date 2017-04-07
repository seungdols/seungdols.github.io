---
layout: "post"
title: "javascript-Deferred-and-promise"
description: "JavaScript의 Deferred와 Promise 이야기"
date: "2017-04-07 12:09"
tags: [jQuery, javascript, pattern]
comments: true
---

# JavaScript Deferred 객체와 promise

요즘 업무를 하다보니 느낀 사실인데, 비동기 처리나, 동기처리에 관한 이슈가 늘 나를 덮치곤 했다.
그런 이유로 JavaScript를 좀 더 잘하고 싶은 마음에 찾아본 자료를 정리해보았다.

## Deferred Object


jQuery에서는 promise를 사용할 수 있도록 `Deferred` 객체를 제공합니다.
물론, jQuery.Promise()를 지원한다. 3.0부터 아마 제대로 지원한다고 생각하면 될 것 같습니다.
그러나, 사용하는 jQuery 버전이 3.0 이하인 경우에는 jQuery.Promise()를 사용할 수는 없습니다.
jQuery.Deferred()가 존재하므로 이를 이용하여 promise를 사용할 수 있어 Callback Hell에서 벗어 날 수 있습니다.

```javascript

var seungdolsProfileUpdateFunction = function() {
  var Deferred = jQuery.Deferred();
  try {
    Deferred.resolve('성공');
  } catch(error) {
    Deferred.reject(error);
  }
  return Deferred.promise();
};

var promise = seungdolsProfileUpdateFunction();

//1
promise.done(function(message) {
  console.log("success");
}).fail(function(error) {
  console.log("fail");
}).always(function() {
   console.log('All about success');
});
//2
promise.done(function(message) {
  console.log("success");
});
promise.fail(function(error) {
  console.log("fail");
});
promise.always(function() {
   console.log('All about success');
});
```
다만, 여기서 주요하게 봐야 할 점은 `Deferred()`객체를 생성하여 사용할 경우, promise를 쓰려면, `Deferred.promise()`를 리턴해 사용해야 합니다.


## Promise

이 패턴이 나온 계기는 콜백 헬이라는 코드를 분리시키고 콜백 헬에서 벗어나고자 나온 패턴으로 jQuery에서도 지원합니다.
물론, 기타 수 많은 라이브러리에서도 지원한다. 크롬은 자체 내장 Promise 또한 32버전 이후 부터 지원하고 있습니다.

```javascript
jQuery.ajax(
  //
).done(function () {
  //work1

  //work2
});  
```
이러한 구조라면, 상당히 애매해지죠? work1이 먼저 수행 할지, work2가 먼저 수행 할지 모릅니다.
그러한 니즈로 인해 찾다 보니 `Deferred`객체를 알게 되었습니다. 그에 따르는 `Promise` 패턴을 또 보게 되었습니다.

일단, jQuery 1.5 이후 버전부터 포함 된 기능이며, Ajax 같은 비동기 함수를 핸들링하기 위한 도구로 나타났습니다.
(단, jQuery 3.0 이하 버전에서 상당히 복잡한 promise를 쓸 경우 문제가 발생 할 수 있다고 합니다.
이유는 jQuery 자체에서 구현한 걸 사용한 문제이나, 3.0부터는 commonJS Promise/A를 구현하여 사용했다고 합니다.)

참고

* [Promises/A+](https://promisesaplus.com/)
* [Promises/A wiki](http://wiki.commonjs.org/wiki/Promises/A)

#### jQuery API

> The jqXHR Object

> The jQuery XMLHttpRequest (jqXHR) object returned by $.ajax() as of jQuery 1.5 is a superset of the browser's native XMLHttpRequest object. For example, it contains responseText and responseXML properties, as well as a getResponseHeader() method. When the transport mechanism is something other than XMLHttpRequest (for example, a script tag for a JSONP request) the jqXHR object simulates native XHR functionality where possible.
As of jQuery 1.5.1, the jqXHR object also contains the overrideMimeType() method (it was available in jQuery 1.4.x, as well, but was temporarily removed in jQuery 1.5). The .overrideMimeType() method may be used in the beforeSend() callback function, for example, to modify the response content-type header:

> The jqXHR objects returned by $.ajax() as of jQuery 1.5 implement the Promise interface, giving them all the properties, methods, and behavior of a Promise (see Deferred object for more information). These methods take one or more function arguments that are called when the $.ajax() request terminates. This allows you to assign multiple callbacks on a single request, and even to assign callbacks after the request may have completed. (If the request is already complete, the callback is fired immediately.) Available Promise methods of the jqXHR object include

* jqXHR.done(function( data, textStatus, jqXHR ) {});
An alternative construct to the success callback option, refer to deferred.done() for implementation details.

* jqXHR.fail(function( jqXHR, textStatus, errorThrown ) {});
An alternative construct to the error callback option, the .fail() method replaces the deprecated .error() method. Refer to deferred.fail() for implementation details.

* jqXHR.always(function( data|jqXHR, textStatus, jqXHR|errorThrown ) { }); (added in jQuery 1.6)
An alternative construct to the complete callback option, the .always() method replaces the deprecated .complete() method.
In response to a successful request, the function's arguments are the same as those of .done(): data, textStatus, and the jqXHR object. For failed requests the arguments are the same as those of .fail(): the jqXHR object, textStatus, and errorThrown. Refer to deferred.always() for implementation details.

* jqXHR.then(function( data, textStatus, jqXHR ) {}, function( jqXHR, textStatus, errorThrown ) {});
Incorporates the functionality of the .done() and .fail() methods, allowing (as of jQuery 1.8) the underlying Promise to be manipulated. Refer to deferred.then() for implementation details.

jQuery의 ajax() 함수는 jqXHR 객체를 반환하는데, 1.5부터 이 객체는 promise 인터페이스를 구현하여, promise 메소드의 주요 기능을 구현했다고 합니다. jqXHR객체에 대해 더 알고 싶다면, [Deferred 객체](http://api.jquery.com/category/deferred-object/)에 대해 더 찾아 보면 될 것 같습니다.

> Deprecation Notice: The jqXHR.success(), jqXHR.error(), and jqXHR.complete() callbacks are removed as of jQuery 3.0. You can use jqXHR.done(), jqXHR.fail(), and jqXHR.always() instead.

```javascript
jQuery.ajax({
  url: "/seungdols/update",
  success: sucFunc,
  error: failFunc
});
```

위처럼 작성했다면 이제는 아래 코드처럼 변화했습니다.

```javascript
var promise = jQuery.ajax({
  url: "/seungdols/update",
  data : params
});

promise.done(sucFunc);
promise.fail(failFunc);
```

`then()`를 이용하여 성공시 콜백과 실패시 콜백을 둘 다 하나로 합칠 수 있습니다.

```javascript
var promise = jQuery.ajax({
  url: "/seungdols/update",
  data: params
});

promise.then(sucFunc, failFunc);
```

그렇다면, promise의 장점을 무엇일까?

1. 다수의 `done`, `fail`함수를 호출 할 수 있습니다.
2. ajax 수행 이후에도 done(), fail()을 콜 할 수 있으며, 콜백이 즉시 실행 됩니다.
3. promise를 서로 합칠 수 있습니다.

```javascript
var promise1 = jQuery.ajax({
  url: "/seungdols/profile/update",
  data : params
});
var promise2 = jQuery.ajax({
  url: "/seungdols/profile/info",
  data: params
});

jQuery.when(promise1, promise2).done(function(xhrObject1, xhrObject2) {
 //work
});
```

4. jQuery 1.8부터 then() chaining이 가능해졌습니다.

```javascript
var promise = jQuery.ajax({
  url: "/seungdols/profile/update",
  data: params
});

function getThumnailImg() {
  return jQuery.ajax({
    url: "/seungdols/profile/thumnail",
    data: userId
  });
}

promise.then(getThumnailImg)
       .then(function(data) {
          //work1
        })
       .then(successFunc, failFunc);
```
단, `then() chaining`이 가능해지면서 편리해진 것도 사실이지만, 중요한 점으로는 우선, 위 콜백 형태의 경우 `anonymous function`에서 `Error`가 발생하거나, `getThumnailImg Function error`가 발생할 경우, `failFunc`가 동작해야 하나, jQuery에서는 그렇게 동작하지 않는 문제로 인해 무분별한 `promise pattern` 사용을 금하는 게 좋다고 한다.

`Promise pattern`은 `ECMA Script 6`에 정식 포함 되었으며, Chrome 브라우저에서도 32이후 부터 `native promise`가 지원되기 시작했다.
당연히 node.js에도 포함 되어 있을 터...추가적으로 `promise`를 쓰는 방식이 2가지가 존재한다고 합니다.
이것에 대해서는 좀 더 연구가 필요로 해보입니다.

## [new Promise vs Promise.resolve()](http://han41858.tistory.com/11)

1. Async 로직을 감싸거나 할 때는 new Promise()를 쓰자!
2. sync 로직인 경우 Promise.resolve()를 쓰자!


## 참고

* [제이쿼리 프로미스(promise)와 Deferred](https://www.zerocho.com/category/jQuery/post/57c90814addc111500d85a19)
* [[JavaScript] 바보들을 위한 Promise 강의 - 도대체 Promise는 어떻게 쓰는거야?](http://programmingsummaries.tistory.com/325)
* [AJAX & Deferreds](http://jqfundamentals.com/chapter/ajax-deferreds)
* [jQuery - Deferred Object](https://api.jquery.com/category/deferred-object/)
* [JavaScript Promise](https://hyunseob.github.io/2016/03/14/javascript-promise/)
* [promise - MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)
* [[자바스크립트] Promise 이해하기](http://yubylab.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-Promise-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)
* [Promise 를 사용하는 두 가지 방법, new Promise, Promise.resolve()](http://han41858.tistory.com/11)
* [Deferred and promise in jQuery](http://blog.naver.com/PostView.nhn?blogId=djawl_2&logNo=50184671092)
 * [원본 Deferred and promise in jQuery](https://bitstorm.org/weblog/2012-1/Deferred_and_promise_in_jQuery.html)
* [Asynch JS: The Power Of $.Deferred](https://www.html5rocks.com/ko/tutorials/async/deferred/)
* [비동기 프로그래밍을 위한 Promise와 Deferred 알아보기](http://webframeworks.kr/tutorials/angularjs/angularjs_promise_deferred/#tocAnchor-1-3)
* [Promise를 위한 jQuery Deferred Object](http://poiemaweb.com/jquery-deferred)
