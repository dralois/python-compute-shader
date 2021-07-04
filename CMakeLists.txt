cmake_minimum_required(VERSION 3.18)

# Create project
project(ComputeShader
        VERSION 1.0
        DESCRIPTION "OpenGL compute shader wrapper for python"
        LANGUAGES CXX
)

# C++ 11
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Sources
set(HEADER_LIST
    Source/Module.cpp
    Source/Module.def
    Source/OpenGL.cpp
    Source/OpenGL.hpp
)

# Create library
add_library(ComputeShader MODULE ${HEADER_LIST} ${SOURCE_LIST})

# Find OpenGL
find_package(OpenGL)

# Add OpenGL library
target_link_libraries(ComputeShader PRIVATE OpenGL::GL)

# Find python for development
find_package(Python 3.7 EXACT REQUIRED Development)

# Bug: Enforce no debug on Windows
if(WIN32)
    set_target_properties(Python::Python PROPERTIES
                        IMPORTED_IMPLIB_DEBUG "${Python_LIBRARY_RELEASE}"
                        IMPORTED_LOCATION_DEBUG "${Python_RUNTIME_LIBRARY_RELEASE}"
    )
endif()

# Add python development library
target_link_libraries(ComputeShader PRIVATE Python::Python)

# Output settings
if(CMAKE_SYSTEM_NAME STREQUAL "Windows")
    set_target_properties(ComputeShader PROPERTIES
        SUFFIX ".pyd"
    )
endif()
