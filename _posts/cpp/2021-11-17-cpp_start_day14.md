---
title:  "씹어먹는 C++ 14일차"
excerpt: 10장 C++ STL - set/map + unordered

categories:
  - 'cpp'
tags:
  - cpp
  - book
  - STL

toc: true
toc_sticky: true

date: 2021-11-17
last_modified_at: 2021-11-17

---

❗참고 : <https://modoocode.com/224>

# C++ 표준 템플릿 라이브러리 (Standard Template Library, STL)

* 표준 STL은 std namespace에 존재함
* STL = container + iterator + algorithm
* 아래의 세 가지 라이브러리를 의미
  + 컨테이너(container) : 임의 타입의 객체를 보관
  + 반복자(iterator) : 컨테이너에 보관된 원소에 접근
  + 알고리즘(algorithm) : 반복자들을 가지고 일련의 작업을 수행

## Sequence container

* Like array
* 객체를 순차적으로 보관
* vector, list, deque

## Associative container

* key를 이용하여 value를 찾아줌
* key-value 구조를 가짐
* template library 이므로 key와 value가 임의 type의 객체가 될 수 있음

# set / multiset

* 특정 key가 연관 컨테이너에 존재하는지 유무
* set : 중복된 원소가 존재하지 않음
* multiset : 중복된 원소가 존재할 수 있음

## set

* 원소를 추가/삭제하는 작업의 시간 복잡도 : O(logN)
* 원소를 찾는(find) 작업의 시간 복잡도 : O(logN)
  + vector의 find는 O(N)

```cpp
#include <iostream>
#include <set>

template <typename T>
void print_set(std::set<T>& s) {
  // print all date in set
  std::cout << "[" ;
  // for (typename std::set<T>::iterator it = s.begin() ; it != s.end(); ++it) {
  for (auto it = s.begin() ; it != s.end(); ++it) {
    std::cout << *it << " ";
  }
  std::cout << " ] " << std::endl;
}

int main() {
  std::set<int> s;
  s.insert(10);
  s.insert(20);
  s.insert(50);
  s.insert(30);

  std::cout << "순서대로 정렬됨 " << std::endl;
  print_set(s);

  int findNum = 20;
  auto it = s.find(findNum);
  if (it != s.end()) {
    std::cout << findNum << " is in set" << std::endl;
  } else {
    std::cout << findNum << " isn't in set" << std::endl;
  }

  return 0;
}

```


# map / multimap

* 특정 key가 존재한다면 이에 대응되는 값이 무엇인지 질의
  + set보다 사용하는 메모리가 큼
  + key의 존재 유무만 궁금하다면 set 사용을 추천
* map : 중복된 원소가 존재하지 않음
* multimap : 중복된 원소가 존재할 수 있음

## map

* key와 key에 대응되는 value를 같이 보관함 
* 원소를 추가/삭제하는 작업의 시간 복잡도 : O(logN)
* 원소를 찾는(find) 작업의 시간 복잡도 : O(logN)

```cpp
#include <iostream>
#include <map>
#include <string>

template <typename K, typename V>
void print_map(std::map<K, V>& m) {
  // 맵의 모든 원소들을 출력하기
  for (auto it = m.begin(); it != m.end(); ++it) {
    std::cout << it->first << " " << it->second << std::endl;
  }
}

int main() {
  std::map<std::string, double> mlist;  // 2022 공휴일
  
  mlist.insert(std::pair<std::string, double>("신정", 1.1));  // 시작부터 토요일..
  mlist.insert(std::pair<std::string, double>("설날", 2.1));
  mlist.insert(std::pair<std::string, double>("31절", 3.1));
  mlist.insert(std::pair<std::string, double>("근로자의날", 5.1));  // 일ㅠ
  mlist.insert(std::pair<std::string, double>("어린이날", 5.5));
  mlist.insert(std::pair<std::string, double>("부처님오신날", 5.8));  // 일ㅠㅠ

  // 타입을 지정하지 않아도 간단히 std::make_pair 함수로 객체 생성 가능
  mlist.insert(std::make_pair("현충일", 6.6));
  mlist.insert(std::make_pair("광복절", 8.15));
  mlist.insert(std::make_pair("추석", 9.10));  // 토...!ㅠㅠ

  // 혹은 insert 를 안쓰더라도 [] 로 바로 원소를 추가
  mlist["개천절"] = 10.3;
  mlist["한글날"] = 10.9;  // 일ㅠㅠㅠㅠ
  mlist["크리스마스"] = 12.25;  // 일...잃은 공휴일 6일.....ㅂㅇ...

  print_map(mlist);

  std::cout << "key의 value는? " << mlist["설날"] << std::endl;
}
```
