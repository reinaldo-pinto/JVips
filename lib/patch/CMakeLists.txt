cmake_minimum_required(VERSION 2.6)

project(imagequant C)

option(BUILD_SHARED_LIBS "Build shared libraries" OFF)
option(BUILD_WITH_SSE "Use SSE" ON)

if(BUILD_WITH_SSE)
  add_definitions(-DUSE_SSE=1)
endif()

include_directories(${CMAKE_SOURCE_DIR})

if(CMAKE_COMPILER_IS_GNUCC)
  add_compile_options("-std=c99")
endif()

add_library(imagequant SHARED 
  libimagequant.c
  blur.c
  mediancut.c
  mempool.c
  nearest.c
  pam.c
  kmeans.c)

install(TARGETS imagequant
  RUNTIME DESTINATION "${CMAKE_INSTALL_PREFIX}/bin"
  LIBRARY DESTINATION "${CMAKE_INSTALL_PREFIX}/lib")

install(FILES libimagequant.h
  DESTINATION "${CMAKE_INSTALL_PREFIX}/include")

configure_file("${CMAKE_SOURCE_DIR}/imagequant.pc.in.cmake"
  "${CMAKE_INSTALL_PREFIX}/lib/pkgconfig/imagequant.pc"
  @ONLY)
