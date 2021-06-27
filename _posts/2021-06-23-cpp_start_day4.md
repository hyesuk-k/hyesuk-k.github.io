---
title:  "씹어먹는 C++ 4일차"
excerpt: 5장 객체지향프로그래밍의 시작

categories:
  - 'cpp'
tags:
  - cpp
  - book

toc: true
toc_sticky: true

date: 2021-06-23
last_modified_at: 2021-06-26

---

❗참고 : <https://modoocode.com/135>

# 객체와 클래스

* 절차지향적 언어
  + C, Pascal
  + 함수(Procedure)를 지향하는 언어
  + 프로그램을 설계할 때 중요한 부분을 하나의 함수로 만들어 쪼개서 처리함

+ 객체지향적 언어
  + C++, Java, Python, C# 등
  + 객체 중심?

## 객체란?

* 함수의 인자로 구조체를 넘겨주기 와 변수에 해당하는 함수를 호출하기

* 객체란 변수들과 참고자료들로 구성된 소프트웨어
  + 자신의 상태를 나타내는 변수
  + 자신의 행동을 수행하는 함수

* 추상화(Abstraction)
  + 객체가 현실 세계에 존재하는 것들을 나타내기 위한 방법
  + 변수 및 함수로 추상화

* 인스턴스 변수와 인스턴스 메소드
  + 객체에 정의된 변수와 함수를 의미
  + 클래스를 이용하여 만들어진 객체

* 캡슐화(Encapsulation)
  + 외부에서 인스턴스 변수를 직접 수정하지 않음
  + 인스턴스 변수는 인스턴스 함수를 이용하여 수정
  + 객체가 내부적으로 어떻게 동작하는지 모르더라도 사용할 수 있도록 함

## 클래스

* 객체의 설계도
* 클래스를 이용하여 실제 객체를 생성

```cpp
<Class 이름> <클래스의 인스턴스 명>
```

* 멤버 변수와 멤버 함수
  + 클래스 안에 선언된 변수와 함수
  + 클래스는 인스턴스가 생성되어야 실재함
  + 멤버 변수와 멤버 함수도 인스턴스가 생성되어야 실재

* 클래스와 접근 지시자
  + 접근 지시자는 private, public 등을 의미
  + 클래스 내에서의 변수와 함수는 별도 접근 지시자가 없을 시, 기본으로 private로 설정됨


# 생성자와 함수의 오버로딩

## 함수의 오버로딩

* C언어에서는 하나의 이름을 가지는 함수는 딱 1개

* C++에서는 하나의 이름을 여러 함수가 사용 가능
  + 함수 호출 시 인자를 이용해 함수들을 구별
  + 함수 호출 시, 함수 이름은 동일하더라도 인자가 다르면 다른 함수


```cpp
#include <iostream>

void print (int x) { std::cout << "int: " << x << std::endl; }
void print (double x) { std::cout << "double: " << x << std::endl; }

int main () {
    int a = 1;
    char b = 'c';
    double c = 3.2f;

    print(a);
    print(b);
    print(c);

    return 0;
}
```

* C++ 컴파일러가 함수를 오버로딩 하는 과정
  + 자신과 타입이 정확하게 일치하는 함수를 찾는다
  + 정확하게 일치하는 함수가 없는 경우, 형변환을 통해 일치하는 함수를 찾는다
  + 포괄적인 형변환 수행 후 일치하는 함수를 찾는다
  + 유저 정의된 타입 변환으로 일치하는 함수를 찾는다

## 생성자

* 객체 생성 시 자동으로 호출되는 함수
* 객체를 생성 후 바로 초기화

```cpp
// 객체를 초기화 하는 역할
// 별도 반환값이 없음

/* 클래스 이름 */ (/* 인자 */) {}
```

* example

```cpp
Date(int year, int month, int day)

Date day(2021, 6, 25);  // 암시적 방법 (implicit)
Date day = Date(2021, 06, 25);  // 명시적 방법 (explicit)
```

### Default constructor

* 생성자를 별도로 정의하지 않더라도 디폴트 생성자가 호출됨
* 인자를 하나도 가지지 않는 생성자
* 클래스에서 개발자가 명시적으로 생성자를 정의하지 않은 경우, 컴파일러가 자동으로 추가해주는 생성자
* 단, 개발자가 별도의 생성자를 추가하는 경우, 컴파일러는 디폴트 생성자를 호출하지 않음

* ※주의사항 : 인자가 없는 생성자를 호출하기 위해서는 "Date day();"가 아닌, "Date day;"로 작성해야 함

* 개발자가 Default constructor를 정의하는 방법 예)

```cpp
#include <iostream>

class Date {
  int year_;
  int month_;
  int day_;

public:
  void ShowDate();

  Date() {
    year_ = 2021;
    month_ = 6;
    day_ = 25;
  }
};

void Date::ShowDate() {
  std::cout << "오늘은 " << year_ << "년 " << month_ << "월 " << day_ << "일 입니다." << std::endl;
}

int main() {
  Date day = Date();
  Date day2;

  day.ShowDate();
  day2.ShowDate();

  return 0;
}
```

* 명시적으로 Default constructor 사용하기
  + C++ 11 이상부터 지원

```cpp
class Test {
  public:
    Test() = default;  // Default Constructor를 정의하라는 의미
}
```

### Constructor overloading

* 생성자 정의 시, 호출 인자에 차이를 주어 오버로딩 가능

