---
title:  "vim 설정하기"
excerpt: vimrc 및 tags, cscope 설정

categories:
  - 'dev_utils'
tags:
  - dev_utils
  - vim
  - setup

toc: true
toc_sticky: true

date: 2021-07-07
last_modified_at: 2021-07-10
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

* mkcscope.sh 생성

```
#!/bin/sh

rm -rf cscope.files cscope.files

find . \( -name '*.c' -o -name '*.cpp' -o -name '*.cc' -o -name '*.h' -o -name '*.s' -o -name '*.S' \) -print>cscope.files

cscope -i cscope.files
```

* mkcscope.sh 권한 수정

```
sudo chmod +x mkcscope.sh
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

* [vundle for vimrc 확인](../../assets/files/v_vimrc.xml)

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

