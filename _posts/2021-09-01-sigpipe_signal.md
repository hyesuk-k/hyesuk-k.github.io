---
title:  "SIGPIPE Signal"
excerpt: socket programming (exception)

categories:
  - 'network'
tags:
  - network
  - socket
  - signal
  - sigpipe

toc: true
toc_sticky: true

date: 2021-09-01
last_modified_at: 2021-09-01

---

## Server에서 data send 시, SIGPIPE 발생

```cpp
(gdb) c
Continuing.
[Detaching after fork from child process 2274]
[Detaching after fork from child process 2275]
[Detaching after fork from child process 2276]
...

Thread 8 received signal SIGPIPE, Broken pipe.
[Switching to Thread 32437.32573]
0x00007f8b929cba8c in send () from target:/lib64/libpthread.so.0
```

* client에서 server로 통신할 때, 2번 connection 을 수행함
    + 1. connection 수행 후, socket_fd 얻어오면, 해당 socket_fd close
    + 2. 재 connection 수행하여 socket_fd 를 얻어옴
    + 이 때, Server는 connection이 끊긴걸 모름 ...
        - client에서 별도 처리 없음 + 서버에서도 별도 처리 없음
    + server에서 connection 맺었다고 생각되는 fd로 data를 send
        - sigpipe 발생

## SIGPIPE 란? (13 in x86/ARM)

* [signal man page](https://man7.org/linux/man-pages/man7/signal.7.html)
    - Broken pipe: write to pipe with no readers; see pipe(7)

* [signal wiki](https://en.wikipedia.org/wiki/Signal_(IPC))
    - The SIGPIPE signal is sent to a process when it attempts to write to a pipe without a process connected to the other end

* 연결이 종료된 다른 네트워크 소켓/프로세스 등에 send/write를 시도할 때 발생되는 signal

* SIGPIPE 발생 시, 기본 동작은 "프로그램의 종료"
    + [참고](https://www.masterraghu.com/subjects/np/introduction/unix_network_programming_v1.3/ch05lev1sec13.html)
        - The default action of this signal is to terminate the process, so the process must catch the signal to avoid being involuntarily terminated.

## SIGPIPE 처리

* 보통 server에서는 SIGPIPE를 받아도 다운되지 않도록 예외처리를 수행

### CASE 1. SIGPIPE SIGNAL 별도 처리 필요 시,

```cpp
#include <signal.h>

/* Catch Signal Handler functio */
void signal_callback_handler(int signum){

        printf("Caught signal SIGPIPE %d\n",signum);
}

/* Catch Signal Handler SIGPIPE */
signal(SIGPIPE, signal_callback_handler);
```

### CASE 2. SIGPIPE signal IGNORE 처리 시,

```cpp
#include <signal.h>

signal(SIGPIPE, SIG_IGN);
```

### CASE 3. gdb에서도 SIGPIPE 발생 무시

* gdb 실행 시 아래 명령어  실행

```
handle SIGPIPE nostop noprint pass
```

* 계정의 home dir에 .gdbinit 생성 후 위 내용 추가
    + default로 SIGPIPE 무시


## Refs

* http://nomoreid.egloos.com/1169779
    + SIGPIPE 발생 시, 프로그램이 죽어도 코어 없음
* SIGPIPE? : https://kldp.org/node/59225
* IPC : https://mangkyu.tistory.com/9
