---
layout: post
title: "Infra: Semaphore Install"
description: "Ansible GUI Tool - Semaphore 설치 경험기"
date: "2017-09-26 00:57"
tags: [infra, ansible, semaphore]
comments: true
---

# Ansible GUI Tool #1 - Semaphore 설치 경험기

사실 원래, **Ansible Tower**라는 막강한 툴이 있으나, 생각보다 정말 많이…**비싸다.**

그래서 대안으로 **Open Source Project**인 **Semaphore**를 설치하기로 했다. 그런데, 가이드는 정말 간단했다. 

*그래서 더 불안했다.*



## Semaphore Install

[Semaphore Project](https://github.com/ansible-semaphore/semaphore)

위 위키 페이지를 가면, 아래와 같은 *Install* 가이드를 준다. 

1. Copy download link for your OS from [Releases](https://github.com/ansible-semaphore/semaphore/releases) page
2. (linux) curl -L <link> > /usr/bin/semaphore
3. Run semaphore -setup
4. Continue setup (see below for more detail)

참고로, **Server OS, CPU 버전**이 무엇인지 확인 해두자.

```bash
$ curl -L <https://github.com/ansible-semaphore/semaphore/releases/download/v2.4.1/semaphore_linux_386> > /home/seungdols/applications/semaphore
$ chmod +x semaphore
$ ./semaphore -setup
```

설치시, **db password**를 *지정하면, 에러가 난다.*

Guide문서에도 **troubleshooting** 항목에 보면, `default`로 **db**의 문을 개방해두라고 한다.

이때 그만 두었어야 했다.

아무튼, 시작을 하자면, **setup**은 정말 간편하다.

하라는 대로 **default**로 *enter*만 쳐주면 된다.

그리고, `Playbook path`를 물어보는데, 아래의 경로로 셋팅을 해주었다.

```bash
$ pwd
/home/seungdols/applications/ansible
```

그러면, 하위에 semaphore_config.json file 하나가 뚝 떨어진다.

그리고 명령어를 실행시키면 된다.

```bash
$ nohup ./semaphore -config /home/seungdols/applications/ansible/semaphore_config.json &
```

이렇게 하면, 사실 가이드 상으로는 끝이다.

그러나, 내가 무언가 **config** 파일을 변경해서 재시작 하려고하면, 이게 말썽이다.

*죽어도 실행이 안된다.*

그래서 찾은건 **config**를 건드리면, **service**가 안뜬다는 정보 정도를 알아냈다.

원인은 모르겠지만, 관리의 부재인 탓인지도 모르겠다.

왜냐하면, 가이드 문서상은 `8010` 포트가 **default port**인데, 이 녀석이 실제로 동작하는 **port**는 `3000`이었다. 실제로 **setup**시에도 `example <http://localhost:8010>` **port**라고 나온다.



이렇게 해봤는데, 생각보다 Semaphore의 기능이 단순하며, 뭘 어떻게 써먹어야 하는지도 애매했다. 

그래서 느낌상, **foreman**을 설치하고, 그 위에 **foreman-ansible** 모듈을 설치 할 것 같다.

### 참고

- <https://gist.github.com/thedumbtechguy/0fae1ae931042829b73426630f3cd168>
- <https://github.com/ansible-semaphore/semaphore/wiki/Installation>




