cmake_minimum_required(VERSION 2.8)

project(SECP256K1)
set(PROJECT_NAME SECP256K1)

#-----------------------------------------------------------------------------
# Options for building

option(BUILD_VERBOSE       "Verbose build output." ON)
option(BUILD_TESTS         "Build the tests." OFF)
option(BUILD_BENCHMARK     "Build the benchmark." OFF)

option(ENDOMORPHISM                  "Enable  endomorphism." OFF)
set(FIELD  "builtin"       CACHE <TYPE>  "Field operations: gmp|64bit|64bit_asm|32bit|builtin")
set(SCALAR "builtin"       CACHE <TYPE>  "SCALAR operations: gmp|64bit|64bit_asm|32bit|builtin")
set(BIGNUM "none"       CACHE <TYPE>  "Bignum Implementation: gmp|openssl|none")


if(BUILD_VERBOSE)
    set(CMAKE_VERBOSE_MAKEFILE true)
endif()

#-----------------------------------------------------------------------------
# Print system information

message(STATUS "CMake version:   ${CMAKE_VERSION}")
message(STATUS "System:          ${CMAKE_SYSTEM}")
message(STATUS "Processor:       ${CMAKE_SYSTEM_PROCESSOR}")

message(STATUS "Verbose:         ${SECP256K1_BUILD_VERBOSE}")
message(STATUS "Testing:                ${BUILD_TESTS}")
message(STATUS "Benchmark:              ${BUILD_BENCHMARK}")

message(STATUS "Build Options -------------------------------")
message(STATUS "Endomorphism:           ${ENDOMORPHISM}")
message(STATUS "Field operations:       ${FIELD}")
message(STATUS "Scalar operations:       ${SCALAR}")
message(STATUS "Bignum Implementation:  ${BIGNUM}")

#-----------------------------------------------------------------------------
# Collect into bin or lib subdirectories

SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/lib
        CACHE PATH "Single directory for all static libraries.")
SET(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/lib
        CACHE PATH "Single directory for all dynamic libraries on Unix.")

#-----------------------------------------------------------------------------
# Check which compiler to use and set compiler flags

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99")

if(MSVC)
    add_definitions(-D_UNICODE)
endif()

message(STATUS "C Compiler ID: ${CMAKE_C_COMPILER_ID}")
message(STATUS "C Flags:       ${CMAKE_C_FLAGS}")
message(STATUS "C link flags:  ${CMAKE_C_LINK_FLAGS}")

message(STATUS "C++ Compiler ID: ${CMAKE_CXX_COMPILER_ID}")
message(STATUS "C++ Flags:       ${CMAKE_CXX_FLAGS}")
message(STATUS "C++ link flags:  ${CMAKE_CXX_LINK_FLAGS}")

#-----------------------------------------------------------------------------
# System libraries used for linking.

if(UNIX AND NOT APPLE)
    list(APPEND OPENTXS_SYSTEM_LIBRARIES rt)
    list(APPEND OPENTXS_SYSTEM_LIBRARIES dl)
endif()

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


#-----------------------------------------------------------------------------
# Collect all binaries into bin subdirectory

set(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/bin)
set(LIBRARY_OUTPUT_PATH  ${CMAKE_CURRENT_BINARY_DIR}/lib)


#-----------------------------------------------------------------------------
# Source Definitions

if(ENDOMORPHISM)
    add_definitions(-DUSE_ENDOMORPHISM)
endif()

if(FIELD MATCHES 64bit_asm)
    if(NOT "${CMAKE_SYSTEM_PROCESSOR}" MATCHES "x86_64")
        message(FATAL_ERROR "64bit_asm field support explicitly requested but is not compatible with this host.")
    else()
        enable_language(ASM_NASM)
        if("ASM${ASM_DIALECT}" STREQUAL "YASM")
            add_definitions(-DUSE_FIELD_5X52)
            add_definitions(-DUSE_FIELD_5X52_ASM)
            set(USE_ASM 1)
        else()
            message(FATAL_ERROR "64bit_asm field support explicitly requested but yasm was not found.")
        endif()
    endif()
elseif(FIELD MATCHES gmp) #default
    set(USE_GMP 1)
    add_definitions(-DUSE_FIELD_GMP)
    add_definitions(-DHAVE_LIBGMP)
elseif(FIELD MATCHES 64bit)
    add_definitions(-DUSE_FIELD_5X52)
    add_definitions(-DUSE_FIELD_5X52_INT128)
elseif(FIELD MATCHES 32bit)
    add_definitions(-DUSE_FIELD_10X26)
elseif(FIELD MATCHES builtin)
    add_definitions(-DUSE_FIELD_INV_BUILTIN)
    add_definitions(-DUSE_FIELD_10X26)
else()
    message(FATAL_ERROR "Invalid field implementation.")
endif()

if(SCALAR MATCHES builtin) #default
    add_definitions(-DUSE_SCALAR_INV_BUILTIN)
    add_definitions(-DUSE_SCALAR_8X32)
endif()


if(BIGNUM MATCHES gmp) #default
    set(USE_GMP 1)
    add_definitions(-DHAVE_LIBGMP)
    add_definitions(-DUSE_NUM_GMP)
    add_definitions(-DUSE_FIELD_INV_NUM)
elseif(BIGNUM MATCHES openssl)
    find_package(OpenSSL REQUIRED)
    set(USE_SSL 1)
    add_definitions(-DUSE_NUM_OPENSSL)
    add_definitions(-DUSE_FIELD_INV_BUILTIN)
elseif(BIGNUM MATCHES none)
    add_definitions(-DUSE_NUM_NONE)
else()
    message(FATAL_ERROR "Invalid field implementation.")
endif()

if(USE_GMP)
    find_path(GMP_INCLUDE_DIR gmp.h)
    if(GMP_INCLUDE_DIR STREQUAL "GMP_INCLUDE_DIR-NOTFOUND")
        message(FATAL_ERROR "Couldn't find gmp.h. Please install GMP.")
    else()
        message(STATUS "Found GMP: ${GMP_INCLUDE_DIR}")
    endif()
endif()



#-----------------------------------------------------------------------------
# Build source

#Fix RPATH
set(CMAKE_MACOSX_RPATH 1)

SET(CMAKE_INCLUDE_CURRENT_DIR TRUE)

add_subdirectory(include)
add_subdirectory(src)
