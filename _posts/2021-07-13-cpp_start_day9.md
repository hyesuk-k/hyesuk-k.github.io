---
title:  "씹어먹는 C++ 9일차"
excerpt: 7장 클래스의 상속 (가상 함수와 상속에 관련한 잡다한 내용들)

categories:
  - 'cpp'
tags:
  - cpp
  - book

toc: true
toc_sticky: true

date: 2021-07-13
last_modified_at: 2021-07-13

---

❗참고 : <https://modoocode.com/135>

# 상속에 관련된 잡다한 내용들

* virtual keyword
  + Parent와 Child Class에 virtual void f() 가 선언되어 있는 경우,

```cpp
Parent* p = new Parent();
Parent* c = new Child();

p->f(); // Parent의 f가 호출됨
c->f(); // Child의 f가 호출됨
```

## virtual 소멸자

* 상속 시에, 소멸자는 virtual function으로 선언해야 함
  + 

```cpp
class Parent {
  public:
    Parent() {std::cout << "P 생성자 호출" << std::endl;}
    virtual ~Parent() {std::cout << "P 소멸자 호출" << std::endl;}  // virtual이 아닌 경우,
                                                                    // main의 delete p에서 Parent의 소멸자를 호출
                                                                    // Child 부분 메모리 누수 발생 가능
}

class Child : public Parent {
  public:
    Child() {std::cout << "C 생성자 호출" << std::endl;}
    ~Child() {std::cout << "C 소멸자 호출" << std::endl;}
}

int main () {
  Parent *p = new Child();
  delete p;
}
```

## virtual function table

* 장점
  + virtual function == 가상 함수 이지만 실제 존재하는 함수이고, 정상 호출도 가능
  + 모든 함수를 virtual function으로 생성 시, 모두 dynamic binding을 수행하도록 할 수 있음

* 단점
  + virtual keyword가 붙은 함수 호출의 경우, overhead가 존재

* c++ 컴파일러는 가상 함수가 하나라도 존재하는 클래스에 대해서 가상 함수 테이블을 생성함
  + 가상 함수 테이블 (virtual function table;vtable)
  + virtual function을 호출 시, runtime에 어떤 함수를 수행할 지 선택해야 함

