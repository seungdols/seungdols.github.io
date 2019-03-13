---
layout: post
title: "DCEVM(Dynamic Code Evolution VM)을 적용해보자"
description: "DCEVM(Dynamic Code Evolution VM)을 적용해보자"
date: "2019-03-13 18:16"
tags: [java,hot swap, programming]
comments: true
---

DCEVM을 설치 해서 조금 더 빠르게 수정된 부분을 반영하여 Tomcat이 해당 수정 코드를 빠르게 반영 하고자 설치하려고 한다. 

원래는 **JRebel**이라는 유료 툴을 이용하면, 빠르게 Hot Swap하여 수정 된 코드를 반영할 수 있다. 

그러나 연간 $550정도로 구독 모델이 굉장히 비싸서 사용하기가 어려워서 대체제를 찾다가 발견한 것이 바로 *DCEVM*이다. 



아래와 같이 설치를 따라해보도록 하자. 


1. [JDK version](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)에 맞는 [binary jar 파일](https://github.com/dcevm/dcevm/releases)을 받는다. 
2. 해당 파일을 실행 시키는데, `sudo` 권한이 필요로 하다. 
3. `sudo java -jar` 명령어를 사용하면 된다.

    ![Dynamic_Code_Evolution_VM_Installer.png](/blog/assets/img/post/2019/Dynamic_Code_Evolution_VM_Installer.png)

4. 그림과 같이 DCEVM binary 버전에 맞는 Directory를 선택 후 Replace by DCEVM 클릭, 이후 Install DCEVM as altjvm 클릭
5. 이후 terminal에서 java version을 확인 해보면 된다.
6. `java —version`
7. 아래와 같이 Dynamic Code Evolution 64-bit Server VM이 나오면 정상적으로 변경 된 것이다.

    ![dcevm_terminal.png](/blog/assets/img/post/2019/dcevm_terminal.png)

8. 이후 IntelliJ Plugin 중에서 `DCEVM intergration` 을 다운 받고, 재시작 한다.

    ![DCEVM_intergration](/blog/assets/img/post/2019/dcevm_plugin.png)
    
이렇게 설정 한 뒤에는 Tomcat이 다시 돌지 않고, 바로 바뀐 파일만 Hot Swap을 하게 된다. 그래서 속도가 훨씬 빠르다. 
특히나, **JRebel**의 경우에는 바이트코드를 변환하는 방식이라 속도가 좀 느린데, DCEVM의 경우는 그렇지 않다. 

위와 같이 설정 한 뒤에는 code를 수정한 뒤, update Application을 해주면 정상적으로 되는 것 같다. 

### 참고

* [aragorn/home](https://github.com/aragorn/home/wiki/DCEVM): DCEVM을 설치하는 데에 대한 부분이 정리가 되어 있고, DCEVM에 대한 설명이 있는데, 읽어보면 좀 재미가 있다. 클래스 파일을 바꿔치는 느낌? 파일의 바이트 코드를 바꾸는 방식이 아니라, 파일 자체를 **Watch**하는 수준의 Hot Swap을 진행한다고 서술 하고 있는데, 실제로 어떻게 동작 하는지는 더 찾아 봐야 할 것 같다. 

* [DCEVM](http://dcevm.github.io/): **DCEVM**소개 페이지이다. 파일 다운로드도 가능하다.

* [dcevm/dcevm](https://github.com/dcevm/dcevm): **DCEVM**의 github page이다.
