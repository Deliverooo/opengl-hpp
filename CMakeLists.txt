cmake_minimum_required(VERSION 3.13)

# Select C++20 as the standard for C++ projects.
set(CMAKE_CXX_STANDARD 20)
# If C++20 is not available, downgrading to an earlier standard is NOT OK.
set(CMAKE_CXX_STANDARD_REQUIRED ON)
# Do not enable compiler specific language extensions.
set(CMAKE_CXX_EXTENSIONS OFF)

project(opengl-hpp)

add_subdirectory(extern/glad)

set(GL_HPP_SRC
		src/opengl.cpp
)

set(GL_HPP_LIB glad)

option(GL_HPP_BUILD_SHARED "Build as a dll" ON)

if (${GL_HPP_BUILD_SHARED})
	add_library(opengl-hpp SHARED)
else ()
	add_library(opengl-hpp STATIC)
endif ()

target_sources(opengl-hpp PRIVATE ${GL_HPP_SRC})
target_include_directories(opengl-hpp PUBLIC include)

target_link_libraries(opengl-hpp PUBLIC ${GL_HPP_LIB})

if (${GL_HPP_BUILD_SHARED})
	target_compile_definitions(opengl-hpp PUBLIC GL_HPP_BUILD_DLL=1)

else ()
	target_compile_definitions(opengl-hpp PUBLIC GL_HPP_BUILD_DLL=0)
endif ()

target_compile_definitions(opengl-hpp PRIVATE GL_HPP_DLL_EXPORT)