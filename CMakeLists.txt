cmake_minimum_required(VERSION 3.14)
project(elheif)

set(CMAKE_EXPORT_COMPILE_COMMANDS 1)

add_subdirectory(./core)
add_subdirectory(./tests)
if (EMSCRIPTEN)
    add_subdirectory(./wasm)
endif()