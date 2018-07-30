---
layout: post
title: "systemd guide"
description: "systemd에 대해 알아보자."
date: "2018-07-04 15:57"
tags: [programming, linux, systemd]
comments: true
---


# CentOS7 systemd 사용하기


RHEL7의 OS의 큰 변화로는 init 데몬에서 systemd 데몬으로 변경되었다. 

`service` 파일은 크게 `Unit, Service, Install` 로 나뉜다고 생각하면 쉽다.

```text
[Unit]
Description=Sample Service
Requires=local-fs.target
After=local-fs.target

[Service]
Type=simple
PIDFile=/var/run/sample.pid
ExecStart=/usr/sbin/sampled -d
ExecStop=/usr/sbin/sampled -k

[Install]
WantedBy=multi-user.target
```

출처: http://fmd1225.tistory.com/93 [fmd1225's One day]

systemd 서비스 등록을 위해서는 서비스 등록용 파일이 필요로 하다. 

해당 파일을 `/lib/systemd/system/[service name].service` or `/etc/systemd/system/[service name].service` 의 경로에 생성 해주어야 한다. 



사실 위 파일에 들어가는 옵션은 아래에서 알아보고, 예시로 logstash를 서비스로 등록한다고 했을때, 아래와 같이 파일을 생성한다. 

```bash
$ sudo vi /lib/systemd/system/logstash.service
```



##### 예시) logstash.service

```text
[Unit]
Description=logstash service

[Service]
Type=simple
User=logstash
Group=logstash
ExecStart=/home/seungdols/apps/logstash/bin/logstash -f home/seungdols/apps/logstash/conf/logstash-test.conf
WorkingDirectory=/
Nice=19
LimitNOFILE=16384

[Install]
WantedBy=multi-user.target
```

위와 같이 생성할 수 있으며 사용하기 위해서 파일의 권한을 변경해준다. 

```bash
$ sudo chmod 644 /lib/systemd/system/logstash.service
```

시스템이 `reboot`되거나 할 경우에도 재시작을 하기 위해서는 아래와 같이 설정을 해준다. 

```bash
$ sudo systemctl enable logstash.service
```

수동으로 시작하기 위해서는 아래 명령어를 입력하면 된다. 

```bash
$ sudo systemctl start logstash.service #종료는 stop 명령어가 있다.
```

상태를 확인하는 방법으로는 `status`명령어로 확인 하는 방법이 있다. 

```bash
$ sudo systemctl status logstash.service
```



## Systemd option

