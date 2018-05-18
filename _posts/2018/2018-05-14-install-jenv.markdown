---
layout: post
title: "install jenv"
description: "Java 버전 여러개 사용하기"
date: "2018-05-14 22:08"
tags: [java, programming, install guide]
comments: true
---



# java 여러개 버전 사용하기 (MacOS)



### Jenv 및 java 설치

[jenv](http://www.jenv.be/) 를 통해 Java environment를 관리 하려고 합니다. 

```bash
$ brew install caskroom #미리 설치 되어 있어야 합니다.
$ brew install jenv
$ brew cask install java #가장 최신 버전으로 설치 됩니다. 
$ brew cask install java8 #java8 버전 최신 릴리즈로 설치됩니다.
```

일단 위와 같이 실행을 하여 java를 설치해주시면 됩니다. 

#### cask 오류 발생할 경우 

```bash
❯ brew cask install java8
Error: Cask 'java8' is unavailable: No Cask with this name exists.
```

위와 같은 에러가 발생할 경우 아래와 같이 입력해주시면 됩니다. 

```bash
brew cask install caskroom/versions/java8
```

### 설치된 java 확인

```bash
$ cd /Library/Java/JavaVirtualMachines
$ ll
drwxr-xr-x - root 14 5 21:42 jdk-10.0.1.jdk
drwxr-xr-x - root 27 4  2017 jdk1.8.0_131.jdk #기존에 사용하던 버전
drwxr-xr-x - root 14 5 21:45 jdk1.8.0_172.jdk
```

### jenv에 java version 등록하기

```bash
$ mkdir -p ~/.jenv/versions
$ jenv add /Library/Java/JavaVirtualMachines/jdk-10.0.1.jdk/Contents/Home
$ jenv add /Library/Java/JavaVirtualMachines/jdk1.8.0_172.jdk/Contents/Home 
```

등록이 잘 되었는지 확인을 해봅니다. 

```bash
$ jenv versions
```



### jenv로 java version 변경하기

#### global

```bash
$ jenv global 1.8.0.172
❯ jenv versions
  system
  1.8
* 1.8.0.172 (set by /Users/seungdols/.jenv/version)
  10.0
  10.0.1
  oracle64-1.8.0.172
  oracle64-10.0.1
```

#### local

```bash
~/dev/practice/java9 master*
❯ jenv local 10.0

~/dev/practice/java9 master*
❯ jenv versions
  system
  1.8
  1.8.0.172
* 10.0 (set by /Users/seungdols/dev/practice/java9/.java-version)
  10.0.1
  oracle64-1.8.0.172
  oracle64-10.0.1
```


