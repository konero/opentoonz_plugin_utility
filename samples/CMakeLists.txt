cmake_minimum_required(VERSION 3.20)
project(opentoonz_plugin_utility_samples LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if(MSVC)
    add_compile_options(/MP /openmp)
else()
    find_package(OpenMP QUIET)
    if(OpenMP_CXX_FOUND)
        add_compile_options(${OpenMP_CXX_FLAGS})
        add_link_options(${OpenMP_CXX_FLAGS})
    endif()
endif()

find_package(OpenCV REQUIRED)

if(WIN32)
    set(PLUGIN_UTILITY_LIB "${CMAKE_CURRENT_SOURCE_DIR}/../lib/${CMAKE_CFG_INTDIR}/opentoonz_plugin_utility.lib")
else()
    set(PLUGIN_UTILITY_LIB "${CMAKE_CURRENT_SOURCE_DIR}/../lib/${CMAKE_CFG_INTDIR}/libopentoonz_plugin_utility.a")
endif()

set(LIBS ${OpenCV_LIBS} ${PLUGIN_UTILITY_LIB})

include_directories(../plugin_sdk/core ../include ${OpenCV_INCLUDE_DIRS})

add_subdirectory(amp)
add_subdirectory(blur)
add_subdirectory(snp)
