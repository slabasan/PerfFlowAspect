set(SMOKETESTS
    smoketest
    smoketest2
)

#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Xclang -load -Xclang ../weaver/weave/libWeavePass.so")
set(perfflow_deps "-L../runtime -lperfflow_runtime -lcrypto")

message(STATUS "Adding CXX unit tests")
foreach(TEST ${SMOKETESTS})
    message(STATUS " [*] Adding test: ${TEST}")
    add_executable(${TEST} ${TEST}.cpp)
    set_source_files_properties(${TEST}.cpp COMPILE_FLAGS "-v -std=c++11 -Xclang -load -Xclang ../weaver/weave/libWeavePass.so -fPIC")
    target_link_libraries(${TEST} ${perfflow_deps})
endforeach()

## copy test harness to build directory (with test executables)
#set(COPY_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}")
#set(COPY_DEST_DIR "${CMAKE_CURRENT_BINARY_DIR}")
#set(COPY_FILE_NAME "t0001-cbinding-basic.t")
#
#add_custom_command(
#    OUTPUT ${COPY_DEST_DIR}/${COPY_FILE_NAME}}
#    COMMAND ${CMAKE_COMMAND} -E copy ${COPY_SOURCE_DIR}/${COPY_FILE_NAME} ${COPY_DEST_DIR}/${COPY_FILE_NAME}
#)

install(TARGETS ${SMOKETESTS}
        DESTINATION test)

install(PROGRAMS ${CMAKE_CURRENT_SOURCE_DIR}/t0001-cbinding-basic.t
        DESTINATION test)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../../common/sharness
        DESTINATION ${CMAKE_INSTALL_PREFIX}/../common)
