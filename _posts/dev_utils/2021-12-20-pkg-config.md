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
last_modified_at: 2021-12-22
---

* Linux 계열에서의 사용 기준 (ubuntu 20.04)

# pkg-config란?

* 라이브러리에 대한 메타정보 조회
* 소스 코드에서 소프트웨어를 compile할 목적으로 설치된 library들을 조회하기 위해, 통일된 인터페이스를 제공
* 설치된 라이브러리에 대해 다양한 정보를 출력한다.
  + C/C++ 컴파일러를 위한 매개변수
  + Linker를 위한 매개변수
  + 패키지 버전
* gcc 혹은 g++ compile 시 flags들을 저장해둔 파일

# pkg-config 간단 사용법

## pkg-config 확인 방법

* ex) openssl

```
./config -fPIC --prefix=/usr/local/OpenSSL
make
make install
```

* pc file 내용

```
vi /usr/local/OpenSSL/lib/pkgconfig/openssl.pc

prefix=/usr/local/OpenSSL
exec_prefix=${prefix}
libdir=${exec_prefix}/lib
includedir=${prefix}/include

Name: OpenSSL
Description: Secure Socket Layer and cryptography libraries and tools
Version: 1.1.1l
Requires: libssl libcrypto
```


## pkg-config 실행 방법

* cflags 옵션과 library 이름 넣기

```cpp
pkg-config --cflags <library 이름>
```


# Issues

* make install 이후 pkg-config에서 인식되지 않을 때, (혹은 바뀌지 않는 경우)
* pkg-config의 default 위치 (/usr/lib)와 설치할 library 의 pkg-config의 default 위치가 상이한지 확인


```
cp /usr/local/lib/pkgconfig/*.pc /usr/local/pkgconfig

> /usr/lib/pkgconfig 디렉토리에 *.pc 파일이 있는지 확인
> open source에서 /usr/local/lib/pkgconfig 디렉토리 내 .pc 파일이 있다면 복사할 것
```


# ref

* [pkg-config](https://www.freedesktop.org/wiki/Software/pkg-config/)
* [pkg-config man page](https://linux.die.net/man/1/pkg-config)
