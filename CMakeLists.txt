cmake_minimum_required(VERSION 2.8)
project(sisyphus)

# Build setup

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")

set(CMAKE_BUILD_TYPE "Release")
add_definitions("-std=c++11 -O3 -msse2 -msse3")

# Packages

find_package(Pangolin REQUIRED)

include_directories(${Pangolin_INCLUDE_DIRS})
include_directories(include)
include_directories(3rd_party)

# Build library

file(GLOB_RECURSE lib_src src/*.cpp)
file(GLOB_RECURSE 3rd_src 3rd_party/*.cpp)

add_library(sisyphus SHARED ${3rd_src} ${lib_src})
target_link_libraries(sisyphus ${Pangolin_LIBRARIES})

# Build examples

add_executable(ply_io_example examples/ply_io_example.cpp)
target_link_libraries(ply_io_example sisyphus ${Pangolin_LIBRARIES})

add_executable(pca_example examples/pca_example.cpp)
target_link_libraries(pca_example sisyphus ${Pangolin_LIBRARIES})

# Install target

install(TARGETS sisyphus LIBRARY DESTINATION /usr/local/lib)
install(DIRECTORY include/ DESTINATION /usr/local/include)

# Uninstall target
configure_file(
        "${CMAKE_CURRENT_SOURCE_DIR}/cmake_uninstall.cmake.in"
        "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
        IMMEDIATE @ONLY)

add_custom_target(uninstall COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)