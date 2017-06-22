---
layout: "post"
title: "redis info"
description: "짜집기편 - 레디스에 대해 알아보자!"
date: "2017-06-22 15:16"
tags: [redis]
comments: true
---

# Redis

in-memory data structure store, used as a database, cache and message broker. It supports data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs and geospatial indexes with radius queries. Redis has built-in replication, Lua scripting, LRU eviction, transactions and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster.



## 개념

#### Key/Value Store

레디스는 기본적으로 key/value 저장소로, 특정 키 값에 값을 저장하는 구조로 되어 있다.

다만, 이 모든 데이터들이 메모리에 저장 되어 있으므로, 매우 빠른 read/write 속도를 낼 수 있다. 고로, 전체적인 저장 가능한 데이터의 용량은 물리적인 용량을 넘어설 수 없다.



#### [다양한 데이터 타입](https://redis.io/topics/data-types)

memcached와 다른 점이라면, 레디스의 경우 키 값에 매핑하는 값에 다양한 데이터 타입을 매핑 시킬 수 있다. 고로, memcached보다는 다양한 데이터 타입을 지원하므로 더 복잡한 구조를 저장 할 수 있다는 장점이 있다.

##### String

일반적인 문자열이라 생각하면 되나, 바이너리까지 저장 할 수 있어서, JPEG 이미지나, 직렬화된 루비 객체도 저장 가능하다. 최대 512 MB까지 저장 가능하다.

##### Sets

레디스에서 문자열들의 무순서 집합 자료형이라고 생각하면 된다. 이 데이터형은 교집합, 차집합등의 연산이 가능한데, 굉장이 빠른 속도를 보장한다.

##### Sorted Set

set에 score가 추가 된 sets으로 생각하면 된다. sets과 유사하다.

sorted set은 요소에 대한 추가/삭제/업데이트가 굉장히 빠르다.

더군다나, sorted set에서 데이터는 오름차순으로 내부적으로 정렬 되며, range query, top query등 query가 가능하다.

##### Hashes

value내에 field/string value의 쌍을 저장 할 수 있는 데이터 타입이다.

예시이며, 성별, 이름, 나이등 여러 필드를 가지는 사용자를 저장하는데 적합하다.

```text
@cli
HMSET user:1000 username antirez password P1pp0 age 34
HGETALL user:1000
HSET user:1000 password 12345
HGETALL user:1000
```



##### List

문자열의 단순한 나열로, 생각하면 된다. 입력한 순서에 의해 정렬이 된다. 다만, 레디스에서 리스트는 좀 특별한데, Head/Tail에 데이터를 추가/삭제가 가능하다. 고로, Deque라는 자료구조를 생각하면 쉽다.



## 설치

우선 공식 Redis Home의 다운로드를 받는다. 생각보다 가볍다.

```bash
sudo wget https://github.com/antirez/redis/archive/4.0-rc3.tar.gz
```

### make 방식

```bash
cd /home/seungdols

tar xzvf 4.0-rc3.tar.gz #압축 해제
cd redis-4.0-rc/
sudo make
```

다만, 이렇게 할 경우 conf 파일들이 기본 설정으로 셋팅 되어, 수작업으로 변경을 해주거나 해야 한다. 기본 포트 설정은 6379로 설정 된다.

우선, 포트를 7000번으로 수정하려고 하면, 디렉토리를 만들어주고, conf파일을 옮겨주어야 한다.

```bash
mkdir 7000
cp redis.conf 7000/redis.conf
```

그리고, 7000/redis.conf의 파일에 있는 port를 변경해주어야 한다. 그리고, pidfile의 경로에 있는 redis_6379.pid를 7000으로 변경 해주어야 한다.

dbfilename의 dump.rdb는 기존에는 포트가 같이 있었는지 모르지만, 4.0에서는 port 번호는 없어져 변경할 필요는 없다.

이렇게 설정을 했다면, 서버를 띄워보자.

```bash
cd /home/seungdols/redis-4.0-rc3/
src/redis-server 7000/redis.conf & #7000/redis.conf 파일을 기반으로 redis를 띄운다.
```

 다만, 여기서 redis를 안전하게 shutdown하는 방법을 못찾았는데, 아무래도 kill을 이용하면 위험하긴 할 것 같다. 그래서 초보자들은 install.sh를 이용하여 설치하는 방법이 제일 깔끔하다. 필요로하는 service 파일들도 /etc/init.d/ 하위에 옮겨 준다.



### install.sh 방식

```bash
cd redis-4.0-rc3/
cd utils/
sudo ./install_server.sh
```

