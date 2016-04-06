add_subdirectory(test)


set(BITCOIN-CXX-SOURCES
        ${CMAKE_CURRENT_SOURCE_DIR}/crypter.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/db.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/rpcdump.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/rpcwallet.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/wallet.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/wallet_ismine.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/walletdb.cpp
        PARENT_SCOPE
        )