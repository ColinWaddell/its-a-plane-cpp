cmake_minimum_required(VERSION 3.0.0)
project(its-a-plane VERSION 0.1.0)
include(ExternalProject)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)

# Executatable properties
add_executable(its-a-plane main.cpp)

set_target_properties(its-a-plane
    PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/lib"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/lib"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/bin"
)


# RGB Matrix library
ExternalProject_Add(
    rpi_rgb_led_matrix
    DEPENDS ""
    SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/vendor/rpi-rgb-led-matrix/lib/"
    BUILD_IN_SOURCE "${CMAKE_CURRENT_SOURCE_DIR}/vendor/rpi-rgb-led-matrix/lib/"
    CONFIGURE_COMMAND ""
    BUILD_COMMAND make
    INSTALL_COMMAND ""
)

target_include_directories(its-a-plane PUBLIC ${CMAKE_SOURCE_DIR}/vendor/rpi-rgb-led-matrix/include)
target_link_libraries(its-a-plane ${CMAKE_SOURCE_DIR}/vendor/rpi-rgb-led-matrix/lib/librgbmatrix.so.1)

# restc-cpp library
find_package(ZLIB REQUIRED)
find_package(OpenSSL REQUIRED)
find_package(Boost REQUIRED COMPONENTS
    system
    program_options
    filesystem
    date_time
    context
    coroutine
    chrono
    log
)

include(cmake/external-projects.cmake)

add_dependencies(${PROJECT_NAME} externalRestcCpp)
set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 14)
target_link_libraries(${PROJECT_NAME}
    restc-cpp
    ${Boost_LIBRARIES}
    ${ZLIB_LIBRARIES}
    ${OPENSSL_LIBRARIES}
)

add_definitions(
    -DBOOST_COROUTINE_NO_DEPRECATION_WARNING=1
    -DBOOST_ALL_DYN_LINK=1
)
