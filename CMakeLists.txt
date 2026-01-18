cmake_minimum_required(VERSION 3.20)
project(nccc-integ)

cmake_path(SET NCCC_ROOT NORMALIZE ${CMAKE_CURRENT_LIST_DIR}/nccc)

# NCCC utility

# NCCC test modules
add_subdirectory(${NCCC_ROOT}/tests/testmodule nccc-test)

# Interpreters

## js: node (Binding only)
add_subdirectory(${NCCC_ROOT}/js/node/node-nccc node-nccc)

## js: Duktape2
add_subdirectory(proj/duktape duktape)

