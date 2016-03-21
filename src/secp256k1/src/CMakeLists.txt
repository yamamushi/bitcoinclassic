set(NAME SECP256K1)

include_directories(SYSTEM ${PROJECT_SOURCE_DIR}/include)

set(SECP256K1_SRC
        bench_ecdh.c
        bench_internal.c
        bench_recover.c
        bench_schnorr_verify.c
        bench_sign.c
        gen_context.c
        secp256k1.c
        tests.c
        )

add_subdirectory(java)
add_subdirectory(modules)

add_library(${NAME} SHARED ${SECP256K1_SRC})
add_library(${NAME}-static STATIC ${SECP256K1_SRC})

set_target_properties(${NAME}-static PROPERTIES OUTPUT_NAME ${NAME})
