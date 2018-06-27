set(TARGET_NAME LearningGL)

file(GLOB_RECURSE SRC . *.h)
message("${SRC}")

add_executable(${TARGET_NAME} main.cpp ${SRC})