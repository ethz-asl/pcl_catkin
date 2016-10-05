cmake_minimum_required(VERSION 2.8.3)
project(pcl_catkin)

find_package(catkin_simple REQUIRED)

catkin_simple()

include(ExternalProject)

set(PCL_CATKIN_INCLUDE ${CATKIN_DEVEL_PREFIX}/include/pcl)
file(MAKE_DIRECTORY ${PCL_CATKIN_INCLUDE})

ExternalProject_Add(pcl_src
  GIT_REPOSITORY  https://github.com/PointCloudLibrary/pcl
  GIT_TAG pcl-1.8.0
  UPDATE_COMMAND ""
  CMAKE_ARGS -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX}
             -DCMAKE_BUILD_TYPE:STRING=Release)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} pcl_src)

cs_install()
install(DIRECTORY ${PCL_CATKIN_INCLUDE}
  DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.hpp"
  PATTERN "*.h"
  PATTERN ".svn" EXCLUDE
)

cs_export(INCLUDE_DIRS ${PCL_CATKIN_INCLUDE}
CFG_EXTRAS pcl-extras.cmake)
