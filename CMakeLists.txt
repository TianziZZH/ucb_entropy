cmake_minimum_required(VERSION 3.5)
project(fast_sampen
        VERSION 1.0
        LANGUAGES CXX)

set(APP_NAME fast_sampen)
set(LIB_NAME sampen)
set(CMAKE_CXX_STANDARD 11)

set(CMAKE_BUILD_TYPE DEBUG)
add_definitions(-g)

option(PACKAGE_TESTS "Build the tests" on)

set(CMAKE_CXX_FLAGS_DEBUG "-Wall -g -DDEBUG")
set(CMAKE_CXX_FLAGS_RELEASE "-Wall -O3")
set($ENV{PKG_CONFIG_PATH} $ENV{HOME}/local/lib/pkgconfig)
if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
  set(CMAKE_CXX_FLAGS "-stdlib=libc++")
endif()

message(STATUS "PKG_CONFIG_PATH: " $ENV{PKG_CONFIG_PATH})
message(STATUS "Build Type: " ${CMAKE_BUILD_TYPE})
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

set(CMAKE_EXPORT_COMPILE_COMMANDS on)
add_subdirectory(src)

if(PACKAGE_TESTS)
        enable_testing()
        include(GoogleTest)
        add_subdirectory(test)
endif()
