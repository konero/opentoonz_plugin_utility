cmake_minimum_required(VERSION 3.20)
project(opentoonz_plugin_utility LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    add_compile_definitions(x64)
endif()

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

set(SOURCES src/lib.cpp)

add_library(opentoonz_plugin_utility STATIC ${SOURCES} include/toonz_utility.hpp)

target_include_directories(opentoonz_plugin_utility
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/plugin_sdk/core>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        ${OpenCV_INCLUDE_DIRS}
)

if(TARGET OpenCV::OpenCV)
    target_link_libraries(opentoonz_plugin_utility PRIVATE OpenCV::OpenCV)
else()
    target_link_libraries(opentoonz_plugin_utility PRIVATE ${OpenCV_LIBS})
endif()

set_target_properties(opentoonz_plugin_utility PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/lib/$<CONFIG>"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/lib/$<CONFIG>"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/bin/$<CONFIG>"
)
