---
title:  "씹어먹는 C++ 1일차" 
excerpt: 1장 c++ 시작하기

categories:
  - 'cpp'
tags:
  - cpp
  - book

toc: true
toc_sticky: true

date: 2021-06-05
last_modified_at: 2021-06-06

---

❗참고 : <https://modoocode.com/135>

# 첫 c++ 프로그램 분석하기


```cpp
#include <iostream>

int main() {
    std::cout << "Hello, World!!" << std::endl;
    return 0;
} 
```
* c와 다르게 c++은 header include 시 '.h' 가 붙지 않음
  + [참고사이트](https://stackoverflow.com/questions/2799682/c-includes-with-and-without-h)

# namespace

* std : c++ 표준 라이브러리의 모든 함수, 객체 등이 정의되어 있는 namespace
* 어떤 정의된 객체에 대해 어디 소속인지를 지정하는 의미


```cpp
std::cout
```
* std namespace에 정의된 cout임을 의미


```cpp
#include "header1.h"
#include "header2.h'

using header1::foo;

int main() {
  foo();  // header1에 있는 함수를 호출

  return 0;
}
```
* header:: 없이 header1 namespace 내의 foo 함수를 사용하는 방법


```cpp
#include "header1.h"
#include "header2.h"

using namespace header1;

int main() {
    foo();  // header1에 있는 함수를 호출
    bar();  // header1에 있는 함수를 호출

    return 0;
}
```
* "using name space header1
  + 해당 cpp 내에서 "header1::" 없이 해당 header 에 선언된 파일을 사용하는 방법


# 이름 없는 공간


```cpp
#include <iostream>

namespace {
    //이 함수는 이 파일 안에서만 사용 가능
    // static int OnlyInThisFile() 과 동일한 의미
int OnlyInThisFile() {}

// static int x 와 동일
int only_in_this_file = 0;
}  // namespace

int main() {
    OnlyInThisFile();
    only_in_this_file = 3;

    return 0;
}
```
* header file을 통해 include 되더라도 익명의 namespace안에 정의된 모든 것들은 사용할 수 없음


# C++와 C언어의 공통 문법 구조

* 변수 정의
  + 대부분 C와 비슷 (변수의 시작은 숫자가 오면 안됨)
  + [google c++ 변수이름 짓기 Guide](https://google.github.io/styleguide/cppguide.html#Variable_Names)
    - 변수 : 이름 내 띄어쓰기는 _로 구분
    - 함수 : 이름 내 띄어쓰기는 대문자로 구분


```cpp
int arr[10];
int *parr = arr;

int i;
int *pi = &i;
```
* 포인터 : *와 &의 역할은 그대로 유지


* 다른 문법 구조
  + 변수의 선언 위치가 고정되어 있지 않음
  + 아래 문법은 C와 동일함
    - 반복문 (for, while)
    - 조건문 (if-else, switch)
    - 제어문 (break, continue)


```cpp
#include <iostream>

int main()
{
    int win_number = 10;
    std::cout << "숫자를 입력하세요" << std::endl;

    int input_number;

    while (true) {
        std::cout << "정수를 입력하세요 : ";
        std::cin >> input_number;
        if (win_number == input_number) {
            std::cout << "정답입니다" << std::endl;
            break;
        } else {
            std::cout << "다시 정수를 입력하세요." << std::endl;
        }
    }
    return 0;
}
```
* cout은 << 를 이용하여 출력
* cin은 >>를 이용하여 사용자 입력을 출력함
  + scanf 와 다르게 &와 변수 format을 지정해주지 않아도 cin이 알아서 처리함
