find_package(GSL REQUIRED)

add_subdirectory("${PROJECT_SOURCE_DIR}/libs/googletest" "libs/googletest")

macro(package_add_test TESTNAME)
    # create an executable in which the tests will be stored
    add_executable(${TESTNAME} ${ARGN})
    # link the Google test infrastructure, mocking library, and a default main function to
    # the test executable.  Remove g_test_main if writing your own main function.
    target_link_libraries(${TESTNAME} gtest gmock gtest_main)
    # gtest_discover_tests replaces gtest_add_tests,
    # see https://cmake.org/cmake/help/v3.10/module/GoogleTest.html for more options to pass to it
    gtest_discover_tests(${TESTNAME}
        # set a working directory so your project root so that you can find test data via paths relative to the project root
        WORKING_DIRECTORY ${PROJECT_DIR}
        PROPERTIES VS_DEBUGGER_WORKING_DIRECTORY "${PROJECT_DIR}"
    )
    set_target_properties(${TESTNAME} PROPERTIES FOLDER test)
endmacro()

package_add_test(test_binary_search test_binary_search.cpp)

include_directories(${CMAKE_SOURCE_DIR}/include)
add_executable(test_swr test_swr.cpp)
target_link_libraries(test_swr sampen)

add_executable(test_grid test_grid.cpp)
target_link_libraries(test_grid sampen)
