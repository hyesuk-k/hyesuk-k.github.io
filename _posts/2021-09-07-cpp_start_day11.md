---
title:  "씹어먹는 C++ 11일차"
excerpt: 9장 C++ 템플릿 (template)

categories:
  - 'cpp'
tags:
  - cpp
  - book
  - template

toc: true
toc_sticky: true

date: 2021-09-07
last_modified_at: 2021-09-07

---

❗참고 : <https://modoocode.com/219>

# C++ Template

* 개발자가 원하는 타입을 넣어주면 알아서 코드를 찍어내는 틀
* generic programming
* 일반적인 코드를 작성하고, 이 코드를 정수나 문자열과 같은 여러 타입에 대하여 재사용 하는 기법

## Class template 정의

```cpp
template <typename T>
class class이름 {
  // template name T를 데이터 타입으로 사용 가능
}
```

## Function template

```cpp
template <typename T>
T functionName(T x, T y) {
  // 함수 구현
}
```


# Class Template 사용

* 정의된 템플릿을 사용하는 방법
  + class template instantiation (템플릿 인스턴스화)
  + class template에 인자를 전달해서 실제 코드를 생성함

```cpp
class이름<type> typeName;  // T에 type이 전달됨
class이름<int> intName; // T에 int가 전달됨
class이름<std::string> stringName; // T에 std::string이 전달됨
```

## Class Template specialization

* 다른 매개변수 사용 등의 특수한 상황에 대해 따로 처리하는 경우

```cpp
template<>  // 전달하는 템플릿 인자가 없더라도 특수화하고 싶은 경우
```

활용 예시, 

```cpp
template <typename A, typename B, typename C>  // class template 정의
class test {};
```

* A가 int고, C가 double인 경우에 대해 따로 처리하고 싶을 때, 
  + A와 C에 대해서는 fix 시키고, B에 대해서는 template 적용

```cpp
template <typename B>
class test<int, B, double> {};
```

* B 도 특수화 하고 싶은 경우,

```cpp
template <>
class test<int, int, double> {};
```

# Function Template 사용

```cpp
#include <iostream>
#include <string>

template <typename T>
T test(T& a, T& b) {
  return a > b ? a : b;
}

int main() {
  int a = 1, b = 2;
  std::cout << test(a, b) << std::endl;

  std::string s = "test", t = "programming";
  std::cout << test(s, t) << std::endl;

  return 0;
}
```

* Func test도 인스턴스화 전에는 compile time에 코드로 변환되지 않음
* test(a, b)가 호출되는 부분에서 인스턴스화 됨
  + test<int>(a, b) 또는 test<string>(s, t)와 같은 형태로 컴파일러가 알아서 변환해 사용함


# template의 활용

## Functor

* 함수 객체 (Fuction Object)
  + 함수는 아니지만 함수인척 하는 객체


## 타입이 아닌 템플릿 인자 (non-type template arguments)

```cpp
#include <iostream>

template <typename T, int num>
T add_num(T t) {
  return t + num;
}

int main() {
  int x = 3;
  std::cout << add_num<int, 5>(x) << std::endl;
  return 0;
}

실행 결과
8
```
* 코드 설명
  + template의 인자로 T를 받고, 추가로 함수의 인자처럼 int num을 추가로 받음
  + 해당 템플릿 인자는 add_num 함수를 호출 시, <> 을 통해 전달함
* 템플릿 인자로 전달할 수 있는 type
  + 정수 (bool, char, int, long)
  + 포인터
  + enum
  + std::nullptr_t (null pointer)
  + float과 double은 제외됨 (C++20 에서 변경됨)

## Default template arguments

```cpp
template <typename T, default args (type name = default value)>
```


```cpp
template <typename T, int num = 5>
T add_num(T t) {
  return t + num;
}

int main() {
  int x = 3;
  std::cout << add_num(x) << std::endl;
  return 0;
}
```

