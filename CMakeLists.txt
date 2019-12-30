cmake_minimum_required(VERSION 3.8)

project(CppNumericalSolvers
  VERSION 1.0.0
  DESCRIPTION "C++ numerical solvers for eigen3"
  LANGUAGES CXX
  )

# source files
set(header_files
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/boundedproblem.h
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/meta.h
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/problem.h
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/timer.h
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/linesearch/armijo.h
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/linesearch/morethuente.h
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/linesearch/wolfeheuristic.h
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/solver/bfgssolver.h
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/solver/cmaesbsolver.h
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/solver/cmaessolver.h
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/solver/conjugatedgradientdescentsolver.h
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/solver/gradientdescentsolver.h
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/solver/isolver.h
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/solver/lbfgsbsolver.h
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/solver/lbfgssolver.h
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/solver/neldermeadsolver.h
   ${CMAKE_CURRENT_SOURCE_DIR}/include/cppoptlib/solver/newtondescentsolver.h
   )

# main target
add_library(cppoptlib INTERFACE)
target_sources(cppoptlib INTERFACE "$<BUILD_INTERFACE:${header_files}>")
target_compile_features(cppoptlib INTERFACE cxx_std_11)
target_include_directories(cppoptlib SYSTEM INTERFACE
                               $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/>
                               $<INSTALL_INTERFACE:include/>)

# Setup package config
include(CMakePackageConfigHelpers)
set(CONFIG_PACKAGE_INSTALL_DIR lib/cmake/cppoptlib)

write_basic_package_version_file(
  cppoptlib-config-version.cmake
  COMPATIBILITY SameMajorVersion
  )

# installation
install(DIRECTORY include/cppoptlib
  DESTINATION include
  )

install(TARGETS cppoptlib
  EXPORT cppoptlib-targets
  DESTINATION lib
  )

install(EXPORT cppoptlib-targets
  DESTINATION ${CONFIG_PACKAGE_INSTALL_DIR}
  )

install(FILES
  ${CMAKE_CURRENT_SOURCE_DIR}/cppoptlib-config.cmake
  ${CMAKE_CURRENT_BINARY_DIR}/cppoptlib-config-version.cmake
  DESTINATION ${CONFIG_PACKAGE_INSTALL_DIR}
  # NAMESPACE cppoptlib::
  )

# subdirectories
option(CPPOPTLIB_BUILD_TEST "build tests of cppoptlib" OFF)
if(${CPPOPTLIB_BUILD_TEST} OR (CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR))
  # enable_testing()
  # add_subdirectory(src/test)
endif()

option(CPPOPTLIB_BUILD_EXAMPLES "build examples of cppoptlib" OFF)
if(BUILD_EXAMPLES)
  # add_subdirectory(src/examples)
endif()

# linear algebra library
# find_package (Eigen3 3.3 REQUIRED NO_MODULE)
