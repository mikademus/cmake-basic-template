cmake_minimum_required(VERSION 3.10)

# Define the subproject
project(ProjectB)

# Add the executable
add_executable(ProjectB src/main.cpp)

# Link the shared library
target_link_libraries(ProjectB PRIVATE SharedLib)

# Include directories from SharedLib
target_include_directories(ProjectB PRIVATE
    $<TARGET_PROPERTY:SharedLib,INTERFACE_INCLUDE_DIRECTORIES>
)
