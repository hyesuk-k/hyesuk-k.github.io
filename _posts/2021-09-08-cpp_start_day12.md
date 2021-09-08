---
title:  "씹어먹는 C++ 12일차"
excerpt: 9-2장 C++ Template Meta Programming

categories:
  - 'cpp'
tags:
  - cpp
  - book
  - template

toc: true
toc_sticky: true

date: 2021-09-08
last_modified_at: 2021-09-08

---

❗참고 : <https://modoocode.com/221> <https://modoocode.com/222>

# Template Meta Programming (TMP)

* type은 어떠한 객체에 무엇을 저장하느냐를 지정하기 위해 사용했었음
* template을 사용하면, 객체를 생성하지 않더라도 type에 값을 부여할 수 있고 그 type을 이용하여 연산을 할 수 있음
* type은 반드시 compile time에 확정되어야 하므로, compile time에 연산이 종료됨
* Meta Programming : type을 가지고 compile time에 생성되는 코드로 프로그래밍 하는 것
  + C++의 경우, template을 활용하므로 TMP라고 부름

* TMP의 활용

```cpp
template <int N>
struct Factorial {
  static const int result = N * Factorial<N - 1>::result;  // 재귀함수와 같은 역할
}

template <>
struct Factorial<1> {  // template의 특수화를 활용하여, n이 1일 때를 따로 처리해줌
  static const int result = 1;
};

int main() {
  std::cout << Factorial<10>::result << std::endl;
  return 0;
}
```

* static
  + 위치에 상관 없이 전역으로 메모리에 할당되어 있음
  + 프로그램이 처음 시작될 때 초기화되는 메모리 영역
  + 함수 내에 선언되는 경우, 함수 내부에서의 접근으로 제한
    - life cycle은 프로그램의 시작부터 끝까지, 즉 함수를 빠져나가도 없어지지 않고 남아있음
* const
  + 해당 변수를 초기화한 이후에는 값을 바꾸지 못하도록 함
  + 함수 내에서 초기화되는 경우, 함수를 실행할 때마다 초기화를 수행함
  + 객체에 사용되는 경우 (객체의 상수화), 객체의 데이터를 변환하지 않음을 의미
* static const
  + 고정해놓고 함수 내에서만 사용할 상수를 선언할 용도로 사용함


# auto keywoard

* [추가 참고](https://devdocs.io/cpp/keyword/auto)
* c++ 11에서 지원
* 선언된 변수의 type을 추론하도록 컴파일러에게 지시함
* 변수의 type이 자동으로 결정됨 (type inference)


```cpp
auto 변수명
```


* 활용 예시
  + template으로 인해 복잡해진 코드를 정리할 때 활용
  + const, const& 과 같은 한정자와 함께 활용
  + [range-base-loop](https://en.cppreference.com/w/cpp/language/range-for)

```cpp
auto iNum = 5;
auto dNum = 5.0;

int add(int a, int b) {
  return a+b;
}
auto sumNum = add(5,5);  // add()가 int를 return 하므로 sumNum은 int가 됨


const auto autoNum = 1;

vector<int> v = {1,2,3,4,5};
// auto는 vector<int>::iterator 대신 사용 가능
for (auto it = v.begin(); it != v.end(); it++) {
  *it *= 2;
}
```




