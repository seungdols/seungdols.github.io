---
layout: "post"
title: "server infra automation"
description: "서버 환경을 자동화 하는 방법을 찾아보자"
date: "2017-06-20 21:46"
tags: [server, automation, infra]
comments: true
---

# Server Infra Automation 작업

후보군

* Chef
* Ansible
* ~~puppet~~
* Docker

## Ansible

시스템 환경 설정 및 배포 자동화 플랫폼으로 요즘 핫하게 각광 받고 있는 툴이다.
### 특징
* simple format (YAML)
* agentless
* Python

[Ansible 시작하기](http://theeye.pe.kr/archives/2582)를 좀 따라해보고 개발기에서 한 번 시도 해볼만 하다.
Ansible에는 [playbook](http://theeye.pe.kr/archives/2597)이라는게 있는데, 이는 Ansible의 환경설정, 배포를 가능케하는 언어라고 한다. 그리고 YAML 문법으로 정책이 기술 되어 있어, 로드밸런서를 모니터링 하는 복잡한 환경에서도 사용 가능하다. ansible-pull도 있어서 node들이 conf등을 pull하고 싶을 경우 이용 가능하다.


* [ansible 설치 & 기본 사용하기](http://arisu1000.tistory.com/27761)
* [ansible 기본 설명](https://brunch.co.kr/@jiseon3169ubie/2)
* [ansible playbooks 요약 정리](https://moonstrike.github.io/ansible/2016/09/22/Ansible-Playbooks.html)

## 참고

* [ansible-license](https://www.ansible.com/tower-editions)
* [ansible-cloudformation](https://www.slideshare.net/awskr/ansible-cloudformation)
* [ansible의 이해와 활용](https://www.slideshare.net/deview/1a7ansible)
* [Ansible 시작하기](http://theeye.pe.kr/archives/2582)

## Chef

환경의 메타 데이터를 관리하고, 노드의 역할을 조정하는 OPS의 프레임워크라고 한다..[출처](https://www.slideshare.net/park11111111/whatischef-korean-29495303)

생각으로는 서버 환경 설정 툴인줄 알았으나, 그게 아닌가..음..자세히 보니 위 설명이 맞는 듯하다. 사실 툴의 개념보다는 프레임워크로 생각해야 한다.

환경의 구성 관리를 통해 서버 구성을 자동으로 조정 하는 녀석이라는 말이다. 즉, 서버 환경 설정 자동화 툴은 아니고, 많은 기능 중의 일부 기능인 것이다.

### 특징

* agent
* Ruby
* DSL

우선, Chef의 경우 서버 인프라 환경 자동화 툴이 아닌 일부 기능으로 존재하는 것이 맞고, 이 기능을 주로 쓰면 서버 환경을 자동화 하고, 노드 간 설정을 Refresh 해준다는 장점이 존재한다.
더 불어 아예 개발 환경 까지도 VM을 올리게 되면, vagrant + chef 조합을 많이 사용하는 것 같다.

Chef는 server, client, node, chef indexer등의 구조를 가지고 있으나, 이에 대한 것보다는 Cookbook에 더 관심을 가져야 한다. (실제로 사용자가 건드릴 건 Cookbook이 다라고 한다.)

### Cookbook 개념

쿡북은 한 개이상의 레시피, 정의, 자원, 속성, 파일, 라이브러리, 템플릿, 메타 데이터를 묶어 놓은 것이다. chef에서 쿡북은 배포의 기본 단위이다. 예로, MySQL을 위한 쿡북은 defualt, client, server 3개의 레시피와 설정 파일은 위한 3개의 템플릿, 2개의 속성, 1개의 메타데이터로 구성 된다.

#### Resource

세프 내에서 작업의 기본 단위다. 패키지나, 파일, 디렉토리, 서비스, 심지어 cron, mount도 자원으로 다룰 수 있다. resource에 지정된 동작은 적용 시점에 각 노드에서 실행 된다.
이를 통해 사용자나, 그룹을 생성, 삭제 하거나 노드 내 서비스를 재시작 할 수 있고, 로컬 파티션, 원격 디렉토리를 마운트 한 후 사용할 수도 있다.

#### Definition

기존 자원을 이용해 사용자 고유의 자원을 만들어 사용할 수 있다.

#### Recipe

레시피에는 한 개 이상의 자원이 어떤 순서로 관리되는지를 정의한다. 레시피는 다른 레시피를 포함할 수 있고, 쿡북에 정의 된 속성 뿐만 아니라 레시피를 적용하는 대상 노드에 정의 된 모든 속성 역시 이용 할 수 있다.

#### Provider

자원에 지정된 동작을 실제로 수행하는 주체이다.

#### Library

반복적이거나 레시피에 기술하기에는 너무 길고, 한 눈에 알아보기 힘든 내용을 레시피 외부에 정의하고, 레시피에서 사용할 수 있도록 해준다. Ruby, Chef의 DSL을 확장하는 형태로 작성 가능하다.
libraries 디렉터리에 위치하며, 존재하는 라이브러리 파일은 실행 시 자동으로 로딩 되고, 레시피나 속성, 정의에서 사용 가능하다.

#### Attribute

레시피 적용 시 노드에서 사용 될 값을 지정할 수 있다. 속성이 중요한 이유는 대상 OS마다 다르게 지정해야 할 값을 레시피에 기술 일일이 기술하거나 각각을 별도의 파일로 만들 필요가 없게 해주기 때문이다.
아파치 서버를 예로 들면, 데비안, 우분투 계열과 레드햇 계열 리눅스는 기본적으로 설치 시에 정해지는 아파치 설정 파일 경로나 이름 등이 좀 다르다. 이를 레시피에 일일이 기술하거나, 여러 종류의 레시피를 만든다면, 내용도 쓸데 없이 길어지고 복잡도만 늘어난다.
속성을 이용해 이 문제를 쉽게 해결 할 수 있다.

#### File

모든 것이 작성하는 코드만을 이용해 해결된다면 세상은 정말 행복할 것이다. 하지만 실제로는 종종 필요한 파일을 직접 업로드해야 할 경우가 발생한다. 예를 들어 각 리눅스/유닉스 시스템의 패키지 관리자가 지원하지 않는 파일을 설치해야 한다거나, 직접 제작한 파일을 각 시스템에 배포해야 하는 경우가 그러하다.

#### Template

파일과 매우 비슷하지만 한 가지 결정적인 다른 점은 노드에 템플릿이 전송될 때 렌더링 작업을 거친 후 전송된다는 것이다. 각 노드의 시스템 속성 및 정의된 속성이 템플릿 렌더링 시 사용된다. 이것이 의미하는 바는 앞서 속성에서 언급한 바와 같이 각 노드 별로 달라야 하는 값 때문에 설정 파일을 여러 개 만들 필요가 없다는 것이다.

#### Role

노드에 적용될 여러 레시피와 속성의 집합이다. 노드 하나에 하나 이상의 역할을 할당할 수 있다. 역할은 레시피나 속성을 역할의 특성에 맞게 쉽게 조합 가능하며, 역할 이름이 무엇을 하는지 명확히 나타내주기 때문에 차후 유지보수에도 도움이 된다.

#### Metadata

셰프 서버가 각 노드에 제공하는 몇 가지 기본적인 정보를 담고 있다. 각 쿡북의 최상위 디렉터리에 metadata.rb 파일을 만들고 필요한 내용을 기술하면 해당 파일은 JSON파일로 컴파일된 후 셰프 환경에서 사용된다.


## 참고


* [내컴에선 잘되던데? - vagrant로 서버와 동일한 개발환경 꾸미기](https://www.slideshare.net/kthcorp/h3-2012-vagrant)
* [Vagrant와 chef로 개발서버 구축 자동화하기](https://www.slideshare.net/arawnkr/vagrant-chef)
* [Chef로 서버팜 깔끔하게 요리하기, Part1 아키텍쳐와 구성](https://www.ibm.com/developerworks/community/blogs/9e635b49-09e9-4c23-8999-a4d461aeace2/entry/215?lang=en)

## Docker + Kubernates

### 특징

도커를 이용해 개발 서버들을 모두 컨테이너 형식으로 생성/삭제 할수 있다.
한 번 설정 된 이미지를 가지고 같은 컨테이너를 생성하는 것은 쉽다.
다만, 관리하는 툴이 필요로 한데, Kubernates를 이용하면 된다.

### 내 생각

도커와 함께 쿠버네이트를 이용해 가상화 하는 방법은 참으로 좋은 방법이긴 하지만, 서버 환경 셋팅을 자동화 하는 방식에서는 맞지 않는다.

왜냐하면, 도커의 경우 이미지를 만들어 필요한만큼 가상화로 생성한다.
그런데, 이 때, 톰캣 설정이나, 아파치 설정을 바꿔서 변경해야 한다는 것은 이미지를 삭제하고, 다시 새로운 이미지를 올리는 것과 같은 행태로 봐야 한다.

이 말은, 도커와 쿠버네이트 조합으로 서버 환경 전체를 스냅샷으로 생성/삭제하는 방식인것이지, 서버 환경만 자동화 한다는 관점에서는 다소 어긋난 접근일 수 있지 않나 생각해본다.


### 참고

* [Docker로 서버 개발 편하게 하기](https://www.slideshare.net/jhgod/docker-62527504)
* [Docker활용법-개발환경 구성하기](http://raccoonyy.github.io/docker-usages-for-dev-environment-setup/)
* [Docker + Kubernates를 이용한 빌드 서버 가상화 사례](https://www.slideshare.net/naver-labs/docker-kubernetes)
