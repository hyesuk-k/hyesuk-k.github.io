---
title:  "GDB 설정하기"
excerpt: gdbinit

categories:
  - 'dev_utils'
tags:
  - dev_utils
  - setup
  - gdb

toc: true
toc_sticky: true

date: 2021-10-15
last_modified_at: 2021-10-28

---

# gdb

* [GDB](https://www.gnu.org/software/gdb/)
* [gdb-한글](http://coffeenix.net/doc/develop/gdb-man.html)
* gcc / g++ 의 경우, 컴파일 시 __-g__ 옵션이 필요

## gdb 설치

* 필요 시,

```cpp
sudo apt-get install -y gdb
```

## gdb 실행하기

```
gdb
gdb -q -s <symbol-file> `pidof <process-name>`
gdb -q <binary>
gdb -q <binary> <core-file>
gdb -q <binary> <process-id>
gdb -p <process-id>
```

## gdb command 옵션

```
gdb [-help] [-nh] [-nx] [-q] [-batch] [-cd=dir] [-f] [-b bps] [-tty=dev] [-s symfile] [-e prog] [-se prog] [-c core] [-p procID] [-x cmds] [-d dir] [prog|prog procID|prog core]
```
* -nx
  + .gdbinit에서 명령어 실행하지 않음

* -q
  + quiet 옵션
  + gdb version과 설정 등등에 대한 출력 없이 바로 gdb가 실행됨

* -batch
  + 배치모드로 실행
  + -x로 지정된 파일을 처리

* -cd=dir
  + 현재 dir 대신 지정한 dir 위치를 사용함

* -f
  + fullname

* -b bps
  + 원격 디버깅에서 사용
  + 시리얼 인터페이스의 line speed 설정

* -tty=dev
  + 프로그램의 표준 입력과 출력을 위해 dev를 사용함

# gdbinit

* gdb 실행 시, 여러 설정을 저장할 수 있는 file
* home dir 하위에 .gdbinit file 생성

```cpp
vi ~/.gitinit
```

## 유용한 gdb 설정들

```
# gdb를 intel CPU 문법으로 설정, AT&T 방식으로 분석하려면 intel 대신 at로 변경
set disassembly-flavor intel

# binary file의 메모리 크기를 max size 로 변경
set max-value-size unlimited

# Thread 메시지 출력하지 않도록 설정 (start / end)
set print thread-events off

# sigpipe signal 발생 시, 멈추지 않고 진행
# print 도 싫으면 nostop noprint 로 변경, 다른 signal도 동일하게 처리 가능
handle SIGPIPE nostop pass pass

# full string 출력
set print elements 0

```


# gdbserver

* Remote debugging
* [GDB_Server](https://sourceware.org/gdb/current/onlinedocs/gdb/Server.html#Server)

## gdbserver 사용법

* target system에 gdbserver 설치

```
sudo apt-get install gdbserver
```

* gdbserver 실행

```
gdbserver localhost:<listening port> <binary>
```

* src 위치에서 gdb 실행

```
gdb
```

* target 접속 (2가지)

```
(gdb) target remote <ip>:<listening port>

OR

ssh ternnuling을 이용한 port-forward
-> target에 ssh로 직접 연결 불가, 중간에 다른 장비를 거쳐가야하는 경우

$ ssh -J <중간장비>@<중간장비ip> -L <port>:localhost:<port> <target_name>@<target ip>

(gdb) target extended_remote :<port>
```

# GDB TUI

* [TextUserInterface](https://sourceware.org/gdb/current/onlinedocs/gdb/TUI.html#TUI)
* gdb에서 source 확인 가능


## 사용방법

### gdb 실행 시 옵션으로 수행

```
$ gdb -q <execute file> -tui
```

### gdb 실행 중 단축키

* Ctrl + x + a
  - turn on / off 둘 다 가능


