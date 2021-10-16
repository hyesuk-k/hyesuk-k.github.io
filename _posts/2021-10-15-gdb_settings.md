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
last_modified_at: 2021-10-16

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

# gdbinit

* gdb 실행 시, 여러 설정을 저장할 수 있는 file
* home dir 하위에 .gdbinit file 생성

```cpp
vi ~/.gitinit
```

```
# gdb를 intel CPU 문법으로 설정
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
  - turn on / turn off 둘 다 가능


