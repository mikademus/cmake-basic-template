cmake_minimum_required(VERSION 3.10)

# Define the top-level target.
project(TopProject)

# Define an interface library for common code
add_library(Shared INTERFACE)

# Parsing options
option(GLOB_SHARED_SOURCES "Use GLOB_RECURSE to find shared source files" ON)
option(GLOB_SHARED_HEADERS "Use GLOB_RECURSE to find shared include directories" OFF)

# Add shared source files
#----------------------------------
if (GLOB_SHARED_SOURCES)
    file(GLOB_RECURSE SHARED_SOURCES "src/*.cpp")
else()
    set(SHARED_SOURCES 
        # Manually list shared source files here if not using GLOB
        # e.g., "${PROJECT_SOURCE_DIR}/src/shared1.cpp"
    )
endif()

# Helper function to add shared sources to a target
function(add_shared_sources target)
    target_sources(${target} PRIVATE ${SHARED_SOURCES})
endfunction()

# Add include directories
#----------------------------------
if (GLOB_SHARED_HEADERS)
    file(GLOB_RECURSE SHARED_HEADER_FILES "include/*.h")
    
    # Extract unique directories from header files
    set(SHARED_INCLUDE_DIRS "")
    foreach(SHARED_HEADER_FILE ${SHARED_HEADER_FILES})
        get_filename_component(SHARED_HEADER_DIR ${SHARED_HEADER_FILE} DIRECTORY)
        list(APPEND SHARED_INCLUDE_DIRS ${SHARED_HEADER_DIR})
    endforeach()
    list(REMOVE_DUPLICATES SHARED_INCLUDE_DIRS)
else()
    # Explicitly list include directories
    set(SHARED_INCLUDE_DIRS
        ${PROJECT_SOURCE_DIR}/include
    )
endif()

# Configure include directories for the Shared library.
# Adding the include folders using the BUILD_INTERFACE and
# INSTALL_INTERFACE CMake generators provides access 
# to them as library includes (i.e. #include<SharedHeader.h>)
target_include_directories(Shared INTERFACE
    $<BUILD_INTERFACE:${SHARED_INCLUDE_DIRS}>
    $<INSTALL_INTERFACE:include>
)

# Include subdirectories for each subproject
#----------------------------------
add_subdirectory(project_a)
add_subdirectory(project_b)