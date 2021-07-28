cmake_minimum_required(VERSION 3.0.0)
project(its-a-plane VERSION 0.1.0)

include(CTest)
enable_testing()

include(ExternalProject)

ExternalProject_Add(
    rpi_rgb_led_matrix
    DEPENDS ""
    SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/vendor/rpi-rgb-led-matrix/lib/"
    BUILD_IN_SOURCE "${CMAKE_CURRENT_SOURCE_DIR}/vendor/rpi-rgb-led-matrix/lib/"
    CONFIGURE_COMMAND ""
    BUILD_COMMAND make
    INSTALL_COMMAND ""
)

add_executable(its-a-plane main.cpp)

target_include_directories(its-a-plane PUBLIC ${CMAKE_SOURCE_DIR}/vendor/rpi-rgb-led-matrix/include)
target_link_libraries(its-a-plane ${CMAKE_SOURCE_DIR}/vendor/rpi-rgb-led-matrix/lib/librgbmatrix.so.1)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)

set_target_properties(its-a-plane
    PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/lib"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/lib"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/bin"
)
