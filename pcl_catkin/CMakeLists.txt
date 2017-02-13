cmake_minimum_required(VERSION 2.8.3)
project(pcl_catkin)

find_package(catkin_simple REQUIRED)

catkin_simple()

include(ExternalProject)

set(PCL_CATKIN_INCLUDE ${CATKIN_DEVEL_PREFIX}/include/pcl-1.8)
file(MAKE_DIRECTORY ${PCL_CATKIN_INCLUDE})

if(NOT CMAKE_BUILD_TYPE)
  MESSAGE(FATAL_ERROR "CMAKE_BUILD_TYPE must not be empty!")
endif()
string(TOUPPER ${CMAKE_BUILD_TYPE} UC_BUILD_TYPE)

ExternalProject_Add(pcl_src
  GIT_REPOSITORY  https://github.com/ethz-asl/pcl
  GIT_TAG pcl_1.8.0_devel
  UPDATE_COMMAND ""
  CMAKE_ARGS -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX}
             -DCMAKE_BUILD_TYPE:STRING=${CMAKE_BUILD_TYPE}
             "-DCMAKE_CXX_FLAGS_${UC_BUILD_TYPE}=${CMAKE_CXX_FLAGS_${UC_BUILD_TYPE}} --std=c++0x"
)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} pcl_src)

cs_install()
install(DIRECTORY ${PCL_CATKIN_INCLUDE}
  DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.hpp"
  PATTERN "*.h"
  PATTERN ".svn" EXCLUDE
)

cs_export(CFG_EXTRAS pcl-extras.cmake)
