---
title:  "씹어먹는 C++ 3일차"
excerpt: 4장 C++의 세계로

categories:
  - 'cpp'
tags:
  - cpp
  - book

toc: true
toc_sticky: true

date: 2021-06-16
last_modified_at: 2021-06-21

---

❗참고 : <https://modoocode.com/135>

# C++의 메모리

* [memory layout in c](https://www.geeksforgeeks.org/memory-layout-of-c-program/)

* Stack
  + 지역 변수가 저장됨
  + 컴파일러에 의해 어느정도 안정성이 보장됨

* Heap
  + 동적 할당 변수
  + 프로그램 실행 시에 자유롭게 할당하고 해제할 수 있음


# 동적 할당

* C에서의 동적 할당
  + malloc/calloc/realloc 함수로 메모리 할당, free ()로 메모리 해제
  + type의 크기를 명시

* C++에서의 동적 할당
  + new와 delete를 이용

```cpp
#include <iostream>

int main() {
  int arr_size;
  std::cout << "array size : ";
  std::cin >> arr_size;

  int *list = new int[array_size];

  for (int i = 0 ; i < arr_size ; a++) {
    std::cin >> list[i];
  }

  for (int i = 0 ; i < arr_size ; i++) {
    std::cout << i << "th element of list : " << list[i] << std::endl;
  }

  delete[] list;

  return 0;
}
```

* c++의 변수 범위
  + 중괄호 안에서 동일한 이름을 가진 변수를 선언 후 사용시 변수의 scope는 괄호 안 까지
  + for 문의 초기식에 선언된 i는 for문 내에서만 사용되면서 소멸함!
    - 이전/이후에 사용되는 int i와 다른 변수임


* C와 C++의 차이 (동적 할당)

<div class="datatable-begin"></div>

| 차이 | C | C++ |
| :--: |---|-----|
| 할당 호출 함수 | malloc, calloc, realloc | new + C 함수들 사용 가능|
| 해제 호출 함수 | free | delete |
| library | stdlib.h include 필요 | C++ 기본 제공 함수 (별도 library include 불필요) |
| 반환 값 | 'void *'를 반환 (형변환 필요) | 할당하려는 data type의 pointer를 반환 (형변환 불필요) |
| 할당과 초기화 | 메모리 동적 할당만 가능 | 할당과 동시에 초기화 가능 (int* ptr = new int(10)  // 10으로 초기화) |
| 할당 exp. | int *ptr = (int*)malloc(sizeof(int)*4); | int *ptr = new int; |
| 해제 exp. | free(ptr) | delete ptr |


<div class="datatable-end"></div>