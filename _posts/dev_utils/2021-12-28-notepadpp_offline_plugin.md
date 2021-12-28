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
last_modified_at: 2021-12-28
---

* 1-1. plugin [다운로드](https://sourceforge.net/projects/npp-plugins/)
* 1-2. dll 파일 확인
    + ex: C:\Program Files\Notepad++\plugins\NPPJSONViewer > Notepad++ 설치 위치에 따라 달라질 수 있음
    + NPPJSONViewer.dll 확인
* 2. 압축해제하기 혹은 dll 파일 복사
* 3. Notepad++ 에 추가하기 (관리자 권한으로 실행해야 함)

![import_plugin]({{"/assets/img/tool/notepadpp_import_plugin.png"}})

* 4. Notepad++ 재시작

EX) [Json Viewer](https://github.com/kapilratnani/JSON-Viewer)

