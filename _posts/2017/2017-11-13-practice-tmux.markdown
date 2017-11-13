---
layout: post
title: "Practice tmux"
description: "tmux에 대해 알아보자"
date: "2017-11-13 22:41"
tags: [tmux, programming]
comments: true
---



# tmux 사용 정리


맥에서 일단 설치를 해야 합니다.


[homebrew](https://brew.sh/index_ko.html)가 설치 되어 있으리라 생각하며, homebrew 설치를 건너 띄겠습니다.



## homebrew install

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```



## tmux install

```bash
$ brew install tmux
```

 위 처럼 입력하면 알아서 샥샥 설치가 됩니다.



## tmux conf .. Aka. tmux이쁘게 쓰기..?😁



구글 검색하면, 상위에 랭크된 깃허브 레포지토리를 가봅시다.



1. [tmux-themepack](https://github.com/jimeh/tmux-themepack)
2. [.tmux](https://github.com/gpakosz/.tmux)
3. [maglev](https://github.com/caiogondim/maglev)
4. [tmux-colors-solarized](https://github.com/seebi/tmux-colors-solarized)
5. [tmux-onedark-theme](https://github.com/odedlaz/tmux-onedark-theme)
6. [tmux-powerline](https://github.com/erikw/tmux-powerline)
7. [tmux-config](https://github.com/tony/tmux-config)



위와 같은 레포가 많습니다. 원하는 이쁜 녀석으로 골라 셋팅을 해줍니다.

참고로, tmux plugin도 있습니다.

- [tpm](https://github.com/tmux-plugins/tpm)

저는 7번 tmux-config를 셋팅 한걸로 기억하고 있습니다.


![seungdols-tmux screenshot](/blog/assets/img/post/2017/tmux_image.png)


tmux의 장점은 화면을 분할 할 수 있다는 것이 장점입니다.

tmux를 이용해 화면 분할을 하나의 세션 내에서 자유롭게 할 수 있죠. 특히나, session으로 관리함으로써 2명이 1개의 세션에 attach 하여 같은 화면을 볼 수도 있구요. 장점은 많습니다.



### 용어



- Prefix
  - 실제 단축키를 눌르기 전에 눌러야 하는 키를 말하며, 보통 `ctrl + b`를 말한다.
- Session
  - tmux가 관리하는 최상위 단위로 세션 단위로 attach/detach가 된다.
- Window
  - 세션안에 탭 같은 기능으로 하나의 세션 내에 여러개의 윈도우를 가질 수 있다.
- Pane
  - 윈도우 안에 존재하는 화면 단위로, 세로 분할, 가로 분할 할 수 있다. 각 분할된 화면이 Pane이다.



## 사용법



#### 세션 실행



```bash
$ tmux

$ tmux new -s sessionName
$ tmux new-session -s sessionName
```



#### 세션 종료

```bash
$ exit #in tmux

# or
$ tmux kill-session -t sessionName # in terminal, out tmux
```



#### 세션 attach

```bash
$ tmux attach -t sessionName #이름 지정 안했다면, 숫자
```



#### 세션 detach

```bash
$ tmux detach # iterm2에서 tmux det만 쳐도 가능
```



#### 세션 목록

```bash
$ tmux ls
```



------



### Window



- 윈도우 생성
  - `<prefix> + c`
- 윈도우 이름 변경
  - `<prefix> + ,`
- 이전, 다음 윈도우 이동
  - 이전
    - `<prefix> + n`
  - 다음
    - `<prefix> + p`
- 모든 윈도우 리스트 보기
  - `<prefix> + w`



### Pane

- 세로 분할
  - `<prefix> + %`
- 가로 분할
  - `<prefix> + "`
- 팬 이동
  - `<prefix> + q + Number`
  - `<prefix> +  q + 방향키`
- 줌
  - `<prefix> + z`



### 참고



- [터미널에서 tmux 사용해보기](http://egloos.zum.com/mcchae/v/11246020)
- [터미널 멀티플렉서 tmux를 배워보자](https://bluesh55.github.io/2016/10/10/tmux-tutorial/)
