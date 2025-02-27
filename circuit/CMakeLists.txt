cmake_minimum_required(VERSION 2.8)
project(ssles)
add_subdirectory(../ethsnarks ../.build/ethsnarks EXCLUDE_FROM_ALL)

if (CMAKE_VERSION VERSION_GREATER "3.0")
  set(CMAKE_CXX_STANDARD 11)
  set(CMAKE_CXX_STANDARD_REQUIRED ON) #...is required...
else()
  include(CheckCXXCompilerFlag)
  CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)

  if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
  elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
  else()
        message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
  endif()
endif()

add_library(ssles_circuit SHARED ssles_circuit.cpp)
target_link_libraries(ssles_circuit ethsnarks_common SHA3IUF)
set_property(TARGET ssles_circuit PROPERTY POSITION_INDEPENDENT_CODE ON)
set_property(TARGET ssles_circuit PROPERTY CXX_STANDARD 11)

add_executable(ssles_cli ssles_cli.cpp)
target_link_libraries(ssles_cli ethsnarks_common SHA3IUF)
set_property(TARGET ssles_cli PROPERTY CXX_STANDARD 11)
