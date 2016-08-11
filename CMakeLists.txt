cmake_minimum_required(VERSION 2.8.3)
project(pcl_catkin)

find_package(catkin_simple REQUIRED)

catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/pcl)

ExternalProject_Add(pcl_src
                    GIT_REPOSITORY  https://github.com/PointCloudLibrary/pcl
                    UPDATE_COMMAND ""
                    CMAKE_ARGS -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX} -DCMAKE_BUILD_TYPE:STRING=Release)


cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} pcl_src)

cs_install()
install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/pcl
  DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.hpp"
  PATTERN "*.h"
  PATTERN ".svn" EXCLUDE
)

cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include/pcl/
CFG_EXTRAS pcl-extras.cmake)
