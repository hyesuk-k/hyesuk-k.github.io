---
title:  "씹어먹는 C++ 8일차"
excerpt: 7장 클래스의 상속

categories:
  - 'cpp'
tags:
  - cpp
  - book

toc: true
toc_sticky: true

date: 2021-07-01
last_modified_at: 2021-07-04

---

❗참고 : <https://modoocode.com/135>

# 표준 string 클래스

* c++의 표준 문자열 클래스
* 짧은 문자열의 경우, 동적 메모리를 할당하지 않고 지역변수로 보관
* 문자열을 복사할 때, 실제 데이터 복사가 아닌 원래 문자열을 가리키기만 함

```cpp
#include <iostream>
#include <string>

int main() {
    std::string s = "abc";

    std::cout << s << std::endl;

    return 0;
}
```

* string 클래스의 인자를 const char *로 받는 생성자를 호출한 형태

* c 언어와의 차이
    + 문자열 비교를 strcmp와 같은 별도 함수를 이용하지 않고, "==", "!=" 를 사용함
        - 연산자 오버로딩을 이용
    + 이 외에 크기 비교 ">=", "<=" 등도 제공
    + 이 외에, length, insert, erase, replace 등의 함수를 지원

# 상속 (Inheritance)

* C++에서는 상속을 통해 다른 클래스의 정보를 물려받아 사용 가능

```cpp
class Base {
  std::string s;
public:
  Base() : s("기반") { std::cout << "기반 클래스 " << std::endl; }

  void what() { std::cout << s << std::endl; }
}

class Derived : public Base {
  std::string s;

public:
  Derived() : Base(), s("파생") {
    std::cout << "파생 클래스 " << std::endl;

    what();
    // Base에서 what 을 물려 받았으므로
    // Derived 에서 호출 가능
  }
}
```

* 상속 방법
  + Derived 클래스가 Base 클래스를 public 형식으로 상속 받겠다는 의미

```cpp
class Derived : public Base
```

# protected

* C++ 접근 지시자
  + private
    - 자신만 접근 가능
  + protected
    - 상속 받는 클래스에서는 접근 가능
  + public
    - 모든 클래스에서 접근 가능

* class 를 상속 받을 때의 접근 지시자에 따라 상속 받는 클래스에서 기반 클래스의 멤버들의 실제 동작이 결정됨
  + public 상속 : 기반 클래스의 접근 지시자들에 영향 없이 그대로 동작
  + protected 상속 : 파생 클래스 입장에서 public이 protected로 변경되고, 나머지는 유지
  + private 상속 : 파생 클래스의 모든 접근 지시자가 private로 변경됨

* 예제 : class Derived 가 Base 클래스를 private로 상속받는 경우,

```cpp
#include <iostream>
#include <string>

class Base {
 public:
  std::string parent_string;

  Base() : parent_string("기반") { std::cout << "기반 클래스" << std::endl; }

  void what() { std::cout << parent_string << std::endl; }
};
class Derived : private Base {
  std::string child_string;

 public:
  Derived() : child_string("파생"), Base() {
    std::cout << "파생 클래스" << std::endl;
  }

  void what() { std::cout << child_string << std::endl; }
};
int main() {
  Base p;
  // Base 에서는 parent_string 이 public 이므로
  // 외부에서 당연히 접근 가능하다.
  std::cout << p.parent_string << std::endl;

  Derived c;
  // 반면에 Derived 에서는 parent_string 이
  // (private 상속을 받았기 때문에) private 이
  // 되어서 외부에서 접근이 불가능하다.
  std::cout << c.parent_string << std::endl;

  return 0;
}
```

