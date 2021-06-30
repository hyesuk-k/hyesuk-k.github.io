---
title:  "씹어먹는 C++ 7일차"
excerpt: 6장 연산자 오버로딩 - 2

categories:
  - 'cpp'
tags:
  - cpp
  - book

toc: true
toc_sticky: true

date: 2021-06-30
last_modified_at: 2021-06-30

---

❗참고 : <https://modoocode.com/135>

# 잡다한 연산자들의 오버로딩

```cpp
a = a + "-1.1 + i3.923";  // 1
a = "-1.1 + i3.923" + a;  // 2
```

* 1의 경우, a.operator+("i3.923")으로 변환 가능
* 2의 경우, 변환 불가능

* 컴파일런는 이항 연산자 (피연산자를 두 개 취하는 연산자 : +, -, *, /, ->, = 등)를 아래와 같은 두 개의 방식으로 해석함

```cpp
임의의 연산자 @에 대해서, a@b는, 

1) a.operator@(b);  // operator@는 a 클래스의 멤버 변수로 사용됨
2) operator@(a, b);  // 일반적인 함수를 의미함
```

# friend keyword

* 연산자 오버로딩을 위해 사용됨
* 클래스의 멤버 변수에 대한 접근 권한을 줌
* 캡슐화 측면에서는 좋지 않음, setter / getter 함수로 처리
    + 가독성을 위해 사용

* 활용 예제 : 

```cpp
friend 클래스명;
friend 리턴값 함수명(매개변수);
```

```cpp
friend Complex operator+(const Complex& a, const Complex& b);
```

* operator+ 함수는 Complex 클래스의 private / public과 관계 없이 객체의 내부 정보에 접근 가능

* <b>주의</b> : friend 키워드는 단방향으로 접근 가능함
    + 아래 예제에서 A는 B를 friend로 지정
    + 클래스 B는 A의 private 멤버인 x에 접근 가능
    + 클래스 A는 B의 friend가 아니므로 B의 private 멤버인 y에 접근 불가

```cpp
class A {
private:
    int x;

    friend B;
}

class B {
private:
    int y;
}
```

# 정리

* 두 개의 동등한 객체 사이에서의 이항 연산자는 멤버 함수가 아닌 외부 함수로 오버로딩을 추천
    + Complex의 operator+(const Complex&, cconst Complex&)와 같은 형태
* 두 개의 객체 사이의 이항 연산자 이지만 한 객체만 값이 변경되는 등의 동등하지 않은 이항 연산자는 멤버 함수로 오버로딩을 추천
    + operator+=는 이항 연산자 이지만 operator+=(const Complex&)
* 단항 연산자는 멤버 함수로 오버로딩을 추천
    + operator++의 경우 멤버 함수로 오버로딩
* 일부 연산자들은 반드시 멤버 함수로만 오버로딩 할 것!


