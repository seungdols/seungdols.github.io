---
layout: "post"
title: "don't work run java on Mac OS"
description: "Mac OS​ intelliJ에서 Java 실행이 안 될 때!"
date: "2017-05-28 22:44"
tags: [java, mac, intellij]
comments: true
---


# Mac에서 Java run 안 될 때!

`Class JavaLaunchHelper is implemented in both ~ ` 라는 에러를 발견 한다면, 운이 나쁜 겁니다....ㅠㅠ 😭😭😭

이럴때는 어떻게 해야하는지 알려 드리겠습니다.
스택 오버 플로우를 찾아 보니 아래와 같습니다.
이 문제에 대해서 java9 or java8.152 업데이트에서 fixed 될 예정이라네요.

> You can find all the details here:

> IDEA-170117 "objc: Class JavaLaunchHelper is implemented in both ..." warning in Run consoles
It's the old bug in Java on Mac that got triggered by the Java Agent being used by the IDE when starting the app. This message is harmless and is safe to ignore. Oracle developer's comment:

> The message is benign, there is no negative impact from this problem since both copies of that class are identical (compiled from the exact same source). It is purely a cosmetic issue.
The problem is fixed in Java 9 and in Java 8 update 152.

> If it annoys you or affects your apps in any way (it shouldn't), the workaround for IntelliJ IDEA is to disable idea_rt launcher agent by adding idea.no.launcher=true into idea.properties (Help | Edit Custom Properties...). The workaround will take effect on the next restart of the IDE.

고로, 지금은 `idea.properties` 옵션 파일 안에 `idea.no.launcher=true`을 추가 해주면 됩니다.

참고

* [objc3648-class-javalaunchhelper-is-implemented-in-both](https://stackoverflow.com/questions/43003012/objc3648-class-javalaunchhelper-is-implemented-in-both)
