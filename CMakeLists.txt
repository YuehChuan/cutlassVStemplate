﻿cmake_minimum_required(VERSION 3.8 FATAL_ERROR)
set(CMAKE_CUDA_COMPILER "C:/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v12.2/bin/nvcc.exe")
set(CMAKE_CUDA_ARCHITECTURES 70 89) #70 RTX3070 89 RTX4080/4090
project(cudaVStemplate LANGUAGES CXX CUDA)
set(CMAKE_CUDA_FLAGS ${CMAKE_CUDA_FLAGS} "-g -G")  # enable cuda-gdb
set(CMAKE_CUDA_STANDARD 17)#cutlass3.2 depends on c++ 17 standard 
set(CUDA_PATH "C:/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v12.2")

include_directories(${PROJECT_SOURCE_DIR}/include)
link_directories(${PROJECT_SOURCE_DIR}/lib)

#thrust, cub  
include_directories(${CUDA_PATH}/include/)
include_directories(${PROJECT_SOURCE_DIR}/include/cuda-12.2-header/)

#CUDA
link_directories(${CUDA_PATH}/lib/x64)
link_directories(${PROJECT_SOURCE_DIR}/lib/cuda-12.2-lib/x64/)

#cutlass
include_directories(${PROJECT_SOURCE_DIR}/lib/cutlass/include)
include_directories(${PROJECT_SOURCE_DIR}/lib/cutlass/tools/util/include)


set(BUILD_TEST "ON")
set(CUDA_TEST "ON")

if(BUILD_TEST)
  message("BUILD_TEST")



else()
  message("Welcome !!!")
endif()


if(CUDA_TEST)
add_executable(basic_vector "cuda-example/basic_vector.cu")

# Required for CUDA builds
#set_target_properties(basic_vector PROPERTIES CUDA_SEPARABLE_COMPILATION  ON)

add_executable(basic_gemm "cutlass-example/basic_gemm.cu")
add_executable(gemm_bias_relu "cutlass-example/gemm_bias_relu.cu")




else()
  message("CUDA!!!")
endif()

add_executable (main "main.cpp" )
target_link_libraries( main  )

if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET main PROPERTY CXX_STANDARD 20)
endif()

