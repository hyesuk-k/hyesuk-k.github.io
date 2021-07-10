---
title:  "Raspberry Pi 설치"
excerpt: 친구가 준 Raspberyy Pi 설정 기록

categories:
  - 'rpi'
tags:
  - RaspberryPi
  - setup

toc: true
toc_sticky: true

date: 2021-06-15
last_modified_at: 2021-07-10
---

* 참고 사이트
  + <a href="https://www.raspberrypi.org/">raspberrypi official site</a>
  + <a href="https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up">User Guide for setting</a>

# 1. 준비물

* raspberry pi (v2 Model B 1GB) <a href="https://www.raspberrypi.org/products/raspberry-pi-2-model-b/">참고</a>
* raspberry pi power supply
  + Pi 4는 USB-C type
  + Pi 1,2,4은 micro USB <a href="https://www.raspberrypi.org/products/raspberry-pi-universal-power-supply/">Power Supply</a>
* micro SD card (최소 8G 이상)
  + SD card reader 혹은 해당 card를 읽을 수 있는 노트북
* HDMI 케이블 / 모니터 / 키보드 / 마우스
* 선택 : 무선 인터넷 용 <a href="http://prod.danawa.com/info/?pcode=1890786">동글</a>


# 2. Micro SD Card 세팅

* SD card에 raspberry pi용 OS 설치 (이미 설치되어 있는 경우, 해당 단계는 넘기기)

## 2-1. SD Card Fomat (선택)

* SD Card에 데이터가 있는 경우, format 수행

## 2-2. Raspberry Pi OS 설치

* <a herf="https://www.raspberrypi.org/software/">OS Download</a>
![Image-Download Pi OS]({{"/assets/img/rpi/rpi_os_download.PNG"}})
  + 사용중인 OS의 설치 파일을 선택하여 다운로드

* Installer 실행 후, OS/SD Card를 선택
  + OS : RASPBERRY PI OS (32-BIT)
  + SD Card : Raspberry 설치를 위해 연결한 SD Card로 선택
  + Write 버튼 클릭
![Istall Pi OS]({{"/assets/img/rpi/rpi_imager.PNG"}})

* 설치 완료 후 SD Card Reader Card 제거 (약 1시간 정도 소요된 듯)
![Installed OS in SD]({{"/assets/img/rpi/rpi_imager_end.PNG"}})

# 3. Raspberry Pi 주변 기기 연결하기

* SD Card 및 주변 기기들 연결하기
  + 키보드 / 마우스 / 모니터 / 무선 인터넷 동글 (선택) / 
  + <a href="https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up/3"> 그림 참고 </a>

* 전원 어댑터 연결 후 HDMI 선으로 모니터 연결

* "Welcome Raspberry Pi" 화면 확인
  + Raspberry Pi Desktop setup
     - Set Country
     - Change Password (default pwd : raspberry)
     - Monitor set (외곽에 black 이 보이는지? 확인하라는 뜻인듯)
     - Select WiFi Network (skip..., WiFi 이름이 한글이라 목록에 안뜨는것 같음 -> 맞았음)
     - Update Software (skip, WiFi 설정 후 실행)
  + Setup Complete! 후 재시작

# 4. 무선 인터넷 설정 (선택)

* 결론 : 무선 인터넷 이름은 영어+숫자로 설정한다.

* WiFi 이름이 안나와서 삽질...
  + iwconfig 명령어로 무선 장치 조회 : wlan0로 설정되어 있음

```
$iwconfig
```

  + wifi list 검색 : 내 WiFi만 안나옴,ㅠ

```
$sudo iwlist wlan0 scan
```

* 한글 WiFi 이름을 <b>영어+숫자 조합</b>으로 변경 후 설정 성공

* SW Update 수동 실행

```
$ sudo apt update
$ sudo apt upgrade
```

# 5. 한글 설정 (선택)

* 결론 : 한글 폰트와 fcitx를 사용하자..

