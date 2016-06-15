---
layout: "post"
title: "Install Scala on Ubuntu14.04"
description: "스칼라를 우분투에 설치해보자."
date: "2016-06-15 14:56"
tags: [scala, ubuntu, function programming]
comments: true
---


### scala install
```bash
wget www.scala-lang.org/files/archive/scala-2.11.7.deb
sudo dpkg -i scala-2.11.7.deb
```

### sbt installation
```bash
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 642AC823
sudo apt-get update
sudo apt-get install sbt
```

### java install
```bash
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
```

### git install
```bash
sudo apt-get install git
```

위와 같이 따라하게 되면, 스칼라를 설치 할 수 있다. 보통 기본적으로 스칼라가 `openjdk7`을 설치하라고 하는데 이렇게 따라하니 oracle-java8에서도 스칼라를 설치 할 수 있었다.

물론 이미 java가 설치 되어 있다면, **scala install & sbt**만 설치하면 될 것이다.

#### sbt란 ?

스칼라를 위한 `gradle` 같은 라이브러리 의존성 관리 패키지라고 할 수 있을까?
아무래도 그렇게 이해할 수 있을 것 같다. _하지만, 중요한 점은 스칼라는 `JVM` 위에서 동작한다는 점이다._

고로, 자바 개발자들이 쉽게 접근이 가능하고, 함수형으로 넘어가기 위한 발판에 해당 하는 언어라고 평가 받고 있다.


참고

* [nacyot](http://wiki.nacyot.com/documents/scala/#.V2DxqR8Vwbl)
* [bashshell code](https://gist.github.com/bigsnarfdude/b2eb1cabfdaf7e62a8fc)
