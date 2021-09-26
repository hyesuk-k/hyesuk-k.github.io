---
title:  "씹어먹는 C++ 13일차"
excerpt: 10장 C++ STL

categories:
  - 'cpp'
tags:
  - cpp
  - book
  - STL

toc: true
toc_sticky: true

date: 2021-09-26
last_modified_at: 2021-09-26

---

❗참고 : <https://modoocode.com/223>

# C++ 표준 템플릿 라이브러리 (Standard Template Library, STL)

* 표준 STL은 std namespace에 존재함
* STL = container + iterator + algorithm
* 아래의 세 가지 라이브러리를 의미
  + 컨테이너(container) : 임의 타입의 객체를 보관
  + 반복자(iterator) : 컨테이너에 보관된 원소에 접근
  + 알고리즘(algorithm) : 반복자들을 가지고 일련의 작업을 수행

# Container - std::vector

## Sequence container

* Like array
* 객체를 순차적으로 보관
* vector, list, deque

## Associative container

* key를 이용하여 value를 찾아줌

# Iterator

* Like container의 원소에 접근할 수 있는 포인터
* iterator 멤버 타입으로 정의되어 있음

## 일반적인 사용 예 (using vector)

* ex) **vector** 의 경우, 반복자를 얻기 위해 **begin()** 함수와 **end()** 함수를 사용함

```cpp
#include <iostream>
#include <vector>

int main() {
  std::vector<int> v;
  v.push_back(10);
  v.push_back(20);
  v.push_back(30);

  for(std::vector<int>::iterator itr = v.begin() ; itr != v.end() ; ++itr) {
    std::cout << *itr << std::endl;  // 10\n20\n30
  }

  std::vector<int>::iterator itr = v.begin() + 2;
  std::cout << "3번째 원소 : " << *itr << std::endl;  // 30

  return 0;
}
```

## 범위 기반 for문 (range based for loop)

* C++11 부터 지원

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {
  vector<int> v;

  v.push_back(10);
  v.push_back(20);
  v.push_back(30);

  // range-based for
  for (int elem : v) {
    cout << " 원소 : " << elem << endl;
    // LIKE elem = v[i];
  }

  return 0;
}
```

# Container - std::list

* 양방향 연결구조를 가진 자료형
* **vector** 와 달리 임의의 위치에 있는 원소에 바로 접근할 수 없음
  + 시작 원소와 마지막 원소의 위치만 기억하고 있음
  + **[]** 와 **at** 함수가 정의되어 있지 않음 (특정 index를 이용해 위치를 찾는)
* **vector** 에 비해 임의의 위치에 빠르게 데이터를 추가/삭제 가능

```cpp
#include <iostream>
#include <list>

int main() {
  std::list<int> lst;

  lst.push_back(1);
  lst.push_back(2);
  lst.push_back(3);

  for (std::list<int>::iterator itr = lst.begin() ; itr != lst.end() ; ++itr) {
    std::cout << *itr << std::endl;
  }
  return 0;
}
```

* **vector**와 다르게 _itr+x_ 연산은 할 수 없음
* 아래 연산만 가능
  + itr++  // itr ++
  + itr--  // --itr

# std::deque (deque, double ended queue)

* **vector** 와 비슷하게 임의의 위치에 있는 원소에 접근 가능
* **vector** 와 다르게
  + 맨 뒤 혹은 맨 앞에 원소를 추가/제거 시에 O(1)로 수행 가능
  + 원소들이 메모리 상에서 연속적으로 존재하지 않음
    - 이로 인해, 실제 데이터가 어디에 저장되었는지를 위한 추가적인 메모리가 필요함
    - libc++ 라이브러리의 경우, 1개 원소 보관 시 추가되는 비용이 8배 (!)

```cpp
#include <iostream>
#include <deque>

template <typename T>
void printDq(std::deque<T>& dq) {
  std::cout << "[";
  for (const auto& elem : dq) {
    std::cout << elem << " ";
  }
  std::cout << "]" << std::endl;
}

int main() {
  std::deque<int> dq;

  dq.push_back(1);
  dq.push_back(2);
  dq.push_pront(3);

  std::cout << "초기 dq 상태" << std::endl;
  printDq(dq);
  // 3 1 2

  std::cout << "맨 앞의 원소 제거" << std::endl;
  dq.pop_front();
  printDq(dq);
  // 1 2
  
  return 0;
}
```

# 사용 예시

* 상황에 따라 다른 라이브러리를 활용하지만 주로 고려해야할 점

* 일반적인 상황에서는 보통 **vector** 를 사용
* 중간에 원소를 추가하거나 제거하는 작업이 많고, 원소를 순차적으로 접근하면 **list** 를 사용
* 맨 처음과 맨 마지막에 원소를 추가/삭제하는 작업이 많은 경우 **deque** 를 사용