* 한글 입력을 위한 ibus 설치 및 한글 폰트 설치
  + <a href="https://hangeul.naver.com/font">네이버 나눔</a>

```
$ sudo apt-get install ibus ibus-hangul
$ sudo apt install fonts-nanum fonts-nanuum-coding
```

* nabi 설치

```
$ sudo apt-get install nabi
```

* [한영 전환] 설정하기
  + 설정 -> [기본 설정 / Preferences] -> IBus [환경 설정 / Preferences] 선택
  + 입력 방식 선택 후 추가 (A) 선택
  + 한국어 (Hangul) 추가

* font 변경
  + 설정 -> [기본 설정 / Preferences] -> Appearance Settings 선택
  + System -> Font 선택 후 설치된 한글 Font로 변경 및 적용

>> 여기까지 안됨111

* Locale 변경
  + Language : en / Country : US / Character Set : UTF-8
  + reboot!

>> 안됨2222

* 검색 시도...

```
$ sudp apt install fonts-unfonts-core
```

* Locale 변경
  + Language : ko / Country : KR / Character Set : UTF-8

* ibus 삭제 후 fcitx 설치 (중간에 진짜 삭제할거니? 와 진짜 설치 할거니? 메시지에 y 눌러주기)

```
$ sudo apt remove ibus ibus-hangul
$ sudo apt install fcitx fcitx-hangul
```

* 부팅 시 한글 입력기 자동 실행 설정

```
$ sudo nano /etc/default/im-config

IM_CONFIG_DEFAULT_MODE=fcitx 로 변경 후 저장
```

* 다시 reboot 후 테스트!

>> 성공! fcitx 로 설치해서 사용하자. (ctrl+space 로 한영 변환 가능)


# 6. 원격접속 설정

## 6-1. xrdp를 이용한 Raspberry Pi 원격 접속하기
  
* tightvncserver/xrdp 설치

```
$ sudo apt-get install tightvncserver xrdp
```

* 설정 -> Raspberry Pi Configuration 선택-> Interfaces에서 SSH/VNC Enable

## 6-2. Raspberry Pi에 고정IP 할당하기

* ifconfig로 내 IP 주소 확인하기
* 네트워크 설정 파일 수정 (무선 사용 중이므로 wlan0 참고)

```
$ ifconfig
$ sudo vi/nano /etc/dhcpcd.conf

static ip_address=내 IP 192.168.0.48 / 192.168.0.1
static routers=gw 주소 (맨 마지막을 1로 설정)
저장

$ sudo /etc/init.d/networking restart
```

* reboot!

## 6-3. 원격 데스킅톱 (in Windows)

* 실패함

![Image-Failed remote]({{"/assets/img/rpi/rpi_vnc_fail.PNG"}})

* vnc-server와 xrdp 제거 후 xrdp만 재설치

```
$ sudo apt-get purge realvnc-vnc-server
$ sudo apt-get purge xrdp
$ reboot
$ sudo apt-get install xrdp
```

* tightvncserver 설치 및 vnccserver 비밀번호 생성

```
$ sudo apt-get install -y tightbncserver
$ vncserver
> password 입력
```

* id : pi / pass word : 변경한 비밀번호로 재접속 시도

>> 성공!

# 7. 개발 환경 구축

* vim 설치
* git
* c++ / python 등

```
$ sudo apt-get install vim
$ sudo apt-get install git
$ sudo apt-get install gcc
$ sudo apt-get install python3
```


# 기타

* ? apt 와 apt-get의 차이는? <a href="https://salsa.debian.org/apt-team/apt">참고</a>
  + apt-get : for retrieval of packages and information about them
from authenticated sources and for installation, upgrade and
removal of packages together with their dependencies
  + apt     : is a high-level command-line interface for better interactive usage

* 단축키
  + [ctrl + alt + t] : terminal 열기
  + [ctrl + space] 또는 [한영키] : 한영 변환