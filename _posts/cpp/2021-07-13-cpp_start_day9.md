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
last_modified_at: 2021-07-14

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

## Pure virtual function과 Abstract class

### 순수 가상 함수 (pure virtual function)

* 반드시 오버라이딩 되어야만 하는 함수
* 본체가 없기 때문에, 이 함수를 호출하는 것은 불가능 함
* 순수 가상 함수가 포함된 클래스는 객체 생성을 하면 안됨
* format
```cpp
  virtual <return type> <function name>() = 0;
```

* 예시, 

```cpp
#include <iostream>

class Animal {
 public:
  Animal() {}
  virtual ~Animal() {}
  virtual void speak() = 0;  // 반드시 오버라이딩 되어야만 하는 함수
                              // 무엇을 하든지 정의되어 있지 않은 함수
};

class Dog : public Animal {
 public:
  Dog() : Animal() {}
  void speak() override { std::cout << "왈왈" << std::endl; }
};

class Cat : public Animal {
 public:
  Cat() : Animal() {}
  void speak() override { std::cout << "야옹야옹" << std::endl; }
};

int main() {
  Animal* dog = new Dog();
  Animal* cat = new Cat();

  dog->speak();
  cat->speak();
}
```

* pure virtual function을 가진 Animal은 객체 생성을 할 수 없음
  + 아래와 같이 사용하는 것을 방지
  ```cpp
  Animal a;
  a.speak();  // ??
  ```

### 추상 클래스 (abstract class)

* 순수 가상 함수를 최소 한 개 이상 포함하고 있는 클래스
* 반드시 <b>상속</b>되어야 하는 클래스
* 객체를 생성할 수 없음
  + 그러나, 추상 클래스를 가리키는 포인터는 생성 가능
* 인스턴스화 시키기 위해, 해당 클래스를 상속 받는 클래스를 만들어서 순수 가상 함수를 오버라이딩

* 추상 클래스 사용 목적
  + "설계도" 목적으로 사용됨
  + 추상 클래스 자체로는 인스턴스화 시킬 수 없음
  + 반드시 다른 누군가 상속받아 오버라이딩 해야함

* 추상 클래스를 가리키는 포인터
```cpp
Animal* dog = new Dog();
Animal* cat = new Cat();

dog->speak();
cat->speak();
```

# 다중 상속 (multiple inheritance)

* 한 클래스가 다른 여러 개의 클래스를 상속 받는 것

```cpp
class A {
 public:
  int a;
};

class B {
 public:
  int b;
};

class C : public A, public B {
 public:
  int c;
};
```

* ? : 생성자들의 호출 순서
  + 상속 순서에 따라 생성자가 호출됨
  + 위 예시에서는 class C : public A, public B 이므로 A->B->C 순서대로 호출됨
  + class C : public B, public A 라면, B->A->C 순서대로 호출됨

## 다중 상속 시, 주의점

* 부모 클래스에 동일한 이름의 멤버 변수/함수가 있는 경우, 자식 클래스는 둘을 구별할 수 없음
* 다이아몬드 상속 (diamond inheritance)
  + 손자 클래스?

```cpp
#include <iostream>

class CommonP {
public:
  int number;
};
class D : public CommonP {
  
};
class M : public CommonP{

};
class C : public D, public M{

};
```

![diamond_inheritance]({{"/assets/img/cpp/diamond_inheritance.png"}})

* 문제점? 
  + CommonP 클래스에 number라는 멤버 변수가 있으므로, D와 M 모두 number이라는 멤버 변수가 있음
  + C는 D와 M을 다중 상속 받았으므로, number 멤버 변수를 구별할 수 없게됨

* 해결 방법
  + D 와 M 클래스가 CommonP 클래스를 상속받을 때, virtual로 상속받음
  + C에서 다중 상속을 하더라도, CommonP 클래스를 한 번만 포함하도록 지정함
    - C에서는 D, M, CommonP의 생성자를 모두 호출함

```cpp
#include <iostream>

class CommonP {
public:
  int number;
};
class D : public virtual CommonP {
  
};
class M : public virtual CommonP{

};
class C : public D, public M{

};
```

# 참고

* [다중 상속 가이드라인](https://isocpp.org/wiki/faq/multiple-inheritance#virtual-inheritance-where)

