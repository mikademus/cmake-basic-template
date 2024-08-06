cmake_minimum_required(VERSION 3.10)

# Define the subproject
project(ProjectB)

# Define the target. 
# * The target name defaults to the subproject name.
# * The cpp file with the main function defaults to main.cpp
#   in the subproject folder.
set(TARGET_B ${PROJECT_NAME})
set(MAIN_FILE "${CMAKE_SOURCE_DIR}/main.cpp")

add_executable(${TARGET_B} ${MAIN_FILE})

# Link the common interface library
target_link_libraries(${TARGET_B} PRIVATE Shared)
target_sources(${TARGET_B} PRIVATE ${SHARED_SOURCES})

set(SOURCES
    # "${PROJECT_SOURCE_DIR}/src/source1.cpp"
)

target_sources(${TARGET_B} PRIVATE ${SOURCES})

set(INCLUDE_DIRS
    # ${PROJECT_SOURCE_DIR}/include
)

target_include_directories(${TARGET_B} PRIVATE ${INCLUDE_DIRS})