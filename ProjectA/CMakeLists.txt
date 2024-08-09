cmake_minimum_required(VERSION 3.10)

# Define the subproject
project(ProjectA)

# Add the executable
add_executable(ProjectA src/main.cpp)

# Link the shared library
target_link_libraries(ProjectA PRIVATE SharedLib)

# Include directories from SharedLib
target_include_directories(ProjectA PRIVATE
    $<TARGET_PROPERTY:SharedLib,INTERFACE_INCLUDE_DIRECTORIES>
)
