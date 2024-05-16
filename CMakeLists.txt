# Copyright 2024 ICUBE Laboratory, University of Strasbourg
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
#
# Author: Thibault Poignonec (thibault.poignonec@gmail.com)

cmake_minimum_required(VERSION 3.11)
project(dsp_filters_ros2)

# Setup
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17) # Default to C++17
  set(CMAKE_CXX_STANDARD_REQUIRED ON)
endif()

find_package(ament_cmake REQUIRED)
include(FetchContent)


set(DEPENDENCIES
  # None
)

foreach(Dependency IN ITEMS ${DEPENDENCIES})
  find_package(${Dependency} REQUIRED)
endforeach()


# Downdload SDK binaries
#------------------------
fetchcontent_declare(
  dsp_filters_binaries
  GIT_REPOSITORY https://github.com/vinniefalco/DSPFilters.git
  GIT_TAG        master
)
fetchcontent_makeavailable(dsp_filters_binaries)

# Set library paths
#-------------------------------
set(
  root_DIR
  ${dsp_filters_binaries_SOURCE_DIR}/shared/DSPFilters
)
set(include_DIR ${root_DIR}/include)
set(src_DIR ${root_DIR}/source)

# Retrieve all source files
file(GLOB SRC_FILES ${src_DIR}/*.cpp)

# Setup vendor library
#----------------------
add_library(${PROJECT_NAME} SHARED ${SRC_FILES})
target_compile_features(${PROJECT_NAME} PUBLIC cxx_std_17)
target_include_directories(${PROJECT_NAME} PUBLIC
  $<BUILD_INTERFACE:${include_DIR}>
  $<INSTALL_INTERFACE:include>
)
ament_target_dependencies(${PROJECT_NAME} PUBLIC ${DEPENDENCIES})
target_compile_definitions(${PROJECT_NAME} PRIVATE "DSP_FILTERS_BUILDING_DLL")


# Testing
#----------
if(BUILD_TESTING)
  # TODO: Add some tests?
endif()

# Install
#----------
install(
  DIRECTORY ${include_DIR}
  DESTINATION .
)

install(
  DIRECTORY ${include_DIR}/DspFilters/
  DESTINATION include/${PROJECT_NAME}
)

install(TARGETS ${PROJECT_NAME}
  EXPORT export_${PROJECT_NAME}
  RUNTIME DESTINATION bin
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
)

ament_export_targets(export_${PROJECT_NAME} HAS_LIBRARY_TARGET)
ament_export_dependencies(${DEPENDENCIES})
ament_package()
