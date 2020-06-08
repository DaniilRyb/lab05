# lab05
Repository was created for work Travis CI (lab05)
## Laboratory work V

<a href="https://yandex.ru/efir/?stream_id=vQw_LH0UfN6I"><img src="https://raw.githubusercontent.com/tp-labs/lab05/master/preview.png" width="640"/></a>

Данная лабораторная работа посвещена изучению фреймворков для тестирования на примере **GTest**

```sh
$ open https://github.com/google/googletest
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab05** на сервисе **GitHub**
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial


Создание переменных среды и установка их значений
```sh
$ export GITHUB_USERNAME=Evgengrmit

$ alias gsed=sed 
```

Начало работы в каталоге workspace
```sh

$ cd ${GITHUB_USERNAME}/workspace
$ pushd . 

$ source scripts/activate
```
Настройка git-репозитория **lab05** для работы
```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab05 projects/lab05
$ cd projects/lab05
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab05
```
Подключение к репозиторию подмодуля **Google Test**, выбор версии с помощью переключения ветки
```sh
$ mkdir third-party

$ git submodule add https://github.com/google/googletest third-party/gtest
Cloning into 'C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/projects/lab05/third-party/gtest'...
remote: Enumerating objects: 20517, done.
remote: Total 20517 (delta 0), reused 0 (delta 0), pack-reused 20517
Receiving objects: 100% (20517/20517), 7.59 MiB | 4.69 MiB/s, done.
Resolving deltas: 100% (15174/15174), done.
warning: LF will be replaced by CRLF in .gitmodules.
The file will have its original line endings in your working directory

$ cd third-party/gtest && git checkout release-1.8.1 && cd ../..
switching to 'release-1.8.1'.
$ git add third-party/gtest
$ git commit -m "added gtest framework"
```

```sh


$ gsed -i "" '/option(BUILD_EXAMPLES "Build examples" OFF)/a\
option(BUILD_TESTS "Build tests" OFF)
' CMakeLists.txt


$ cat >> CMakeLists.txt <<EOF

if(BUILD_TESTS)
  enable_testing()
  add_subdirectory(third-party/gtest)
  file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
  add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
  target_link_libraries(check \${PROJECT_NAME} gtest_main)
  add_test(NAME check COMMAND check)
endif()
EOF
```

```sh
$ mkdir tests
$ cat > tests/test1.cpp <<EOF
#include <print.hpp>

#include <gtest/gtest.h>

TEST(Print, InFileStream)
{
  std::string filepath = "file.txt";
  std::string text = "hello";
  std::ofstream out{filepath};

  print(text, out);
  out.close();

  std::string result;
  std::ifstream in{filepath};
  in >> result;

  EXPECT_EQ(result, text);
}
EOF
```
Сборка проекта
```sh

$ cmake -H. -B_build -DBUILD_TESTS=ON
-- Building for: Visual Studio 15 2017
CMake Warning (dev) in CMakeLists.txt:
  No project() command is present.  The top-level CMakeLists.txt file must
  contain a literal, direct call to the project() command.  Add a line of
  code such as

$ cmake --build _build --target test
Running tests...
Test project /Users/Daniil_Rip/DaniilRyb/workspace/projects/lab05/_build
    Start 1: check
1/1 Test #1: check ............................   Passed    0.10 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.10 sec
```

```sh
$ _build/check

...
[  PASSED  ] 1 test.

$ cmake --build _build --target test -- ARGS=--verbose
Running tests...
100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.01 sec
```
Модифицируем `.travis.yml` и `README.md`
```sh
$ gsed -i "" 's/lab05/lab05/g' README.md
$ gsed -i "" 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml
$ gsed -i "" '/cmake --build _build --target install/a\
- cmake --build _build --target test -- ARGS=--verbose
' .travis.yml
```
Проверка `.travis.yml`
```sh
$ travis lint
Hooray, .travis.yml looks valid :)
```

```sh
$ git add .travis.yml
$ git add tests
$ git add -p 
diff --git a/CMakeLists.txt b/CMakeLists.txt
...

Stage this hunk [y,n,q,a,d,j,J,g,/,e,?]? y
$ git commit -m"added tests"
$ git push origin master
```

```sh
$ travis login --
Successfully logged in as Evgengrmit!
$ travis enable
DaniilRyb/lab05: enabled :)
```

```sh
$ mkdir artifacts
$ screencapture -T 20 artifacts/screenshot.png
```
## Report

```sh
$ popd
$ export LAB_NUMBER=05
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ atom REPORT.md
$ gist REPORT.md
```

## Links

- [C++ CI: Travis, CMake, GTest, Coveralls & Appveyor](http://david-grs.github.io/cpp-clang-travis-cmake-gtest-coveralls-appveyor/)
- [Boost.Tests](http://www.boost.org/doc/libs/1_63_0/libs/test/doc/html/)
- [Catch](https://github.com/catchorg/Catch2)

```
Copyright (c) 2015-2020 The ISC Authors
```
