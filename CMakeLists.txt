cmake_minimum_required(VERSION 3.10)
project(simple C)
include(GNUInstallDirs)

add_library(private STATIC private.c)
target_include_directories(private PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>)
add_library(public public.c)
target_link_libraries(public PRIVATE private)
if(NOT BUILD_SHARED_LIBS)
  install(TARGETS private EXPORT Public_Targets LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR} ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})
endif()

install(TARGETS public EXPORT Public_Targets LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR} ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})
install(EXPORT Public_Targets FILE Public_Targets.cmake NAMESPACE Public:: DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/Public)
