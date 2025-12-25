cmake_minimum_required(VERSION 3.21)
project(yuni-duktape)

cmake_path(SET dukroot NORMALIZE 
    ${CMAKE_CURRENT_LIST_DIR}/../../ext/js/duktape2)
set(duk-srcroot ${dukroot}/src-separate)
set(duk-extras ${dukroot}/extras)
include_directories(${duk-srcroot} ${duk-extras}/print-alert)

add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/../../nccc/js/duktape/duk-nccc
    duk-nccc)

# YuniDuk

# FIXME: Tentative
cmake_path(SET srcroot NORMALIZE
    ${CMAKE_CURRENT_LIST_DIR}/../../nccc/js/duktape/duk-yuni)
add_definitions("-DSCRIPTROOT=\"${srcroot}\""
    #-DDUK_USE_DEBUG=1
    #-DDUK_USE_DEBUG_LEVEL=1
    #-DDUK_USE_DEBUG_WRITE=dukdebugwrite
)


# FIXME: Check if we stil need this
if(WIN32)
    add_definitions(
        -DDUK_USE_GET_MONOTONIC_TIME_WINDOWS_QPC=1
        )
else()
    add_definitions(
        -DDUK_USE_GET_MONOTONIC_TIME_CLOCK_GETTIME=1
        )
endif()

include(${CMAKE_CURRENT_LIST_DIR}/duk-srcs.cmake)

set(srcs)
foreach(e IN LISTS duk-srcs)
    list(APPEND srcs ${duk-srcroot}/${e})
endforeach()

list(APPEND srcs ${duk-extras}/print-alert/duk_print_alert.c)

# Loader
cmake_path(SET yuniduk 
    NORMALIZE
    ${CMAKE_CURRENT_LIST_DIR}/../../nccc/js/duktape/duk-yuni)

list(APPEND srcs ${yuniduk}/yuniduk.c)

add_executable(yuniduk ${srcs})
target_link_libraries(yuniduk duk-nccc)

