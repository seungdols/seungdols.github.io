---
layout: post
title: "삭제한 local branch 복구하기"
description: "삭제한 깃 브래앤치 복구해보자!"
date: "2017-12-13 00:24"
tags: [git, programming]
comments: true
---


# Git branch를 지워버렸을때, 침착하게 복구하자!

## reflog를 기억하라!

```bash
$ git branch # check 
$ git reflog 
$ git checkout -b [deleted-branchName] HEAD@{number}
```

위의 명령어만 기억하면 된다. 사실, 깃은 모든 Action을 파일로 떨군다고 생각해도 될 것 같다. 

오오..갓토발즈형님!!

즉, 삭제한 브랜치도 `reflog`를 통하여 복구 할 수 있다. 

> HEAD가 가리키는 커밋이 바뀔 때마다 Git은 자동으로 그 커밋이 무엇인지 기록한다. 새로 커밋하거나 브랜치를 바꾸면 Reflog도 늘어난다. "Git 레퍼런스" 절에서 배운 git update-ref 명령으로도 Reflog를 남길 수 있다. 이 것이 git update-ref를 꼭 사용해야 하는 이유중에 하나다. git reflog 명령만 실행하면 언제나 발자취를 돌아볼 수 있다

위의 인용은 [git-scm.com](https://git-scm.com/book/ko/v1/Git%EC%9D%98-%EB%82%B4%EB%B6%80-%EC%9A%B4%EC%98%81-%EB%B0%8F-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%B3%B5%EA%B5%AC)에서 하였습니다. 

즉, Git을 잘 쓰는 방법은 자주 망가뜨려 보되, `git update-ref`를 잘 쓰는 것이 좋다. 

그럼 다음에는 어떻게 망가뜨릴지 고민해보겠습니다. 

