---
title:  "씹어먹는 C++ 5일차"
excerpt: 5장 클래스의 키워드

categories:
  - 'cpp'
tags:
  - cpp
  - book

toc: true
toc_sticky: true

date: 2021-06-27
last_modified_at: 2021-06-27

---

❗참고 : <https://modoocode.com/253>

# static

* 클래스 내에 static 함수와 변수가 존재할 수 있음

## static 변수

* 전역변수와 같으나 클래스 하나에만 종속되는 변수
* 어떤 class의 static 멤버 변수는, 객체가 소멸될 때 소멸하지 않고 프로그램이 종료될 때 소멸됨
* static 멤버 변수는 class의 모든 객체가 공유하여, 모든 객체들이 단 한 개의 static 멤버 변수를 사용함

```cpp
#include <iostream>

class Test{
  static int s;
  int a;
  int b;
  const int c;
public:
  Test();
  Test(int a, int b);
  Test(int a, int b, int c);
  
  void show_values();
  ~Test() { s--; }
};

int Test::s= 0;

Test::Test()
  : a(10), b(20), c(30) {
    s++;
}

Test::Test(int a, int b)
  : a(a), b(b), c(30) {
    s++;
}

Test::Test(int a, int b, int c)
  : a(a), b(b), c(c) {
    s++;
}

void Test::show_values() {
  std::cout << a << " | " << b << " | " << c << std::endl;
  std::cout << "ststic value = " << s << std::endl;
}

int main() {
  Test t1;
  t1.show_values();

  Test t2(1, 5);
  t2.show_values();

  Test t3(100, 200, 300);
  t3.show_values();

  return 0;
}

-- 실행 결과 --
10 | 20 | 30
ststic value = 1
1 | 5 | 30
ststic value = 2
100 | 200 | 300
ststic value = 3
```

## static 함수

* 특정 객체에 종속되지 않고 클래스 전체에 딱 1개만 정의
* 멤버 함수는 객체를 만들어 멤버 함수를 호출

```
(객체).(멤버 함수)
```

* static 함수는 객체가 없어도 클래스 자체에서 호출 가능
  + 어떤 객체도 static 함수를 소유하고 있지 않음

```
(클래스)::(static 함수)
```

# this

* 객체 자신을 가리키는 포인터의 역할
* 모든 멤버 함수 내에서는 this 키워드가 정의 되어 있음
* 클래스 내에 정의된 함수 중 this 키워드가 없는 경우는 static 함수 뿐

```cpp
static bool bigger_ten;

Test& Test::func_t(int a) {
  num = a;  // this->num = a;
  if (num > 10)  // if (this->num > 10)
    bigger_ten = true;
  else
    bigger_ten = false;
  
  return *this;
}
```

# const 

* 변수의 값을 바꾸지 않고 읽기만 하는 상수 같은 멤버 함수를 상수 함수로 선언 가능

* 상수 함수를 정의하는 방법

```cpp
(기존의 함수의 정의) const;

const int default_number;

int Test::getNumber() const { return default_number; }
// getNumber 함수는 상수 멤버 함수로 정의됨
```

# explicit

* implicit의 반대말, 명시적
* 위 키워드를 사용하여 생성자를 선언하면, 암시적 변환을 수해하지 않음
* 해당 생성자가 복사 생성자의 형태로도 호출되는 것을 막음

# mutable

* 변경 가능한

```cpp
#include <iostream>

class A {
  mutable int data_;
  int data2_;

 public:
  A(int data) : data_(data) {}
  void DoSomething(int x) const {
    data_ = x;
    // data_ 변수가 mutable로 선언되어 가능
    data2_; = x;
    // 에러 : const 함수 내에서 멤버 변수 값 변경 불가
  }

  void PrintData() const { std::cout << "data: " << data_ << std::endl; }
};

int main() {
  A a(10);
  a.DoSomething(3);
  a.PrintData();
}
```