```cpp
#include <iostream>

class Date {
  int year_;
  int month_;
  int day_;

public:
  Date() {
    year_ = 2021;
    month_ = 6;
    day_ = 25;
  }

  Date(int year, int month, int day) {
    year_ = year;
    month_ = month;
    day_ = day;
  }
}
```

## 복사 생성자와 소멸자

* new 와 malloc의 차이
  + new로 객체를 동적 할당하는 경우, 객체 생성 후 생성자를 자동 호출

### 소멸자 (Destructor)

* 생성한 객체가 delete될 때, 자동으로 호출되는 함수
* Default destructor도 존재 (아무런 작업 없음)
* 소멸자는 함수의 인자가 없음
* 오버로딩도 되지 않음
* 활용 예
  + 객체 내에서 동적 할당된 메모리 해제
  + 쓰레드 사이의 lock 제어

```cpp
~(클래스의 이름)
```

### 복사 생성자

```cpp
T(const T& a);
```

* 다른 클래스 T의 객체 a를 상수 레퍼런스로 받음
* a가 const이므로 복사 생성자 내부에서는 a의 데이터를 수정 불가, 값의 복사만 가능

* 주의 : 아래 두 가지 코드는 서로 다른 의미를 가짐
  + 생성자는 객체가 생성될 때만 호출됨

```cpp
T t1(1, 2);
T t2(t1);
T t3 = t1;
```

* 복사 생성자가 호출 (t1 -> t2, t3)

```cpp
T t1(1, 2);
T t2;
t2 = t1;
```

* t2에 t1 대입 연산

* C++ 컴파일러는 Default copy constructor를 지원한다
  + 단, Default constructor, Default Destructor와 다르게 '복사'를 수행

### Default copy constructor의 한계

* 얕은 복사 (shallow copy)만 지원
  + 복사된 객체가 같은 동적 할당된 메모리를 가리키는 경우, 소멸자에서 해당 메모리를 delete 하면 문제가 발생됨

* 깊은 복사 (deep copy)가 필요한 경우, 개발자가 따로 복사 생성자를 정의해아함

* 깊은 복사 (deep copy)
  + 객체의 실제 값(value)를 복사
  + 객체를 복사할 때, 해당 객체와 인스턴스 변수까지 복사
  + value를 복사하여 새 주소에 저장하므로 참조가 공유되지 않음

* 얕은 복사 (shallow copy)
  + 객체의 참조값(주소값)을 복사
  + 복사된 인스턴스 변수는 원본 객체의 인스턴스 변수와 같은 메모리 주소를 참조
  + 얕은 복사된 객체의 메모리 주소값이 변경되면 원본 / 복사 객체의 인스턴스 변수 값이 같이 변경됨

## 생성자의 초기화 리스트 (Initializer list)

* 생성자 이름 뒤에 ":" 와 함께 나타남
* 생성자 호출과 동시에 멤버 변수를 초기화 해줌

* 생성자 초기화 리스트 사용

```cpp
(생성자 이름) : var1(arg1), var2(arg2) {}
```

* 사용 예제 )

```cpp
#include <iostream>

class Test {
  int a;
  int b;
  int c;

public:
  Test();
  Test(int a, int b);
  void show_values();
};

Test::Test() : a(10), b(20), c(30) {}

Test::Test(int a, int b)
    : a(a), b(b) {}  // c 값은 설정하지 않음

void Test::show_values() {
  std::cout << a << " | " << b << " | " << c << std::endl;
}

int main() {
  Test t1;
  Test t2(1, 5);

  t1.show_values();
  t2.show_values();
}

-- 실행 결과 --
10 | 20 | 30
1 | 5 | 0
```

* 생성자와 초기화 리스트의 차이
  + 생성자는 생성 후 초기화를 수행 like "int a; a = 10;"
  + 초기화 리스트는 생성과 초기화를 동시에 수행 like "int a = 10;"
  + 초기화 대상이 <b>class</b>라면?
    - 생성자의 경우, 복사 생성자가 호출됨
    - 초기화 리스트의 경우, 디폴트 생성자가 호출된 후 대입을 수행함
  + <b> 레퍼런스와 상수 </b>: 생성과 동시에 초기화 되어야 함
    - class 내부에 레퍼런스 변수나 상수가 포함될 경우, 반드시 초기화 리스트를 사용

* 생성자와 초기화 리스트

```cpp
Test::Test() {
  a = 10;
  b = 20;
  c = 30;
}

Test::Test() : a(10), b(20), c(30) {}
```

* 무조건 초기화 리스트를 사용해야 하는 경우,

```cpp
#include <iostream>

class Test{
  int a;
  int b;
  const int c;

public:
  Test();
  Test(int a, int b);
  Test(int a, int b, int c);

  void show_values();
};

Test::Test()
  : a(10), b(20), c(30) {}

Test::Test(int a, int b)
  : a(a), b(b), c(30) {}  // c가 const 이므로 무조건 초기화가 수행되어야 함 

Test::Test(int a, int b, int c)
  : a(a), b(b), c(c) {}

void Test::show_values() {
  std::cout << a << " | " << b << " | " << c << std::endl;
}

int main() {
  Test t1;
  Test t2(1, 5);
  Test t3(100, 200, 300);

  t1.show_values();
  t2.show_values();
  t3.show_values();
}

-- 실행 결과 --
10 | 20 | 30
1 | 5 | 30
100 | 200 | 300
```

