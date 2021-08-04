---
title:  "개발 환경 설정하기"
excerpt: 개발 tools

categories:
  - 'dev_utils'
tags:
  - dev_utils  
  - setup

toc: true
toc_sticky: true

date: 2021-07-30
last_modified_at: 2021-08-03
---

* raspberry pi os format 후 세팅 기념ㅠㅠㅠㅠㅠ
* 필요 시, 추가 및 수정 예정...

# utils

* vim
    + 이전 vim_settings 포스팅 [참고](https://hyesukk.github.io/dev_utils/vim_settings/)
    + colorscheme 변경하기 (default / )
* putty vim에서 방향키 동작 안할 때 (keyboard : VT100+ 설정)
    + vi ~/.exrc
    + 추가 후 저장 : set nocp
* tmux
    + 세션 내 마우스 스크롤 기능
        - default : ctrl + b +[ 및 q
        - custom : ~/.tmux.conf

```
sudo apt-get install -y vim tmux
```

* tmux custom

```
vi ~/.tmux.conf

# 마우스 이용 가능
set -g mouse on
# 마우스 스크롤 사용하기
set -g terminal-overrides 'xterm*:smcup@:rmcup@'
```

# Configuration Management (GIT / TIG / etc...)

* git과 tig 설치

* git 설정

```
sudo apt-get install -y git-all tig

git config --global user.name (hyesukk)
git config --global user.mail
```

* commit editor를 vim으로 변경

```
git config --global core.editor "vim"
```

* git commit message template

```
vi ~/.gitmessage.txt

----
[type] : [category] title

# 상세 내용은 아래에 작성


# ================================================
# git blog / coding test repo는 type 생략
# Type List
# new   : 신규 기능
# fix   : 버그
# style : 공백, 괄호, lint 등 코드랑 상관 없는 수정
# docs  : 문서
# test  : 테스트
# etc   : makefile 수정 / 3rdparty 추가 등
----

git config --global commit.template ~/.gitmessage.txt

```

* tig설정
    + ~/.tigrc 필요 시, 

* ~/.bashrc 설정 추가 (혹은 ~/.bash_aliases)

```
if [ -f ~/bash/git ]; then
  . ~/bash/git
fi
```

* ~/bash/git 내용

```
# Terminal colours (after installing GNU coreutils) 

NM="\[\033[0;38m\]"  # means no background and white lines 
GR="\[\033[0;90m\]"  # dark grey 
HI="\[\033[0;37m\]"  # change this for letter colors 
HII="\[\033[0;31m\]" # change this for letter colors 
SI="\[\033[0;33m\]"  # this is for the current directory 
IN="\[\033[0m\]" 
COLOR_RED="\033[0;31m" 
COLOR_YELLOW="\033[0;33m" 
COLOR_GREEN="\033[0;32m" \
COLOR_OCHRE="\033[38;5;95m" 
COLOR_BLUE="\033[0;34m" 
COLOR_WHITE="\033[0;37m" 
COLOR_RESET="\033[0m" 

parse_git_branch() { 
          git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ \1 /' 
} 

#export PS1="$NM[ $HI\u $HII\h $SI\w$NM ] $IN" 
export PS1="$HII\h $SI\w$GR\$(parse_git_branch)$NM] $IN"
```


# compile / build

* build-essential 설치 (gcc, g++, make, perl 등등)
    + gcc g++ 버전 업 (8.3 이라 변경 안함)

```
sudo apt-get install -y build-essential

dpkg -l 로 설치된 버전 확인 (gcc / g++)

pi@raspberrypi:~ $ gcc --version
gcc (Raspbian 8.3.0-6+rpi1) 8.3.0
Copyright (C) 2018 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

pi@raspberrypi:~ $ dpkg -l | grep gcc
ii  gcc                                    4:8.3.0-1+rpi2                          armhf        GNU C compiler
ii  gcc-4.9-base:armhf                     4.9.4-2+rpi1+b19                        armhf        GCC, the GNU Compiler Collection (base package)
ii  gcc-5-base:armhf                       5.5.0-8                                 armhf        GCC, the GNU Compiler Collection (base package)
ii  gcc-6-base:armhf                       6.5.0-1+rpi1+b1                         armhf        GCC, the GNU Compiler Collection (base package)
ii  gcc-7-base:armhf                       7.3.0-19                                armhf        GCC, the GNU Compiler Collection (base package)
ii  gcc-8                                  8.3.0-6+rpi1                            armhf        GNU C compiler
ii  gcc-8-base:armhf                       8.3.0-6+rpi1                            armhf        GCC, the GNU Compiler Collection (base package)
ii  libgcc-8-dev:armhf                     8.3.0-6+rpi1                            armhf        GCC support library (development files)
ii  libgcc1:armhf                          1:8.3.0-6+rpi1                          armhf        GCC support library

sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 20 
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 40
```


# bash 수정하기

* Workspace 생성하기

```
mkdir Workspace
```

* bash 수정하기

```
vi ~/.bash_aliases

아래 내용 추가하기
---------------------------------------------------------
alias vibash='vi ~/.bash_aliases'
alias reso='source ~/.bashrc'

alias dv='cd ~/Workspace'
---------------------------------------------------------

source ~/.bashrc
```

# font

* 네이버 나눔체 사용 중

```
sudo apt install fonts-nanum fonts-nanum-coding -y
```

# samba

* samba 설치 및 설정

```
sudo apt-get install -y samba
sudo smbpasswd -a <userid>

insert password: 

sudo vi /etc/samba/smb.conf

---
ubuntu 20.04 : 주석 해제 및 read only 설정을 yes -> no로 변경 
pi : 중간에 smb.conf 설정 변경 물어보면서 주석은 해제됨,  read only 는 no로 변경 필요

[homes]
  comment = Home Directories
  browseable = no
  read only = no
  valid users = %s
---

sudo service smbd restart

sudo pdbedit -L -v
추가한 userid 정보 확인
```

* windows + R 키에서 \\ip\\<userid> 로 접속 가능
  + pi에는 userid 떼고 접속 가능
  + window10 > ubuntu20.04 접근 시, 조직의 보안 정책에서 인증되지 않은 게스트 액세스를 차단 ~~~
    - [제어판-프로그램-Windows 기능 켜기/끄기] : smb 관련 전부 체크
    - windows + R 키 : gpedit.msg : 컴퓨터구성 -> 관리 템플릿 -> 네트워크 -> Lanman 워크스테이션 -> 보안되지 않은 게스트 로그온 사용 설정 
    - 재부팅...

# putty

* keyboard 설정 (Function key 등등)
  + echo $TERM 으로 서버 터미널 에뮬 확인
    - xterm : Terminal -> Keyboard -> The Function keys and keypad 에서 Xterm R6설정
  ```
  khs-dl ~/Workspace/~~~ master ] echo $TERM
  xterm
  ```

* backspace key
  + Terminal -> Keyboard -> The Backspace key : Control-H

