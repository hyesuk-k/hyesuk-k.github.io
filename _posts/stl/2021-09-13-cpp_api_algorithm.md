---
title:  "C++ Refer Algorithm"
excerpt: <algorithm>에 정의된 function 정리

categories:
  - 'stl'
tags:
  - cpp
  - cpp-api
  - algorithm-header

toc: true
toc_sticky: true

date: 2021-09-13
last_modified_at: 2021-09-13

---

# algorithm


# std::all_of, std::any_of, std::none_of

* 반복자를 사용하여 범위 내에 있는 값이 특정 조건을 만족하는지 확인
* all_of : first ~ last가 모두 true인가?
* any_of : first ~ last 중 하나라도 true인가?
* none_of : first ~ last가 모두 false인가?

## Defined

```cpp
template< class InputIt, class UnaryPredicate >
bool all_of( InputIt first, InputIt last, UnaryPredicate p );

template< class InputIt, class UnaryPredicate >
bool any_of( InputIt first, InputIt last, UnaryPredicate p );

template< class InputIt, class UnaryPredicate >
bool none_of( InputIt first, InputIt last, UnaryPredicate p );
```

## Parameters

* first/last : InputIt(반복자)로 p를 검사할 범위
* p : parameter가 1개인 함수로 bool type 혹은 bool type으로 변환 가능해야 함
* InputIt : must 

## Possible Implementation

```cpp
template< class InputIt, class UnaryPredicate >
constexpr bool all_of(InputIt first, InputIt last, UnaryPredicate p)
{
    return std::find_if_not(first, last, p) == last;
}

template< class InputIt, class UnaryPredicate >
constexpr bool any_of(InputIt first, InputIt last, UnaryPredicate p)
{
    return std::find_if(first, last, p) != last;
}

template< class InputIt, class UnaryPredicate >
constexpr bool none_of(InputIt first, InputIt last, UnaryPredicate p)
{
    return std::find_if(first, last, p) == last;
}
```



# 참고

* [devdocs](https://devdocs.io/cpp-algorithm/)
* [c++ reference](https://en.cppreference.com/w/)
