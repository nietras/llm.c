cmake_minimum_required(VERSION 3.12)
project(gpt2)

set(CMAKE_C_STANDARD 17)
set(CMAKE_CXX_STANDARD 23)
#set(CMAKE_C_FLAGS "-O3 -Ofast -Wno-unused-result")
set(CMAKE_EXE_LINKER_FLAGS "")

# Check if OpenMP is available
find_package(OpenMP)
if(OpenMP_FOUND)
    if(MSVC)
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /openmp:experimental")
        set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS}")
    else()
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
        set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
    endif()
    message(STATUS "NICE Compiling with OpenMP support")
else()
    message(WARNING "OOPS Compiling without OpenMP support")
endif()

if(MSVC)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /arch:AVX2")
endif()


# Add executable targets
add_executable(train_gpt2 train_gpt2.c)
add_executable(test_gpt2 test_gpt2.c)

# Link libraries
#target_link_libraries(train_gpt2)
#target_link_libraries(test_gpt2)

# Set output directories
set_target_properties(train_gpt2 PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set_target_properties(test_gpt2 PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
