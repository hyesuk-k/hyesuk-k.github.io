---
title:  "vim 설정하기"
excerpt: vimrc 및 tags, cscope 설정

categories:
  - 'dev_utils'
tags:
  - dev_utils
  - vim
  - VimVundle
  - cscope
  - ctags

toc: true
toc_sticky: true

date: 2021-07-07
last_modified_at: 2021-11-09
---

* Ubuntu 20.04 LTS 기준으로 설정

# vim 설치

```
sudo apt-get install vim
```

# .vimrc 기본 설정

* vim 설치 후 ~/.vimrc를 생성한 후 사용하고 싶은 옵션을 입력하고 저장

```
vi ~/.vimrc

set <옵션>
```

* 예시

```
syntax on
set mouse=a " use mouse in vim
set ts=4    " tabstop  (1tab to 4 space)
set sw=4    " shift width
set ex      " expandtab (tab to space)
set number  " show line number
set ai      " auto indent
set si      " smart indent
set hls     " highlight search word
set ic      " ignore case
set scs     " smart case (override ic when pattern has upper case chars)
filetype off
```

* vim 옵션 별 설명 : 아무 파일이나 vi로 연 다음 아래 명령어 중 읽기 편한 명령어를 사용

```
:help 'option
:options
:set all
```

# ctags

* [ctags란?](https://ko.wikipedia.org/wiki/Ctags) 

```
1. ctags 설치
sudo apt install ctags

2. source file의 root 경로로 이동 및 tags 파일 생성
ctags -R

3. .vimrc에 아래 내용 추가
set tags=./tags;/
set tagbsearch
```

* 사용 방법
    + ctrl + ] : 원하는 심볼 위에서 누를 것, 해당 위치로 이동
    + ctrl + t : 이전 위치로 돌아가기

# cscope

