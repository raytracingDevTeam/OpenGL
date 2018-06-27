cmake_minimum_required(VERSION 3.0)

project(LearningGL)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if (MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /std:c++17")
endif()

set(EXECUTABLE_OUTPUT_PATH "${CMAKE_HOME_DIRECTORY}/bin")


include_directories(${CMAKE_SOURCE_DIR}/src)
add_subdirectory(src)

add_subdirectory(ext/glfw)
include_directories(ext/glfw/include)


find_package(OpenGL REQUIRED)
if (OpenGL_FOUND)
    message("OpenGL include dirs: ${OPENGL_INCLUDE_DIRS}")
    include_directories(${OPENGL_INCLUDE_DIRS})
    message("OpenGL include dirs: ${OPENGL_LIBRARIES}")
    link_libraries(${OPENGL_LIBRARIES})
    # target_link_libraries(OpenGL ${OPENGL_LIBRARIES})
endif()


set(GLEW_USE_STATIC_LIBS ON)
set(glew-cmake_BUILD_SHARED OFF)
add_subdirectory(ext/glew)
include_directories(ext/glew/include)
set_target_properties(LearningGL PROPERTIES COMPILE_DEFINITIONS "GLEW_STATIC")


target_compile_features(LearningGL PUBLIC cxx_std_17)