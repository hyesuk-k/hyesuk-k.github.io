---
title:  "chrome 확장 프로그램 offline 설치"
excerpt: CRX Extractor / Downloader 사용

categories:
  - 'etc'
tags:
  - chrome
  - crx
  - etc

toc: true
toc_sticky: true

date: 2021-10-26
last_modified_at: 2021-10-26

---

# 준비물

* online 접속 가능한 chrome
  + [CRX Extractor/Downloader](https://chrome.google.com/webstore/detail/crx-extractordownloader/ajkhmmldknmfjnmeedkbkkojgobmljda)
  + 설치하고 싶은 확장 프로그램
    - ex) [OneTab](https://chrome.google.com/webstore/detail/onetab/chphlpgkkbolifaimnlloiipkdnihall?hl=ko)

* VPN망에서 접속 가능한 chrome
  + ex) 사내망 / 업무망 / 폐쇄망 기기

# 방법 (using CRX)

1. CRX Extractor 확장 프로그램 설치
2. 확장프로그램 > OneTab 접속
3. 해당 웹 페이지에서 마우스 우클릭으로 메뉴 열기
4. Download CRX -> ZIP으로 선택함 (.crx 로 선택해도 상관 없을 것 같은데...안해봄!)
![Download CRX for this extension]({{"../../assets/img/etc/etc_crx_download.png"}})

5. 다운로드 된 ZIP 파일을 폐쇄망으로 이동 및 압축 해제 (OneTab 폴더에 압축 해제)
6. 폐쇄망 chrome에서 ["확장 프로그램 열기"](chrome://extensions/)
```
chrome://extensions/
```
7. 개발자 모드 켜기
![turn_on_dev_mode_in_chrome]({{"../../assets\img\etc\etc_chrome_dev_mode.png"}})
8. "압축해제된 확장 프로그램을 로드합니다." 선택
![turn_on_dev_mode_in_chrome]({{"../../assets\img\etc\etc_chrome_import_crx.png"}})
9. 압축 해제된 폴더 선택
10. 폐쇄망에 적용 완료
![chrome_extension_in_vpn]({{"../../assets\img\etc\etc_chrome_vpn_extension.png"}})

