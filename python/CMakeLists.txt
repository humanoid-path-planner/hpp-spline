STRING(REGEX REPLACE "-" "_" PY_NAME ${PROJECT_NAME})

# Define the wrapper library that wraps our library
add_library(${PY_NAME} SHARED spline_python.cpp)
target_link_libraries(${PY_NAME} eigenpy::eigenpy)
#~ target_link_libraries( centroidal_dynamics ${Boost_LIBRARIES} ${PROJECT_NAME} )
# don't prepend wrapper library name with lib
set_target_properties(${PY_NAME} PROPERTIES PREFIX "")

IF(APPLE)
  # We need to change the extension for python bindings
  SET_TARGET_PROPERTIES(${PY_NAME} PROPERTIES SUFFIX ".so")
ENDIF(APPLE)

INSTALL(TARGETS ${PY_NAME} DESTINATION ${PYTHON_SITELIB})

ADD_PYTHON_UNIT_TEST("python-spline" "python/test/test.py" "python")
