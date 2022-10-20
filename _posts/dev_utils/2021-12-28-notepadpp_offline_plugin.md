---
title:  "notepad++ plugin install (offline)"
excerpt: 업무망 notepad++에 plugin install 하기

categories:
  - 'dev_utils'
tags:
  - dev_utils
  - notepad_pp

toc: true
toc_sticky: true1

date: 2021-12-28
last_modified_at: 2022-10-20
---

h1. NPP에 수동으로 플러그인 설치

* 1. plugin [다운로드 참고](https://sourceforge.net/projects/npp-plugins/)
* 2. dll 파일 확인
    + ex: C:\Program Files\Notepad++\plugins\NPPJSONViewer > Notepad++ 설치 위치에 따라 달라질 수 있음
    + NPPJSONViewer.dll 확인
* 3. 압축해제하기 혹은 dll 파일 복사
* 4. Notepad++ 에 추가하기 (관리자 권한으로 실행해야 함)

![import_plugin]({{"/assets/img/tool/notepadpp_import_plugin.png"}})

* 5. Notepad++ 재시작

h1. 유용한 NPP Plugin

* Json Viewer
  + [NPPJsonV Source](https://github.com/kapilratnani/JSON-Viewer)
  + [NPPJsonV release](https://github.com/kapilratnani/JSON-Viewer/releases/)
  + [NPPJsonV Download in Sourceforge](https://sourceforge.net/projects/nppjsonviewer/files/JSONViewer%20Unicode/)

* 문서 비교
  + [Compare-release](https://github.com/pnedev/comparePlus/releases)

* 자동 저장
  + [AutoSave - zip](https://github.com/francostellari/NppPlugins/tree/main/AutoSave)

* hex editor
  + [NPP_HexEdit](https://github.com/chcg/NPP_HexEdit)


* 기타
  + [플러그인]-[플러그인 관리] 에서 repository 주소 확인이 가능하므로 해당 주소에서 수동으로 dll 파일 받아서 사용

h1. 참고

* [Window only]
  + notepad++을 기본 설치 경로인 C:\ 하위에 설치하는 경우, notepad++을 관리자 권한으로 실행해야 플러그인이 설치된다 (directory 생성 후 dll을 이동시키기 때문에..)
