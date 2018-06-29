cmake_minimum_required(VERSION 3.0)

project(LearningGL)

set(CMAKE_VERBOSE_MAKEFILE ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if (MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /std:c++17")
endif()

set(EXECUTABLE_OUTPUT_PATH "${CMAKE_HOME_DIRECTORY}/bin")


include_directories(${CMAKE_SOURCE_DIR}/src)

add_subdirectory(ext/glfw)
include_directories(ext/glfw/include)


find_package(OpenGL REQUIRED)
if (OpenGL_FOUND)
    message("OpenGL include dirs: ${OPENGL_INCLUDE_DIRS}")
    include_directories(${OPENGL_INCLUDE_DIRS})
    message("OpenGL include dirs: ${OPENGL_LIBRARIES}")
    link_libraries(${OPENGL_LIBRARIES})
endif()


find_package(glew)

if (NOT GLEW_FOUND)
    set(GLEW_USE_STATIC_LIBS ON)
    set(glew-cmake_BUILD_SHARED OFF)
    add_subdirectory(ext/glew)
    include_directories(ext/glew/include)
else()
    include_directories(${GLEW_INCLUDE_DIRS})
endif()

if (APPLE)
    find_library(COCOA_LIBRARY Cocoa)
    find_library(OpenGL_LIBRARY OpenGL)
    find_library(IOKIT_LIBRARY IOKit)
    find_library(COREVIDEO_LIBRARY CoreVideo)
    set(EXTRA_LIBS
            ${COCOA_LIBRARY}
            ${OpenGL_LIBRARY}
            ${IOKIT_LIBRARY}
            ${COREVIDEO_LIBRARY}
    )
endif()



file(GLOB_RECURSE SRC ./src *.h)
message("${SRC}")

add_executable(LearningGL ./src/main.cpp ${SRC})


set_target_properties(LearningGL PROPERTIES COMPILE_DEFINITIONS "GLEW_STATIC")
target_compile_features(LearningGL PUBLIC cxx_std_17)

target_link_libraries(LearningGL
        glfw
        ${OPENGL_LIBRARIES}
        ${GLEW_LIBRARIES}
        ${EXTRA_LIBS}
)