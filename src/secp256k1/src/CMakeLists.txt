set(NAME SECP256K1)

include_directories(SYSTEM
        ${PROJECT_SOURCE_DIR}
        ${PROJECT_SOURCE_DIR}/include
        ${PROJECT_SOURCE_DIR}/modules/ecdh
        ${PROJECT_SOURCE_DIR}/modules/recovery
        ${PROJECT_SOURCE_DIR}/modules/schnorr
        )

set(SECP256K1_SRC
        gen_context.c
        secp256k1.c
        )

add_subdirectory(java)
add_subdirectory(modules)

add_library(${NAME} SHARED ${SECP256K1_SRC})
add_library(${NAME}-static STATIC ${SECP256K1_SRC})

set_target_properties(${NAME} PROPERTIES OUTPUT_NAME ${NAME} COMPILE_FLAGS "-fPIC")

if(USE_SSL)
    include_directories(SYSTEM ${OPENSSL_INCLUDE_DIR})
    target_link_libraries(${NAME} ${OPENSSL_LIBRARIES})
    target_link_libraries(${NAME}-static ${OPENSSL_LIBRARIES})
endif()

if(USE_GMP)
    target_link_libraries(${NAME} gmp)
    target_link_libraries(${NAME}-static gmp)
endif()

set_target_properties(${NAME} PROPERTIES OUTPUT_NAME secp256k1)
set_target_properties(${NAME}-static PROPERTIES OUTPUT_NAME secp256k1)


#-----------------------------------------------------------------------------
# Build tests
if(BUILD_TESTS)
    add_definitions(-DENABLE_OPENSSL_TESTS)
    add_executable(tests tests.c)

    if(USE_GMP)
        target_link_libraries(tests secp256k1_static gmp crypto)
    else()
        target_link_libraries(tests secp256k1_static crypto)
    endif()

    enable_testing()
endif()


#-----------------------------------------------------------------------------
# Build benchmark

