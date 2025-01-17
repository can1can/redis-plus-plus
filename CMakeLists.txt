project(redis++)

cmake_minimum_required(VERSION 3.13)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_CXX_STANDARD 11)
include(${CMAKE_ROOT}/Modules/ExternalProject.cmake)
ExternalProject_Add( hiredis GIT_REPOSITORY "https://github.com/redis/hiredis.git"
        GIT_TAG "master"
        UPDATE_COMMAND ""
        PATCH_COMMAND ""
        CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=/tmp/3rdparty/stage
        TEST_COMMAND "" )

set(PROJECT_SOURCE_DIR ${PROJECT_SOURCE_DIR}/src/sw/redis++)

file(GLOB PROJECT_SOURCE_FILES "${PROJECT_SOURCE_DIR}/*.cpp")

set(STATIC_LIB static)
set(SHARED_LIB shared)

add_library(${STATIC_LIB} STATIC ${PROJECT_SOURCE_FILES})
add_library(${SHARED_LIB} SHARED ${PROJECT_SOURCE_FILES})


target_include_directories(${STATIC_LIB} PUBLIC HIREDIS)
target_include_directories(${SHARED_LIB} PUBLIC HIREDIS)

set(HIREDIS_LIB /tmp/3rdparty/stage/lib/libhiredis.so)
target_link_libraries(${SHARED_LIB} ${HIREDIS_LIB})

set_target_properties(${STATIC_LIB} PROPERTIES OUTPUT_NAME ${PROJECT_NAME})
set_target_properties(${SHARED_LIB} PROPERTIES OUTPUT_NAME ${PROJECT_NAME})

set_target_properties(${STATIC_LIB} PROPERTIES CLEAN_DIRECT_OUTPUT 1)
set_target_properties(${SHARED_LIB} PROPERTIES CLEAN_DIRECT_OUTPUT 1)

install(TARGETS ${STATIC_LIB}
        ARCHIVE DESTINATION lib)

install(TARGETS ${SHARED_LIB}
        LIBRARY DESTINATION lib)

set(HEADER_PATH "sw/redis++")
file(GLOB HEADERS "${PROJECT_SOURCE_DIR}/*.h*")
install(FILES ${HEADERS} DESTINATION ${CMAKE_INSTALL_PREFIX}/include/${HEADER_PATH})