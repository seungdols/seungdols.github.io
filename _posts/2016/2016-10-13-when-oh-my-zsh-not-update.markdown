---
layout: "post"
title: "when oh my zsh not update"
description: "zsh 업데이트 오류시 시도 해보자."
date: "2016-10-13 23:05"
tags: [zsh, terminal, oh-my-zsh]
comments: true
---


## oh-my-zsh란 ?

쉘의 일종인데 bash 보다 더 향상 된 기능을 포함 하고 있다.

그런지라 나도 리눅스 환경에서는 항상 zsh를 사용한다.

그런데 언제부턴지는 모르지만 **항상** 업데이트는 실패를 하더라..

그러다가 이번에는 못참겠다하고 검색을 했는데 답이 나왔다.

**에러 메시지**

```bash
[Oh My Zsh] Would you like to check for updates? [Y/n]: y
Upgrading Oh My Zsh
Cannot pull with rebase: You have unstaged changes.
Please commit or stash them.
There was an error updating. Try again later?
```

그런데 찾아보니 이미 많은 이들이 겪는 문제였다. 그래서 해결 하는 방법은

다음과 같다.

```bash
cd "$ZSH" && git stash && upgrade_oh_my_zsh
```

무척이나 간단하지만, 몰랐다면 계속 몰랐을 내용임에는 틀림 없다.

출처 : [github-robbyrussell/oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh/issues/1984)
