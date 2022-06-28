cmake_minimum_required(VERSION 3.22)
project(c__modules)

set(CMAKE_CXX_STANDARD 23)

# Add target to build iostream module
add_custom_target(std_modules ALL
        COMMAND ${CMAKE_COMMAND} -E echo "Building standard library modules"
        COMMAND g++ -fmodules-ts -std=c++${CMAKE_CXX_STANDARD} -c -x c++-system-header iostream vector
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
        )

# Function to set up modules in GCC
function(prepare_for_module TGT)
    target_compile_options(${TGT} PUBLIC -fmodules-ts)
    set_property(TARGET ${TGT} PROPERTY CXX_EXTENSIONS OFF)
    add_dependencies(${TGT} std_modules)
endfunction()

add_executable(std_modules_example std_modules/main.cpp)
prepare_for_module(std_modules_example)
