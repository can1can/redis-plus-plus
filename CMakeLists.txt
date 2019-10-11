project(redis++)

cmake_minimum_required(VERSION 3.13)
set(CMAKE_CXX_FLAGS "-std=c++11 -Wall -W -Werror -fPIC")

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_CXX_STANDARD 11)

set(PROJECT_SOURCE_DIR ${PROJECT_SOURCE_DIR}/src/sw/redis++)

file(GLOB PROJECT_SOURCE_FILES "${PROJECT_SOURCE_DIR}/*.cpp")

#set(STATIC_LIB static)
set(SHARED_LIB shared)

#add_library(${STATIC_LIB} STATIC ${PROJECT_SOURCE_FILES})
add_library(${SHARED_LIB} SHARED ${PROJECT_SOURCE_FILES})

set(HIREDIS /tmp/3rdparty/stage/lib)
#target_include_directories(${STATIC_LIB} PUBLIC HIREDIS)
target_include_directories(${SHARED_LIB} PUBLIC HIREDIS)

set(HIREDIS_LIB hiredis)
target_link_libraries(${SHARED_LIB} ${HIREDIS_LIB})

#set_target_properties(${STATIC_LIB} PROPERTIES OUTPUT_NAME ${PROJECT_NAME})
set_target_properties(${SHARED_LIB} PROPERTIES OUTPUT_NAME ${PROJECT_NAME})

#set_target_properties(${STATIC_LIB} PROPERTIES CLEAN_DIRECT_OUTPUT 1)
set_target_properties(${SHARED_LIB} PROPERTIES CLEAN_DIRECT_OUTPUT 1)

#install(TARGETS ${STATIC_LIB} ARCHIVE DESTINATION lib)

install(TARGETS ${SHARED_LIB}
        LIBRARY DESTINATION lib)

set(HEADER_PATH "sw/redis++")
file(GLOB HEADERS "${PROJECT_SOURCE_DIR}/*.h*")
install(FILES ${HEADERS} DESTINATION ${CMAKE_INSTALL_PREFIX}/include/${HEADER_PATH})
