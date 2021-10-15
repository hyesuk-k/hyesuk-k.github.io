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
last_modified_at: 2021-10-15

---

# gdb

* [GDB](https://www.gnu.org/software/gdb/)
* [gdb-한글](http://coffeenix.net/doc/develop/gdb-man.html)



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


