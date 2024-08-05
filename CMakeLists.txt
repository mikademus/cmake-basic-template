cmake_minimum_required(VERSION 3.10)

project(ReplaceWithProjectName)

# Define the target. 
# * The target name defaults to the project name. 
# * The name of the cpp file containing the main function is 
#   assumed to be the same as the target name.
set(TARGET ${PROJECT_NAME})
set(MAIN_FILE "${CMAKE_SOURCE_DIR}/main.cpp")

add_executable(${TARGET} ${MAIN_FILE})

# Parsing options
option(GLOB_SOURCES "Use GLOB_RECURSE to find source files" ON)
option(GLOB_HEADERS "Use GLOB_RECURSE to find include directories" OFF)

# Add source files
#----------------------------------
if (GLOB_SOURCES)
    file(GLOB_RECURSE SOURCES "src/*.cpp")
else()
    set(SOURCES 
        # Manually list source files here if not using GLOB
        # e.g., "${PROJECT_SOURCE_DIR}/src/source1.cpp"
    )
endif()

target_sources(${TARGET} PRIVATE ${SOURCES})

# Add include directories
#----------------------------------
if (GLOB_HEADERS)
    file(GLOB_RECURSE HEADER_FILES "include/*.h")
    
    # Extract unique directories from header files
    set(INCLUDE_DIRS "")
    foreach(HEADER_FILE ${HEADER_FILES})
        get_filename_component(HEADER_DIR ${HEADER_FILE} DIRECTORY)
        list(APPEND INCLUDE_DIRS ${HEADER_DIR})
    endforeach()
    list(REMOVE_DUPLICATES INCLUDE_DIRS)
else()
    # Explicitly list include directories
    set(INCLUDE_DIRS
        ${PROJECT_SOURCE_DIR}/include
        # Add other directories as needed
    )
endif()

target_include_directories(${TARGET} PRIVATE ${INCLUDE_DIRS})
