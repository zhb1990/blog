+++
date = '2025-07-13T17:25:37+08:00'
draft = true
title = 'C++ Modules'
+++

# 通过`cmake`来构建`C++ Modules`的项目

## 编译环境

1. `cmake`版本=`4.0.3`
2. `clang`或`gcc`或者`vs2022`(注意下支持`c++23`的版本)
3. `ninga`

## 使用`STD Module`

现有目录如下

```
项目根目录/
│
├── CMakeLists.txt
│
└── main.cpp
```

```c++
// main.cpp
import std;

int main() {
    std::println("Hello, C++23 STD Module!");
}
```

要让其编译成功，需要设置`CMakeLists.txt`的变量：

1. 设置`CXX_MODULE_STD`或`CMAKE_CXX_MODULE_STD`变量为`ON`。
2. `C++`版本设为23。
3. 设置`STD Module`的实验性`UUID`。

```cmake
cmake_minimum_required(VERSION 4.0.3)

set(CMAKE_EXPERIMENTAL_CXX_IMPORT_STD "d0edc3af-4c50-42ea-a356-e2862fe7a444")

project(hello)

# 使用全局变量
set(CMAKE_CXX_MODULE_STD ON)
set(CMAKE_CXX_STANDARD 23)

add_executable(hello main.cpp)
```

关于`cmake`模块支持的文档，[可以查看](https://cmake.org/cmake/help/latest/manual/cmake-cxxmodules.7.html)

> {{< collapse summary="**关于`cmake`实验性`UUID`**" >}}
查询实验性`UUID`需前往`CMake`的[Github仓库](https://github.com/Kitware/CMake)或[Gitlab仓库](https://gitlab.kitware.com/cmake)，在项目的`Help/dev/experimental.rst`文件中，可以找到`C++ import std support`栏目，提供了相关内容。

需要注意的是，此`UUID`与`CMake`版本挂钩，必须使用和你版本匹配的`Tag`下的`UUID`。你可以在本地通过`cmake --version`命令查看本地`CMake`版本。
{{</ collapse >}}

最后，生成配置时指定生成器为`ninga`。

```shell
cmake -B build -G "Ninja"
```