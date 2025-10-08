+++
title = "CMake Tutorial"
slug = "cmake-tutorial"
date = 2025-10-08
+++

## Intro
CMake is a `Makefile` generator. It is a powerful tool that can help you manage your project's build system.

This is done by recursively configuring the project with `CMakeLists.txt` files.

In this post, I will go through various scenarios of using CMake.

## Basic Usage
To use Cmake, you usually find the `CMakeLists.txt` file in the root directory of your project. And you can build the project by running the following command:

```bash
# create build directory and generate Makefile in the build directory
cmake -B build . 

# Use the generated Makefile to build the project
cmake --build build
```

**Alternatively, you can use the following command to build the project:**
```bash
mkdir build
cd build
cmake ..
make
```

## Scenario
- Making a executable
- Making a shared library
- Making a executable and library. And use the library in the executable

## Scenario 1: Making a executable

You can see the source code in [cmake-tutorial/build-executable](https://github.com/jinho-choi123/cmake-tutorial/tree/main/build-executable). Please read the README.md file for more information.

`add_executable` is a CMake command that adds an executable to the project.

```cmake
project(build-executable)

cmake_minimum_required(VERSION 3.20)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(build-executable csrc/main.cpp)
```

## Scenario 2: Making a shared library

You can see the source code in [cmake-tutorial/build-library](https://github.com/jinho-choi123/cmake-tutorial/tree/main/build-library). Please read the README.md file for more information.


`add_library` is a CMake command that adds a shared library to the project. This automatically generates shared library file.

`target_include_directories` is a CMake command that adds include directories to the project. Since the header files for `custom_math` library is in `${CMAKE_CURRENT_SOURCE_DIR}/include`, we add it to the project.

```cmake
project(build-library)

cmake_minimum_required(VERSION 3.20)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CSRC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/csrc)
set(INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include)

set(CSRC_FILES ${CSRC_DIR}/custom_math.cpp)

add_library(custom_math SHARED ${CSRC_FILES})


target_include_directories(custom_math PUBLIC ${INCLUDE_DIR})
```

## Scenario 3: Making a executable and library. And use the library in the executable

You can see the source code in [cmake-tutorial/build-library-and-executable](https://github.com/jinho-choi123/cmake-tutorial/tree/main/build-library-and-executable). Please read the README.md file for more information.

`add_subdirectory` is a CMake command that adds a subdirectory to the project. It finds the `CMakeLists.txt` file in the subdirectory and recursively configures it.

`target_link_libraries` is a CMake command that links a library to the executable/library.

```cmake
#Cmake file to build the custom_math library
cmake_minimum_required(VERSION 3.9.1)
project(CmakeHello)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR}/bin)
message(${CMAKE_BINARY_DIR})
add_subdirectory(lib/)

add_executable(cmake_hello exec/main.cpp)
target_link_libraries(cmake_hello custom_math)
```

## For more complex scenarios
I have setup complex tutorials that use CMake+setuptools+pybind11+spdlog+google-test+pytest. Please checkout the following repository: [hyperaccel-logging-tutorial](https://github.com/jinho-choi123/hyperaccel-logging-tutorial).

## References
[1] [https://github.com/jinho-choi123/cmake-tutorial](https://github.com/jinho-choi123/cmake-tutorial)
[2] [https://medium.com/@onur.dundar1/cmake-tutorial-585dd180109b](https://medium.com/@onur.dundar1/cmake-tutorial-585dd180109b)
[3] [https://junstar92.tistory.com/204](https://junstar92.tistory.com/204)
[4] [https://gist.github.com/dongbum/d1d49e38a20f9cf52ea39f9ce2702160](https://gist.github.com/dongbum/d1d49e38a20f9cf52ea39f9ce2702160)
