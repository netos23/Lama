cmake_minimum_required(VERSION 3.16)
project(lama_byterun C)

set(CMAKE_C_STANDARD 11)

# Ensure the runtime target is available
if(NOT TARGET runtime)
    add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/../runtime ${CMAKE_BINARY_DIR}/third_party/Lama/runtime)
endif()

add_executable(byterun byterun.c)

target_include_directories(byterun PRIVATE ${CMAKE_CURRENT_LIST_DIR}/../runtime)

target_compile_options(byterun PRIVATE -g -fstack-protector-all)
if(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    target_compile_options(byterun PRIVATE -arch x86_64)
endif()

target_link_libraries(byterun PRIVATE runtime)
