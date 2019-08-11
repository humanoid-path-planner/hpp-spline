cmake_minimum_required(VERSION 2.6)
project(spline)
INCLUDE(cmake/base.cmake)
INCLUDE(cmake/test.cmake)
INCLUDE(cmake/boost.cmake)
INCLUDE(cmake/hpp.cmake)
INCLUDE(cmake/python.cmake)

SET(PROJECT_NAME hpp-spline)
SET(PROJECT_DESCRIPTION
  "template based classes for creating and manipulating spline and bezier curves. Comes with extra options specific to end-effector trajectories in robotics."
  )

# Disable -Werror on Unix for now.
SET(CXX_DISABLE_WERROR True)
SET(CMAKE_VERBOSE_MAKEFILE True)

find_package(Eigen3 REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIR})

SETUP_HPP_PROJECT()

OPTION (BUILD_PYTHON_INTERFACE "Build the python binding" ON)
IF(BUILD_PYTHON_INTERFACE)
  # search for python
  FINDPYTHON()

  SET(BOOST_COMPONENTS python)
  SEARCH_FOR_BOOST()

  add_subdirectory (python)
ENDIF(BUILD_PYTHON_INTERFACE)


ADD_SUBDIRECTORY(include/hpp/spline)
ADD_SUBDIRECTORY(tests)

SETUP_HPP_PROJECT_FINALIZE()
