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
last_modified_at: 2021-07-08

---

# google test

* [google test](https://github.com/google/googletest)
* C++ Test를 위해 google에서 만든 Framework

# Install

* Ubuntu 20.04LTS기준

* 기반 패키지
  + build tools : cmake, make
    - [bazel](https://bazel.build/) build tool도 지원하지만 cmake만 사용할 예정
  + c++ compiler : gcc, g++

## google test 설치

### apt 설치

```
sudo apt-get install libgtest-dev

cd /usr/src/gtest
sudo cmake CMakeList.txt
sudo make
sudo cp *.a /usr/lib
```

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

```cpp
#include <iostream>
#include <gtest/gtest.h>

int getTen() {
  return 10;
}

TEST (tc_name, t_name) {
  EXPECT_EQ(10, getTen());
}

int main(int argc, char **argv) {
  ::testing::InitGoogleTest(&argc, argv);
  return RUN_ALL_TEST();
}
```
