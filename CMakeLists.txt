cmake_minimum_required(VERSION 3.16)
project(trading)

set(CMAKE_CXX_STANDARD 17)

# Set a default build type if none was specified
# Based on: https://github.com/openchemistry/tomviz/blob/master/cmake/BuildType.cmake
set(DEFAULT_BUILD_TYPE "Release")
if(EXISTS "${CMAKE_SOURCE_DIR}/.git")
  set(DEFAULT_BUILD_TYPE "Debug")
endif()
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "Setting build type to '${DEFAULT_BUILD_TYPE}' as none was specified.")
  set(CMAKE_BUILD_TYPE "${DEFAULT_BUILD_TYPE}" CACHE STRING "Choose the type of build." FORCE)
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

add_executable(trading main.cpp)

if(WIN32) # Install dlls in the same directory as the executable on Windows
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
endif()

include(FetchContent)
FetchContent_Declare(cpr GIT_REPOSITORY https://github.com/libcpr/cpr.git
                         GIT_TAG 2553fc41450301cd09a9271c8d2c3e0cf3546b73)
FetchContent_MakeAvailable(cpr)

FetchContent_Declare(spdlog GIT_REPOSITORY https://github.com/gabime/spdlog.git
                         GIT_TAG 7e635fca68d014934b4af8a1cf874f63989352b7)
FetchContent_MakeAvailable(spdlog)

FetchContent_Declare(json GIT_REPOSITORY https://github.com/nlohmann/json.git
                         GIT_TAG bc889afb4c5bf1c0d8ee29ef35eaaf4c8bef8a5d)
FetchContent_MakeAvailable(json)

target_link_libraries(trading PRIVATE cpr::cpr spdlog::spdlog nlohmann_json::nlohmann_json)