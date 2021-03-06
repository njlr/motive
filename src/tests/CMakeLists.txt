# Copyright (c) 2015 Google, Inc.
#
# This software is provided 'as-is', without any express or implied
# warranty.  In no event will the authors be held liable for any damages
# arising from the use of this software.
# Permission is granted to anyone to use this software for any purpose,
# including commercial applications, and to alter it and redistribute it
# freely, subject to the following restrictions:
# 1. The origin of this software must not be misrepresented; you must not
# claim that you wrote the original software. If you use this software
# in a product, an acknowledgment in the product documentation would be
# appreciated but is not required.
# 2. Altered source versions must be plainly marked as such, and must not be
# misrepresented as being the original software.
# 3. This notice may not be removed or altered from any source distribution.
cmake_minimum_required(VERSION 2.8.12)

project(motive_tests)

# gtest seems to prefer the non-DLL runtime on Windows, which conflicts with
# everything else.
option(
  gtest_force_shared_crt
  "Use shared (DLL) run-time lib even when Google Test is built as static lib."
  ON)

# Import gtest if it's not already present.
if(NOT TARGET gtest)
  set_compiler_flags_for_external_libraries()
  add_subdirectory(${dependencies_gtest_dir} googletest)
  restore_compiler_flags()
endif()

# Set some variables describing file locations.
set(GUNIT_INCDIR "${dependencies_gtest_dir}/include")
if(EXISTS "${dependencies_gtest_dir}/../../third_party")
  set(GUNIT_HACK_INCDIR "${dependencies_gtest_dir}/../..")
endif()
set(GTEST_LIBDIR "${dependencies_gtest_dir}")

# Include helper functions and macros used by Google Test.
include(${GTEST_LIBDIR}/cmake/internal_utils.cmake)
config_compiler_and_linker()
string(REPLACE "-W4" "-W3" cxx_default "${cxx_default}")
string(REPLACE "-Wshadow" "" cxx_default "${cxx_default}")
string(REPLACE "-Wextra" "" cxx_default "${cxx_default}")

# This is the directory into which the executables are built.
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/../../bin)

include_directories(${GUNIT_INCDIR}
                    ${GUNIT_HACK_INCDIR}
                    ${CMAKE_CURRENT_SOURCE_DIR}
                    ${dependencies_flatbuffers_dir}/include)

# Common libraries for tests.
if(NOT MSVC)
  find_package(Threads)
endif()

set(COMMON_LIBS "gtest;motive;${CMAKE_THREAD_LIBS_INIT}")

# PUT ADDITIONAL UNIT TEST BINARIES BELOW!
# The commands should be of the form:
#
# test_executable(<test-name>)
#
# Where <test-name> is the name of the output test executable and the basename
# of the source file for the test.  For example, test_executable(CallbackTests)
# generates an executable called CallbackTests which is the result of compiling
# CallbackTests.cpp.

function(test_executable name)
  cxx_executable_with_flags(${name}_test "${cxx_default}" "${COMMON_LIBS}"
      ${CMAKE_CURRENT_SOURCE_DIR}/${name}_test/${name}_test.cpp ${ARGN})
  mathfu_configure_flags(${name}_test)
  target_link_libraries(${name}_test flatbuffers)
endfunction()

test_executable(angle)
test_executable(curve)
test_executable(curve_util)
test_executable(float)
test_executable(motive)
test_executable(range)
test_executable(spline)
test_executable(table)

