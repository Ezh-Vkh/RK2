cmake_minimum_required(VERSION 3.29.3)
project(ChainOfResponsibilityPattern)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_TESTS "Build tests" OFF)

if(BUILD_TESTS)
  add_compile_options(--coverage)
endif()

add_library(chain_of_responsibility STATIC
    ${CMAKE_CURRENT_SOURCE_DIR}/main.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/Duck.h
    ${CMAKE_CURRENT_SOURCE_DIR}/MallardDuck.h
    ${CMAKE_CURRENT_SOURCE_DIR}/ModelDuck.h
    ${CMAKE_CURRENT_SOURCE_DIR}/FlyRocketPowered.h
)

target_include_directories(chain_of_responsibility PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(chain_of_responsibility gcov)

if(BUILD_TESTS)
  enable_testing()
  add_subdirectory(third-party/gtest)
  file(GLOB CHAIN_OF_RESPONSIBILITY_TEST_SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/tests/*.cpp)
  add_executable(check ${CHAIN_OF_RESPONSIBILITY_TEST_SOURCES})
  target_link_libraries(check chain_of_responsibility gtest_main gmock_main)
  add_test(NAME check COMMAND check)
endif()

include(CPack.cmake)