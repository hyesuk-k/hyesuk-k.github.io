---
title:  "C++20 Modules"
excerpt: c++20에 추가된 modules 개념 정리 (with gcc)

categories:
  - 'cpp'
tags:
  - cpp
  - modernCpp

toc: true
toc_sticky: true

date: 2022-10-25
last_modified_at: 2022-10-25

---

# 3.2 Module

※ gcc compiler 기준으로 정리

## Contents

- [Module이란?](#module이란)
- [Module이 필요한 이유?](#module이-필요한-이유)
- [Module의 사용](#module의-사용)
- [GCC support](#gcc-support-for-modules)

## Module이란?

* [cppreference_dot_com_modules](https://en.cppreference.com/w/cpp/language/modules)
* 컴파일 시간 단축, 매크로 필요성 제거, 헤더 파일 필요성 제거, 보기 싫은 매크로 편법 제거 등 다양한 장점을 제공
* keyword : module, import, export
    + module : module의 이름 지정
    + import : module을 가져올 때 사용    
    + export : module을 내보낼 때 사용
* compiler 별로 지원 방법이 상이 (.ixx 확장자, .cppm/.cpp 확장자, 별도 확장자는 없으나 -fmodules-ts 옵션을 이용)
* java의 package나 c#의 namespace와 동일한 역할

### Module의 장점

* 모듈은 단 한 번만 도입되며, 비용이 사실상 0이다
* 모듈은 도입하는 순서에 따른 차이가 없다
* 모듈에서는 기호 중복 정의 문제가 거의 발생하지 않는다
* 모듈은 코드의 논리적 구조를 표현하는 데 유리하다
    + 어떤 이름을 모듈 밖으로 노출할 것인지 명시적으로 지정 가능
    + 다수의 모듈을 하나의 모듈로 묶어서 패키지로 제공 가능
* 모듈의 사용으로 소스를 인터페이스와 구현부로 분리할 필요가 없다


## Module이 필요한 이유?

### C++ 프로그램의 빌드 과정

![gcc_build](https://github.com/hyesukk/TIL/blob/main/contents/c++20/img/3-2_make_cpp_to_exe.png?raw=true)

1. Preprocessor : main.cc -> main.i
    + 소스 파일에 있는 #include나 #define 같은 지시자(directive) 또는 지시문들을 처리
    + #include 지시문을 헤더파일의 내용으로 치환
    + #define으로 정의된 부분을 심볼 테이블에 저장하고, 심볼 테이블에 저장된 문자열과 같은 내용을 만나면 치환
    + #if, #else, #elif, #ifdef, #ifndef, #endif 와 같은 지시자를 해석하여 조건에 따라 치환
    + g++ -E 옵션을 이용하여 확인 가능

2. Compiler : main.i -> main.s
    + compiler가 고수준 언어를 저수준 언어로 나타냄
    + 주어진 번역 단위에 담긴 소스 코드를 해석하여 assembly 코드로 변환

3. Assembler : main.s -> main.o
    + assembly 코드를 object code로 변환
    + object file들은 자신이 정의하지 않은 symbol들을 참조할 수 있음
    + linux object file은 5개의 section으로 구성(Text / Data / Symbol Table / Relocation Infomation / Debugging Infomation)

4. link : main.o -> main.exe
    + 실행파일 혹은 정적/공유 라이브러리를 출력
    + object 또는 library 등에 정의된 symbol을 해석하고 재배치
    + symbol 해석 : 각 object file에 있는 symbol들을 어디에 참조시킬지 결정
    + 재배치 : object file에 있는 데이터의 주소나 코드의 메모리 참조 주소를 배치


### 빌드 과정의 문제점

* #include를 반복하여 치환
    + #include 구문을 헤더파일로 치환할 때, 같은 header를 반복해서 치환 가능하다
    + 아래 예제와 같이 header에 <iostream> 헤더가 각각 선언되는 경우, preprocessor에 의해 총 세 번 치환됨

```cpp
// hello.cc

#include "hello.h"
void hello() {
    std::cout << "hello";
}
------------------
// hello.h

#include <iostream>
void hello();
------------------
// world.cc

#include "world.h"
void world() {
    std::cout << "world";
}
------------------
// world.h

#include <iostream>
void world();
------------------
// helloWorld2.cc

#include <iostream>

#include "hello.h"
#include "world.h"

int main() {
    hello();
    world();
    std::cout << "\n";
}
```

* 전처리기 매크로의 위험
    + 매크로를 포함하는 순서에 따라 프로그램의 의미가 달라질 수 있음
    + 매크로가 프로그램의 기존 매크로나 식별자와 충돌할 수 있음
    + include 순서에 따라 RED가 나타내는 값이 달라짐

```cpp
// webcolors.h
#define RED 0xff0000

// productinfo.h
#define RED 0
```

* 기호 중복 정의
    + ODR(One Definition Rule)
        - 임의의 번역 단위에 함수의 정의는 많아야 하나여야 한다
        - 프로그램 안에서 함수의 정의는 많아야 하나여야 한다
    + 중복 정의(multiple definition) : 프로그램이 ODR을 위반하는 것
    + header.h와 header.h를 포함하는 header2.h가 있고, main.cc가 header.h와 header2.h를 모두 포함하는 경우, 중복 정의 오류가 발생
    + c++20 이전의 중복 선언 방지 방법 : #ifndef, #pragma once(주의 필요) 지시자 사용 

```cpp
// header.h
void func() {}

//header2.h
#include "header.h"

// main.cc
#include "header.h"
#include "header2.h"
int main() {}
```

## Module의 사용

* 첫 예제
    + build
        - g++-11 -std=c++20 -fmodules-ts math.cc main.cc -o main
        - g++ 11이상 및 std c++20 이상 지원하므로 명시
        - *-fmodules-ts* 옵션을 이용하여 모듈 기능을 활성화

```cpp
// math.cc
export module math;
export int add(int num1, int num2) {
    return num1 + num2;
}

// main.cc
#include <iostream>
import math;

int main() {
    std::cout << add (2000, 200) << "\n";
    return 0;
}

// gcm.cache : The default mapper generates CMI files in a gcm.cache directory.
$ ls gcm.cache/
math.gcm
```

### GCC Module 옵션

|옵션|설명|비고|
|:---|:---|:---|
|-fmodules-ts|모듈 기능을 활성화 한다| |
|-fmodule-header|헤더 단위를 컴파일 한다| |
|-fmodule-mapper=<값>|모듈 매퍼를 지정한다| [cmi](https://gcc.gnu.org/wiki/cxx-modules#CMI_Location) |
|-fno-module-lazy| 모듈 지연 적재(lazy loading)을 비활성화 한다| |


### Module 내보내기

* 개별 내보내기 : 각각의 이름에 export 키워드를 붙여서 개별적으로 내보내기

```cpp
export module math;
export int mult(int num1, int num2);
export void doTheMath();
```

* 그룹 내보내기 : 여러 이름을 하나의 그룹으로 묶어서 내보내기

```cpp
export module math;

export {
    int mult(int num1, int num2);
    void doTheMath();
}
```

* namespace 내보내기 : namespace에 export를 붙여서 내보내기
    + namespace를 이용한 내보내기의 경우, fully qualified name을 사용하여 특정 이름에 접근해야 한다 (math::doTheMath)
    + 내부 링키지[internal linkage](https://blog.seulgi.kim/2017/08/cpp-linkage.html) 가 아닌 이름만 내보낼 수 있다

```cpp
export module math;

export namespace math {
    int mult(int num1, int num2);
    void doTheMath();
}
```

### Module 구조

```cpp
module; // 전역 모듈 조각, 생략 가능
// module 키워드와 선언 사이에서 컴파일에 필요한 여러 헤더를 포함하는 용도로 사용됨

#include <아직 모듈화되지 않은 라이브러리 헤더들>  // iostream과 같은 library 외에 C header는 모두 미지원

export module math; // 모듈 선언, 여기서부터 번역단위 끝가찌 module purview 영역에 해당된다

import <들여올 모듈>
<내보내지 않을 선언들> // 이 모듈 안에서만 사용할 선언들
// import한 모듈은 모듈 링키지를 가짐
// 내보내지 않을 선언들과 함께 모듈 외부에서는 볼 수 없다

export namespace math {
    <내보낼 선언들>  // 모듈 사용자가 사용 가능한 선언들 
}
```

### 모듈 인터페이스와 모듈 구현 단위

* 모듈이 커지면, module interface unit과 module implementation unit으로 분할

* module interface unit
    + 하나의 모듈에는 하나의 module interface unit만 사용

```cpp
// mathInt.cc
module;

#include <vector>

export module math;  // 모듈 인터페이스 단위에는 모듈 선언을 내보내는 문장이 있어야 함 
export namespace math {
    // math 모듈이 내보낼 이름
    int add(int num1, int num2);
    int getProduct(const std::vector<int>& vec);
}
```

* module implementation unit
    + 하나의 모듈에 여러 개의 모듈 구현 단위가 있을 수 있다

```cpp
// mathImp.cc
module;

#include <numeric>
#include <vector>   // 책에는 없는데...-c옵션으로 컴파일하면 에러나서 추가함

module math;  // module 선언만 있고 export는 붙이지 않음 
namespace math {
    int add(int num1, int num2) {
        return num1 + num2;
    }

    int getProduct(const std::vector<int>& vec) {
        return std::accumulate(vec.begin(), vec.end(), 1, std::multiplies<int>());
    }
}
```

* math 모듈을 사용하는 client

```cpp
// main.cc
#include <iostream>
#include <vector>

import math;

int main() {
    std::cout << "\n";
    std::cout << "math::add(2000, 200): " << math::add(2000, 200) << "\n";
    std::vector<int> myVec{1,2,3,4,5,6,7,8,9,10};
    std::cout << "math::getProduct(myVec) : " << math::getProduct(myVec) << "\n";
    return 0;
}
```

```
g++-11 -std=c++20 -fmodules-ts mathInt.cc -c
g++-11 -std=c++20 -fmodules-ts mathInt.cc mathImp.cc -c
g++-11 -std=c++20 -fmodules-ts main.cc -c
g++-11 -std=c++20 main.o mathInt.o mathImp.o -o main
```

### 하위 모듈과 모듈 분할

* 하위 모듈 (submodule)
    + 하나의 모듈이 다른 여러 모듈을 도입한 후 다시 내보낼 수 있음
    + math < math.math1과 math.math2를 import 후 export
    + 하위 모듈은 독립적으로도 사용 가능

```cpp
//mathModule
export module math;
export import math.math1;
export import math.math2;

// mathModule1
export module math.math1;
export int add(int num1, int num2) {
    return num1 + num2;
}  // 개별 내보내기

// mathModule2
export module math.math2;
export {
    int mul(int num1, int num2) {
        return num1*num2;
    }
}  // 그룹 내보내기

// main.cc
#include <iostream>
import math;
int main() {
    std::cout << "add(3, 4): " << add(3, 4) << "\n";
    std::cout << "mul(3, 4): " << mul(3, 4) << "\n";
    return 0;
}

// main2.cc
#include <iostream>
import math.math1;
int main() {
    std::cout << "add(3, 4): " << add(3, 4) << "\n";
    return 0;
}
```

* 모듈 분할 (module partition)
    + 하나의 분할은 하나의 모듈 인터페이스와 0개 이상의 모듈 구현 단위로 구성
    + 이러한 분할을 하나로 통합하는 모듈 파일을 1차 모듈 인터페이스 단위 또는 인터페이스 파일이라고 부른다
    + 한 분할의 이름은 반드시 1차 모듈 인터페이스 단위가 선언하는 모듈 이름으로 시작한다
    + 분할 모듈과 달리 독립적으로 사용할 수 없다
    + <분할이 속한 모듈 명>:<분할의 이름>

```cpp
// mathPart.cc
export module math;
export import :math1;
export import :math2;

// mathPart1.cc
export module math:math1;
export int add(int num1, int num2) {
    return num1+num2;
}

// mathPart2.cc
export module math:math2;
export {
    int mul(int num1, int num2) {
        return num1*num2;
    }
}

// main.cc
#include <iostream>
import math;
int main() {
    std::cout << "add(3, 4): " << add(3, 4) << "\n";
    std::cout << "mul(3, 4): " << mul(3, 4) << "\n";
    return 0;
}
```

### 모듈 내 템플릿

* 모듈을 미사용할 때 템플릿

```cpp
// temp.h
template <typename T, typename T2>
auto sum(T num1, T2 num2) {
    return num1 + num2;
}

// main.cc
#include <temp.h>  // header file을 직접 include
int main() {
    sum(1, 1.5);  // 템플릿의 인스턴스 화
    return 0;
}
```

* 모듈을 사용할 때 템플릿

```cpp
//mathModule.cc
export module math;
export namespace math {
    template <typename T, typename T2>
    auto sum(T num1, T2 num2) {
        return num1 + num2;
    }
}

// main.cc

#include <iostream>
import math;

int main() {
    std::cout << math::sum(2000, 200) << "\n";  // 2200
    std::cout << math::sum(2013.5, 0.5) << "\n";  // 2014
    std::cout << math::sum(2017, false) << "\n";  // 2017
    return 0;
}

```

### Module Linkage

* 내부 링키지 (internal linkage)
    + C++20 이전에도 지원
    + 번역 단위 밖에서 접근 불가
    + static으로 선언된 namespace 및 anonymous namespace
* 외부 링키지 (external linkage)
    + C++20 이전에도 지원
    + 번역 단위 밖에서 접근 가능
    + static으로 선언되지 않은 이름, 클래스 형식과 멤버, 변수, 템플릿
* 모듈 링키지 (module linkage)
    + C++20에서 module이 도입되면서 추가
    + 해당 모듈 안에서만 접근 가능
    + 모듈 내 선언된 이름 중 외부 링키지가 아니며, 모듈이 내보내지 않은 이름들

```cpp
module;

#include <iostream>
#include <typeinfo>
#include <utility>

export module math;

// namespace 밖에 선언되어 밖에서 호출 안됨 
template <typename T>
auto showType(T&& t) {
    return typeid(std::forward<T>(t)).name();
}

export namespace math {
    template <typename T, typename T2>
    auto sum(T num1, T2 num2) {
        auto res = num1 + num2;
        // 합과 그 합의 형식을 나타내는 문자열
        return std::make_pair(res, showType(res));        
    }
}

// main.cc

#include <iostream>
import math;

int main() {
    auto [val, message] = math::sum(2000, 200);
    std::cout << "val: " << val << ", " << message << "\n";  // 2200, int
    auto [val, message] = math::sum(2013.5, 0.5);
    std::cout << "val: " << val << ", " << message << "\n";  // 2014, double
    auto [val, message] = math::sum(2017, false);
    std::cout << "val: " << val << ", " << message << "\n";  // 2017, int
    return 0;
}
```


### header 단위

* 사용법 : #include 지시문에서 import 지시자로 바꾸고 끝에 세미콜론을 추가
    + include 와 동일한 순서로 헤더를 찾음
        - 현재 디렉토리를 먼저 검색 -> 시스템 경로 검색
    + 컴파일러는 import 지시문으로 부터 모듈 비슷한 것을 생성하여 모듈로 취급
        - 매크로를 포함
* 이러한 헤더 단위는 #include 보다 컴파일 속도가 빠르다

```cpp
#include <vector>       >> import <vector>;
#include "myHeader.h"   >> import "myHeader.h";
```

* header 단위 사용

```cpp
// head.h

#include <iostream>
void hello();

// head.cc
import "head.h";
void hello() {
    std::cout << "Hello World: header units\n";
}

// main.cc
import "head.h";

int main() {
    hello();
    return 0;
}
```

g++-11 -std=c++20 -fmodule-header head.h
g++-11 -std=c++20 -fmodules-ts head.cc main.cc -o main

* 한 가지 단점
    + 모든 헤더를 모듈로 사용할 수는 없음
    + c 헤더들은 도입 할 수 없음  <cstring>은 c의 <string.h>를 감싼것 뿐이므로 사용할 수 없음


## GCC support for modules

### Compiler 별 지원

* [Compiler support for C++20 features](https://en.cppreference.com/w/cpp/compiler_support/20)
![gcc_module_지원](https://github.com/hyesukk/TIL/blob/main/contents/c++20/img/3-2_compiler_support_modules.png?raw=true)

* [GCC](https://gcc.gnu.org/projects/cxx-status.html)
    + [gcc 11 변경 내용](https://gcc.gnu.org/gcc-11/changes.html#cxx)
    + "Modules, Requires -fmodules-ts and some aspects are incomplete. Refer to C++ 20 Status"



# 번외

* 책이 쓰여진 2020년 말 기준, gcc에서는 module 기능을 거의 지원하지 않았음.
* gcc에서 module 기능이 정식으로 포함된 버전은 gcc 11 부터로 27 Apr 2021 에 릴리즈 됨.
* gcc를 이용하여 module 기능 테스트 시, **gcc 버전 확인** 필수

* Ubuntu 20.04에 gcc 11 설치하기

```
 sudo add-apt-repository ppa:ubuntu-toolchain-r/test
 sudo apt-get update
 sudo apt-get install -y gcc-11 g++-11

> 여기까지 수행 후, 기존 버전의 gcc를 그대로 사용하려면 필요할 때, g++-11과 같이 버전을 명시하여 사용
> gcc-11 / g++-11 을 기본 버전으로 사용하려면 아래와 같이 매핑

 sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 110 --slave /usr/bin/g++ g++ /usr/bin/g++-11
 sudo update-alternatives --config gcc

```

* [GNU linker - ld](https://linux.die.net/man/1/ld)
    + lazy loading
        - on-demand loading
        - 당장 필요하지 않은 리소스들은 추후 필요한 시점에 loading
    + lazy binding
        - 함수 호출 시점에 해당 함수의 주소만 shared library에서 알아오는 것
        - 실행 파일의 크기가 작고, 적은 메모리를 사용하여 실행 속도가 빠르다


# refs

* [gcc module](https://gcc.gnu.org/wiki/cxx-modules)
    + [Module Mapper](https://gcc.gnu.org/onlinedocs/gcc/C_002b_002b-Module-Mapper.html)
* [compile에 대한 이해](https://bradbury.tistory.com/226)
* [pragma once로 회피되지 않는 경우](https://teus.me/819)
* [module설명](https://kukuta.tistory.com/389)
* [modern c++](https://www.modernescpp.com/index.php/c-20-module-interface-unit-and-module-implementation-unit)
* [c++20 module guide](https://itnext.io/c-20-modules-complete-guide-ae741ddbae3d)
* [gcc command option-module](https://splichal.eu/scripts/sphinx/gcc/_build/html/gcc-command-options/c%2B%2B-modules.html)