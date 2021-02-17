add_executable(ForHelloWorld main.cpp hello_world.f90)
set(CMAKE_EXE_LINKER_FLAGS "-pgc++libs")
target_link_libraries(ForHelloWorld Cabana::cabanacore)
add_test(NAME Fortran_tutorial_01 COMMAND ForHelloWorld)
