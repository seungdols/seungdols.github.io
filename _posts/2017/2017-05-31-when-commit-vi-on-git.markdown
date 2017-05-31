---
layout: "post"
title: "when commit vi on git"
description: "Git에서 Vi error가 발생 할 때 조치하기"
date: "2017-05-31 18:06"
tags: [git, vi]
comments: true
---

# 어느 순간 Commit을 하려 했으나, 안되는 증상이 발생!

```bash
error: There was a problem with the editor 'vi'.
Please supply the message using either -m or -F option.
```

찾아 보니 이렇다. 무슨 문제로 인한 vi editor 버그라는데 갑자기 생긴 이유는 모르겠다.

그런데, 해결방법은 참 쉽다.


```bash
git config --global core.editor `vim`
```

위 명령어처럼 git 전역 설정에 `vim` editor 설정을 등록하는 방법이 있다.

맥에서는 아래의 방법을 써도 된다.

```bash
git config --global core.editor /usr/bin/vim
```

이상 seungdols 올림.
