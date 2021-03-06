message(STATUS "Building UWUW unit tests")

# Libraries
set(LINK_LIBS)
list(APPEND LINK_LIBS pthread)
list(APPEND LINK_LIBS gtest)
if (BUILD_STATIC)
  list(APPEND LINK_LIBS dagmc-static)
  list(APPEND LINK_LIBS pyne_dagmc-static)
else()
  list(APPEND LINK_LIBS dagmc)
  list(APPEND LINK_LIBS pyne_dagmc)
endif (BUILD_STATIC)
list(APPEND LINK_LIBS ${DAG_LINK_LIBS})

# Include directories
include_directories(${CMAKE_SOURCE_DIR}/uwuw)
include_directories(${GTEST_INCLUDE_DIR})
include_directories(${CMAKE_SOURCE_DIR}/pyne)
include_directories(${HDF5_INCLUDE_DIRS})

# Setup unit tests
set(DRIVER uwuw_unit_test_driver.cpp)
setup_test(uwuw_unit_tests       cpp ${DRIVER} "${LINK_LIBS}")
setup_test(uwuw_unit_tests_tally cpp ${DRIVER} "${LINK_LIBS}")

# uwuw_preprocessor tally test source
list(APPEND LINK_LIBS uwuw_preprocessor)
setup_test(uwuw_unit_tests_preprocessor cpp ${DRIVER} "${LINK_LIBS}")

# Install h5m files
install(FILES test_uwuw.h5m DESTINATION tests)
install(FILES mat_lib.h5    DESTINATION tests)
install(FILES dag_file.h5m  DESTINATION tests)

# Enable unit tests
enable_testing()
