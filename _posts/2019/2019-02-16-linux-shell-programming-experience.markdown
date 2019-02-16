---
layout: post
title: "Solve find missing argument to -exec"
description: "find missing argument to -exec"
date: "2019-02-16 18:36"
tags: [linux, shell, programming]
comments: true
---

# Solve find missing argument to -exec

어느날, 서버 작업을 하던 중에 쉘 스크립트를 하나 만들어야 했다. **log rotate**를 해야하는 상황이었다.

그래서 스크립트를 작성하는데, 계속 위와 같은 `find missing argument to -exec`오류가 발생했다. 

내가 작성한스크립트의 일부가 아래와 같다. 

```bash
/usr/bin/find $NGINX_LOG_ARCHIVE_DIR -name "access.$YESTERDAY.log" -exec /usr/bin/gzip -f '{}'\;

위 코드에서 계속 에러가 발생했는데, 해당 오류를 찾으려고 검색해보니 굉장히 쉬우면서 황당한 오류였다. 

알고보니 `-exec`는 `\;` terminate operator가 필요로 한 건 맞는데, `‘{}’ \;` 공백이 필요로 하다.

아래와 같이 수정하면 된다. 

```bash
/usr/bin/find $NGINX_LOG_ARCHIVE_DIR -name "access.$YESTERDAY.log" -exec /usr/bin/gzip -f '{}’ \;```
```

## 참고 

* [find-missing-argument-to-exec](https://stackoverflow.com/questions/2961673/find-missing-argument-to-exec)
