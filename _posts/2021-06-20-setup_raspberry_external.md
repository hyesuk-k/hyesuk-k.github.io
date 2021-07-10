---
title:  "Raspberry Pi 외부 접속 설정"
excerpt: 친구가 준 Raspberyy Pi 설정 기록

categories:
  - 'rpi'
tags:
  - RaspberryPi
  - setup

toc: true
toc_sticky: true

date: 2021-06-20
last_modified_at: 2021-07-10
---

# 설정 참고

* ip TIME 사용 기준
* 고정 IP 주소가 할당되어 있는 기준에서 시작 ([Raspberry Pi 설치]({% post_url 2021-06-15-setup_raspberry %}))
* 외부에서는 ssh로만 붙을 수 있게 설정함


# ip TIME 설정

* 192.168.0.1 접속
    + default 계정 정보 : admin / admin

* 외부 접속을 위한 <a href="https://ko.wikipedia.org/wiki/%ED%8F%AC%ED%8A%B8_%ED%8F%AC%EC%9B%8C%EB%94%A9">port forwarding</a>설정하기
    + 같은 인터넷 망 (같은 WiFi 안)에서는 내부 IP를 이용하여 접속 가능
    + 외부에서는 ip TIME과 같은 공유기의 공인 IP를 통해 접속 가능

* [고급설정]-[NAT/라우터 관리]-[포트포워드 설정]에서 새 규칙 추가    
    + 1. 규칙 이름 설정 (포트포워드 사용자정의)
    + 2. 내부 IP 주소 입력 (raspberry pi에서 사용중인 고정 IP 주소 입력)
    + 3. 프로토콜 : TCP
    + 4. 외부 포트 (외부에서 접속할 때 사용할 포트) 설정. 단, 사용중이지 않은 포트로 설정
    + 5. 내부 포트 설정 (ssh 포트 설정)
        - ssh 접속 포트 확인 (주석처리 되어 있는 경우, default 22를 사용)

* SSH Port 정보 참고 (주석'#' 은 기본 port 22 사용중)
```
pi@pi:~ $ cat /etc/ssh/ssh_config | grep -i port
#   Port 22
```

* [기본설정]-[시스템 요약 정보]에서 <b>외부 IP 주소</b> 확인하기
![checkExt]({{"/assets/img/rpi/rpi_ext.PNG"}})

# ssh 접속

* PuTTY 또는 GitBash 등으로 ssh 접속

```
USER@LAPTOP-4LNTS4VO MINGW64 /
$ ssh -p <외부포트> <pi 이름>@<ip TIME 외부 IP 주소>
<pi 이름>@<ip TIME 외부 IP 주소>'s password:
Linux pi 5.10.17-v7+ #1421 SMP Thu May 27 13:59:01 BST 2021 armv7l

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sun Jun 20 16:45:49 2021 from 192.168.0.1
pi@pi:~ $
```
