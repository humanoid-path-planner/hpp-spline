cmake_minimum_required(VERSION 3.1)

SET(PROJECT_NAME hpp-spline)
SET(PROJECT_DESCRIPTION "template based classes for creating and manipulating spline and bezier curves. Comes with extra options specific to end-effector trajectories in robotics.")

INCLUDE(cmake/hpp.cmake)
INCLUDE(cmake/python.cmake)
INCLUDE(cmake/boost.cmake)

# Disable -Werror on Unix for now.
SET(CXX_DISABLE_WERROR True)
SET(CMAKE_VERBOSE_MAKEFILE True)
SET(DOXYGEN_USE_MATHJAX YES)
SET(PROJECT_USE_CMAKE_EXPORT True)

ADD_PROJECT_DEPENDENCY(Eigen3 REQUIRED)

COMPUTE_PROJECT_ARGS(PROJECT_ARGS LANGUAGES CXX)
PROJECT(${PROJECT_NAME} ${PROJECT_ARGS})

OPTION (BUILD_PYTHON_INTERFACE "Build the python binding" ON)
IF(BUILD_PYTHON_INTERFACE)
  # search for python
  FINDPYTHON()
  SEARCH_FOR_BOOST_PYTHON(REQUIRED)
  ADD_PROJECT_DEPENDENCY(eigenpy REQUIRED)
  add_subdirectory(python)
ENDIF(BUILD_PYTHON_INTERFACE)

SET(${PROJECT_NAME}_HEADERS
  include/hpp/spline/bernstein.h
  include/hpp/spline/bezier_polynom_conversion.h
  include/hpp/spline/curve_abc.h
  include/hpp/spline/exact_cubic.h
  include/hpp/spline/MathDefs.h
  include/hpp/spline/polynom.h
  include/hpp/spline/spline_deriv_constraint.h
  include/hpp/spline/bezier_curve.h
  include/hpp/spline/cubic_spline.h
  include/hpp/spline/curve_constraint.h
  include/hpp/spline/quintic_spline.h
  include/hpp/spline/helpers/effector_spline.h
  include/hpp/spline/helpers/effector_spline_rotation.h
  )

ADD_LIBRARY(${PROJECT_NAME} INTERFACE)
TARGET_INCLUDE_DIRECTORIES(${PROJECT_NAME} INTERFACE ${EIGEN3_INCLUDE_DIR} $<INSTALL_INTERFACE:include>)
INSTALL(TARGETS ${PROJECT_NAME} EXPORT ${TARGETS_EXPORT_NAME} DESTINATION lib)

ADD_SUBDIRECTORY(tests)
