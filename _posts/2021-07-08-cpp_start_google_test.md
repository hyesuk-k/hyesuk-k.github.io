---
title:  "google test 사용하기"
excerpt: c++ google test 설치 및 테스트

categories:
  - 'dev_utils'
tags:
  - cpp
  - gtest

toc: true
toc_sticky: true

date: 2021-07-08
last_modified_at: 2021-07-09

---

# google test

* [google test](https://github.com/google/googletest)
* C++ Test를 위해 google에서 만든 Framework

# Install

* Ubuntu 20.04 LTS  / Raspbian GNU/Linux 10 (buster)

* 기반 패키지
  + build tools : cmake, make
    - [bazel](https://bazel.build/) build tool도 지원하지만 cmake만 사용할 예정
  + c++ compiler : gcc, g++

## google test 설치

### apt 설치

```
sudo apt-get install -y libgtest-dev
sudo apt-get install -y cmake
cd /usr/src/gtest
sudo mkdir build
cd build
sudo cmake ..
sudo make
sudo cp *.a /usr/lib
```

* library
  + /usr/lib/libgtest.a
  + /usr/lib/libgtest_main.a
* header
  + /usr/local/include/gtest


### git clone 후 설치

```
git clone https://github.com/google/googletest
cd googletest/googletest
mkdir build
cd build
cmake ..
make
```

# Usage

* 참고 : https://www.eriksmistad.no/getting-started-with-google-test-on-ubuntu/

## simple test code

```cpp
#include <gtest/gtest.h>

// Demonstrate some basic assertions.
TEST(tc_name, t_name) {
//  Expect two strings not to be equal.
    EXPECT_STRNE("hello", "world");
//  Expect equality.
    EXPECT_EQ(7 * 6, 42);
}

int main(int argc, char **argv) {
  ::testing::InitGoogleTest(&argc, argv);
  return RUN_ALL_TESTS();
}
```

## CMakeLists.txt 생성

```
$ vi CMakeLists.txt

cmake_minimum_required(VERSION 2.6)

# Locate GTest
find_package(GTest REQUIRED)
include_directories(${GTEST_INCLUDE_DIRS})

# Link runTests with what we want to test and the GTest and pthread library
add_executable(runTests hello_gtest.cc)
target_link_libraries(runTests ${GTEST_LIBRARIES} pthread)
```

## compile

```
cmake CMakeLists.txt
make
./runTests
```

## 결과

```
pi@pi:~/Workspace/test $ ./runTests
[==========] Running 1 test from 1 test case.
[----------] Global test environment set-up.
[----------] 1 test from tc_name
[ RUN      ] tc_name.t_name
[       OK ] tc_name.t_name (0 ms)
[----------] 1 test from tc_name (0 ms total)

[----------] Global test environment tear-down
[==========] 1 test from 1 test case ran. (1 ms total)
[  PASSED  ] 1 test.
```
