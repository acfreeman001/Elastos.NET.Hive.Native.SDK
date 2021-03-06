cmake_minimum_required(VERSION 3.5)
cmake_policy(VERSION 3.5)
project(elastos-hive)

set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)
include(HiveDefaults)
include(HivePackages)
include(AddSubModule)

if(WIN32)
    set(ENABLE_SHARED_DEFAULT TRUE)
    set(ENABLE_STATIC_DEFAULT FALSE)
else()
    set(ENABLE_SHARED_DEFAULT TRUE)
    set(ENABLE_STATIC_DEFAULT TRUE)
endif()

if(CMAKE_CROSSCOMPILING)
    set(ENABLE_APPS_DEFAULT FALSE)
    set(ENABLE_TESTS_DEFAULT FALSE)
else()
    set(ENABLE_APPS_DEFAULT TRUE)
    set(ENABLE_TESTS_DEFAULT TRUE)
endif()

set(ENABLE_SHARED ${ENABLE_SHARED_DEFAULT} CACHE BOOL "Build shared library")
set(ENABLE_STATIC ${ENABLE_STATIC_DEFAULT} CACHE BOOL "Build static library")
set(ENABLE_APPS ${ENABLE_APPS_DEFAULT} CACHE BOOL "Build demo applications")
set(ENABLE_TESTS ${ENABLE_TESTS_DEFAULT} CACHE BOOL "Build test cases")
set(ENABLE_DOCS FALSE CACHE BOOL "Build APIs documentation")

add_subdirectory(deps)
add_subdirectory(src)

if(ENABLE_TESTS)
    add_subdirectory(tests)
endif()

# Alias target for package. But it fix issue about making package
# inrelevant to udpatation of cmake scripts.
add_custom_target(dist
        COMMAND ${CMAKE_MAKE_PROGRAM} package
        COMMENT "Package Hive Distrubtion")