해당 옵션은 `potatogim wiki`인 [이곳](https://potatogim.net/wiki/Systemctl)에서 발췌하였습니다.

### Unit

- `Description=`

  해당 유닛에 대한 상세한 설명을 포함한다.

- `Requires=`

  상위 의존성을 구성한다. 필요 조건으로서, 이 목록이 포함하는 유닛이 정상적일 경우 유닛이 시작된다.

- `RequiresOverridable=`

  "Requires=" 옵션과 유사하다. 하지만 이 경우 "사용자에 의해서" 서비스가 시작하는데 상위 의존성이 있는 유닛 구동에 실패 하더라도 이를 무시하고 유닛을 시작한다. (즉 상위 의존성을 무시한다.) 자동 시작의 경우 적용되지 않는다.

- `Requisite=, RequisiteOverridable=`

  "Requires=" 와 "RequiresOverridable=" 와 유사하다. 상위 의존성 유닛이 시작되지 않았다면 바로 실패를 반환한다.

- `Wants=`

  "Requires=" 보다 다소 완화된 옵션이다. 충분 조건으로서, 상위 의존성 목록에 포함된 유닛이 시작되지 않았더라도 전체 수행 과정에 영향을 끼치지 않는다. 이 옵션은 하나의 유닛을 다른 유닛과 연계할 경우 사용하게 된다.

- `BindsTo=`

  "Requires=" 와 매우 유사하다. 네트워크 카드가 물리적 장애가 발생한 경우와 같이 systemd의 개입이 없이 갑작스레 서비스가 사라진 경우 관련된 유닛도 같이 중지하도록 설정한다.

- `PartOf=`

  "Requires=" 와 매우 유사하다. 상위 의존성의 유닛을 중지하거나 재시작하는 경우 해당 유닛 또한 중지나 재시작을 수행한다. 하나의 서비스가 다른 하나의 일부로서 의존성을 만들어내는 경우 필요하다.

- `Conflicts=`

  베타적 관계를 설정한다. 만일 '유닛1' 의 "Conflicts=" 설정이 '유닛2' 로 되어 있다면 '유닛1'이 시작된 경우 '유닛2'가 중지되고, '유닛1'이 중지된 경우 '유닛2'가 시작한다. 이 옵션은 "After=" 와 "Before=" 옵션과는 독립적으로 작동한다.

- `Before=, After=`

  유닛 시작의 전후 관계를 설정한다. 해당 설정은 "Requires=" 설정과는 독립적이다. "Before=" 에 나열된 유닛이 시작되기 전에 실행하고 "After=" 은 해당 유닛이 시작된 이후 나열된 유닛이 실행한다. 시스템 종료 시에는 반대 순서로 동작한다.

- `OnFailure=`

  해당 유닛이 "실패" 상태가 되면 수행할 유닛 목록 (예 "/" 파일 시스템 마운트 실패시 복구 모드 수행)

- `PropagatesReloadTo=, ReloadPropagatedFrom=`

  재구성(reload) 명령을 다른 유닛에게 전파하거나 혹은 지정된 다른 유닛으로부터 재구성(reload) 명령을 전파 받는다.

- `RequiresMountsFor=`

  절대 경로로 지정하여 유닛을 구동하는데 필요한 마운트 목록을 자동으로 구성하여 "Requires=", "After=" 을 수행한다. 즉 필요한 마운트 경로가 준비되어 있는지 점검하고 마운트를 미리 진행한다.

- `OnFailureIsolate=[yes|no]`

  "yes" 인 경우 "OnFailure=" 에 선언된 목록과는 격리 모드(isolation mode)로 작동한다. 즉 해당 유닛에 종속성이 없는 모든 유닛은 중지된다. "OnFailureIsolate=yes" 인 경우 "OnFailure=" 옵션에 하나의 유닛만 설정 할 수 있다.

- `IgnoreOnIsolate=[yes|no]`

  "yes"인 경우 다른 유닛과 격리(isolation)할 때 중지하지 않는다.



### Service

- `Type=[simple|forking|oneshot|notify|dbus]`

  유닛 타입을 선언한다."simple" (기본값) - 유닛이 시작된 경우 즉시 systemd는 유닛의 시작이 완료됐다고 판단한다. 다른 유닛과 통신하기 위해 소켓을 사용하는 경우 이 설정을 사용하면 안된다."forking" - 자식 프로세스 생성이 완료되는 단계까지를 systemd가 시작이 완료되었다고 판단하게 된다. 부모 프로세스를 추적할 수 있도록 PIDFile= 필드에 PID 파일을 선언해 주어야 한다."oneshot" - "simple" 과 다소 유사하지만 단일 작업을 수행하는데 적합한 타입이다. 또한 실행 후 해당 실행이 종료되더라도 RemainAfterExit=yes 옵션을 통해 유닛을 활성화 상태로 간주할 수 있다."notify" - "simple" 과 동일하다. 다만 유닛이 구동되면 systemd에 시그널을 보낸다. 이때 시그널에 대한 내용은 libsystemd-daemon.so에 선언되어 있다."dbus" - D-Bus에 지정된 BusName 이 준비될 때까지 대기한다. 다시 말해서, D-Bus가 준비된 후에 유닛이 시작되었다고 간주한다.

- `RemainAfterExit=[yes|no]`

  유닛이 종료된 후에도 활성화 상태로 판단한다.

- `GuessMainPID=[yes|no]`

  이 옵션은 "Type=forking"이고, "PIDFile="(빈 값)인 경우에 작동한다. systemd가 유닛이 정상적으로 시작되었는지 명확한 판단이 어려운 경우에 사용된다. 만일 여러 데몬 프로세스로 구성된 유닛이라면 PID를 잘못 추측할 수도 있다. 그로 인해 오류 검출이나 자동 재시작 등의 작업이 불가능 할 수 있는데, 이 옵션은 이러한 문제를 방지하는 기능을 한다.

- `PIDFile=`

  PID 파일의 절대 경로를 지정한다. 만일 유닛 타입이 forking 이라면 해당 설정을 추가해 주어야 한다.

- `BusName=`

  D-Bus의 버스 이름을 지정한다. Type=dbus 인 경우 필수 사항이다. 다른 Type 의 경우라도 D-Bus 버스 이름을 다르게 사용하는 경우 별도로 설정을 해주는 것이 좋다.

- `Environment=`

  해당 유닛에서 사용할 환경 변수를 선언한다. 또한 반드시 "Exec*=" 옵션보다 상단에 위치해야 한다. 예제는 아래와 같다.

  `Environment='ONE=one' 'TWO=two two'`

- `EnvironmentFile=`

  해당 유닛에서 사용할 환경 변수 파일을 정의한다. 환경 변수 파일에서 "#" 와 ";" 로 시작되는 라인은 주석으로 처리된다. "Environment=" 와 같이 사용하는 경우 "Environment=" 옵션값이 먹게 된다. 또한 반드시 "Exec*=" 옵션보다 상단에 위치해야 한다.

- `ExecStart=`

  시작 명령을 정의한다. 실행 명령어는 반드시 절대 경로 또는 변수(`${STRINGS}`와 같이)로 시작해야 하며, 다중 명령어를 지원한다. 예제는 아래와 같다.

  ExecStart="commnad1"

  ExecStart="command2"

  ExecStart="command3"

  혹은 ExecStart="command1; command2; command3"

- `ExecStop=`

  중지 명령을 정의한다. 사용법은 "ExecStart="와 동일하다. 중지 방식은 "KillMode=" 로 지정된다.

- `KillMode=[control-group|process|none]`

  중지 방법에 대해서 선언한다."control-group" (기본값) - 해당 유닛의 그룹에 포함된 모든 프로세스를 중지 시킨다."process" - 메인 프로세스만 중지 시킨다."none" - 아무런 행동도 하지 않는다.*여기에서 그룹은 어떤 유닛과 그에 종속성을 가지는 다른 유닛들을 말한다.*

- `ExecReload=`

  재구성을 수행할 명령을 정의한다.

- `ExecStartPre=, ExecStartPost=, ExecStopPre=, ExecStopPost=`

  유닛 시작, 중지 등의 동작과 관련하여 수행할 추가 명령을 정의한다. 사용법은 "ExecStart=" 동일하다.

- `RestartSec=`

  재시작 명령을 수행할 때, 중지 후 다시 시작하기까지 대기(sleep)하는 시간을 설정한다. 기본값은 "100ms" 이다. 각각 "min", "s", "ms" 단위로 설정한다. 해당 설정은 Restart= 옵션이 있는 경우에만 적용된다.

- `TimeoutStartSec=`

  유닛의 시작이 완료될 때까지 대기하는 시간을 설정한다. 기본값은 90초(90s)이며, 만일 Type=oneshot 인 경우 해당 설정이 해당 설정이 적용되지 않는다. 만일 시작 완료에 대해 반환이 될 때까지 대기하려면 "TimeoutStartSec=0" 으로 설정한다.

- `TimeoutStopSec=`

  유닛의 중지가 완료될 때까지 대기하는 시간을 설정한다. 기본값은 90초(90s)이며, 위의 "TimeoutStartSec=" 옵션과 동일하게 "TimeoutStopSec=0" 으로 설정하면 반환이 될 때까지 대기한다. "TimeoutStopSec" 에 설정된 시간 안에 종료되지 않으면 SIGKILL 시그널을 보내서 강제로 종료시킨다.

- `TimeoutSec=`

  "TimeoutStartSec=" 와 "TimeoutStopSec=" 을 동시에 설정한다.

- `WatchdogSec=`

  유닛 시작 후에 주기적으로 생존을 감시하기 위한 질의(keep-alive ping)를 하는데, 이 질의가 반환되까지의 대기 시간을 설정한다. "Restart=" 옵션이 "on-failure", "always" 인 경우 유효하며, 여기에 지정된 시간 안에 응답이 없다면 자동으로 유닛을 재시작한다. 기본값은 "0" 으로 유닛의 생존을 감시하지 않는다.

- `Restart=[no|on-success|on-failure|on-watchdog|on-abort|always]`

  유닛이 죽었거나 "WatchdogSec="에 지정된 생존 응답 시간이 초과한 경우 재시작한다. "ExecStartPre=", "ExecStartPost=", "ExecStopPre=", "ExecStopPost=", "ExecReload=" 에 설정된 유닛의 경우에는 포함되지 않으며, 이 유닛에만 해당된다."no" (기본값) - 유닛을 다시 시작하지 않는다."on-success" - 유닛이 정상적으로 종료됐을 경우에만 재시작한다. 종료 시 "0"을 반환했거나, SIGHUP, SIGINT, SIGTERM, SIGPIPE와 같은 시그널 또는 "SuccessExitStatus=" 설정에서 지정된 반환 코드 목록에 따라 모두 성공으로 인식해 재시작을 하게 된다."on-failure" - 유닛이 비정상적으로 종료된 경우에 재시작한다. 반환이 "0"이 아닌 경우나 비정상적인 시그널을 받고 종료된 경우, "WatchdogSec="에 지정된 생존 응답 시간을 초과한 경우에 유닛을 재시작한다."on-watchdog" - "WatchdogSec="에 지정된 생존 응답 시간을 초과한 경우에 유닛을 재시작한다."on-abort" - 지정되지 않은 반환을 받은 경우에 유닛을 재시작한다."always" - 종료 상태에 관계없이 무조건 재시작하며, 수동으로 중지하더라도 다시 시작됨을 주의해야 한다.

- `SuccessExitStatus=`

  성공으로 판단할 시그널을 설정해 준다. 아래의 예를 참고하자.

  `"SuccessExitStatus=1 2 8 SIGKILL"`

- `RestartPreventExitStatus=`

  재시작하지 않을 반환을 정의하며, 굳이 재시작하지 않아도 되는 반환이 있다면 유용하다.

  `"RestartPreventExitStatus=1 6 SIGABRT"`

- `PermissionsStartOnly=[yes|no]`

  "User=", "Group=" 옵션 등과 같이 권한 설정을 적용하여 시작한다. 해당 설정은 "ExecStart="에서만 적용되며, "ExecStartPre=", "ExecStartPost=", "ExecReload=", "ExecStop=", "ExecStopPost="에서는 적용되지 않는다.

- `User=, Group=`

  유닛의 프로세스를 수행할 사용자/그룹의 이름을 지정한다.

- `RootDirectoryStartOnly=[yes|no]`

  프로세스의 루트 디렉토리를 지정한다. `chroot()` 함수를 사용하여 구동된다. 해당 설정은 "ExecStart="에서만 적용 되며 "ExecStartPre=", "ExecStartPost=", "ExecReload=", "ExecStop=", "ExecStopPost="에서는 적용되지 않는다.

- `RootDirectory=`

  `chroot()` 함수로 변경할 디렉토리를 지정한다.

- `WorkingDirectory=`

  프로세스의 작업 디렉토리를 지정한다. 별도의 지정이 없으면 유닛은 최상위 디렉토리("/")를 작업 디렉토리로 사용한다. 특정 디렉토리 상에서 실행되는 프로세스에서 필요하다.

- `NonBlocking=[yes|no]`

  소켓 기술자에 `O_NONBLOCK` 플래그를 사용한다. yes일 경우 STDIN/STDOUT/STDERR을 제외한 모든 소켓에 `O_NONBLOCK` 플래그가 지정된다. 즉 사용되는 모든 소켓이 비봉쇄로 동작한다.

- `NotifyAccess=[none|main|all]`

  서비스 상태 알림 소켓으로의 접근을 제어한다. 해당 접근은 `sd_notify()` 함수를 사용한다."none" - 유닛 상태에 대한 모든 정보를 무시한다."main" (기본값) - 메인 프로세스에 대해서만 상태 정보 알림을 허용한다."all" - 모든 유닛. 즉 컨트롤 그룹의 유닛 상태 정보 알림을 허용한다."Type=notify" 혹은 "WatchdogSec="가 설정된 경우 "NotifyAccess="를 "main" 혹은 "all"로 설정하여 접근 가능하게 설정해야 한다.

- `Sockets=`

  유닛에서 사용하는 소켓의 이름을 지정한다. 기본으로 "<유닛명>.socket"으로 생성되지만 지정된 이름으로 소켓을 사용하는 경우 별도의 설정이 가능하다. 또한 하나의 유닛에서 여러 소켓 목록을 일괄적으로 관리하는 경우 "Sockets=" 옵션이 여러번 사용될 수도 있다. 만일 "Sockets=" 옵션이 아무런 설정 없이 단독으로 사용되는 경우 소켓 목록이 리셋되게 된다.

- `StartLimitInterval=, StartLimitBurst=`

  위의 두 설정값을 이용하여 제한된 시간에 너무 많은 재시작 ("Restart=") 이 발생되는 것을 방지한다. 기본값에 따르면 10초 간격으로 5번까지 서비스 시작을 허용하고 이후에 재시작 이벤트가 발생하면 자동으로 재시작하지 않도록 설정한다. 유닛 재시작 이벤트가 무한으로 발생하는 것을 방지하며, 추후 관리자가 수동으로 복구할 수 있도록 한다."StartLimitInterval=" 옵션의 경우 기본값 10초(10s), "0" 으로 설정할 경우 비활성화한다."StartLimitBurst=" 옵션의 경우 기본값 5회.

- `StartLimitAction=[none|reboot|reboot-force|reboot-immediate]`

  만일 복구 재시도가 제한된 설정 (Service Recovery Limit > StartLimitInterval * StartLimitBurst) 내에 완료되지 않는다면 어떠한 조치를 취할 것인지 정의한다."none" (기본값) - 아무런 조치도 하지 않는다."reboot" - 시스템을 재부팅한다. (systemctl reboot과 동일)"reboot-force" - 메모리 상의 데이터를 디스크와 동기화한 후에 시스템을 강제로 제부팅한다. (systemctl reboot -f와 동일)"reboot-immediate" - 메모리 상의 데이터를 디스크와 동기화하지 않고 바로 시스템을 강제로 재부팅한다.

- `Nice=`

  해당 유닛의 프로세스의 nice 값을 지정한다. "-20" 부터 "19" 까지 정수형으로 등록한다.

- `OOMScoreAdjust=`

  OOM(Out Of Memory) killer 작동 시 프로세스 우선 순위를 미리 지정할 수 있다. "-1000" 에서 "1000" 까지 정수형으로 등록한다.

- `UMask=`

  umask 값을 선언한다. 별도의 설정이 없으면 기본값은 "0022" 이다.

- `SyslogFacility=`

  로그 시설을 설정할 수 있다. "kern, user, mail, daemon, auth, syslog, lpr, news, uucp, cron, authpriv, ftp, local0, local1, local2, local3, local4, local5, local6, local7" 등의 값으로 설정 가능하다.

- `SyslogLevel=`

  로그 수준을 설정할 수 있다. "emerg, alert, crit, err, warning, notice, info, debug" 등 설정이 가능하다.

- `TCPWrapName=`

  TCP 래퍼를 사용하기 위한 설정이다.

- `PAMName=`

  PAM 보안을 사용하기 위한 설정이다.

 

### Install

- `Alias=`

  유닛의 별칭을 지정한다. "systemctl enable" 명령어를 통해서 별칭을 생성할 수 있다. 별칭은 유닛 파일 확장자(유닛 타입)를 가지고 있어야 한다.

  (service, socket, mount, swap 등이 있다. 예: httpd.service 의 Alias=apache.service)

- `WantedBy=, RequiredBy=`

  "systemctl enable" 로 유닛을 등록할 때 등록에 필요한 유닛을 지정한다. 해당 유닛을 등록하기 위한 종속성 검사 단계로 볼 수 있다. 따라서 해당 설정은 [Unit] 섹션의 "Wants=" 와 "Requires=" 옵션과 관계 있다.

- `Also=`

  "systemctl enable"과 "systemctl disable"로 유닛을 등록하거나 해제할 때, 여기에 지정된 다른 유닛도 같이 등록, 해제하도록 할 수 있다.



##### 출처

- https://potatogim.net/wiki/Systemctl
- http://victorydntmd.tistory.com/215
- http://fmd1225.tistory.com/93
- http://shoaly.tistory.com/46
- http://gafani.tistory.com/entry/CentOS7-systemd-%EC%97%90-%EC%84%9C%EB%B9%84%EC%8A%A4-%EB%93%B1%EB%A1%9D%ED%95%98%EA%B8%B0
