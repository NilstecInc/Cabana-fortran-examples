if( Kokkos_ENABLE_CUDA )
  if("${CMAKE_Fortran_COMPILER_ID}" STREQUAL "PGI")
    add_definitions(-DSIMD_SIZE=1  -DVEC_LEN=32 -DUSE_GPU=1)
    set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -acc=gpu -cuda -gpu=cc70,cuda11.1,rdc")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} --relocatable-device-code=true")
    set(CMAKE_EXE_LINKER_FLAGS "-pgc++libs -acc=gpu -cuda -gpu=cc70,cuda11.1,rdc")
  else()
    message("Only PGI is supported for compiling fortran cuda code.\n Skip.")
    return()
  endif()
else()
  if( Kokkos_ENABLE_OPENMP )
    add_definitions(-fopenmp)
  endif()
  add_definitions(-DSIMD_SIZE=32 -DVEC_LEN=32 -DUSE_GPU=0)
endif()

include_directories(${MPI_Fortran_INCLUDE_PATH} ${CMAKE_CURRENT_BINARY_DIR}/../Fortran_features)
 
add_executable(AdvForPush ptl_module.F90 update_phase_module.F90 push_module.F90 cabana_cpp_interface.cpp main.f90)

target_link_libraries(AdvForPush Cabana::cabanacore ${MPI_Fortran_LIBRARIES} kokkos_fortran_wrappers)
add_test(NAME Fortran_advanced_example_02 COMMAND AdvForPush)
set_property(TARGET AdvForPush PROPERTY LINKER_LANGUAGE Fortran)
