cmake_minimum_required(VERSION 3.24.1)

# set the project nama and version
project(Chinese VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

configure_file(ChineseConfig.h.in ChineseConfig.h)

add_executable(Chinese main.cpp)

target_include_directories(Chinese PUBLIC "${PROJECT_BINARY_DIR}")
