---
layout: "post"
title: "apt-get update fault"
description: "Ubuntu apt-get update 오류와 Grup Repair"
date: "2016-06-11 00:30"
tags: [ubuntu]
comments: true
---


##### 디렉토리 변경
```text
cd /etc/apt
```

##### 업데이트 서버 변경 (우분투 한국 서버 kr.archive.ubuntu.com )
```bash
sudo sed -i 's,http://.*ubuntu.com,http://old-releases.ubuntu.com,g' sources.list
```

##### Daum 서버
```bash
sudo sed -i 's,http://.*ftp.duam.net,http://old-releases.ubuntu.com,g' sources.list
```

##### 변경 완료 후 목록 갱신
```bash
sudo apt-get update
```

#### 우분투/윈도우 멀티 부팅시 윈도우 재설치 후 Grup 복구

먼저 우분투 부팅 usb나 dvd로 부팅 후 우분투 설치 없이 사용함 클릭 후 우분투에 접속

```bash
$ sudo add-apt-repository ppa:yannubuntu/boot-repair
$ sudo apt-get update
$ sudo apt-get install boot-repair -y
```
위 코드를 터미널에 하나씩 차례로 입력위 코드를 터미널에 하나씩 차례로 입력
아래의 코드를 실행아래의 코드를 실행

```bash
$ sudo boot-repair
```

부트 리페어가 뜨면 권장 기본복구 버튼 클릭 후 절차대로 하면 끝.
