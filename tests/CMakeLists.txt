cmake_minimum_required(VERSION 3.0 FATAL_ERROR)

project(neither_tests LANGUAGES CXX)

enable_testing()

# let's add GoogleTest to the mix
# source: https://github.com/google/googletest/blob/master/googletest/README.md

include(ExternalProject)

ExternalProject_Add(googletest
  GIT_REPOSITORY    https://github.com/google/googletest.git
  GIT_TAG           release-1.10.0
  SOURCE_DIR        "${CMAKE_CURRENT_BINARY_DIR}/googletest-src"
  BINARY_DIR        "${CMAKE_CURRENT_BINARY_DIR}/googletest-build"
  CONFIGURE_COMMAND ""
  BUILD_COMMAND     ""
  INSTALL_COMMAND   ""
  TEST_COMMAND      ""
)

add_subdirectory(
	${CMAKE_CURRENT_BINARY_DIR}/googletest-src
	${CMAKE_CURRENT_BINARY_DIR}/googletest-build
	EXCLUDE_FROM_ALL
	)

enable_testing()

# cmake function that adds a test
function(add_neither_test neither_testname neither_testfile)
	add_executable(${neither_testname} ${neither_testfile})
	add_dependencies(${neither_testname} googletest)
	target_link_libraries(${neither_testname}
		gtest_main
		neither
		)

	add_test(NAME    ${neither_testname} COMMAND ${neither_testname})
endfunction(add_neither_test)

add_neither_test(test_either either.cpp)
add_neither_test(test_lift lift.cpp)
add_neither_test(test_maybe maybe.cpp)
add_neither_test(test_try try.cpp)
