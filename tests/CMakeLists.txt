cmake_minimum_required(VERSION 3.28)

find_package(doctest CONFIG REQUIRED)

set(test_target "test_sparrow_ipc_lib")

set(
    SPARROW_IPC_TESTS_SRC
    include/sparrow_ipc_tests_helpers.hpp
    # TODO move all the files below under src?
    main.cpp
    test_utils.cpp
    test_primitive_array_serialization.cpp
    test_null_array_serialization.cpp
)

add_executable(${test_target} ${SPARROW_IPC_TESTS_SRC})
target_link_libraries(${test_target}
    PRIVATE
        sparrow-ipc
        doctest::doctest
)
target_include_directories(${test_target}
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/include
        ${CMAKE_BINARY_DIR}/generated
        ${CMAKE_SOURCE_DIR}/include
)
add_dependencies(${test_target} generate_flatbuffers_headers)
add_test(NAME sparrow-ipc-tests COMMAND ${test_target})
