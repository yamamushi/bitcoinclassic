cmake_minimum_required(VERSION 2.8)

project(leveldb)
set(PROJECT_NAME leveldb)
set(NAME leveldb)

#-----------------------------------------------------------------------------
# Options for building

option(LEVELDB_BUILD_VERBOSE       "Verbose build output." ON)

if(LEVELDB_BUILD_VERBOSE)
    set(CMAKE_VERBOSE_MAKEFILE true)
endif()

#-----------------------------------------------------------------------------
# Print system information

message(STATUS "CMake version:   ${CMAKE_VERSION}")
message(STATUS "System:          ${CMAKE_SYSTEM}")
message(STATUS "Processor:       ${CMAKE_SYSTEM_PROCESSOR}")

message(STATUS "Verbose:         ${UNIVALUE_BUILD_VERBOSE}")

#-----------------------------------------------------------------------------
# Collect into bin or lib subdirectories

SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/lib
        CACHE PATH "Single directory for all static libraries.")
SET(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/lib
        CACHE PATH "Single directory for all dynamic libraries on Unix.")

#-----------------------------------------------------------------------------
# Check which compiler to use

message(STATUS "C++ Compiler ID: ${CMAKE_CXX_COMPILER_ID}")
message(STATUS "C++ Flags:       ${CMAKE_CXX_FLAGS}")
message(STATUS "C++ link flags:  ${CMAKE_CXX_LINK_FLAGS}")

#-----------------------------------------------------------------------------
# System libraries used for linking.


#-----------------------------------------------------------------------------
# Third party libraries

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")

#-----------------------------------------------------------------------------
# Force out-of-source build

if(${CMAKE_CURRENT_SOURCE_DIR} STREQUAL ${CMAKE_CURRENT_BINARY_DIR})
    message(FATAL_ERROR "CMake generation is not allowed within the source directory!
Remove the CMakeCache.txt file and try again from another folder, e.g.:

   rm CMakeCache.txt
   mkdir build
   cd build
   cmake ..
")
endif()

#-----------------------------------------------------------------------------
# Default to Release build

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE STRING
            "Choose the type of build, options are: None Debug Release RelWithDebInfo MinSizeRel."
            FORCE)
endif()

set(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/bin)
set(LIBRARY_OUTPUT_PATH  ${CMAKE_CURRENT_BINARY_DIR}/lib)

#-----------------------------------------------------------------------------
# Misc

if(CMAKE_COMPILER_IS_GNUCXX)
    message("Compiler: Gnu")
    set(COMMON_FLAGS "${COMMON_FLAGS} -fno-builtin-memcmp")
endif()

if(${CMAKE_SYSTEM_NAME} MATCHES "CYGWIN_*")
    message("Platform: Cygwin")
    set(PLATFORM OS_LINUX)
    set(COMMON_FLAGS "${COMMON_FLAGS} -lpthread -DOS_LINUX -DCYGWIN")
elseif(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    set(PLATFORM OS_MACOSX)
    set(COMMON_FLAGS "${COMMON_FLAGS} -DOS_MACOSX -DLEVELDB_PLATFORM_POSIX")
    add_definitions(-D LEVELDB_PLATFORM_POSIX -D OS_MACOSX)
endif()

#-----------------------------------------------------------------------------
# MEMENV_SOURCES=helpers/memenv/memenv.cc

include_directories(SYSTEM
        ${PROJECT_SOURCE_DIR}
        ${PROJECT_SOURCE_DIR}/db
        ${PROJECT_SOURCE_DIR}/include
        ${PROJECT_SOURCE_DIR}/port
        )
set(CMAKE_INCLUDE_CURRENT_DIR TRUE)

add_subdirectory(db)
add_subdirectory(helpers)
add_subdirectory(include)
add_subdirectory(table)
add_subdirectory(util)
add_subdirectory(port)


#Fix RPATH
set(CMAKE_MACOSX_RPATH 1)

add_library(${NAME} SHARED ${LEVELDB-CXX-SRC} ${LEVELDB-CXX-HEADERS})
add_library(${NAME}-static STATIC ${LEVELDB-CXX-SRC} ${LEVELDB-CXX-HEADERS})
