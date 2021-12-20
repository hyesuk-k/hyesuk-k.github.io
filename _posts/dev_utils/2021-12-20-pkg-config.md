---
title:  "pkg-config 설명 및 사용 방법"
excerpt: pkg-config in Unix-like

categories:
  - 'dev_utils'
tags:
  - dev_utils
  - pkg-config

toc: true
toc_sticky: true

date: 2021-12-20
last_modified_at: 2021-12-20
---

* Linux 계열에서의 사용 기준 (ubuntu 20.04)

# pkg-config란?

* 라이브러리에 대한 메타정보 조회
* 소스 코드에서 소프트웨어를 compile할 목적으로 설치된 library들을 조회하기 위해, 통일된 인터페이스를 제공
* 설치된 라이브러리에 대해 다양한 정보를 출력한다.
  + C/C++ 컴파일러를 위한 매개변수
  + Linker를 위한 매개변수
  + 패키지 버전


# pkg-config 간단 사용법


# pkg-config 확인 방법

* ex) openssl

```
./config -fPIC --prefix=/usr/local/OpenSSL111m
```

* 


# ref

* [pkg-config](https://www.freedesktop.org/wiki/Software/pkg-config/)
* [pkg-config man page](https://linux.die.net/man/1/pkg-config)