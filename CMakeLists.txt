# CMake entry point
cmake_minimum_required (VERSION 3.0)
project (Computer_Graphics_Coursework)

find_package(OpenGL REQUIRED)

if( CMAKE_BINARY_DIR STREQUAL CMAKE_SOURCE_DIR )
    message( FATAL_ERROR "Please select another Build Directory!" )
endif()

# Compile external dependencies 
add_subdirectory (external)

include_directories(
	external/glfw-3.1.2/include/
	external/glew-1.13.0/include/
	external/glm-0.9.7.1/
	.
)

set(ALL_LIBS
	${OPENGL_LIBRARY}
	glfw
	GLEW_1130
)

add_definitions(
	-DTW_STATIC
	-DTW_NO_LIB_PRAGMA
	-DTW_NO_DIRECT3D
	-DGLEW_STATIC
	-D_CRT_SECURE_NO_WARNINGS
)

# ==============================================================================
# Coursework 
add_executable(Computer_Graphics_Coursework
	source/main.cpp
	source/vertexShader.glsl
	source/fragmentShader.glsl

	common/shader.hpp
	common/texture.hpp
	common/stb_image.hpp
	common/maths.hpp
	common/maths.cpp
	common/camera.hpp
	common/camera.cpp
	common/model.hpp
	common/model.cpp
	common/light.hpp
	common/light.cpp
)
target_link_libraries(Computer_Graphics_Coursework
	${ALL_LIBS}
)

# Xcode and Visual working directories
set_target_properties(Computer_Graphics_Coursework PROPERTIES XCODE_ATTRIBUTE_CONFIGURATION_BUILD_DIR "${CMAKE_CURRENT_SOURCE_DIR}/Computer_Graphics_Coursework/")

# ==============================================================================
if (NOT ${CMAKE_GENERATOR} MATCHES "Xcode" )

add_custom_command(
	TARGET Computer_Graphics_Coursework POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_CFG_INTDIR}/Computer_Graphics_Coursework${CMAKE_EXECUTABLE_SUFFIX}" "${CMAKE_CURRENT_SOURCE_DIR}/Computer_Graphics_Coursework/"
)

elseif (${CMAKE_GENERATOR} MATCHES "Xcode" )

endif (NOT ${CMAKE_GENERATOR} MATCHES "Xcode" )

