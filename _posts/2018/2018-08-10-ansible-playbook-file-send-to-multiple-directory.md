---
layout: post
title: "ansible-playbook file send to multiple directory"
description: "Ansible-playbook 이용하여 다중 톰캣 서버를 업데이트 해보자."
date: "2018-08-10 00:39"
tags: [programming, linux, ansible, ansible-playbook]
comments: true
---


# Ansible-playbook 여러 디렉토리 확인 후 해당 디렉토리에 파일 전송하기

상황을 간략하게 설명하자면, 이런 상황이 생길 수 있다고 가정했습니다. 
예를 들어, 한 서버 (target)에 `apache`, `tomcat`을 사용한다고 생각해봅시다. 이 때에, `tomcat`은 인스턴스를 2개 사용합니다. 

생각해보면, 한 물리 서버에 인스턴스 2개인 `tomcat`이 존재하는 겁니다. 이 때 `tomcat`을 업데이트 한다고 가정하겠습니다. 
그렇다면, `copy` 혹은 `template` 명령어를 가지고 target 서버 내의 `tomcat` 두 디렉토리에 설정 파일을 전송해야 합니다.
어떻게 작성 할 수 있을 까요? 저는 아래처럼 작성 했습니다.

```yaml
- name: set tomcat config
  template: 
    src: "roles/{{ host }}/templates/{{ item }}-server.xml.j2"
    dest: "/seungdols/dev/app/{{ item }}/conf/server.xml"
    owner: seungdols
    group: seungdols
    mode: 0644
    backup: yes
  with_items:
    - tomcat1
    - tomcat2
```

다만, 이렇게 될 경우에는 문제가 조금 발생합니다. 어떤 문제일까요? 사실 서버 관리를 하다보면, 수 많은 서버들의 설정을 하는데, 미묘하게 다른 경우가 있습니다. 

여기서 저의 개발자적인 고민은 만약! `target`서버 내에 `tomcat2` 디렉토리가 존재 하지 않다면, 어떨까요? 만약 다른 설정은 같지만, `tomcat` 인스턴스가 하나인 서버인 예를 들고 싶습니다. 

그래서 고민이 시작 되었습니다. `Ansible`의 심도 있게 공부한 사람도 아니기때문에 이러한 자그마한 난관도 크게 다가오게 되더군요. 

고민의 답을 찾아 가는 과정을 적도록 하겠습니다. 

```yaml
- name: Check directory of tomcat1
  stat:
    path: /seungdols/dev/app/tomcat1
  register: tomcat1_dir

- name: Check directory of tomcat2
  stat:
    path: /seungdols/dev/app/tomcat2
  register: tomcat2_dir
```

그 첫 시작이 디렉토리 체크였습니다. `tomcat1`, `tomcat2` 디렉토리가 있다면, `stat`으로 체크 후 해당 값을 `register`로 저장하였습니다.

많은 서버들 사이에 `tomcat` 인스턴스가 2개가 올라간 서버와 `tomcat` 인스턴스가 1개만 올라간 서버를 동시에 조작하는 것을 목표로 하기 위해서였습니다. 

그리고, 설정 파일을 보내야 하는데, 어떤 타겟 서버는 `tomcat1`, `tomcat2` 디렉토리가 존재하고, 어떤 서버는 `tomcat1` 디렉토리만 존재한다면, 제가 작성 한 위의 파일 전송하는 시나리오는 실패합니다. 

```yaml
- name: set variable for tomcat directories
  set_fact: tomcat_directory="['tomcat1', 'tomcat2']"
  when: (tomcat1_dir.stat.exists) and (tomcat2_dir.stat.exists)

- name: set variable for tomcat directories
  set_fact: tomcat_directory="['tomcat1']"
  when: tomcat1_dir.stat.exists

- name: set variable for tomcat directories
  set_fact: tomcat_directory="['tomcat2']"
  when: tomcat2_dir.stat.exists
```

그리하여, 디렉토리 존재유무에 대한 리턴 된 값을 이용하여, 특수한 변수를 만들게 되었습니다. 위의 생각을 하게 된 것은 `stackoverflow`의 글을 보고는 생각이 났습니다!

- [참고 - Is it possible to set a fact of an array in Ansible?](https://stackoverflow.com/questions/23507589/is-it-possible-to-set-a-fact-of-an-array-in-ansible)

위의 글을 보고 앗! 왜 변수를 만들 생각을 못했을까 생각했다. 바로 `set_fact`라는 명령어로 변수 값을 만들 수 있다. 물론 `Asnible 2.0`부터는 `loop_vars`라는 것도 쓸 수 있다고 하지만, 저의 목적과는 달랐습니다.

그래서 위의 방식을 조합하여, `template`을 사용하는 것을 약간 변경하여, 저의 목적을 달성 할 수 있을거라 생각했습니다. 

그런데, 변수가 제대로 안찍히는게 아닙니까! 저는 `tomcat_directory`라는 `array`내의 `element` 값을 사용하고자 했습니다. 

```yaml
- name: debug tomcat_directory 
  debug: var={{ item }}
  with_items: 
    - tomcat_directory
```

초기에는 이렇게 값을 찍어 봤으나, 자꾸만 `tomcat_directory`가 찍히는게 아닙니까...:( 그렇지만, 찾아보니 어떻게 값을 찍어야 하는지 찾게 되었습니다. 
제가 원하는 대로 값을 찍으려면, 아래처럼 해야 합니다. 

```yaml
- name: debug tomcat_directory
  debug: var={{ item }}
  with_items: 
    - "{{ tomcat_directory }}"
```

이렇게 해주어야 제가 원하는 `array`내의 `element` 값이 출력됩니다. 

그래서 완성된 것은 아래와 같습니다. 

```yaml
- name: Check directory of tomcat1   
  stat:                              
    path: /seungdols/dev/app/tomcat1 
  register: tomcat1_dir              
                                      
- name: Check directory of tomcat2   
  stat:                              
     path: /seungdols/dev/app/tomcat2
  register: tomcat2_dir 

- name: set variable for tomcat directories
  set_fact: tomcat_directory="['tomcat1', 'tomcat2']"
  when: (tomcat1_dir.stat.exists) and (tomcat2_dir.stat.exists)

- name: set variable for tomcat directories
  set_fact: tomcat_directory="['tomcat1']"
  when: tomcat1_dir.stat.exists

- name: set variable for tomcat directories
  set_fact: tomcat_directory="['tomcat2']"
  when: tomcat2_dir.stat.exists

- name: Set server.xml for tomcat
  template:
    src: "roles/{{ host }}/templates/{{ item }}-server.xml.j2"
    dest: "/seungdols/dev/app/{{ item }}/conf/server.xml"
    owner: seungdols
    group: seungdols
    mode: 0644
    backup: yes
  with_items:
    - "{{ tomcat_directory }}"
  tags:
    - copy_tomcat_config
```

부족하지만, 사실 저는 이렇게 `Ansible`을 사용하고 있습니다.

 `handler`도 자주 사용하진 않지만, 그래도 `playbook`을 여러개 만들고, 다시 합치고 하다보니 나름의 잔머리가 늘은 것 같습니다. 

개발자에게도 유용하다고 생각합니다. 다만, 클라우드 환경에서는 활용하기가 훨씬 쉽지만, 사내 시스템 제약이 있는 경우에는 조금 어려운 것이 사용해본 후기입니다. 

도움이 되셨으면 합니다. 저는 위의 내용을 위해서 중복 시나리오를 작성하는 경우가 굉장히 많았습니다. 플레이북의 리팩토링을 대거 할 생각입니다. 
