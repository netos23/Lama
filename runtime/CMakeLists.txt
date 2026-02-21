cmake_minimum_required(VERSION 3.16)
project(lama_runtime C ASM)

set(DISABLE_WARNINGS "-Wno-shift-negative-value")
set(COMMON_FLAGS "${DISABLE_WARNINGS} -g -fstack-protector-all --std=c11")

if(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    set(COMMON_FLAGS "${COMMON_FLAGS} -arch x86_64")
endif()

add_library(runtime STATIC gc.c runtime.c printf.S)

# Provide fallback symbols for missing custom_data section on Linux
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    target_link_options(runtime INTERFACE "-Wl,--defsym=__start_custom_data=0" "-Wl,--defsym=__stop_custom_data=0")
endif()

target_compile_options(runtime PRIVATE ${COMMON_FLAGS})
target_compile_definitions(runtime PRIVATE LAMA_ENV)
set_source_files_properties(printf.S PROPERTIES COMPILE_OPTIONS "-Wa,--noexecstack")
