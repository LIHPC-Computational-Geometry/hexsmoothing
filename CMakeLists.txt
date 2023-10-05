cmake_minimum_required(VERSION 3.11)
project(hexsmoothing)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(HEXSMOOTHING_LIB_ONLY "Compile the hexsmoothing library only (no executables)" ON)

include_directories(ext/stlbfgs)

find_package(OpenMP)
if(OPENMP_FOUND)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

if (WIN32) 
	set(BUILD_SHARED_LIBS OFF)
endif()

file(GLOB SOURCES utils/*.cpp utils/*.h )
add_library(libBasicUtilies ${SOURCES})

file(GLOB SOURCES lib/*.cpp lib/*.h )
add_library(libhexsmoothing ${SOURCES})
target_link_libraries(libhexsmoothing libBasicUtilies)
target_include_directories(libhexsmoothing PUBLIC ${CMAKE_CURRENT_SOURCE_DIR} ext/)
target_include_directories(libhexsmoothing PUBLIC ext/)

if(OpenMP_CXX_FOUND)
    target_link_libraries(libhexsmoothing OpenMP::OpenMP_CXX)
endif()

if(${HEXSMOOTHING_LIB_ONLY} MATCHES OFF)
  # we need a library to handle mesh loading
  # download ultimaille and include it
  include(FetchContent)
  FetchContent_Declare(
    ultimaille
    URL https://github.com/ssloy/ultimaille/archive/17514ea34bad46f62ab3e3f08ca93c03c371bb18.zip # constexpr issues after this commit
  )
  FetchContent_MakeAvailable(ultimaille)
  include_directories(${ultimaille_SOURCE_DIR})
  # declare executables
  add_executable(elliptic_smoothing ${SOURCES} bin/elliptic_smoothing.cpp)
  target_link_libraries(elliptic_smoothing libhexsmoothing ultimaille)
  add_executable(standard_smoother ${SOURCES} bin/standard_smoother.cpp)
  target_link_libraries(standard_smoother libhexsmoothing ultimaille)
  add_executable(vertex_based_smoothing ${SOURCES} bin/vertex_based_smoothing.cpp)
  target_link_libraries(vertex_based_smoothing libhexsmoothing ultimaille)
endif()