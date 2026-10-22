cmake_minimum_required(VERSION 3.22)
project(vsc-core VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_library(vsc_core INTERFACE)
add_library(vsc::core ALIAS vsc_core)

target_include_directories(vsc_core INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

if(MSVC)
    target_compile_options(vsc_core INTERFACE /W4)
else()
    target_compile_options(vsc_core INTERFACE -Wall -Wextra -Wpedantic)
endif()
