cmake_minimum_required(VERSION 3.10 FATAL_ERROR)

project(cpp-travis-integration-package
    LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 14)

include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup(TARGETS)

add_subdirectory(src)
