---
layout: "post"
title: "inorder grup booting"
description: "우분투 grup의 부팅 순서 변경하기"
date: "2016-06-11 00:28"
tags: [ubuntu, grup]
comments: true
---

```bash
$sudo vi /etc/default/grup
```

- 위 명령어를 치고, GRUP_DEFAULT=0이란 부분 검색

- 변경하고 싶은 부팅 순서의 숫자를 넣는다.

#### 단, 부팅 순서는 0 부터 시작한다.

- 아래의 명령어 입력하여 설정을 변경한다.

```bash
$sudo update-grub
```
- 그럼 끝!
