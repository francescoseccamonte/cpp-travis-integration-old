cmake_minimum_required(VERSION 3.10 FATAL_ERROR)

project(cpp-travis-integration LANGUAGES CXX)

# Project wide C++ standard
set(CMAKE_CXX_STANDARD 14)

# Dynamic libraries preferred by default
option(BUILD_SHARED_LIBS "Build shared libraries" ON)

# Provides BUILD_TESTING option
include(CTest)
if (BUILD_TESTING)
    enable_testing()
endif()

# Setup conan targets -> requires to have run conan install
if(EXISTS ${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
    include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
    conan_basic_setup(TARGETS)
else()
    message(FATAL_ERROR "
        Missing conanbuildinfo.cmake file in your build folder \"${CMAKE_BINARY_DIR}\".
        You need to run \"conan install ${CMAKE_SOURCE_DIR}\".")
endif()

# Put all binaries and libraries in the same place
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

# Convenience default settings for install destinations. Use it to
# install targets to the right place. For instance:
#
#    add_library(mylib)
#    install(TARGETS mylib ${DEFAULT_DESTINATIONS})
#
# It will purposely not install static libraries, since they are
# not needed once the project has been deployed.
set(DEFAULT_DESTINATIONS
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib EXCLUDE_FROM_ALL
    OBJECTS DESTINATION lib EXCLUDE_FROM_ALL)

# Project wide compile options
add_compile_options(
    -Wall -Wextra           # Typical baseline
    -Wpedantic              # Non-standard C++ is used

    -Wshadow                # Variable declaration shadows one from a parent scope
    -Wnon-virtual-dtor      # Class with virtual functions has a non-virtual destructor
    -Wold-style-cast        # C-style casts
    -Wcast-align            # Potential errors dereferencing pointers
    -Woverloaded-virtual    # Overload (not override) a virtual function
    -Wconversion            # Type conversions that may lose data
    -Wsign-conversion       # Sign conversions
    -Wdouble-promotion      # float is implicit promoted to double
    -Wformat=2              # Security issues around functions that format output (like printf)
)

add_subdirectory(src)
