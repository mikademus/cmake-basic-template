cmake_minimum_required(VERSION 3.10)

# Define the shared library
project(SharedLib)

add_library(SharedLib STATIC
    src/shared.cpp
)

# Specify include directories
target_include_directories(SharedLib PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)