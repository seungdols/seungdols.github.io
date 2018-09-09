---
layout: post
title: "Weekly News #24"
description: "그냥 저냥 #위클리뉴스 #24"
date: "2018-09-09 15:52"
tags: [weekly-news, programming]
comments: true
---

# 그냥 저냥 위클리 뉴스 #24

### Java

* [CompletableFuture 비동기 처리로 성능 개선하기](http://blog.woniper.net/361) : woniper님이 개인 프로젝트를 하시는데, 성능 관련 이슈가 생기니 스프링에서 비동기 처리를 하는 방식으로 바꿔서 효과를 보신 것 같다. 사실, 나도 이 부분에 대해서 이제서야 막 보고 있는데, `JavaScript`와 비슷하게 프로그래밍이 가능해졌다는 사실에 놀라고 있다. 대부분의 언어들의 비동기 처리가 비슷한 구조를 가져가는 것 같다. 
* [카카오 개발자 컨퍼런스 2018 참석 후기!](https://jojoldu.tistory.com/335) : 카카오로 합병 이후 컨퍼런스를 최초로 개최한게 아닐까 싶었는데, 사실 가고 싶었는데, 참가자로 선정 되지 못했는데, 창천향로님이 정리를 세세하게 해주셨고, 거의 당일 바로 올려주셨다. "고수들은 타자가 빠른 것 같다"라는 이상한 논제에 점점 부합 되는 분들이 많아지고 있다. 
* [JDBC로 실행되는 SQL에 자동으로 프로젝트 정보 주석 남기기](http://woowabros.github.io/tools/2018/08/16/jdbc-log-sql-projectinfo.html) : outsider님의 기술 뉴스를 보고 괜찮다 생각하고 본 글이다. MSA로 전환하면서, 과도기 시기에 사용하면 좋을 것 같고, MSA에서도 계속 사용하면 좋을 것 같다. 

### Node / JavaScript

* [Node v10.10.0 (Current)](https://nodejs.org/en/blog/release/v10.10.0/) : Node v10.10.0이 릴리즈 되었다. `vm.compileFunction`이 추가 되었는데, 좀 봐야 할 것 같다. `http2`모듈이 이제 정식으로 지원 되는 것 같다. 그 밖에 `os, process, child_process` 등 모듈에 변화가 있었다. 
* [Babel 7 Released](https://babeljs.io/blog/2018/08/27/7.0.0) : babel 7이 벌써 나왔다. 사실, 그간 프론트 엔드에 취중한 개발자보다 서버쪽 개발이다보니, 관심에서 밀렸는데, 벌써 7이 릴리즈 되었다. 선택적으로 설정이 가능한 `overrides`라는 옵션이 추가 된 것으로 보인다.  
* [What’s Coming Up in JavaScript 2018: Async Generators, Better Regex](https://thenewstack.io/whats-coming-up-in-javascript-2018-async-generators-better-regex/) : JavaScript2018에서 핫한 것은 정규식과 `Async Generators`로 보이는데, `Async Generators`는 push-pull model이라고 하는데, 정확하게는 찾아봐야 할 것 같다. 
* [실무에서 사용하는 Vue.js 프로젝트 구조](https://joshua1988.github.io/web-development/vuejs/vue-structure/): 제가 항상 즐겨 찾는 블로그인데요. Vue.js 관련 책도 쓰신 것으로 알고 있습니다. (하지만, 제가 React를 하게 되는 상황이라) Vue.js는 마음속으로만 응원하고 있는데요. 언젠가는 제가 일 하는 업무에 실제 적용을 하고 싶긴 합니다. 사실 여전히 React < Vue.js에 마음을 더 두고 있습니다. 실제 실무에서 사용할 수 있는 프로젝트 구조를 소개해주는데, `소확팁`이 아닐까 합니다. 프로젝트 구조가 뭐가 중요하냐? 이럴 수 있지만, 엄청 중요합니다. 소소한 것 같지만 확실한 꿀팁이라고 생각합니다. 
* [타입스크립트, 써야할까?](https://hyunseob.github.io/2018/08/12/do-you-need-to-use-ts/): 타입스크립트를 써야 하냐 말아야 하나에 대한 고민은 끊이지 않는 것 같습니다. 하지만, 동적 타입보다 정적 타입으로 프로그래밍을 하게 될 경우 버그를 미리 탐지가 가능하다는 점에서는 좋습니다. 물론 그러한 말도 있습니다. 타입 스크립트가 타입 관련 버그를 방지해줌으로 생산성이 올라가게 된다. 하지만, 이는 TDD로도 충분히 커버가 가능하다는 평도 있고요. 제가 생각 하기에는 생산성은 개개인의 차의 문제라 지표화하기 어렵다 판단 되나, 버그를 미리 탐지 할 수 있다는 점은 좋은 것 같습니다. 물론 `Flow`가 더 좋냐는 취향의 문제인 것 같지만, 요즘은 거의 대부분 타입스크립트를 도입하고 있는 것 같습니다. 
* [AMD, CommonJS, UMD 모듈](https://www.zerocho.com/category/JavaScript/post/5b67e7847bbbd3001b43fd73): 지난 포스트이긴 하지만, 너무 좋은 내용이라 첨부하게 되었습니다. AMD, CommonJS, UMD 도대체 뭐가 뭔지 모르겠다 하시는 분은 이 포스팅을 읽어보길 권합니다. 

### 그 밖의 읽을 거리

* [[B급 프로그래머] 9월 1주 소식(빅데이터/인공지능, 암호화폐/블록체인, 읽을거리 부문)](http://jhrogue.blogspot.com/2018/09/b-9-1.html) : B급 프로그래머 블로그의 빅데이터, ML, 블록체인등 위클리 뉴스 모음집이다. 이렇게 많은 자료를 계속 보시고, 정리 해주시는 것이 놀랍다. 나도 다양하게 하고 싶은데, 실질적으로 부족한게 사실인데, Python으로 시각화 하는 방법에 대한 포스팅이 앞으로 점점 더 내가 관심을 갖게 되는 부분인 것 같다. 너무 신기하고, Jupyter notebook과 연동하면, Python은 못할게 없는 수준이 되는 것 같다. 
* [기술 뉴스 #109 : 18-09-01](https://blog.outsider.ne.kr/1400): 조직의 동의를 얻는 법이라는 글이 참 와닿는다. 확실히 조직에서 일하면서 닥치는 상황은 내 안건에 대해 아무런 의견도 없고, 반대만 있을 경우이다. 그 반대가 "귀찮음"에 귀결되는 근거라면, 되게 황당하기도 하다. 더 좋을 것 같으니 도입 하자는 의견에 "귀찮음"을 이유로 반대하는 것은 실로 황당한데, 이러한 상황을 여러번 거치면, 이런 글을 읽고 무릎을 타악! 하고 치게 되는 것 같다. 