다만 이 때에 executable path는 사용자가 직업 지정을 해주어야 한다. 그 외는 상당히 편리하게 변경 가능하다.





## Replication 구성하기

Master/Slave를 구성하기 위해서는 master node의 conf 파일에서 bind ip를 변경해준다.

```Text
#bind 127.0.0.1
bind 0.0.0.0

requirepass <password> #password 설정
masterauth <master-password> #slave node일때 암호 설정
```



그리고, slave node의 conf에서 slaveof 라는 옵션을 지정해주어야 한다.

```wiki
slaveof master_node_ip master_node_port

masterauth <master-password> #slave node일때 암호 설정
repl-ping-slave-period 10
repl-timeout 60
```



#### Master/Slave Test 확인

먼저 master node에서 redis-cli을 접속하여 데이터를 생성 해보겠습니다.

```bash
127.0.0.1:7000> set seungdols 'redis-replication success'
OK
127.0.0.1:7000> get seungdols
"redis-replication success"
```

slave node는 read only로 데이터의 접근만 가능합니다.

```bash
$ src/redis-cli -p 7000 -a 비밀^^
127.0.0.1:7000> get seungdols
"redis-replication success"
127.0.0.1:7000>
```

slave node에도 정확하게 데이터가 들어 간 것을 볼 수 있습니다.



## Redis 구성

![redis master/slave](http://cfile29.uf.tistory.com/image/23714F4A56E6F1C41BC5D5)

이미지 출처 : http://crystalcube.co.kr/176

그런데 위 처럼 할 경우에 master node가 죽을 경우 문제가 생긴다. 이 때에는 어떻게 처리를 할 것인가?



그래서 Redis에서는 master/slave node 간의 역할 문제를 해결해주는 녀석이 있습니다.

그게 바로 sentinel이라고 합니다.

![sentinel 추가한 구성도](http://cfile9.uf.tistory.com/image/234C393456E6F29003FD81)

위 처럼 작업하게 되는데, 이 때 sentinel은 독립 서버에 설치해도 되지만, 동일한 서버에 설치해도 무관하다고 합니다. 정말 만일의 사태에 대비한다면, 따로 분리해두는 것도 좋을듯 합니다.



### sentinel의 역할

master node/slave node를 감시한다.

master node가 죽을 경우 slave node중에서 하나를 master node로 변경 한다.

이전 master node가 되살아날 경우 slave node로 변경해준다.

실질적인 역할은 위의 기능이 다입니다. 그럼에도 불구하고, 수동으로 복구 해야하는 기능을 담당 해주므로 failover에 필수적으로 설정을 해야 합니다.



다만, 문제는 client에서 master node가 바뀌었을 경우 port를 자동으로 감지 할 방법은 없습니다. 이 때, HAproxy를 이용하는 방법이 있습니다.

![HAproxy를 이용한 switching](http://cfile7.uf.tistory.com/image/2702BD3556E6F36120D26F)

위와 같이 운용을 하려면, HAproxy를 두어야 하는데, 이를 둠으로써 client에서는 master node가 변경 되어도 정상적인 서비스가 가능하게 됩니다. 다만, HAproxy가 1대인 경우에 죽는 경우 Redis 전체를 사용 불가한 상황이 발생합니다. 그래서 2대로 이원화하여 사용하고, L4 switch로 묶는 방법이 있습니다.

출처 : http://crystalcube.co.kr/178





참고

- [redis-cluster document](https://redis.io/topics/cluster-tutorial)
- [NoSQL - Redis, Replication(복제) 구성](http://develop.sunshiny.co.kr/1005)
- [레디스 레플리케이션 예제(Redis Replication)](http://jdm.kr/blog/157)
- [Redis 2부 - Replication](http://crystalcube.co.kr/176)
- [In memory dictionary Redis 소개](http://bcho.tistory.com/654)
- [Redis의 SCAN은 어떻게 동작하는가?](http://tech.kakao.com/2016/03/11/redis-scan/)
- [redis-cluster 셋업하기](http://knight76.tistory.com/entry/redis-redis-cluster-%EC%85%8B%EC%97%85%ED%95%98%EA%B8%B0)
- [Redis-Cluster 구성](http://firstboos.tistory.com/entry/Redis-Cluster-%EA%B5%AC%EC%84%B1)
- [Redis Master/Slave 구성하기](http://adahnlim.github.io/2016/05/04/Redis-master-slave/)
- [Redis - 복제(Replication) 구성](http://riceworld.tistory.com/18)
- [Redis 설치 및 Cluster 구성](https://lahuman.github.io/redis-cluster-install/)