* [cscope for vim](http://cscope.sourceforge.net/)

* cscope 설치

```
sudo apt install cscope
```

* cscope.files 생성하기 (혹은 다른 이름도 가능 cscope -i <파일명>)
* -name 뒤에 확장자 추가하기
    + gcc에서 지원하는 c++  파일 확장자 들
        - *.c, *.C, *.cc, *.cpp, *.CPP, *.c++, *.cp, *.cxx
        - *.h, *.hpp, *.hh, *.hxx
* -o (OR expression을 의미)
    + [find_명령어_사용법](https://recipes4dev.tistory.com/156)

* mkcscope.sh 생성 및 /usr/bin 으로 이동

```
#!/bin/sh

rm -rf cscope.files cscope.files

find . \( -name '*.c' -o -name '*.cpp' -o -name '*.cc' -o -name '*.h' -o -name '*.hh' -o -name '*.H' \) -print>cscope.files

cscope -i cscope.files

sudo chmod +755 mkcscope.sh
sudo mv mkcscope.sh /usr/bin
```

* ~/.vimrc 수정

```
set nocsverb
if filereadable("cscope.out")
    cs add cscope.out
elseif filereadable("../cscope.out")
    cs add ../cscope.out
" 아래 내용은 필요할 지 확인 필요, else 로 끝낼 수 있나?
elseif filereadable("/usr/~~~/cscope.out")  " os 마다 다름
    cs add /usr/~~~/cscope.out

endif
set csverb
```

* cscope.out 파일 생성
* 파일 이동 (/usr/~~~/로 이동)

## cscope subsequent 

* 0-9a-zA-Z
    - 지정된 줄 번호로 연결된 파일 수정
* <Space>
    - 다음 페이지 확인
* <Tab>
    - 메뉴와 일치하는 줄 목록을 보여줌
* <Up>
    - 이전 항목으로
* <Down>
    - 다음 항목으로


# Vundle

* [vimVundle](https://github.com/VundleVim/Vundle.vim)

* git clone 혹은 zip 파일을 다운로드 받기 (Vundle Vim 설치하기)

```
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

* bundle 디렉토리 하위에 vundle들 추가하기
    + 1. git clone 받아오기
    + 2. vimrc에 plugin 입력 후 설치
        - :PluginSearch NERD
        - Plugin `scrooloose/nerdtree` vimrc 기입 (vundle#begin과 end 사이)
        - :PluginInstall

```
http://vim-taglist.sourceforge.net/

git clone https://github.com/vim-airline/vim-airline.git ~/.vim/bundle/vim-airline

git clone https://github.com/ctrlpvim/ctrlp.vim.git ~/.vim/bundle/ctrlp.vim
```

* 설치한 vundle
    + NERDTree
    + taglist-plus
    + vim-airline
    + [Ctrlp](http://ctrlpvim.github.io/ctrlp.vim/#installation)
    + Syntastic : 문법 확인, 소스 열 때 문법 체크해서 좀 느려짐...관련 설정은 -Wall, Wextra, Wpedantic 으로 필요에 따라 끌 것

* [vundle for vimrc 확인](../../assets/files/v_vimrc.xml)


# YouCompleteMe

* [YouCompleteMe](https://github.com/ycm-core/YouCompleteMe)
* [UsingVundle](https://stackoverflow.com/questions/47956670/how-to-install-plugins-in-vim-using-vundle)

> 이것저것 해보다가 결국 neovim + coc 로 가기로 함

## Vundle을 이용한 세팅 방법

* 위 기본 개발 환경 있음 (g++, vim, cmake, python, Vundle 설치되어 있는 상태)
*  ~/.vimrc 에 Plugin 추가

```
call vundle#begin()
. . .
Plugin 'Yggdroot/indentLine'
Plugin 'Valloric/YouCompleteMe'
. . .
call vundle#end()
```

* vim 열려 있는 상태에서 plugin install 수행

```
:source %
:PluginInstall
```

* plugin install "Done!" 메시지 확인
![DoneVundleYCM]({{"/assets/img/tool/ycm_install.png"}})

## YCM Compile 

* [YouCompleteMe](https://github.com/ycm-core/YouCompleteMe)
* Quick start, installing all completers
* Install YCM plugin via Vundle
* Install cmake, vim and python
* Install mono-complete, go, node, java and npm
* Compile YCM

```
sudo apt-get install -y build-essential cmake vim-nox python3-dev
sudo apt-get install -y mono-complete golang nodejs default-jdk npm

cd ~/.vim/bundle/YouCompleteMe
python3 install.py --all
```

### 하위 버전 vim인 경우, YCM

* gcc 계열 (+libclang) 과 go 언어에 대한 YCM 설치
    + python2 가 없는 에러 발생 시, 3으로 설치
    + 혹은 python3이 없다는 에러 발생 시 2로 설치
    + 둘 다 없다면 ... python 설치하기!

```
$ sudo python3 ./install.py --clang-completer --go-completer
```

### 주의! VIM 버전 check

! [참고](https://chusiang.medium.com/install-the-vim-8-0-and-youcompleteme-with-make-on-centos-7-4-1573ad780953)

* 설치 후 vim으로 문서 열었을 때, 지원하는 최소 vim 버전이 안맞는 경우 발생

```
vi a.cc
YouCompleteMe unavailable: requires Vim 8.1.2269+.

설치된 vim 버전 확인(vim --version 은 8.1까지만 나와서 vim 실행시킴)
8.1.1401
```

* vim upgrade 실행

```
sudo apt-get upgrade vim

sudo add-apt-repository ppa:jonathonf/vim
sudo apt-get update
sudo apt-get install vim
```

* 그러나 raspberri pi는 add-apt-repository 가 없음ㅠ
    + [찾음](https://itsfoss.com/add-apt-repository-command-not-found/)
    + [이후 vim 8.2 설치](https://websetnet.net/vim-8-2-released-how-to-install-vim-in-ubuntu-linux/)

```
sudo apt-get install -y software-properties-common
sudo apt-get update
```


* vim을 수동으로 설치해본다. >> YCM 사용 안할래...

```
git clone https://github.com/vim/vim
./configure
make -jX && make install
```

## raspberry-pi용 VIM

!참고 : https://roboticsbackend.com/install-use-vim-raspberry-pi/

# coc.nvim

* [coc.nvim](https://github.com/neoclide/coc.nvim)
* YCM과 다른 vim 자동 완성 툴

## 설치 준비

### NeoVim / Vim
  
* https://neovim.io/
* 참고
  + [NeoVim for Linux](https://github.com/neovim/neovim/wiki/Installing-Neovim#linux)
  + [NeoVim for RaspberryPi](https://forums.raspberrypi.com/viewtopic.php?t=180325)


* snipd 를 이용하여 nvim 설치

```
sudo apt install -y snapd
sudo snap install --classic nvim
nvim v0.5.0-dirty from neovim-snap (neovim-snap) installed

pi@raspberrypi:~ ()$ nvim --version
NVIM v0.5.0
Build type: RelWithDebInfo
...
```

#### nvim 을 위한 설정 (선택)

* vim을 위한 설정 파일인 ~/.vimrc
* nvim을 위한 설정 파일인 ~/init.vim


### Node.js 설치

* root 권한으로 실행

```cpp
curl -sL install-node.now.sh/lts | bash

root@raspberrypi:/home/pi# curl -sL install-node.now.sh/lts | bash
  Configuration
> Version:  v16.13.0 (resolved from lts)
> Prefix:   /usr/local
> Platform: linux
> Arch:     armv7l

> Tarball URL: https://nodejs.org/dist/v16.13.0/node-v16.13.0-linux-armv7l.tar.gz
? Install Node.js v16.13.0 to /usr/local? [yN] y
> Installing Node.js, please wait…
✓ Done
```


### 설치 언어에 따른 language server 설치

* 참고 : https://github.com/neoclide/coc.nvim/wiki/Language-servers

### yarn 설치

* root 계정으로 실행하기

```cpp
curl --compressed -o- -L https://yarnpkg.com/install.sh | bash
...

> Successfully installed Yarn 1.22.15! Please open another terminal where the `yarn` command will now be available.
```

## 설치

* coc.nvim의 Quick Start 참고
* coc.nvim 의 [Install coc.nvim](https://github.com/neoclide/coc.nvim/wiki/Install-coc.nvim) 참고

* 1. .vimrc 또는 init.vim 에 아래 내용 추가 (vim or nvim)

```
" Use release branch (recommend)
Plug 'neoclide/coc.nvim', {'branch': 'release'}

" Or build from source code by using yarn: https://yarnpkg.com
Plug 'neoclide/coc.nvim', {'branch': 'master', 'do': 'yarn install --frozen-lockfile'}
```

* 2. restart Vim

* 3. :PlugInstall 실행




# grep

* C 개발 환경 : 파일 확장자를 .c와 .h만 사용

```
grep <옵션 : -Iirn> <찾을 문자열> */*.[ch]
```

* C++에서는? cpp, h, cc, hh 등등..

```
grep <옵션> <찾을 문자열> --include=*.{cpp,cc,hh,h}
// -r 옵션 주면 하위 디렉토리 다 찾아줌
혹은
grep <옵션> <찾을 문자열> */*.{cc,h}
// 실행한 위치에서만 찾음
```

## grep 활용

* 특정 문자열을 단어 단위로 검색

```
grep -w "Word" FILE.txt

Word
```

* 문자열이 포함된 파일명을 표기

```
grep -H "aaa" *

a.txt: aaa
b.txt: aaa
c.txt: aaa
```

* 정규식

```
grep -E "a|b|c" *
-> a or b or c 가 포함된 파일을 찾아라
```

* with pipe 

```
ps -ef | grep "dump"
# 현재 프로세스에서 "dump" 문자열을 포함한 라인을 표기

ss -t4l | grep "8080"
netstat | grep "tcp" | grep "8080"

```

