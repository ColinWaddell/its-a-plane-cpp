cmake_minimum_required(VERSION 3.0.0)
project(its-a-plane VERSION 0.1.0)

include(CTest)
enable_testing()

include_directories(${CMAKE_SOURCE_DIR}/vendor/rpi-rgb-led-matrix/include)
link_directories(${CMAKE_SOURCE_DIR}/vendor/rpi-rgb-led-matrix/lib)

add_executable(its-a-plane main.cpp)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})