cmake_minimum_required(VERSION 2.8)

SET(PROJECT_NAME hpp-spline)
SET(PROJECT_DESCRIPTION "template based classes for creating and manipulating spline and bezier curves. Comes with extra options specific to end-effector trajectories in robotics.")

INCLUDE(cmake/hpp.cmake)
INCLUDE(cmake/test.cmake)
INCLUDE(cmake/python.cmake)

# Disable -Werror on Unix for now.
SET(CXX_DISABLE_WERROR True)
SET(CMAKE_VERBOSE_MAKEFILE True)

find_package(Eigen3 REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIR})

COMPUTE_PROJECT_ARGS(PROJECT_ARGS LANGUAGES CXX)
PROJECT(${PROJECT_NAME} ${PROJECT_ARGS})

OPTION (BUILD_PYTHON_INTERFACE "Build the python binding" ON)
IF(BUILD_PYTHON_INTERFACE)
  # search for python
  FINDPYTHON()
  include_directories( ${PYTHON_INCLUDE_DIRS} )

  find_package( Boost COMPONENTS python REQUIRED )
  include_directories( ${Boost_INCLUDE_DIR} )

  add_subdirectory (python)
ENDIF(BUILD_PYTHON_INTERFACE)

ADD_SUBDIRECTORY(include/hpp/spline)
ADD_SUBDIRECTORY(tests)
