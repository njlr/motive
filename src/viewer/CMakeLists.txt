# Copyright 2016 Google Inc. All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
cmake_minimum_required(VERSION 2.8.12)

project(fplviewer)

# This is the directory into which the executables are built.
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin/${CMAKE_SYSTEM_NAME})
set(tmp_dir ${CMAKE_BINARY_DIR}/obj)

# Add dependency on fplbase.
if (NOT TARGET fplbase)
  set(fplbase_build_samples OFF CACHE BOOL "")
  set(fplbase_build_shader_pipeline OFF CACHE BOOL "")
  set(fplbase_build_mesh_pipeline OFF CACHE BOOL "")
  add_subdirectory("${dependencies_fplbase_dir}" ${tmp_dir}/fplbase)
endif()
include_directories(${dependencies_fplbase_dir}/include)

# We use file_util.h from fplutil.
if (NOT TARGET fplutil)
  set(fplutil_build_tests OFF CACHE BOOL "")
  add_subdirectory("${dependencies_fplutil_dir}/libfplutil"
                   ${tmp_dir}/fplutil)
endif()

# We use the FlatBuffer formats defined by fplbase, so we need their includes.
get_property(FPLBASE_FLATBUFFERS_GENERATED_INCLUDES_DIR
  TARGET fplbase_generated_includes
  PROPERTY GENERATED_INCLUDES_DIR)
include_directories(${FPLBASE_FLATBUFFERS_GENERATED_INCLUDES_DIR})

# Create executable.
add_executable(fplviewer viewer.cpp)
add_dependencies(fplviewer fplbase motive)
mathfu_configure_flags(fplviewer)
target_link_libraries(fplviewer fplbase fplutil motive)
