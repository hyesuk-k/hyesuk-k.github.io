---
title:  "씹어먹는 C++ 10일차"
excerpt: 8장 C++ 표준 입출력 라이브러리

categories:
  - 'cpp'
tags:
  - cpp
  - book

toc: true
toc_sticky: true

date: 2021-07-22
last_modified_at: 2021-07-22

---

❗참고 : <https://modoocode.com/213>

# C++ 입출력 라이브러리

![모두의코드_IOstream](https://modoocode.com/img/180EB4384E4A8EC128AFA3.gif)
* 그림에 대한 상세 설명은 https://modoocode.com/143 를 참고할 것.


## iostream 클래스

* 실제로 입력을 수행하는 클래스
* operator >> 연산자가 정의되어 있음
    + 공백 문자 (띄어쓰기, 엔터, 탭)을 입력시에 무시함
    + 공백 문자에 따라 각각을 분리해서 입력받음

```cpp
std::cin >> a;
```

### ios 클래스의 상태 관리 플래그

* 총 4개의 스트림 상태 관리 플래그가 존재 (0 or 1)
* goodbit : 스트림에 입출력 작업이 가능할 때
* badbit : 스트림에 복구 불가능한 오류가 발생했을 때
* failbit : 스트림에 복구 가능한 오류 발생시
* eofbit : 입력 작업시에 EOF에 도달 시

```cpp
#include <iostream>
#include <string>

int main() {
    int t;
    while (true) {
        std::cin>>t;
        std::cout << "입력 :: "  << t << std::endl;
        if (std::cin.fail()) {  // failbit check
            std::cout << "제대로 입력해주세요." << std::endl;
            std::cin.clear();   // 플래그 초기화
            std::cin.ignore(100, '\n');  // 개행 문자가 나올 때까지 무시 - 최대 100자
        }
        if (t == 0) break;
    }
    return 0;
}
```

## 형식 플래그(format flag)와 조작자(Manipulator)

### 형식 플래그 : std::setf() 활용

```cpp
#include <iostream>
#include <string>

int main() {
    ...
    std::cin.setf(std::ios_base::hex, std::ios_base::basefield);
    ...
}
```

* cin.setf(p1, p2)를 이용하여 입력을 16진수로 받음
    + 인자가 1개인 setf() : 인자로 준 형식 플래그를 적용
    + 인자가 2개인 setf() : p2(basefield)의 내용을 초기화 하고, p1(hex)를 적용

### 조작자 : std::hex를 활용

```cpp
#include <iostream>
#include <string>

int main() {
    ...
    std::cin << std::hex << t;
    ...
}
```

* hex가 cin에서 값을 입력받는 방식을 변경함
* Manipulator : 스트림을 조작하여 입출력 방식을 변경하는 <b>함수</b>
    + std::ios_base& hex(std::ios_base& str);
    + std::endl : 개행 문자 추가 및 buffer flush 수행
        - 문자를 내보낸다고 화면에 출력되지 않음. buffer에 모았다가 한꺼번에 출력함, flush는 버퍼를 비워주는 역할
    
## stream buffer

* 모든 입출력 객체는 이에 대응되는 스트림 객체를 가짐
* C++의 입출력 라이브러리는 이에 대응되는 스트림 버퍼 클래스가 존재함 >> streambuf 클래스
* std::stringstream : 평범한 문자열을 스트림인것처럼 이용할 때 사용함

### streambuf class

* 스트림의 상태를 위해 시작, 다음으로 읽을 문자, 끝을 나타내는 3가지 포인터를 가짐
* 버퍼의 종류에 따라 입력 버퍼 (get area), 출력 버퍼 (put area)을 구분함

# C++ 파일 입출력



